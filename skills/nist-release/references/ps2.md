# PS.2 — Provide a Mechanism for Verifying Software Release Integrity

**Practice ID:** `PS.2`
**Phase:** 5 — Release
**Labels:** `nist-phase-5-release`, `nist-ps2`
**Epic title:** `[NIST PS.2] Provide a Mechanism for Verifying Software Release Integrity`

## Practice description

> Help software acquirers ensure that the software they acquire is legitimate
> and has not been tampered with.

## Tasks (1)

### PS.2.1 — Make software integrity verification information available to software acquirers.

**Implementation notes:**

- For each tagged release, publish SHA-256 checksums of every artifact alongside
  the release itself — for example, on the GitHub Releases page — so an acquirer
  can confirm what they downloaded matches what you built.
- Adopt keyless signing for container images and binaries (sigstore/cosign is the
  common choice) and document the exact verification command in the README, so
  that verifying a release is a copy-paste operation rather than a research
  project.
- If you publish packages to a registry that supports provenance attestation,
  turn it on. On npm this is `npm publish --provenance`; other ecosystems have
  equivalents.
- Establish a key and certificate rotation playbook covering the rotation cadence
  (annual is typical), the revocation procedure, and who approves both.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PS.2`
- `<PRACTICE_NAME>` = `Provide a Mechanism for Verifying Software Release Integrity`
- `<PHASE_N>` = `5`
- `<PHASE_NAME>` = `Release`
- `<PHASE_LABEL>` = `nist-phase-5-release`
- `<PRACTICE_LABEL>` = `nist-ps2`
- `<TASKS>` = the PS.2.1 section above
