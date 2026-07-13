---
name: feature-development-workflow
description: Deliver a GitHub issue from pickup to PR handoff. Use when you are asked to pick up, plan, implement, test, review, push, or open/update a PR for a feature, adapter, worker lane, runtime change, results/persistence change, approval/HITL progression, observability work, or a stacked change against an existing branch.
---

# Issue Delivery

Use this skill to take an issue from assignment to a merge-ready PR: clear intake, scoped plan, correct branch shape, safe implementation, regression tests, independent review, and clean handoff.

## Pick Up The Issue

1. Read the repo's contributor docs (`AGENTS.md`, `tasks/lessons.md`, architecture docs) before editing. Reconcile the issue with the architecture source of truth.
2. Fetch the GitHub issue and any linked PRs. Extract acceptance criteria, constraints, open questions, expected tests, target base branch, and whether the work is stacked on an existing PR.
3. Claim or mark the issue in progress when the repo workflow expects it, using the existing issue labels, assignees, project status, or a concise comment.
4. If the issue builds on a parent PR branch, fetch and inspect the parent PR head/base before branching. Only mutate or push the parent branch when the user owns that branch, explicitly asked for the update, or approved it after inspection; otherwise base the child branch on the remote parent as-is and report the stale-parent risk. Prefer a normal merge from latest `main` over rewriting history unless the repo workflow explicitly requires a rebase.
5. Write `tasks/todo.md` before implementation. Include issue context,   architecture impact, branch/PR shape, implementation slices, test/coverageplan, gates, subagent review, and push/PR steps.
6. Keep `tasks/todo.md` current while delivering the issue: mark progress as tasks complete and add the final review/results section before PR handoff.
7. Check in with the user before coding when the change is architectural, high-risk, or materially ambiguous. Otherwise proceed from the written plan.

## Plan The Work

- Translate the issue into small deliverables with explicit files/packages, behavior changes, failure modes, and regression coverage.
- Decide what kind of change this is: a config/manifest addition, a runtime refactor, a persistence change, a route/API change, a UI change, or a documentation-only update.
- Challenge migrations. Prefer additive configuration/adapter changes over schema changes when the existing generic types can carry the new behavior.
- Identify architecture documentation updates up front. Update the architecture docs in the same PR when changing package boundaries, route families, queues, durable resource types, persisted state, security controls, configuration conventions, or integration patterns.
- Use subagents only for bounded work that can run in parallel: issue/PR research, architecture gap analysis, disjoint implementation slices, or post-commit review. Give coding agents disjoint write scopes and remind them not to revert others' edits. 
- Use Subagents to do adversary redteam design reviews, looking for secruity or implementation issuies, loop this untill the design is agreed as good.

## Branch And Stack

1. Inspect `git status` before changing branches. Preserve user edits.
2. Fetch refs and create a feature branch from the correct base. For stacked work, base the child PR on the parent PR branch, not `main`, and avoid pushing parent-branch changes unless ownership/approval is clear.
3. Keep commits focused and reviewable. Do not merge the PR unless the user explicitly asks for that action in the current turn.

## Implement

- Follow the build-phase secure-coding practices in the [`nist-build`](../nist-build/SKILL.md) skill (code protection, secure coding, build-tool hardening).
- Follow existing package boundaries. Keep durable state and runtime-neutral contracts, schemas, fixtures, and pure helpers in the packages the  architecture designates for them.
- Treat the system of record as the source of truth. Transport/queue layers are wakeups, not durable state.
- Route data-access calls through the existing safe transaction/access wrappers. Do not weaken isolation or access boundaries.
- Keep behavior configuration-driven and generic where the architecture calls for it, so future variants are supported without migrations.
- Register adapters/handlers explicitly, declare what they support, validate input/output with schemas, preserve lane/boundary separation, and use realistic dummy data in tests.
- Make transitions idempotent and fail-closed. Persist output, results, state advancement, and approval activation transactionally before best-effort wakeups.
- Preserve side-by-side compatibility with legacy paths unless the issue explicitly requests cutover.
- Make failures observable. If a step is marked failed without throwing, emit a structured log/capture event. Persist sanitized failure output only, guard logging callbacks, and never forward raw adapter/provider exception objects to logs or telemetry.

## Testing

Add focused unit/regression tests for every new function, branch, and failure mode. Map each acceptance criterion to at least one test. Cover the common areas for the kind of change you made, for example:

- Validation for declarations, mismatches, duplicate ids, dependencies, and checked-in fixtures.
- Repository/persistence tests for creation, claiming, state advancement, approval activation, result persistence, and correct linkage.
- Worker/handler tests for input validation, execution, credential resolution, output validation, missing handlers, boundary mismatch, wake-up failures, and reporter failures.
- Adapter tests with realistic dummy data and mocked dependencies.
- Migration tests for each supported backend when schema changes occur.

Run targeted tests first, then package gates. For touched packages, run the coverage command and verify touched files are at least 80% line/function coverage. Finish with the repo's format/lint/typecheck gates and:

```bash
git diff --check
```

Call out any broad bootstrap file whose whole-file coverage remains low even when the new helper branches are directly covered.

## Review And Handoff

1. Commit only after tests and gates pass.
2. Use a subagent for post-commit bug/security review when the user requested subagents or the change touches runtime, persistence, or security.
3. Fix actionable review findings, rerun the relevant tests/gates, and amend or add a follow-up commit.
4. Push the branch and open/update the PR against the correct base. For stacked work, the child PR targets the parent PR branch.
5. The PR body should list summary, issue link, parent PR link when stacked, implementation notes, subagent review status, tests, coverage notes, and explicit follow-ups. Link the PR back to the issue so GitHub can track the delivery path.
6. Check remote PR status with `gh pr checks` or the GitHub checks surface after pushing. Report pending/failing checks by exact name and fix failures that are in scope.
7. Keep the working tree clean and report any skipped validation with the exact reason.
