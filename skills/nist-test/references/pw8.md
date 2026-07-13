# PW.8 — Test Executable Code to Identify Vulnerabilities and Verify Compliance with Security Requirements

**Practice ID:** `PW.8`
**Phase:** 4 — Test
**Labels:** `nist-phase-4-test`, `nist-pw8`
**Epic title:** `[NIST PW.8] Test Executable Code to Identify Vulnerabilities and Verify Compliance with Security Requirements`

## Practice description

> Help identify vulnerabilities so that they can be corrected before the software
> is released in order to prevent exploitation. Using automated methods lowers the
> effort and resources needed to detect vulnerabilities and improves traceability
> and repeatability. Executable code includes binaries, directly executed bytecode
> and source code, and any other form of code that an organization deems
> executable.

## Tasks (2)

### PW.8.1 — Determine whether executable code testing should be performed to find vulnerabilities not identified by previous reviews, analysis, or testing and, if so, which types of testing should be used.

**Implementation notes:**

- Decide explicitly which categories of testing apply, and state each one. A
  typical set is: unit tests held to a coverage floor (80% is a reasonable
  starting point); integration tests run against real dependencies rather than
  mocked-out datastores and identity providers; dynamic analysis (DAST) against
  the running application; fuzz testing of every input handler that parses
  untrusted data, including request body parsers and any intake path that accepts
  user-supplied code or scripts; and periodic penetration testing by an
  independent party.
- Decide the cadence for each category and write it down. Unit and integration
  tests belong on every pull request, dynamic analysis on every release candidate,
  and penetration testing annually plus after any major architectural change.

### PW.8.2 — Scope the testing, design the tests, perform the testing, and document the results, including recording and triaging all discovered issues and recommended remediations in the development team's workflow or issue tracking system.

**Implementation notes:**

- Build on the test patterns the codebase already has rather than inventing a
  parallel structure. Run your test runner with coverage reporting enabled so the
  coverage floor is measured rather than assumed. Where mocks are unavoidable,
  hold them to a standard: a mock must return realistically shaped data, and the
  tests around it must exercise every branch, including the error paths.
- Add a regression test for every vulnerability you fix, and land it in the
  standing test suite so the same defect cannot quietly return.
- Wire up a dynamic scanner (OWASP ZAP or equivalent) against a staging deployment
  on each release candidate, rather than only scanning source.
- Publish test results from CI and track the coverage trend somewhere visible, so
  a slow decline is noticed before it becomes a gap.
- Route findings into the issue tracker with a `security` label and the same
  severity-based remediation deadlines used for code review findings under PW.7,
  so there is one triage queue and one clock, not two.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PW.8`
- `<PRACTICE_NAME>` = `Test Executable Code to Identify Vulnerabilities and Verify Compliance with Security Requirements`
- `<PHASE_N>` = `4`
- `<PHASE_NAME>` = `Test`
- `<PHASE_LABEL>` = `nist-phase-4-test`
- `<PRACTICE_LABEL>` = `nist-pw8`
- `<TASKS>` = the two PW.8.x sections above
