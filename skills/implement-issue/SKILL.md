---
name: implement-issue
description: Use when the user wants to implement a GitHub issue end-to-end: fetch issue context, claim it (assign yourself, set status to In progress), create a branch, plan, code, validate, push, and open a draft PR.
---

# Implement Issue

Implement a GitHub issue end-to-end: resolve the issue, claim it, create a branch from the default base, plan the change, implement, validate, push, and open a draft PR using the `pr-from-branch` skill.

## Resolve The Issue

Accept:

- Full issue URL: extract owner, repo, and issue number.
- Bare number: infer owner and repo from `gh repo view --json owner,name`.
- `#123`: strip `#` and treat as a bare number.

If no argument or an unparseable argument is provided, stop and ask for an issue link or number.

## Prerequisites

Verify:

```bash
git rev-parse --is-inside-work-tree
git status --porcelain
gh --version
gh auth status
```

The worktree must be clean before branching. If dirty, stop and ask the user to commit or stash; do not include unrelated work.

## Fetch The Issue

Read the issue body and every comment:

```bash
gh issue view "<issue-number>" --repo "<owner>/<repo>" \
  --json number,title,body,labels,assignees,state,url,comments
```

If the issue is closed, stop and confirm with the user before continuing.

Extract acceptance criteria, linked designs or PRs, named files, scope hints, labels, and comment updates that override the original body.

## Claim The Issue

Once the issue is confirmed open and you're going to work it, claim it before branching.

Assign yourself with `@me` (resolves to the authenticated `gh` user — don't hardcode a username):

```bash
gh issue edit "<issue-number>" --repo "<owner>/<repo>" --add-assignee @me
```

Set the project Status to "In progress". Status lives on a GitHub Projects (v2) board, not on the issue, so use the GraphQL API. The issue may sit on more than one board — update each that has a Status field.

1. Find the issue's project items and project IDs:

```bash
gh api graphql -f query='
  query($owner:String!, $repo:String!, $number:Int!) {
    repository(owner:$owner, name:$repo) {
      issue(number:$number) {
        projectItems(first:20) { nodes { id project { id title number } } }
      }
    }
  }' -F owner="<owner>" -F repo="<repo>" -F number="<issue-number>"
```

If `projectItems` is empty, the issue isn't on a board — note it and skip the status update; don't guess which board to add it to.

2. For each item, resolve the "Status" single-select field id and the option whose name matches "In progress" (case-insensitive):

```bash
gh api graphql -f query='
  query($project:ID!) {
    node(id:$project) {
      ... on ProjectV2 {
        field(name:"Status") { ... on ProjectV2SingleSelectField { id options { id name } } }
      }
    }
  }' -F project="<project-id>"
```

3. Apply the update:

```bash
gh api graphql -f query='
  mutation($project:ID!, $item:ID!, $field:ID!, $option:String!) {
    updateProjectV2ItemFieldValue(input:{
      projectId:$project, itemId:$item, fieldId:$field,
      value:{ singleSelectOptionId:$option }
    }) { projectV2Item { id } }
  }' -F project="<project-id>" -F item="<item-id>" -F field="<field-id>" -F option="<option-id>"
```

These are best-effort. If a call fails (e.g. the token lacks the `project` scope — `gh auth refresh -s project` grants it), report it and continue with the implementation rather than aborting.

## Create The Branch

Resolve the base branch from the GitHub default branch, falling back to `main`, then `master`. Verify it exists on origin, fetch it, and branch from `origin/<base>`.

Check recent merged PR branch naming:

```bash
gh pr list --state merged --limit 10 --json headRefName --jq '.[].headRefName'
```

Pick a short kebab-case branch name matching repo convention and derived from the issue title. If the branch already exists locally, stop and ask whether to reuse it or pick another name.

## Read Project Conventions

Before planning, read:

- Root `CLAUDE.md` or `AGENTS.md`.
- Nested convention files under directories likely to change.
- Linked docs or contribution files.
- Relevant `package.json` scripts.

Project instructions override this skill when they conflict.

## Plan

For anything beyond a one-line fix, create a step-by-step plan. If the project requires a task file, write the plan there too.

Cover:

- Files and modules expected to change.
- Tests required and coverage expectations.
- Migrations, env vars, or config changes.
- Manual verification for UI work.
- Design questions requiring user input before coding.

For large or unfamiliar areas, use subagents for focused exploration when available.

## Implement

Work through the plan incrementally and update task status as items complete.

Guidelines:

- Make the minimum change satisfying the issue.
- Add unit tests for new code when project rules require them.
- Mock external dependencies.
- Match neighboring code patterns.
- Use shared components and utilities.
- Never commit secrets or real credentials.

If scope expands materially or the issue appears wrong, stop and re-plan with the user.

## Validate

Run project validation from local instructions and scripts. Common Bun baseline:

```bash
bun fmt
bun lint
bun typecheck
bun test
```

Fix validation failures caused by the work. If a failure is unrelated and pre-existing, report it and stop rather than hiding it.

For UI changes, start the app and verify the main path plus at least one edge case.

## Commit And Push

Stage only intentionally edited files. Never use `git add -A` or `git add .`.

Check staged paths:

```bash
git diff --cached --name-only
```

Use the repo's commit style. Reference the issue in the commit body with `Closes #<issue-number>` or `Refs #<issue-number>`.

Push with upstream tracking:

```bash
git push -u origin "$(git branch --show-current)"
```

If push fails, report the exact error and stop.

## Draft PR

Invoke the `pr-from-branch` skill with the resolved base branch. The PR should be draft and the body must include `Closes #<issue-number>`.

Before creation, verify:

- The title reflects the issue intent.
- The body contains the close keyword.
- Labels match touched paths and existing labels.
- Draft status is set.

If a PR already exists for the branch, report its URL instead of recreating it.

## Hand Back

Summarize:

- Issue claim outcome (assigned to you; Status → "In progress") — flag it if skipped or failed.
- Branch name.
- Draft PR URL.
- Files touched.
- Tests added and passed.
- Manual checks or follow-ups.

## Guardrails

- Never force-push or rewrite history.
- Never close the issue directly.
- Never bypass hooks or validation unless explicitly authorized.
- Never commit `.env` files or generated secrets.
