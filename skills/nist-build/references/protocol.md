# NIST SSDF run protocol

Shared procedure for the six `nist-*` phase skills. Two protocols live here:
the **phase walkthrough protocol** (run by the skill itself) and the
**practice epic protocol** (run once per practice, from a `references/<practice>.md`
file).

Both file GitHub issues. Nothing here writes code.

## Conventions

| Thing | Shape | Example |
| --- | --- | --- |
| Phase label | `nist-phase-<N>-<slug>` | `nist-phase-1-plan` |
| Practice label | `nist-<practice-id-no-dot>` | `nist-po1` |
| Phase epic title | `[NIST Phase <N>] <PHASE_NAME>` | `[NIST Phase 1] Plan & Requirements` |
| Practice epic title | `[NIST <PRACTICE_ID>] <PRACTICE_NAME>` | `[NIST PO.1] Define Security Requirements for Software Development` |

Practice IDs come from NIST SP 800-218 (SSDF v1.1). Task text is quoted verbatim
from the standard; the implementation notes under each task are guidance, not NIST
text, and are meant to be adapted to the repo you are running in.

## Preconditions

Run these once, before either protocol. Stop and tell the user if any fail.

1. `gh auth status` — the GitHub CLI must be authenticated.
2. `gh repo view --json nameWithOwner` — resolves the target repo. If the user
   meant a different repo, ask before proceeding.
3. Labels exist. For the labels this run needs (see the phase/practice file),
   check with `gh label list --search <label>` and create any that are missing:

   ```bash
   gh label create <label> --description "<description>" --color 0E8A16
   ```

   Never file an issue with a label that does not exist — `gh` will fail the
   whole call.

## Phase walkthrough protocol

Substitute `<PHASE_N>`, `<PHASE_NAME>`, `<PHASE_LABEL>`, and `<PRACTICES>` from
the calling skill.

**Step 1 — Confirm scope.** Tell the user which phase you are about to walk, list
the practices in it, and confirm they want to file issues in the resolved repo.
This protocol creates real GitHub issues; do not create any without confirmation.

**Step 2 — Find or create the phase epic.** Search first:

```bash
gh issue list --label "<PHASE_LABEL>" --state all --search "[NIST Phase <PHASE_N>] in:title" --json number,title,state,url
```

If an open epic exists, reuse it and say so. If a closed one exists, ask whether
to reopen or supersede. Otherwise draft a new epic and show it to the user before
creating:

- Title: `[NIST Phase <PHASE_N>] <PHASE_NAME>`
- Labels: `<PHASE_LABEL>`
- Body: the phase's "What this phase is about" section, then a checklist with one
  unchecked line per practice, to be filled in with issue links as you go:

  ```markdown
  ## Practices
  - [ ] <PRACTICE_ID> — <PRACTICE_NAME>
  ```

**Step 3 — Walk the practices in order.** The order in the calling skill is
deliberate: each practice assumes the previous one landed.

**Step 4 — For each practice.** This is the loop; repeat until every practice in
the phase is resolved.

1. Read the practice's reference file (`references/<practice-id-no-dot>.md`).
   Load it only when you reach it — not up front.
2. Check whether its epic already exists:

   ```bash
   gh issue list --label "<PRACTICE_LABEL>" --state all --json number,title,state,url
   ```

3. Report to the user: the practice, what it covers in one line, and whether an
   epic already exists.
4. Ask whether to **file** it (no epic yet), **link** it (epic exists — just
   record it in the phase epic), or **skip** it (not applicable to this repo; ask
   for a one-line reason and note it in the phase epic).
5. If filing: run the practice epic protocol below.
6. Update the phase epic checklist — tick the line and append the issue link:

   ```markdown
   - [x] PO.1 — Define Security Requirements for Software Development — #123
   ```

   Skipped practices stay unchecked with the reason inline:

   ```markdown
   - [ ] PO.3 — Implement Supporting Toolchains — skipped: no CI in this repo yet
   ```

**Step 5 — Close out.** Summarize what was filed, linked, and skipped, with issue
numbers. If the repo uses a project board, offer to add the new issues via the
`github-project-board` skill. Do not add them silently.

## Practice epic protocol

Substitute `<PRACTICE_ID>`, `<PRACTICE_NAME>`, `<PHASE_N>`, `<PHASE_NAME>`,
`<PHASE_LABEL>`, `<PRACTICE_LABEL>`, and `<TASKS>` from the practice's reference
file.

**Step 1 — Check for an existing epic** (skip if the phase walkthrough already
did this):

```bash
gh issue list --label "<PRACTICE_LABEL>" --state all --json number,title,state,url
```

If one exists, link it rather than filing a duplicate.

**Step 2 — Adapt the implementation notes to this repo.** The notes in the
reference file are generic. Before drafting, look at what the repo actually has —
CI workflow files, package manifests, existing security config — and rewrite the
notes to name real files, real tools, and real gaps. A checklist that says
"configure branch protection" is worth less than one that says "branch protection
on `main` is missing required reviews — see `.github/workflows/ci.yml`".

Keep a note generic only when you genuinely cannot tell from the repo.

**Step 3 — Draft the epic body:**

```markdown
> NIST SSDF <PRACTICE_ID> — <PRACTICE_NAME>
> Phase <PHASE_N> — <PHASE_NAME>

## Practice

<the practice description from the reference file>

## Tasks

<one section per task in <TASKS>, each rendered as:>

### <TASK_ID> — <task text, verbatim from NIST>

- [ ] <adapted implementation note>
- [ ] <adapted implementation note>

## Done when

Every task box above is ticked, or explicitly waived with a rationale recorded in
a comment on this issue.
```

**Step 4 — Show the draft to the user and wait.** Print the title, labels, and
full body. Create the issue only after they confirm.

**Step 5 — File it:**

```bash
gh issue create \
  --title "[NIST <PRACTICE_ID>] <PRACTICE_NAME>" \
  --label "<PHASE_LABEL>" \
  --label "<PRACTICE_LABEL>" \
  --body-file <path-to-drafted-body>
```

Use `--body-file` with a temp file, not `--body` — the bodies contain backticks
and newlines that shells mangle.

**Step 6 — Report** the issue URL back to the caller.

## Notes

- **Never file without confirmation.** Every issue-creating step in both protocols
  gates on the user saying yes.
- **Never file duplicates.** Search before creating, every time.
- **One epic per practice.** Tasks are checklist items inside the epic, not
  separate issues. Split a task out into its own issue only if the user asks.
