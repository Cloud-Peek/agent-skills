# PW.7 — Review and/or Analyze Human-Readable Code to Identify Vulnerabilities and Verify Compliance with Security Requirements

**Practice ID:** `PW.7`
**Phase:** 4 — Test
**Labels:** `nist-phase-4-test`, `nist-pw7`
**Epic title:** `[NIST PW.7] Review and/or Analyze Human-Readable Code to Identify Vulnerabilities and Verify Compliance with Security Requirements`

## Practice description

> Help identify vulnerabilities so that they can be corrected before the software
> is released to prevent exploitation. Using automated methods lowers the effort
> and resources needed to detect vulnerabilities. Human-readable code includes
> source code, scripts, and any other form of code that an organization deems
> human-readable.

## Tasks (2)

### PW.7.1 — Determine whether code review (a person looks directly at the code to find issues) and/or code analysis (tools are used to find issues in code, either in a fully automated way or in conjunction with a person) should be used, as defined by the organization.

**Implementation notes:**

- Require *both* for non-trivial pull requests: human peer review (enforced with a
  CODEOWNERS file so the right people are pulled in automatically) and automated
  static analysis (SAST — CodeQL or an equivalent scanner).
- Define what counts as "non-trivial" so the rule is not a judgement call every
  time. A workable definition is any change touching authentication,
  authorization, tenant or user data isolation, sandboxed code execution, billing,
  secret handling, or any new external integration.
- Write the policy down where contributors will actually see it — the contributing
  guide — and reference it from the pull request template so the expectation is
  visible at the moment the change is opened.

### PW.7.2 — Perform the code review and/or code analysis based on the organization's secure coding standards, and record and triage all discovered issues and recommended remediations in the development team's workflow or issue tracking system.

**Implementation notes:**

- Run the static analysis scanner on every push to the default branch and on every
  pull request, and make a failing scan block the merge rather than merely warn.
  Publishing results in SARIF format lets them surface inline on the diff instead
  of being buried in a job log.
- Add an automated security review pass over the branch before merge, and reserve
  a deeper independent review for high-risk pull requests — the ones matching the
  "non-trivial" definition above.
- Record every triaged finding in the issue tracker with a `security` label and a
  severity-based remediation deadline. A CVSS-derived service level agreement
  works well: critical and high findings inside seven days, medium findings inside
  thirty.
- Capture root causes rather than just the fix. Feed them into the incident and
  root-cause work under RV.3 so that a recurring class of issue becomes a lint
  rule or a scanner query, not the same pull request comment written for the
  twentieth time.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PW.7`
- `<PRACTICE_NAME>` = `Review and/or Analyze Human-Readable Code to Identify Vulnerabilities and Verify Compliance with Security Requirements`
- `<PHASE_N>` = `4`
- `<PHASE_NAME>` = `Test`
- `<PHASE_LABEL>` = `nist-phase-4-test`
- `<PRACTICE_LABEL>` = `nist-pw7`
- `<TASKS>` = the two PW.7.x sections above
