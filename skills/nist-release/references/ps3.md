# PS.3 — Archive and Protect Each Software Release

**Practice ID:** `PS.3`
**Phase:** 5 — Release
**Labels:** `nist-phase-5-release`, `nist-ps3`
**Epic title:** `[NIST PS.3] Archive and Protect Each Software Release`

## Practice description

> Preserve software releases in order to help identify, analyze, and eliminate
> vulnerabilities discovered in the software after release.

## Tasks (2)

### PS.3.1 — Securely archive the necessary files and supporting data (e.g., integrity verification information, provenance data) to be retained for each software release.

**Implementation notes:**

- Pick one durable home for the archive — GitHub Releases or a dedicated artifact
  registry both work — and make sure each release deposits the same set of
  material there: the tag, a signed source tarball, container images, and the
  dependency manifest.
- Define a retention policy and write it down. All releases retained for at least
  three years is a reasonable starting point, with a longer window for
  long-term-support versions.
- Make the archive read-only by default, so that only the release automation
  identity can create new entries and no human can quietly overwrite a published
  artifact.

### PS.3.2 — Collect, safeguard, maintain, and share provenance data for all components of each software release (e.g., in a software bill of materials [SBOM]).

**Implementation notes:**

- Generate a software bill of materials (SBOM) for each release in CycloneDX or
  SPDX format and attach it to the release. Use a generator appropriate to your
  ecosystem — for example `cyclonedx-bom`, `syft`, or `cargo-sbom`.
- Cover transitive dependencies across every component you ship, not just the
  top-level application, and include the base images of any containers you
  publish.
- Sign the SBOM (for example with a cosign attestation) so consumers can verify
  the provenance data itself, and cross-link this to the release integrity
  verification information described in PS.2.1.
- Regenerate the SBOM whenever a dependency changes, ideally as a CI step that
  runs on tag push, so the published SBOM never drifts from the shipped artifact.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PS.3`
- `<PRACTICE_NAME>` = `Archive and Protect Each Software Release`
- `<PHASE_N>` = `5`
- `<PHASE_NAME>` = `Release`
- `<PHASE_LABEL>` = `nist-phase-5-release`
- `<PRACTICE_LABEL>` = `nist-ps3`
- `<TASKS>` = the two PS.3.x sections above
