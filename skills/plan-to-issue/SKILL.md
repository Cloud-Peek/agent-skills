---
name: plan-to-issue
description: Use when the user wants to turn the just-approved plan from the current conversation into a self-contained GitHub issue that another agent can implement cold.
---

# Plan To Issue

Turn the most recently approved plan in this conversation into a GitHub issue that another agent can complete without access to the chat. Do not implement the plan. Capture it faithfully as an issue, show the draft to the user, and create the issue only after confirmation.

The downstream agent will likely use the `implement-issue` skill against the issue URL, so the issue body must include explicit context, acceptance criteria, files or modules to touch, conventions, tests, and verification steps.

## Locate The Plan

Use the most recently approved plan in the conversation as the source of truth, including any user refinements made during approval.

If no agreed plan is available, stop and ask the user to point to it. Do not invent a plan.

If approval included changes such as "do it but skip the migration", fold those into the issue so it reflects the final agreement.

## Prerequisites

Verify first. If any check fails, stop and report what is missing.

```bash
git rev-parse --is-inside-work-tree
gh --version
gh auth status
```

Resolve the repo:

```bash
gh repo view --json owner,name,defaultBranchRef \
  --jq '{owner: .owner.login, name: .name, base: .defaultBranchRef.name}'
```

## Fill Plan Gaps

Before drafting, resolve implicit details against the actual codebase. Read files, search with `rg`, and avoid guessing.

Resolve:

- Concrete file paths for loosely named modules or features.
- Relevant project instructions, such as root `AGENTS.md` or `CLAUDE.md` and nested instruction files under touched directories.
- Actual validation scripts from `package.json`.
- Tests required by the repo rules, including where to add or extend unit tests and any coverage expectations.
- Genuine open questions that the plan did not settle.

If filling these gaps exposes a real ambiguity that would make the issue vague or likely wrong, stop and ask before filing.

## Draft The Issue

Title rules:

- Imperative, sentence case, no trailing period.
- Aim for about 50 characters; keep under about 72.
- Use a conventional-commits prefix only if recent merged PRs or issues use one.

Check recent title style when needed:

```bash
gh pr list --state merged --limit 10 --json title --jq '.[].title'
```

Use this body template. Omit optional sections rather than padding them, but always include `Context`, `Files to change`, `Acceptance criteria`, and `Verification`.

```markdown
## Context

<2-4 sentences explaining the problem and goal for an agent with no chat history.>

## Approach

<The agreed plan as concrete ordered steps. Be specific about what changes and in what order.>

## Files to change

- `path/to/file.ts` - <what changes here>
- `path/to/other.ts` - <what changes here>
- <new files to create, with intended paths>

## Acceptance criteria

- [ ] <Observable, checkable outcome 1>
- [ ] <Observable, checkable outcome 2>
- [ ] Unit tests added or updated for the new code; touched package stays at or above required coverage.
- [ ] Required formatting, linting, typechecking, and tests pass.

## Tests required

- <Which test files to add or extend, and the key happy path, validation failure, error, and boundary cases.>

## Conventions

- Follow root `AGENTS.md` or `CLAUDE.md` and any nested instruction files under touched directories.
- Validation commands: `<commands from package.json and project instructions>`.
- <Any area-specific rules, such as tenant RLS, shared UI components, config conventions, or integration patterns.>

## Verification

- <Concrete commands, scenarios, UI flows, or API checks that prove the work.>

## Open questions

<Optional. Only include unresolved decisions the implementer must answer or ask about.>
```

Drafting rules:

- Write for an agent with no chat history. Never rely on "as discussed".
- Name real symbols, files, commands, packages, and tests where possible.
- Do not invent scope the approved plan did not include.
- Do not silently drop plan steps.
- Do not paste credentials or secrets. If env vars are needed, name them and note where examples should be documented.

## Labels

Only use labels that already exist:

```bash
gh label list --limit 200 --json name --jq '.[].name'
```

If the user provides a comma-separated label list, apply only labels that exist and tell the user about skipped labels.

Otherwise choose sensible area labels by mapping files to the existing label list, such as API, frontend, database, CI, or docs labels. Skip labels that do not exist. Never create labels.

## Confirm Before Creating

Print the proposed title, full body, and labels in chat. Ask whether to file as-is, edit the issue, or change labels.

Do not run `gh issue create` until the user confirms.

## Create

After confirmation, write the approved body to a temp file and create the issue:

```bash
BODY_FILE=$(mktemp)
cat > "$BODY_FILE" <<'EOF'
<approved body>
EOF

gh issue create \
  --title "<approved title>" \
  --body-file "$BODY_FILE" \
  --label "<confirmed-label-1>" \
  --label "<confirmed-label-2>"

rm -f "$BODY_FILE"
```

Pass one `--label` flag per confirmed label. If `gh` rejects a missing label, drop that label and retry rather than creating labels.

## Hand Back

Print the new issue URL and a one-line summary. Remind the user that another agent can implement it with:

```text
/implement-issue <issue-url>
```

## Guardrails

- Never implement the plan in this skill.
- Never file a hollow issue when the plan is too vague.
- Never add acceptance criteria that the plan does not imply.
- Never include secrets, credentials, or real tokens in the issue body.
