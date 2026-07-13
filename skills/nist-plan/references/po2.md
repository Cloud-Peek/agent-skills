# PO.2 — Implement Roles and Responsibilities

**Practice ID:** `PO.2`
**Phase:** 1 — Plan & Requirements
**Labels:** `nist-phase-1-plan`, `nist-po2`
**Epic title:** `[NIST PO.2] Implement Roles and Responsibilities`

## Practice description

> Ensure that everyone inside and outside of the organization involved in the
> SDLC is prepared to perform their SDLC-related roles and responsibilities
> throughout the SDLC.

## Tasks (3)

### PO.2.1 — Create new roles and alter responsibilities for existing roles as needed to encompass all parts of the SDLC. Periodically review and maintain the defined roles and responsibilities, updating them as needed.

**Implementation notes:**

- Write down who holds each SDLC role: the security champion, the people who
  review pull requests, the owner of production deploys, and whoever is on point
  for incident response. A role with no name against it is a role nobody has.
- Add a `CODEOWNERS` file so pull requests automatically request review from the
  right owner per area of the codebase, rather than relying on the author to
  remember who to tag.
- Schedule a roles review (annual is typical) and give it a named owner, since
  responsibilities drift as the team changes.

### PO.2.2 — Provide role-based training for all personnel with responsibilities that contribute to secure development. Periodically review personnel proficiency and role-based training, and update the training as needed.

**Implementation notes:**

- Curate training per role rather than sending everyone to the same course.
  Typical tracks: secure coding against the OWASP Top 10, data isolation between
  tenants or users, authentication and authorization with your identity provider,
  threat modeling (for example STRIDE), and incident response.
- Put the required reading into the onboarding checklist — the contributing
  guide, the security requirements checklist, and an architecture walkthrough —
  so a new engineer's first week covers it by default.
- Track completion somewhere durable, whether that is a checklist in the
  repository or the team's usual tracking tool. Untracked training is
  indistinguishable from no training during an audit.

### PO.2.3 — Obtain upper management or authorizing official commitment to secure development, and convey that commitment to all with development-related roles and responsibilities.

**Implementation notes:**

- Get explicit leadership sign-off that the SSDF is the team's security operating
  model, not an aspiration someone will get to later.
- Capture it in a short security charter document that states the commitment,
  names the accountable owner, and sets the expectation that engineers follow the
  documented practices.
- Communicate the commitment through whatever channel actually reaches people (an
  all-hands, a team meeting) and link the charter from onboarding so it survives
  the announcement.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PO.2`
- `<PRACTICE_NAME>` = `Implement Roles and Responsibilities`
- `<PHASE_N>` = `1`
- `<PHASE_NAME>` = `Plan & Requirements`
- `<PHASE_LABEL>` = `nist-phase-1-plan`
- `<PRACTICE_LABEL>` = `nist-po2`
- `<TASKS>` = the three PO.2.x sections above
