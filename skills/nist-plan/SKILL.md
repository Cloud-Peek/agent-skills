---
name: nist-plan
description: Walk NIST SSDF Phase 1 (Plan & Requirements) and file GitHub epics for the five "Prepare the Organization" practices — PO.1 security requirements, PO.2 roles and responsibilities, PO.3 supporting toolchains, PO.4 criteria for security checks, PO.5 secure development environments. Use when starting a new product or a major feature planning cycle, when setting up secure-SDLC governance for a repo, or when the user mentions NIST SSDF, SP 800-218, SSDF phase 1, or the PO practices.
license: Apache-2.0
compatibility: Requires git and an authenticated GitHub CLI (`gh`)
metadata:
  practices: PO.1, PO.2, PO.3, PO.4, PO.5
  standard: NIST SP 800-218 (SSDF v1.1)
---

# NIST SSDF Phase 1 — Plan & Requirements

**Phase label:** `nist-phase-1-plan`

This skill files GitHub issues. It does not write code.

## What this phase is about

Before code is written, the organization sets the conditions for secure
development: the security requirements that software must meet, the people and
roles accountable for meeting them, the toolchains that automate them, the
criteria for checking them, and the protected environments in which development
happens. NIST groups all of these under "Prepare the Organization" (PO).

## Practices

Walk these in order — each one assumes the previous has landed. Read a practice's
reference file only when you reach it, not up front.

| Practice | Name | Reference |
| --- | --- | --- |
| PO.1 | Define Security Requirements for Software Development | [`references/po1.md`](references/po1.md) |
| PO.2 | Implement Roles and Responsibilities | [`references/po2.md`](references/po2.md) |
| PO.3 | Implement Supporting Toolchains | [`references/po3.md`](references/po3.md) |
| PO.4 | Define and Use Criteria for Software Security Checks | [`references/po4.md`](references/po4.md) |
| PO.5 | Implement and Maintain Secure Environments for Software Development | [`references/po5.md`](references/po5.md) |

## How to run

Follow the **phase walkthrough protocol** in
[`references/protocol.md`](references/protocol.md). Substitutions:

- `<PHASE_N>` = `1`
- `<PHASE_NAME>` = `Plan & Requirements`
- `<PHASE_LABEL>` = `nist-phase-1-plan`
- `<PRACTICES>` = `PO.1`, `PO.2`, `PO.3`, `PO.4`, `PO.5`

Phase epic title: `[NIST Phase 1] Plan & Requirements`.

The protocol confirms scope with the user, finds or creates the phase epic, then
walks each practice — checking for an existing epic, asking whether to file, link,
or skip, and updating the phase epic checklist as it goes. It never creates an
issue without confirmation.

## Adapting to the repo

The implementation notes in each reference file are deliberately generic, because
this skill is meant to run against any repository. Before drafting an epic, look
at what the repo actually has — CI workflows, package manifests, branch
protection, existing security docs — and rewrite the notes to name real files and
real gaps. An epic that says "branch protection on `main` has no required status
checks" is worth more than one that says "configure branch protection".
