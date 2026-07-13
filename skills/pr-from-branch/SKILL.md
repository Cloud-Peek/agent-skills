---
name: pr-from-branch
description: Use when the user wants to draft a GitHub pull request title and body from the current branch, choose existing repo labels, and open the PR with gh after confirmation.
---

# PR From Branch

Draft a pull request from the current git branch, show the draft to the user, and only create the PR after confirmation.

## Prerequisites

Verify first. If any check fails, stop and report what is missing.

```bash
git rev-parse --is-inside-work-tree
gh --version
gh auth status
```

If `gh` is missing, direct the user to https://cli.github.com/ and `gh auth login`.

## Gather Branch Context

Resolve the current branch and base branch.

- If the user provides a base branch argument, use it.
- Otherwise auto-detect the GitHub default branch, falling back to `main`, then `master`.
- Fetch the base branch.
- Verify `origin/<base>` exists before diffing.

Read:

```bash
git branch --show-current
git log --reverse --pretty=format:"%h %s%n%b%n---" "origin/<base>..HEAD"
git diff --stat "origin/<base>...HEAD"
git diff --name-only "origin/<base>...HEAD"
git diff "origin/<base>...HEAD"
```

Stop and report if the checkout is detached, the current branch is the base branch, there are zero commits ahead of base, or the diff is huge. For huge diffs, rely on stat, commit messages, touched paths, and sampled key files.

## Draft

Title rules:

- Imperative mood, sentence case, no trailing period.
- Aim for about 50 characters; keep under about 72.
- Use a conventional-commits prefix only if recent merged PR titles use one.
- Include ticket references from the branch name when that matches repo convention.

Check recent style:

```bash
gh pr list --state merged --limit 10 --json title --jq '.[].title'
```

Body template:

```markdown
## Summary

<1-3 sentences describing what changed and why>

## Changes

- <Meaningful changes, grouped by area when useful>

## Why

<Optional, only when the motivation is not obvious>

## Test plan

- <Specific commands, scenarios, and tests added>

## Notes

<Optional follow-ups, limits, or review notes>
```

Write from the diff, not just commit messages. Be specific and do not invent rationale.

## Labels

Only use existing repo labels:

```bash
gh label list --limit 200 --json name --jq '.[].name'
```

Choose area labels by matching touched paths to existing labels. Always add `size:patch` when it exists; otherwise skip it and mention that in the draft.

## Confirm Before Creating

Print the proposed title, body, and labels. Ask the user whether to open the PR as-is, edit it, or change base, draft status, or labels.

Do not run `gh pr create` until the user confirms.

## Create

Write the approved body to a temp file and run `gh pr create` with one `--label` flag per confirmed label.

Offer `--draft`, `--reviewer`, `--assignee @me`, or `--web` when relevant. If the branch is not pushed, let `gh` push it or push first after user approval.

Handle edge cases explicitly:

- PR already exists: offer `gh pr view --web` or `gh pr edit`.
- Uncommitted changes: mention them and ask whether to commit or stash.
- Branch behind base: mention it and offer to rebase or merge.

After success, print the PR URL.
