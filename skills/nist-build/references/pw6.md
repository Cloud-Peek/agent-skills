# PW.6 — Configure the Compilation, Interpreter, and Build Processes to Improve Executable Security

**Practice ID:** `PW.6`
**Phase:** 3 — Build
**Labels:** `nist-phase-3-build`, `nist-pw6`
**Epic title:** `[NIST PW.6] Configure the Compilation, Interpreter, and Build Processes to Improve Executable Security`

## Practice description

> Decrease the number of security vulnerabilities in the software and reduce
> costs by eliminating vulnerabilities before testing occurs.

## Tasks (2)

### PW.6.1 — Use compiler, interpreter, and build tools that offer features to improve executable security.

**Implementation notes:**

- Pin the versions of every tool in the build path — language runtimes,
  compilers, package managers, and build plugins — in a manifest that lives in the
  repository, and let an automated updater (dependabot, renovate, or equivalent)
  propose upgrades so that pinning does not become staleness.
- Build container images from minimal base images (distroless or alpine are the
  usual choices) and pin them by digest rather than by a floating tag, so the base
  cannot change underneath a rebuild.
- Verify the authenticity of everything the build pulls in: pin CI actions and
  reusable workflows to a commit SHA rather than a mutable tag, and verify
  signatures on container images and release artifacts (sigstore/cosign or
  equivalent) before they are consumed.

### PW.6.2 — Determine which compiler, interpreter, and build tool features should be used and how each should be configured, then implement and use the approved configurations.

**Implementation notes:**

- Turn on the strictest setting each tool offers, and write the decision down. For
  TypeScript that means `strict` mode with escape hatches such as `any` requiring
  an explicit justification. For Python it means a strict type-checker
  configuration (mypy or pyright) plus lint and security rules (ruff, bandit).
  Whatever the language, the compiler or interpreter usually already has the
  hardening flags — the work is enabling them and not backing them out.
- Define what a clean build means and enforce it. Continuous integration should
  fail on any new lint, type, or dependency-audit warning, rather than printing
  warnings that nobody reads. Warning tolerance decays to zero attention within a
  quarter.
- Keep every build configuration in version control — CI definitions, compiler and
  type-checker configuration, project and dependency manifests — so that the build
  is reproducible from a checkout and no setting exists only in a web console.

## Filing the epic

Follow the **practice epic protocol** in [`protocol.md`](protocol.md).
Substitutions:

- `<PRACTICE_ID>` = `PW.6`
- `<PRACTICE_NAME>` = `Configure the Compilation, Interpreter, and Build Processes to Improve Executable Security`
- `<PHASE_N>` = `3`
- `<PHASE_NAME>` = `Build`
- `<PHASE_LABEL>` = `nist-phase-3-build`
- `<PRACTICE_LABEL>` = `nist-pw6`
- `<TASKS>` = the two PW.6.x sections above
