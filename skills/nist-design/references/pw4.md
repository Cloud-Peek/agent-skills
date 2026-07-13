# PW.4 — Reuse Existing, Well-Secured Software When Feasible Instead of Duplicating Functionality

**Practice ID:** `PW.4`
**Phase:** 2 — Design
**Labels:** `nist-phase-2-design`, `nist-pw4`
**Epic title:** `[NIST PW.4] Reuse Existing, Well-Secured Software When Feasible Instead of Duplicating Functionality`

## Practice description

> Lower the costs of software development, expedite software development, and
> decrease the likelihood of introducing additional security vulnerabilities
> into the software by reusing software modules and services that have already
> had their security posture checked. This is particularly important for
> software that implements security functionality, such as cryptographic modules
> and protocols.

## Tasks (3)

### PW.4.1 — Acquire and maintain well-secured software components (e.g., software libraries, modules, middleware, frameworks) from commercial, open-source, and other third-party developers for use by the organization's software.

**Implementation notes:**

- Maintain a vetted-component allowlist that new dependencies must clear, with
  explicit criteria: active maintenance, an acceptable license, a published
  advisory or disclosure channel, and signed releases.
- Capture provenance for every component through an SBOM — cross-reference PS.3.2
  — and re-evaluate a component whenever the way you use it changes, not only
  when you first adopt it.
- For cryptography and authentication, prefer a maintained identity provider and
  battle-tested libraries. Never hand-roll a JWT implementation, a symmetric
  cipher mode, or a password hashing scheme.

### PW.4.2 — Create and maintain well-secured software components in-house following SDLC processes to meet common internal software development needs that cannot be better met by third-party software components.

**Implementation notes:**

- Treat your internal shared library — logging, error handling, configuration,
  API clients — as a real product: it needs tests, documentation, and a named
  owner, because everything downstream inherits its bugs.
- The same applies to shared frontend primitives such as buttons, form fields,
  and anything that renders untrusted content. Give them a home, tests, and an
  owner rather than letting them fork across the codebase.
- Hold internal components to the same definition of done as product code
  (cross-reference PO.4.1); an in-house component that skips review is a
  dependency you have chosen not to vet.

### PW.4.4 — Verify that acquired commercial, open-source, and all other third-party software components comply with the requirements, as defined by the organization, throughout their life cycles.

**Implementation notes:**

- Run a dependency audit in CI on every pull request and fail the build on high
  or critical vulnerabilities unless an exception has been explicitly recorded.
- Enable an automated update service such as dependabot or renovate, and let it
  auto-merge low-risk patch updates once CI passes so the backlog does not
  accumulate.
- Track end-of-life dates for the components you depend on and plan replacements
  before support ends, rather than discovering it during an incident.
- Verify component integrity using package signing and provenance where the
  ecosystem supports it (for example npm provenance or sigstore).

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PW.4`
- `<PRACTICE_NAME>` = `Reuse Existing, Well-Secured Software When Feasible Instead of Duplicating Functionality`
- `<PHASE_N>` = `2`
- `<PHASE_NAME>` = `Design`
- `<PHASE_LABEL>` = `nist-phase-2-design`
- `<PRACTICE_LABEL>` = `nist-pw4`
- `<TASKS>` = the three PW.4.x sections above
