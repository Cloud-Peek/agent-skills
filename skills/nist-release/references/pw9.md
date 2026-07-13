# PW.9 — Configure Software to Have Secure Settings by Default

**Practice ID:** `PW.9`
**Phase:** 5 — Release
**Labels:** `nist-phase-5-release`, `nist-pw9`
**Epic title:** `[NIST PW.9] Configure Software to Have Secure Settings by Default`

## Practice description

> Help improve the security of the software at the time of installation to
> reduce the likelihood of the software being deployed with weak security
> settings, putting it at greater risk of compromise.

## Tasks (2)

### PW.9.1 — Define a secure baseline by determining how to configure each setting that has an effect on security or a security-related setting so that the default settings are secure and do not weaken the security functions provided by the platform, network infrastructure, or services.

**Implementation notes:**

- Audit every configuration option the software ships with and record its default
  value. Nothing should ship insecure out of the box: no open ports, no debug mode
  enabled, no unauthenticated endpoints, no weak cryptography, and no default
  passwords.
- For each tunable setting, document the secure baseline value, then test that a
  system running entirely on the defaults still works end to end. A secure default
  that nobody can run is a default everybody overrides.
- Pay particular attention to the settings that carry the most security weight:
  tenant or user data isolation enabled by default (for example, row-level
  security), session timeouts, cross-origin resource sharing (CORS) allowlists,
  any sandbox or execution security profile, object-store bucket policies, and log
  redaction.

### PW.9.2 — Implement the default settings (or groups of default settings, if applicable), and document each setting for software administrators.

**Implementation notes:**

- Keep defaults as configuration-as-code — environment files, Helm values,
  Pydantic settings classes, or the equivalent for your stack — so there are no
  out-of-band defaults hiding in a wiki page or an operator's shell history.
- Document each setting for the administrators who will have to reason about it,
  covering its purpose, valid options, default value, security relevance,
  operational impact, and any related settings.
- Provide a verification command or endpoint that an administrator can run after
  installation to confirm the secure baseline is actually in place.
- Put default values under change control: any change to a default goes through
  pull request review and is called out explicitly in the release notes.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PW.9`
- `<PRACTICE_NAME>` = `Configure Software to Have Secure Settings by Default`
- `<PHASE_N>` = `5`
- `<PHASE_NAME>` = `Release`
- `<PHASE_LABEL>` = `nist-phase-5-release`
- `<PRACTICE_LABEL>` = `nist-pw9`
- `<TASKS>` = the two PW.9.x sections above
