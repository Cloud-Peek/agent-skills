# PW.2 — Review the Software Design to Verify Compliance with Security Requirements and Risk Information

**Practice ID:** `PW.2`
**Phase:** 2 — Design
**Labels:** `nist-phase-2-design`, `nist-pw2`
**Epic title:** `[NIST PW.2] Review the Software Design to Verify Compliance with Security Requirements and Risk Information`

## Practice description

> Help ensure that the software will meet the security requirements and
> satisfactorily address the identified risk information.

## Tasks (1)

### PW.2.1 — Have 1) a qualified person (or people) who were not involved with the design and/or 2) automated processes instantiated in the toolchain review the software design to confirm and enforce that it meets all of the security requirements and satisfactorily addresses the identified risk information.

**Implementation notes:**

- Define a "design review required" trigger so it is not left to judgement: any
  change that introduces a new tenant boundary, a new external integration, a new
  credential or credential type, or that alters isolation, sandboxing, or
  authentication and authorization flows.
- The reviewer must not be the author of the design. On a small or solo team,
  escalate to a peer on another team or to an outside reviewer rather than
  skipping the step.
- Where a human second reviewer is not available, add an independent automated
  review pass in the toolchain so architectural changes still get a second
  opinion.
- Record the review outcome, and any security requirement that was waived along
  with the justification, in the pull request description, the architecture
  decision record, or the threat-model document — somewhere it survives the merge.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PW.2`
- `<PRACTICE_NAME>` = `Review the Software Design to Verify Compliance with Security Requirements and Risk Information`
- `<PHASE_N>` = `2`
- `<PHASE_NAME>` = `Design`
- `<PHASE_LABEL>` = `nist-phase-2-design`
- `<PRACTICE_LABEL>` = `nist-pw2`
- `<TASKS>` = the PW.2.1 section above
