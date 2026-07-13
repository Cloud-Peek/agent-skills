---
name: nist-respond
description: Walk NIST SSDF Phase 6 (Operate & Respond) and file GitHub epics for the three "Respond to Vulnerabilities" practices — RV.1 identifying and confirming vulnerabilities on an ongoing basis, RV.2 assessing, prioritizing, and remediating them, RV.3 root-cause analysis so a class of bug stops recurring. Use when standing up vulnerability management, writing a disclosure policy or SECURITY.md, setting remediation SLAs, running a post-incident review, or when the user mentions NIST SSDF, SP 800-218, SSDF phase 6, or the RV practices.
license: Apache-2.0
compatibility: Requires git and an authenticated GitHub CLI (`gh`)
metadata:
  practices: RV.1, RV.2, RV.3
  standard: NIST SP 800-218 (SSDF v1.1)
---

# NIST SSDF Phase 6 — Operate & Respond

**Phase label:** `nist-phase-6-respond`

This skill files GitHub issues. It does not write code.

## What this phase is about

Once software is in production, security work becomes a question of how quickly
vulnerabilities are noticed and how systematically they are closed. NIST groups
these continuous-operations practices under "Respond to Vulnerabilities" (RV):
identify them (RV.1), triage and fix them (RV.2), and learn from them so the same
class of bug stops recurring (RV.3).

The lessons-learned loop in RV.3 feeds back into Phase 1 requirements and Phase 2
design. Secure development is a cycle, not a finish line — a root cause that keeps
producing the same vulnerability is a defect in the process, not in the code.

## Practices

Walk these in order — each one assumes the previous has landed. Read a practice's
reference file only when you reach it, not up front.

| Practice | Name | Reference |
| --- | --- | --- |
| RV.1 | Identify and Confirm Vulnerabilities on an Ongoing Basis | [`references/rv1.md`](references/rv1.md) |
| RV.2 | Assess, Prioritize, and Remediate Vulnerabilities | [`references/rv2.md`](references/rv2.md) |
| RV.3 | Analyze Vulnerabilities to Identify Their Root Causes | [`references/rv3.md`](references/rv3.md) |

## How to run

Follow the **phase walkthrough protocol** in
[`references/protocol.md`](references/protocol.md). Substitutions:

- `<PHASE_N>` = `6`
- `<PHASE_NAME>` = `Operate & Respond`
- `<PHASE_LABEL>` = `nist-phase-6-respond`
- `<PRACTICES>` = `RV.1`, `RV.2`, `RV.3`

Phase epic title: `[NIST Phase 6] Operate & Respond`.

The protocol confirms scope with the user, finds or creates the phase epic, then
walks each practice — checking for an existing epic, asking whether to file, link,
or skip, and updating the phase epic checklist as it goes. It never creates an
issue without confirmation.

## Adapting to the repo

The implementation notes in each reference file are deliberately generic, because
this skill is meant to run against any repository. Before drafting an epic, check
what already exists — a `SECURITY.md`, a security label, an advisory feed
subscription, an on-call rotation — and rewrite the notes around the real gaps.
The most common finding is that a project has scanners producing findings but no
named owner and no deadline for acting on them, which is an RV.2 problem, not an
RV.1 one.
