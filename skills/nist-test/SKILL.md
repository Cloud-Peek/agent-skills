---
name: nist-test
description: Walk NIST SSDF Phase 4 (Test) and file GitHub epics for the two test practices — PW.7 reviewing and analyzing human-readable code (peer review and SAST), PW.8 testing executable code (dynamic analysis, fuzzing, penetration testing). Use when setting up or auditing a test pipeline, deciding what security testing a repo needs, wiring SAST or DAST into CI, or when the user mentions NIST SSDF, SP 800-218, SSDF phase 4, or the PW.7/PW.8 practices.
license: Apache-2.0
compatibility: Requires git and an authenticated GitHub CLI (`gh`)
metadata:
  practices: PW.7, PW.8
  standard: NIST SP 800-218 (SSDF v1.1)
---

# NIST SSDF Phase 4 — Test

**Phase label:** `nist-phase-4-test`

This skill files GitHub issues. It does not write code.

## What this phase is about

Test is where we look for what slipped past the design and build controls. Two
complementary practices live here: PW.7 covers everything that examines the code
itself (peer review, static analysis), and PW.8 covers everything that exercises
the running executable (dynamic analysis, fuzzing, penetration testing). Both feed
discovered issues into the same workflow and issue tracker, so triage and
remediation are tracked alongside other engineering work.

## Practices

Walk these in order — each one assumes the previous has landed. Read a practice's
reference file only when you reach it, not up front.

| Practice | Name | Reference |
| --- | --- | --- |
| PW.7 | Review and/or Analyze Human-Readable Code to Identify Vulnerabilities and Verify Compliance with Security Requirements | [`references/pw7.md`](references/pw7.md) |
| PW.8 | Test Executable Code to Identify Vulnerabilities and Verify Compliance with Security Requirements | [`references/pw8.md`](references/pw8.md) |

## How to run

Follow the **phase walkthrough protocol** in
[`references/protocol.md`](references/protocol.md). Substitutions:

- `<PHASE_N>` = `4`
- `<PHASE_NAME>` = `Test`
- `<PHASE_LABEL>` = `nist-phase-4-test`
- `<PRACTICES>` = `PW.7`, `PW.8`

Phase epic title: `[NIST Phase 4] Test`.

The protocol confirms scope with the user, finds or creates the phase epic, then
walks each practice — checking for an existing epic, asking whether to file, link,
or skip, and updating the phase epic checklist as it goes. It never creates an
issue without confirmation.

## Adapting to the repo

The implementation notes in each reference file are deliberately generic, because
this skill is meant to run against any repository. Before drafting an epic, look at
what testing the repo already does — the CI workflow, the test suite, any existing
scanning — and rewrite the notes around the real gaps. "There is no dynamic
analysis against a running instance before release" is worth more than "consider
DAST".

Both practices end the same way: findings become tracked issues with a severity and
a remediation deadline. If the repo has no triage path for a security finding, that
gap matters more than which scanner gets wired up first.
