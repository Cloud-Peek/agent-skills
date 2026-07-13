# RV.2 — Assess, Prioritize, and Remediate Vulnerabilities

**Practice ID:** `RV.2`
**Phase:** 6 — Operate & Respond
**Labels:** `nist-phase-6-respond`, `nist-rv2`
**Epic title:** `[NIST RV.2] Assess, Prioritize, and Remediate Vulnerabilities`

## Practice description

> Help ensure that vulnerabilities are remediated in accordance with risk to
> reduce the window of opportunity for attackers.

## Tasks (2)

### RV.2.1 — Analyze each vulnerability to gather sufficient information about risk to plan its remediation or other risk response.

**Implementation notes:**

- File every vulnerability as an issue carrying a `security` label, a CVSS score,
  an exploitability assessment, and a note on the exposed customer surface — which
  accounts and which features are actually reachable.
- Use a standard analysis template so that each report answers the same
  questions: where is the flaw triggered, what is the blast radius, is it
  exploitable in your deployment as configured, and are there compensating
  controls already in place.
- Consistent logging and exception structure across the codebase pays for itself
  here, because it lets you find every call site of an affected pattern quickly.

### RV.2.2 — Plan and implement risk responses for vulnerabilities.

**Implementation notes:**

- Make the risk decision explicit on every issue — remediate, temporarily
  mitigate, accept, or transfer — and name the owner who carries it.
- Set remediation service-level agreements by severity (a common starting point is
  critical within 7 days, high within 30 days, and everything else within 90
  days) and track the exceptions rather than quietly letting them slide.
- When you ship a temporary mitigation such as a configuration flag, a
  web application firewall rule, or a feature toggle, file a follow-up issue for
  the permanent fix so the workaround does not become the fix.
- Plan customer communications: publish advisories through a channel customers
  already watch, such as GitHub Security Advisories, and deliver the patch through
  your standard release channel.
- Update the related architecture decision records and threat models whenever a
  fix invalidates a design assumption (this cross-links to PW.1.2).

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `RV.2`
- `<PRACTICE_NAME>` = `Assess, Prioritize, and Remediate Vulnerabilities`
- `<PHASE_N>` = `6`
- `<PHASE_NAME>` = `Operate & Respond`
- `<PHASE_LABEL>` = `nist-phase-6-respond`
- `<PRACTICE_LABEL>` = `nist-rv2`
- `<TASKS>` = the two RV.2.x sections above
