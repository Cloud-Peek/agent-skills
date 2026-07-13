# RV.3 — Analyze Vulnerabilities to Identify Their Root Causes

**Practice ID:** `RV.3`
**Phase:** 6 — Operate & Respond
**Labels:** `nist-phase-6-respond`, `nist-rv3`
**Epic title:** `[NIST RV.3] Analyze Vulnerabilities to Identify Their Root Causes`

## Practice description

> Help reduce the frequency of vulnerabilities in the future.

## Tasks (4)

### RV.3.1 — Analyze identified vulnerabilities to determine their root causes.

**Implementation notes:**

- Require a root-cause comment before any `security`-labelled issue is closed. It
  should name the category (input validation, authentication or authorization, a
  gap in row-level or tenant isolation, a dependency CVE, a misconfiguration, or a
  logic error), explain why the flaw shipped, and say what would have caught it.
- Keep the write-ups somewhere durable and searchable, such as a
  `docs/post-mortems/` directory, so that the analysis outlives the issue tracker.

### RV.3.2 — Analyze the root causes over time to identify patterns, such as a particular secure coding practice not being followed consistently.

**Implementation notes:**

- Review all root causes on a quarterly cadence and look for clusters — three
  isolation bugs in one quarter is a signal about the design, not three unrelated
  mistakes.
- Convert each detected pattern into automation: a lint rule, a custom CodeQL
  query, a unit-test scaffold, or a question added to the pull request template.
- Fold the recurring pitfalls into onboarding material so new contributors inherit
  the lesson instead of rediscovering it.

### RV.3.3 — Review the software for similar vulnerabilities to eradicate a class of vulnerabilities, and proactively fix them rather than waiting for external reports.

**Implementation notes:**

- After each security fix, sweep the whole codebase for the same pattern, using
  both a text search and a custom static-analysis query.
- File proactive fix pull requests even where no CVE is attached, and treat them
  as medium-priority remediation work rather than optional cleanup.
- See PW.7 and PW.8 for the review and test mechanics.

### RV.3.4 — Review the SDLC process, and update it if appropriate to prevent (or reduce the likelihood of) the root cause recurring in updates to the software or in new software that is created.

**Implementation notes:**

- When the same root cause appears twice, change the process rather than the
  patch: update the phase documentation, write an architecture decision record,
  add a lint rule, require a test, or add a field to the pull request template —
  whichever actually closes the loop.
- Feed the learning back into the earlier phases (Plan, Design, Build, Test, and
  Release) and revise their documentation when the evidence warrants it.
- Record every process change in a `docs/sdlc-changelog.md` so the team can see
  how the development lifecycle has matured over time.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `RV.3`
- `<PRACTICE_NAME>` = `Analyze Vulnerabilities to Identify Their Root Causes`
- `<PHASE_N>` = `6`
- `<PHASE_NAME>` = `Operate & Respond`
- `<PHASE_LABEL>` = `nist-phase-6-respond`
- `<PRACTICE_LABEL>` = `nist-rv3`
- `<TASKS>` = the four RV.3.x sections above
