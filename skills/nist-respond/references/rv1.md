# RV.1 — Identify and Confirm Vulnerabilities on an Ongoing Basis

**Practice ID:** `RV.1`
**Phase:** 6 — Operate & Respond
**Labels:** `nist-phase-6-respond`, `nist-rv1`
**Epic title:** `[NIST RV.1] Identify and Confirm Vulnerabilities on an Ongoing Basis`

## Practice description

> Help ensure that vulnerabilities are identified more quickly so that they can
> be remediated more quickly in accordance with risk, reducing the window of
> opportunity for attackers.

## Tasks (3)

### RV.1.1 — Gather information from software acquirers, users, and public sources on potential vulnerabilities in the software and third-party components that the software uses, and investigate all credible reports.

**Implementation notes:**

- Subscribe to advisory feeds that cover every layer of the dependency stack:
  GitHub Security Advisories, the advisory database of each language ecosystem in
  use, container base-image CVE feeds, and the security mailing lists or advisory
  channels of every major third-party service and datastore you depend on.
- Run your package manager's audit command (`npm audit`, `pnpm audit`,
  `pip-audit`, `cargo audit`) on a daily schedule against the default branch, and
  raise an issue automatically when a new finding appears.
- Route inbound reports from your published security contact address into the
  same triage flow as the automated findings, so nothing depends on one person
  watching an inbox.

### RV.1.2 — Review, analyze, and/or test the software's code to identify or confirm the presence of previously undetected vulnerabilities.

**Implementation notes:**

- Re-run CodeQL or your SAST tool on a schedule across every supported release,
  not only against new commits. New rules routinely find old bugs.
- Schedule a periodic deeper review of the highest-risk code paths — tenant or
  account isolation, any sandboxed or untrusted code execution, and the
  authentication and authorization flows.
- See PW.7 and PW.8 for the review and test mechanics themselves.

### RV.1.3 — Have a policy that addresses vulnerability disclosure and remediation, and implement the roles, responsibilities, and processes needed to support that policy.

**Implementation notes:**

- Publish a `SECURITY.md` in the repository containing a coordinated disclosure
  policy: the contact address, the expected response time, what is in scope, and
  the safe harbour you offer to good-faith researchers.
- Stand up a product security incident response team (PSIRT), even if that is one
  named on-call rotation, with documented playbooks for the routine CVE, the
  zero-day, in-the-wild exploitation, and multi-party or upstream open-source
  incidents.
- Exercise the playbooks in an annual tabletop so that the first time anyone reads
  them is not during an incident.
- Write a communications plan that names who tells customers, by when, and with
  what content, and keep template notification letters ready to fill in.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `RV.1`
- `<PRACTICE_NAME>` = `Identify and Confirm Vulnerabilities on an Ongoing Basis`
- `<PHASE_N>` = `6`
- `<PHASE_NAME>` = `Operate & Respond`
- `<PHASE_LABEL>` = `nist-phase-6-respond`
- `<PRACTICE_LABEL>` = `nist-rv1`
- `<TASKS>` = the three RV.1.x sections above
