---
name: nist-build
description: Walk NIST SSDF Phase 3 (Build) and file GitHub epics for the three build practices — PS.1 protecting code from unauthorized access and tampering, PW.5 secure coding practices, PW.6 hardening the compiler, interpreter, and build toolchain. Use when setting up or auditing a build pipeline, hardening CI, reviewing branch protection and commit signing, or when the user mentions NIST SSDF, SP 800-218, SSDF phase 3, secure coding standards, or the PS.1/PW.5/PW.6 practices.
license: Apache-2.0
compatibility: Requires git and an authenticated GitHub CLI (`gh`)
metadata:
  practices: PS.1, PW.5, PW.6
  standard: NIST SP 800-218 (SSDF v1.1)
---

# NIST SSDF Phase 3 — Build

**Phase label:** `nist-phase-3-build`

This skill files GitHub issues. It does not write code.

## What this phase is about

Build is where source code is written and turned into executables. The security
controls here are about protecting the code from tampering as it moves through the
repository, writing it according to secure-coding practices, and configuring the
toolchain (compilers, interpreters, build systems) so that vulnerabilities are
eliminated or surfaced before testing.

## Practices

Walk these in order — each one assumes the previous has landed. Read a practice's
reference file only when you reach it, not up front.

| Practice | Name | Reference |
| --- | --- | --- |
| PS.1 | Protect All Forms of Code from Unauthorized Access and Tampering | [`references/ps1.md`](references/ps1.md) |
| PW.5 | Create Source Code by Adhering to Secure Coding Practices | [`references/pw5.md`](references/pw5.md) |
| PW.6 | Configure the Compilation, Interpreter, and Build Processes to Improve Executable Security | [`references/pw6.md`](references/pw6.md) |

## How to run

Follow the **phase walkthrough protocol** in
[`references/protocol.md`](references/protocol.md). Substitutions:

- `<PHASE_N>` = `3`
- `<PHASE_NAME>` = `Build`
- `<PHASE_LABEL>` = `nist-phase-3-build`
- `<PRACTICES>` = `PS.1`, `PW.5`, `PW.6`

Phase epic title: `[NIST Phase 3] Build`.

The protocol confirms scope with the user, finds or creates the phase epic, then
walks each practice — checking for an existing epic, asking whether to file, link,
or skip, and updating the phase epic checklist as it goes. It never creates an
issue without confirmation.

## Adapting to the repo

The implementation notes in each reference file are deliberately generic, because
this skill is meant to run against any repository. Before drafting an epic, read
the repo's actual CI configuration, package manifests, and compiler settings, then
rewrite the notes to name real files and real gaps. "Actions are pinned to
floating tags in `.github/workflows/ci.yml`, not commit SHAs" is worth more than
"pin your actions".

PW.5 in particular is about *this codebase's* secure coding standard. If the repo
already documents one — in a contributing guide, an agent instructions file, or a
style guide — the epic should point at it and close the gaps, not invent a second
standard alongside it.
