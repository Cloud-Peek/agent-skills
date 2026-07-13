# PO.5 — Implement and Maintain Secure Environments for Software Development

**Practice ID:** `PO.5`
**Phase:** 1 — Plan & Requirements
**Labels:** `nist-phase-1-plan`, `nist-po5`
**Epic title:** `[NIST PO.5] Implement and Maintain Secure Environments for Software Development`

## Practice description

> Ensure that all components of the environments for software development are
> strongly protected from internal and external threats to prevent compromises of
> the environments or the software being developed or maintained within them.
> Examples of environments for software development include development, build,
> test, and distribution environments.

## Tasks (2)

### PO.5.1 — Separate and protect each environment involved in software development.

**Implementation notes:**

- Document and enforce separation between development, staging, and production:
  distinct credentials, distinct networks, and no database shared across the
  boundary.
- Where the application enforces data isolation between tenants or users — for
  example row-level security, plus any privileged role that deliberately bypasses
  it for background jobs — confirm that none of those privileged paths are
  reachable using non-production credentials.
- Require multi-factor authentication and conditional access for any environment
  that touches customer data, and keep standing production access to a minimum,
  preferring break-glass accounts that are audited when used.
- Prefer ephemeral or containerized build runners over long-lived ones, and never
  self-host a runner on a developer's laptop.

### PO.5.2 — Secure and harden development endpoints (i.e., endpoints for software designers, developers, testers, builders, etc.) to perform development-related tasks using a risk-based approach.

**Implementation notes:**

- Publish a hardening baseline for developer machines: full-disk encryption,
  centralized device management, an enforced screen lock, restrictions on browser
  extensions, a mandatory password manager, and a hardware multi-factor token for
  any account that can reach production.
- Block access to production from unmanaged devices, rather than asking people to
  promise not to.
- Review the baseline on a stated cadence (quarterly is common) and track adoption
  per engineer, so you know the gap rather than assuming there isn't one.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PO.5`
- `<PRACTICE_NAME>` = `Implement and Maintain Secure Environments for Software Development`
- `<PHASE_N>` = `1`
- `<PHASE_NAME>` = `Plan & Requirements`
- `<PHASE_LABEL>` = `nist-phase-1-plan`
- `<PRACTICE_LABEL>` = `nist-po5`
- `<TASKS>` = the two PO.5.x sections above
