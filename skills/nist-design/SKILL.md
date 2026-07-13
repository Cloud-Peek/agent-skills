---
name: nist-design
description: Walk NIST SSDF Phase 2 (Design) and file GitHub epics for the three design practices — PW.1 threat modeling and secure-by-design, PW.2 independent design review, PW.4 reuse of well-secured components instead of duplicating functionality. Use before starting a new major feature, when threat modeling an architecture, when reviewing a design for security compliance, or when the user mentions NIST SSDF, SP 800-218, SSDF phase 2, threat modeling, or the PW.1/PW.2/PW.4 practices.
license: Apache-2.0
compatibility: Requires git and an authenticated GitHub CLI (`gh`)
metadata:
  practices: PW.1, PW.2, PW.4
  standard: NIST SP 800-218 (SSDF v1.1)
---

# NIST SSDF Phase 2 — Design

**Phase label:** `nist-phase-2-design`

This skill files GitHub issues. It does not write code.

## What this phase is about

Design is where security is cheapest to add and most expensive to skip. In this
phase the team uses risk modeling to predict how the software will be attacked,
has those models reviewed by someone who did not draw them, and decides which
security functions to reuse from already-vetted components rather than build from
scratch.

## Practices

Walk these in order — each one assumes the previous has landed. Read a practice's
reference file only when you reach it, not up front.

| Practice | Name | Reference |
| --- | --- | --- |
| PW.1 | Design Software to Meet Security Requirements and Mitigate Security Risks | [`references/pw1.md`](references/pw1.md) |
| PW.2 | Review the Software Design to Verify Compliance with Security Requirements and Risk Information | [`references/pw2.md`](references/pw2.md) |
| PW.4 | Reuse Existing, Well-Secured Software When Feasible Instead of Duplicating Functionality | [`references/pw4.md`](references/pw4.md) |

## How to run

Follow the **phase walkthrough protocol** in
[`references/protocol.md`](references/protocol.md). Substitutions:

- `<PHASE_N>` = `2`
- `<PHASE_NAME>` = `Design`
- `<PHASE_LABEL>` = `nist-phase-2-design`
- `<PRACTICES>` = `PW.1`, `PW.2`, `PW.4`

Phase epic title: `[NIST Phase 2] Design`.

The protocol confirms scope with the user, finds or creates the phase epic, then
walks each practice — checking for an existing epic, asking whether to file, link,
or skip, and updating the phase epic checklist as it goes. It never creates an
issue without confirmation.

## Adapting to the repo

The implementation notes in each reference file are deliberately generic, because
this skill is meant to run against any repository. Before drafting an epic, look
at the actual architecture — where the trust boundaries are, which components
handle untrusted input, what is already documented — and rewrite the notes to name
real surfaces and real gaps. A threat-modeling task that names this system's
actual attack surface is worth more than one that says "run a threat model".

## Note on practice numbering

PW.4 has tasks PW.4.1, PW.4.2, and PW.4.4. There is no PW.4.3 — in SSDF v1.1 it
moved to PW.1.3. The reference files preserve NIST's numbering; do not renumber
them to close the gap.
