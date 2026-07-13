# PW.1 — Design Software to Meet Security Requirements and Mitigate Security Risks

**Practice ID:** `PW.1`
**Phase:** 2 — Design
**Labels:** `nist-phase-2-design`, `nist-pw1`
**Epic title:** `[NIST PW.1] Design Software to Meet Security Requirements and Mitigate Security Risks`

## Practice description

> Identify and evaluate the security requirements for the software; determine
> what security risks the software is likely to face during operation and how
> the software's design and architecture should mitigate those risks; and
> justify any cases where risk-based analysis indicates that security
> requirements should be relaxed or waived. Addressing security requirements and
> risks during software design (secure by design) is key for improving software
> security and also helps improve development efficiency.

## Tasks (3)

### PW.1.1 — Use forms of risk modeling – such as threat modeling, attack modeling, or attack surface mapping – to help assess the security risk for the software.

**Implementation notes:**

- Run a STRIDE-style threat model over each major surface the software exposes:
  the public API or request handlers, any sandboxed or untrusted code execution
  path, tenant boundaries (e.g. row-level security), each datastore and search
  index, object and file storage, and the identity and session flows.
- Pay particular attention to data flows that cross a tenant or user boundary and
  to any path where the software executes code, queries, or tool calls supplied
  from outside the trust boundary.
- Save each threat model to a durable location such as
  `docs/threat-models/<feature>.md`, and revisit it whenever the feature changes
  materially rather than only at initial design.

### PW.1.2 — Track and maintain the software's security requirements, risks, and design decisions.

**Implementation notes:**

- Adopt architecture decision records (for example `docs/adr/NNNN-<title>.md`) for
  security-relevant decisions: the tenant isolation strategy, the sandbox or
  untrusted-execution security model, how secrets are scoped and rotated, and the
  standard error-handling and logging patterns.
- Each record should capture the decision, the rationale, the threats that were
  considered, the residual risk that remains accepted, and a named owner.
- Re-review the records when a threat model is updated, when a dependency in the
  design changes, or after an incident that touches the decision.

### PW.1.3 — Where appropriate, build in support for using standardized security features and services. *(Formerly PW.4.3.)*

**Implementation notes:**

- Reuse standardized building blocks instead of writing your own: a maintained
  identity provider for authentication and sessions, a vetted cryptography
  library, a shared exception hierarchy with structured logging, and a shared UI
  component library for anything that renders user-controlled data.
- Maintain a short "use this, not that" list covering cryptography,
  authentication, logging, and error handling, and keep it where contributors will
  actually see it.
- Enforce the list rather than merely publishing it — a lint rule or a
  pull-request template question is enough to stop someone quietly
  re-implementing a primitive by hand.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PW.1`
- `<PRACTICE_NAME>` = `Design Software to Meet Security Requirements and Mitigate Security Risks`
- `<PHASE_N>` = `2`
- `<PHASE_NAME>` = `Design`
- `<PHASE_LABEL>` = `nist-phase-2-design`
- `<PRACTICE_LABEL>` = `nist-pw1`
- `<TASKS>` = the three PW.1.x sections above
