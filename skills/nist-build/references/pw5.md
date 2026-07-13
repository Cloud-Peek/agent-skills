# PW.5 — Create Source Code by Adhering to Secure Coding Practices

**Practice ID:** `PW.5`
**Phase:** 3 — Build
**Labels:** `nist-phase-3-build`, `nist-pw5`
**Epic title:** `[NIST PW.5] Create Source Code by Adhering to Secure Coding Practices`

## Practice description

> Decrease the number of security vulnerabilities in the software, and reduce
> costs by minimizing vulnerabilities introduced during source code creation
> that meet or exceed organization-defined vulnerability severity criteria.

## Tasks (1)

### PW.5.1 — Follow all secure coding practices that are appropriate to the development languages and environment to meet the organization's requirements.

**Implementation notes:**

- Write down the secure coding conventions for each language in the codebase and
  keep them next to the contributing guide, so that reviewers and new
  contributors are working from the same list rather than from memory.
- Validate every piece of external input at the boundary, using a schema or the
  type system rather than hand-rolled checks. Declare the constraints that
  actually matter — length bounds, allowed values, numeric ranges, required
  fields — and reject anything that does not satisfy them before it reaches
  business logic. Model request and response shapes explicitly instead of passing
  untyped maps around, and keep the response shape separate from the request
  shape so that internal or sensitive fields cannot leak outward.
- Handle errors specifically rather than with a catch-all. Catch the narrow,
  expected failure first and only fall back to a broad handler for the genuinely
  unexpected. When re-raising or wrapping, preserve the original exception and
  its stack trace so that the root cause survives; never swallow an error
  silently. Log failures with structured context — the operation, the actor, the
  identifiers involved — instead of interpolating them into a message string.
- Route every tenant- or user-scoped data access through a single enforced helper
  that applies the scoping predicate, rather than letting each call site write its
  own filtered query. Ad-hoc queries are where isolation bugs live: one forgotten
  `WHERE tenant_id = ...` is a cross-tenant data leak. Make the safe path the only
  path, and treat a raw query against a scoped table as a review blocker.
- Apply authentication and authorization through shared, injected components
  rather than re-implementing the check in each handler, so that a new endpoint
  cannot accidentally ship without one.
- Enforce the static gates in continuous integration on every pull request: your
  formatter, linter, type-checker, and test runner, with a coverage floor (80% is
  a common bar). Gates that only run locally do not hold the line.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PW.5`
- `<PRACTICE_NAME>` = `Create Source Code by Adhering to Secure Coding Practices`
- `<PHASE_N>` = `3`
- `<PHASE_NAME>` = `Build`
- `<PHASE_LABEL>` = `nist-phase-3-build`
- `<PRACTICE_LABEL>` = `nist-pw5`
- `<TASKS>` = the PW.5.1 section above
