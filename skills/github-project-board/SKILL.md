---
name: github-project-board
description: "Use when Codex needs to inspect or update GitHub Projects v2 boards for CloudPeek issues or pull requests: finding project membership, adding items to the correct board, moving cards through Status/phase values, claiming work, moving bug fixes through Open/Claimed/Release Candidate/Closed/Stale, or verifying project-board state after issue or PR work."
---

# GitHub Project Board

## Overview

Manage CloudPeek GitHub Projects v2 board state with live discovery. Always
query the board, field, item, and option IDs immediately before writing; never
reuse stale IDs from notes, logs, or old skill output.

For detailed GraphQL snippets and the current CloudPeek phase map, read
`references/projects-v2-graphql.md` when you need to inspect or mutate a board.

## Workflow

1. Resolve the target:
   - Infer `owner/repo` with `gh repo view --json owner,name` when the user does
     not provide it.
   - Accept issue URLs, PR URLs, `#123`, or bare numbers. Confirm whether a bare
     number is an issue or PR if local context does not make it obvious.
   - If the user names a project board, use that board. Otherwise inspect the
     target's existing project items and labels before choosing a board.

2. Verify GitHub access:
   - Run `gh auth status`.
   - If a Projects v2 GraphQL call fails because the token lacks project scope,
     tell the user to run `gh auth refresh -s project`, then continue with any
     read-only repo work that is still possible.

3. Inspect before writing:
   - Fetch the issue or PR metadata, labels, assignees, state, linked PRs, and
     project items.
   - Fetch active organization projects and the selected project's `Status`
     single-select options.
   - Read the existing item field values when the card already exists so you can
     report old status -> new status.

4. Choose the board and phase:
   - If the target already belongs to one or more projects, update those project
     items unless the user named a specific board.
   - For bug-labelled issues, prefer the `Bug Fixes` project when adding a card
     or interpreting phases.
   - For normal product work, prefer the main `CloudPeek` project unless the
     user names a roadmap/customer/GTM board.
   - Match requested phases case-insensitively, but require an exact single
     option after normalization. If two options could match, ask before writing.

5. Write the smallest safe mutation:
   - Add an issue or PR to a project only when the user requested that board or
     the board choice is deterministic from labels and project conventions.
   - Move the card by updating the `Status` field with the discovered option ID.
   - Assign yourself only when the user asks to claim/start/implement the work,
     or when another skill's workflow explicitly says to claim it.
   - Do not close issues, merge PRs, archive cards, delete items, or mark
     no-fix/stale without an explicit user request or a workflow that requires
     that outcome.

6. Verify and report:
   - Re-read the item or project field value after mutation.
   - Summarize target, board, old phase, new phase, assignment/comment changes,
     and any skipped project updates.
   - If the item is on no board and the user did not authorize adding it, say so
     clearly instead of guessing.

## CloudPeek Phase Guidance

Always verify the live options before writing. Current board conventions:

- `Bug Fixes`: `Open` -> `Claimed` -> `Release Candidate (PR)` ->
  `Closed (merged)`, plus `Stale (fix not required)`.
- Standard work boards such as `CloudPeek`, `Permissions V2 Roadmap`, and
  `CloudPeek GTM`: `Backlog` -> `Ready` -> `In progress` -> `In review` ->
  `Done`.
- `Mastec`: `Todo` -> `In Progress` -> `Done`.

Common mappings:

- Starting or claiming a bug: assign the owner and move `Bug Fixes` to
  `Claimed`.
- Opening a bug-fix PR: ensure the PR body links the issue, then move
  `Bug Fixes` to `Release Candidate (PR)`.
- Merged bug fix: verify the closing PR merged and the issue closed, then move
  `Bug Fixes` to `Closed (merged)` if automation did not.
- Starting standard work: move the selected board to `In progress`.
- Opening a standard-work PR: move the selected board to `In review`.
- Completed standard work: move to `Done` only after merge/close or explicit
  user confirmation.
- No fix required: comment with the reason, move `Bug Fixes` to
  `Stale (fix not required)`, and close the issue only when requested.

## Guardrails

- Prefer `gh api graphql` for Projects v2 operations; classic project and issue
  status commands do not update Projects v2 single-select fields.
- Treat project writes as best-effort when part of a larger implementation
  workflow. If board mutation fails, report the error and continue with the
  code work unless the user specifically asked only for board management.
- Keep project state auditable: mention comments, assignments, linked PRs, and
  status transitions in the final response.
- Do not expose GitHub tokens or raw authorization headers in logs, comments, or
  final output.
