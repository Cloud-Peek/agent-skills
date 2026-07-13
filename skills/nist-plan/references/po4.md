# PO.4 — Define and Use Criteria for Software Security Checks

**Practice ID:** `PO.4`
**Phase:** 1 — Plan & Requirements
**Labels:** `nist-phase-1-plan`, `nist-po4`
**Epic title:** `[NIST PO.4] Define and Use Criteria for Software Security Checks`

## Practice description

> Help ensure that the software resulting from the SDLC meets the organization's
> expectations by defining and using criteria for checking the software's security
> during development.

## Tasks (2)

### PO.4.1 — Define criteria for software security checks and track throughout the SDLC.

**Implementation notes:**

- Write a Definition of Done that every pull request must satisfy, and make it
  concrete: formatting, linting, type checking, tests passing at an agreed
  coverage floor (80% is a common bar), static analysis clean, and human review
  completed.
- Set severity thresholds for vulnerabilities so triage is not re-litigated each
  time. A common shape is that critical and high findings block a release, while
  medium findings must be triaged within a stated number of days.
- Agree on a handful of metrics you will actually look at — pull request review
  turnaround, time to patch a known CVE, the proportion of changes that ship with
  tests, and the rate of defects escaping to production.

### PO.4.2 — Implement processes, mechanisms, etc. to gather and safeguard the necessary information in support of the criteria.

**Implementation notes:**

- Enforce the Definition of Done mechanically through branch protection with
  required status checks, so the gate does not depend on a reviewer remembering
  it.
- Collect the evidence produced by continuous integration — test results,
  coverage, and static analysis findings — somewhere a person can see the trend
  rather than only the last run.
- Restrict who can override a required check, and make every override leave a
  record that says who did it and why.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PO.4`
- `<PRACTICE_NAME>` = `Define and Use Criteria for Software Security Checks`
- `<PHASE_N>` = `1`
- `<PHASE_NAME>` = `Plan & Requirements`
- `<PHASE_LABEL>` = `nist-phase-1-plan`
- `<PRACTICE_LABEL>` = `nist-po4`
- `<TASKS>` = the two PO.4.x sections above
