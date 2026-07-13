# PO.1 — Define Security Requirements for Software Development

**Practice ID:** `PO.1`
**Phase:** 1 — Plan & Requirements
**Labels:** `nist-phase-1-plan`, `nist-po1`
**Epic title:** `[NIST PO.1] Define Security Requirements for Software Development`

## Practice description

> Ensure that security requirements for software development are known at all
> times so that they can be taken into account throughout the SDLC and
> duplication of effort can be minimized because the requirements information can
> be collected once and shared. This includes requirements from internal sources
> (e.g., the organization's policies, business objectives, and risk management
> strategy) and external sources (e.g., applicable laws and regulations).

## Tasks (3)

### PO.1.1 — Identify and document all security requirements for the organization's software development infrastructures and processes, and maintain the requirements over time.

**Implementation notes:**

- Audit the repository's own settings: branch protection on the default branch,
  required reviews, required status checks (CI, SAST, dependency audit).
- Inventory security requirements for the development infrastructure itself,
  covering source control, CI/CD, package registries, secret management, and any
  third-party SaaS in the delivery path.
- Set a review cadence (annual is typical) with a named owner, and record it
  somewhere durable rather than in someone's calendar alone.

### PO.1.2 — Identify and document all security requirements for organization-developed software to meet, and maintain the requirements over time.

**Implementation notes:**

- Security requirements are usually scattered across contributing guides, agent
  instruction files, and tribal knowledge. Pull them into one checklist that a
  reviewer can actually run down — a single `SECURITY-REQUIREMENTS.md` beats five
  half-remembered conventions.
- Confirm the checklist covers: input validation, authentication and
  authorization, data isolation between tenants or users, secrets handling,
  logging requirements, encryption in transit and at rest, and data retention.
- Document the exception process — who may waive a requirement, and where the
  waiver gets recorded.

### PO.1.3 — Communicate requirements to all third parties who will provide commercial software components to the organization for reuse by the organization's own software. *(Formerly PW.3.1.)*

**Implementation notes:**

- List the commercial and third-party components the software depends on —
  identity providers, databases, object stores, queues, analytics, and any vendor
  API in the request path.
- For each, capture the security expectations you rely on: vulnerability
  disclosure channel, SBOM availability, support SLA, and any contract or data
  processing terms.
- Define adoption criteria for new third-party components (for example: active
  maintenance, a published advisory channel, signed releases, a compliance
  attestation where the data warrants it) and put them in the contributing docs so
  the bar is visible before someone adds a dependency.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PO.1`
- `<PRACTICE_NAME>` = `Define Security Requirements for Software Development`
- `<PHASE_N>` = `1`
- `<PHASE_NAME>` = `Plan & Requirements`
- `<PHASE_LABEL>` = `nist-phase-1-plan`
- `<PRACTICE_LABEL>` = `nist-po1`
- `<TASKS>` = the three PO.1.x sections above
