# PS.1 — Protect All Forms of Code from Unauthorized Access and Tampering

**Practice ID:** `PS.1`
**Phase:** 3 — Build
**Labels:** `nist-phase-3-build`, `nist-ps1`
**Epic title:** `[NIST PS.1] Protect All Forms of Code from Unauthorized Access and Tampering`

## Practice description

> Help prevent unauthorized changes to code, both inadvertent and intentional,
> which could circumvent or negate the intended security characteristics of the
> software. For code that is not intended to be publicly accessible, this helps
> prevent theft of the software and may make it more difficult or time-consuming
> for attackers to find vulnerabilities in the software.

## Tasks (1)

### PS.1.1 — Store all forms of code – including source code, executable code, and configuration-as-code – based on the principle of least privilege so that only authorized personnel, tools, services, etc. have access.

**Implementation notes:**

- Audit who can read and write the repository. Confirm that team and
  collaborator membership still matches need-to-know, and remove stale access on
  a fixed cadence (quarterly is a common bar). Machine accounts and CI tokens
  count as access — scope them to the minimum permissions they actually use.
- Protect the default branch: required reviews from the owners named in
  `CODEOWNERS`, no force-push, no branch deletion, and required status checks
  before merge.
- Require signed commits and tags (GPG or SSH signing) so that authorship cannot
  be trivially forged, and document the contributor setup steps somewhere a new
  developer will find them.
- Treat configuration-as-code — CI workflow definitions, infrastructure-as-code,
  deployment manifests — as code. Keep it under the same version control and the
  same branch protections, and never edit production configuration out-of-band.
- Sign the build outputs. Publish release artifacts and container images with a
  signature (for example sigstore/cosign) and record where the corresponding
  verification step happens on the consuming side.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PS.1`
- `<PRACTICE_NAME>` = `Protect All Forms of Code from Unauthorized Access and Tampering`
- `<PHASE_N>` = `3`
- `<PHASE_NAME>` = `Build`
- `<PHASE_LABEL>` = `nist-phase-3-build`
- `<PRACTICE_LABEL>` = `nist-ps1`
- `<TASKS>` = the PS.1.1 section above
