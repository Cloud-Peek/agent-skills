---
name: release-candidate-review
description: Compare one or more release-candidate PRs against their design PR, produce a section-by-section pick-and-mix report (which RC implemented each part of the design best), identify design flaws needing amendment, and open a PR with the report on top of the design branch. Use when the user asks to review/compare RCs against a design, pick the best parts of competing implementations, or judge which RC to promote.
---

# Review release candidates against a design (pick and mix)

This skill captures the full workflow for adversarially reviewing N competing
release-candidate branches against a canonical design PR, deciding per design
section which RC implemented it best, extracting a composite promotion plan,
and feeding flaws back into the design. It was derived from the entity-access
review (design #1587 vs RC #1599 and #1601, report PR #1605) — the pitfalls
listed below are ones that actually occurred there.

## Inputs to collect first

- The **design PR** (number/URL) and its **canonical design document** — the PR
  body usually names one file as the primary review target; everything else on
  the design branch is supporting material.
- Each **RC PR** (number/URL). Read every PR body fully: RC bodies make
  concrete claims ("verified on live PG16", "97 tests", "nothing ever blocks")
  that the review must confirm or refute in code.
- The designated working branch for the report (if this is a remote session,
  it is assigned; otherwise create `<feature>-rc-review`).

## Step 1 — Fetch everything locally

```bash
git fetch origin <design-branch> <rc1-branch> <rc2-branch> main
```

Review from local refs (`git show origin/<branch>:<path>`,
`git diff origin/main...origin/<branch>`) — never via the GitHub file API; it
is too slow and lossy for whole-branch review. Get diffstats for each branch
to size the work.

## Step 2 — Read the canonical design doc yourself

Extract its heading structure (`grep -n '^#'`). The doc's sections become the
review framework. Save a copy to the scratchpad so subagents can read it
without touching the working tree. Do not delegate this step: the synthesis
quality depends on the orchestrator knowing the design cold.

## Step 3 — Fan out parallel adversarial review agents

Spawn **one agent per RC plus one agent against the design branch itself**, in
a single message so they run concurrently. All agents are read-only (`git
show` only, no checkouts — the orchestrator owns the working tree).

Each **RC agent** prompt must include:

- Path to the design-doc copy, with "READ IT FIRST".
- The RC's PR-body claims verbatim, with the instruction to verify each claim
  is actually true in code.
- A fixed section list to grade (derive from the design doc; for a
  permissions/data feature the canonical split is: A. schema/migrations,
  B. backfill, C. shared contracts package, D. core repository/service,
  E. API consumer/route coverage, F. observability/telemetry, G. tests,
  H. design deviations — each deviation flagged as *justified improvement* or
  *violation*).
- Per section: a **grade A–F**, what it does well, defects with `file:line`
  evidence, and a claims-vs-code verdict.
- Explicit adversarial targets: tenant-isolation gaps, fail-open paths,
  expiry/time-zone bugs, unbounded work, floating promises, missing routes
  from the design's mapping tables, and telemetry that lies.
- Cross-pollination of known findings: if one RC already found a bug (e.g. a
  nonexistent SQL builtin), tell the other RC's agent to check for the same
  bug explicitly.

The **design-review agent** hunts flaws in the design itself (doc + any shipped
foundation code): every SQL constraint expression, helper-function semantics,
index redundancy, internal doc inconsistencies (enum drift between doc and
shipped code), gaps an implementer will hit (mode toggles, failure contracts,
atomicity, lifecycle-on-delete, pagination tokens), and security-model holes
(privilege self-escalation, revocation latency, sentinel owners). Findings
ranked Critical/High/Medium/Low with concrete amendment recommendations, plus
a short strengths list.

Where feasible, have agents **execute** what they can: extract a pure package
from a branch and run its tests; note explicitly what was never executed
(migrations never applied to a real database are unverified, no matter how
thorough the string-matching tests look).

## Step 4 — Synthesize the report

Write `docs/<feature>-rc-review-report.md` with this structure:

1. **Executive summary** — grade table (sections × RCs, winner bolded), the
   promotion recommendation, and the release blockers stated plainly with the
   exact failing statement and blast radius.
2. **Section-by-section comparison** — per section: winner, why, what to port
   from the loser anyway, and shared debt both RCs carry.
3. **Composite RC plan** — concrete pick-and-mix: base RC, ordered graft list
   from the other RC(s), small fixes to make during promotion, and **merge
   gates** (process requirements like live-DB migration tests, HTTP-level
   response-invariance tests, and any latency-bound/availability decision that
   must be recorded explicitly).
4. **Design amendments** — numbered, severity-ordered, each with file:line
   evidence and a concrete amendment. Include amendments *proved* by the RCs:
   every place the two RCs invented **divergent** answers marks a design gap,
   and every bug **both** RCs inherited marks a design flaw plus a missing
   process gate.
5. **Design strengths confirmed by both implementations** — so the amendment
   list is not read as a rejection of the design.
6. **Suggested sequencing** — amend design → promote composite → CI gates →
   close the losing RC *with credit* for what survives in the composite.

## Step 5 — Branch, commit, PR

- Reset/create the working branch **on top of the design branch** (`git
  checkout -B <branch> origin/<design-branch>`) so the report lands next to
  the doc it amends.
- Commit the report, push, open the PR **with the design branch as base**, and
  subscribe to PR activity. Expect and address the first automated-review
  round (precision fixes to the report are normal).

## Pitfalls that actually happened (check every time)

- **Convergent inherited bugs.** Both RCs copied the design's
  `jsonb_object_length()` call — a builtin that does not exist in PostgreSQL.
  Any flaw in the design's shipped code will appear in every RC unless one
  caught it; grade the catching, and turn the flaw into both a design
  amendment and a CI merge gate.
- **String-match tests are not verification.** Schema-consistency tests that
  grep migration text can *assert the broken string*, locking the bug in.
  Only a live-database apply + insert counts as verifying SQL.
- **Failure-mode precision matters.** A `CHECK` calling a missing function
  directly fails at `CREATE TABLE` (DDL time); the same call inside a lazily
  parsed PL/pgSQL function applies fine and then rejects every insert at
  runtime — with `EXCEPTION WHEN OTHERS` masking the real error. Name the
  exact failing statement per branch; do not blur apply-time and request-time
  failures.
- **Migration numbering vs a moving main.** Designs that hardcode a migration
  number go stale; check what main has (`git ls-tree origin/main:<migrations
  dir>`) and verify each RC renumbered.
- **PR-body claims vs code.** Check each claim individually: "byte-identical
  responses" may be tested for one route only; "everything behind the flag"
  may exclude a throwing provisioning path; "250ms deadline" may race the
  product operation and fabricate fault telemetry on ordinarily slow requests.
- **Look for what the loser still wins.** Even a release-blocked RC usually
  contains graft-worthy parts (stricter validators, privacy-bounded
  telemetry, typed faults, membership contracts). The deliverable is a
  composite plan, not just a verdict.
- **Un-executed admissions point at the defects.** When an RC's PR admits a
  verification gap ("no live migration run", "no HTTP coverage"), the
  release-blocking defect is usually inside exactly that gap.