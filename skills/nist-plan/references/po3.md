# PO.3 — Implement Supporting Toolchains

**Practice ID:** `PO.3`
**Phase:** 1 — Plan & Requirements
**Labels:** `nist-phase-1-plan`, `nist-po3`
**Epic title:** `[NIST PO.3] Implement Supporting Toolchains`

## Practice description

> Use automation to reduce human effort and improve the accuracy,
> reproducibility, usability, and comprehensiveness of security practices
> throughout the SDLC, as well as provide a way to document and demonstrate the
> use of these practices. Toolchains and tools may be used at different levels of
> the organization, such as organization-wide or project-specific, and may address
> a particular part of the SDLC, like a build pipeline.

## Tasks (3)

### PO.3.1 — Specify which tools or tool types must or should be included in each toolchain to mitigate identified risks, as well as how the toolchain components are to be integrated with each other.

**Implementation notes:**

- Inventory what the continuous integration pipeline already runs — formatting,
  linting, type checking, tests, static analysis, build — before deciding what is
  missing.
- Define the mandatory tool categories rather than the specific vendors, so the
  requirement survives a tool swap. A usual set is: static application security
  testing (for example CodeQL), software composition analysis via your package
  manager's audit command (`npm audit`, `pnpm audit`, `pip-audit`, `cargo audit`),
  secret scanning (for example gitleaks or the platform's native scanning), a
  formatter, a linter, a type checker, a test runner, and container image scanning
  if you ship images.
- Document how the tools hand off to each other, in particular the interchange
  formats — SARIF for static analysis findings, SBOM formats such as CycloneDX or
  SPDX for dependency inventory.

### PO.3.2 — Follow recommended security practices to deploy, operate, and maintain tools and toolchains.

**Implementation notes:**

- Pin tool versions so builds are reproducible. Pin the language and runtime
  version in whatever file your toolchain manager reads, and pin third-party CI
  actions to a commit SHA rather than a mutable tag.
- Keep the pipeline definition in version control and reviewed like code. Avoid
  configuring build steps by clicking around in the CI provider's web interface,
  because those changes are invisible in the diff.
- Adopt an automated update policy for the tools themselves (for example
  dependabot or renovate) so pinned versions do not quietly rot.

### PO.3.3 — Configure tools to generate artifacts of their support of secure software development practices as defined by the organization.

**Implementation notes:**

- Make continuous integration publish its evidence rather than just passing or
  failing: static analysis results in SARIF, test reports, coverage output, and
  build logs.
- Persist provenance for each release — the commit SHA, build timestamp, and the
  versions of the tools that produced the artifact — so a build can be traced back
  later.
- Write down the retention policy: how long build artifacts, scan results, and
  audit logs are kept, and where.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PO.3`
- `<PRACTICE_NAME>` = `Implement Supporting Toolchains`
- `<PHASE_N>` = `1`
- `<PHASE_NAME>` = `Plan & Requirements`
- `<PHASE_LABEL>` = `nist-phase-1-plan`
- `<PRACTICE_LABEL>` = `nist-po3`
- `<TASKS>` = the three PO.3.x sections above
