---
name: nist-release
description: Walk NIST SSDF Phase 5 (Release) and file GitHub epics for the three release practices — PS.2 verifying software release integrity (signing, checksums, provenance), PS.3 archiving each release and producing an SBOM, PW.9 shipping secure settings by default. Use when preparing or auditing a release process, adding artifact signing or SBOM generation, reviewing default configuration for insecure defaults, or when the user mentions NIST SSDF, SP 800-218, SSDF phase 5, SBOM, or the PS.2/PS.3/PW.9 practices.
license: Apache-2.0
compatibility: Requires git and an authenticated GitHub CLI (`gh`)
metadata:
  practices: PS.2, PS.3, PW.9
  standard: NIST SP 800-218 (SSDF v1.1)
---

# NIST SSDF Phase 5 — Release

**Phase label:** `nist-phase-5-release`

This skill files GitHub issues. It does not write code.

## What this phase is about

Release is the moment software leaves our control and reaches an acquirer. The
controls in this phase make that handoff trustworthy: a way for the acquirer to
verify that what they received has not been tampered with, an archive of
everything that shipped (so it can be reconstructed later when a CVE lands against
a dependency), and secure-by-default configuration so the software is not
compromised on first install.

## Practices

Walk these in order — each one assumes the previous has landed. Read a practice's
reference file only when you reach it, not up front.

| Practice | Name | Reference |
| --- | --- | --- |
| PS.2 | Provide a Mechanism for Verifying Software Release Integrity | [`references/ps2.md`](references/ps2.md) |
| PS.3 | Archive and Protect Each Software Release | [`references/ps3.md`](references/ps3.md) |
| PW.9 | Configure Software to Have Secure Settings by Default | [`references/pw9.md`](references/pw9.md) |

## How to run

Follow the **phase walkthrough protocol** in
[`references/protocol.md`](references/protocol.md). Substitutions:

- `<PHASE_N>` = `5`
- `<PHASE_NAME>` = `Release`
- `<PHASE_LABEL>` = `nist-phase-5-release`
- `<PRACTICES>` = `PS.2`, `PS.3`, `PW.9`

Phase epic title: `[NIST Phase 5] Release`.

The protocol confirms scope with the user, finds or creates the phase epic, then
walks each practice — checking for an existing epic, asking whether to file, link,
or skip, and updating the phase epic checklist as it goes. It never creates an
issue without confirmation.

## Adapting to the repo

The implementation notes in each reference file are deliberately generic, because
this skill is meant to run against any repository. Before drafting an epic, look at
how the project actually ships — tagged releases, container images, published
packages, or a deployment with no artifact at all — and rewrite the notes around
that. A project that deploys a service and publishes nothing has a very different
PS.2 than one that publishes a library other people install.

If the software ships no artifact to an external acquirer, say so and consider
skipping PS.2 with a recorded reason rather than filing an epic that cannot be
completed.
