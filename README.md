# agent-skills

CloudPeek and CyberPeek agent skills, published in the open
[Agent Skills](https://agentskills.io) format.

Every skill is a directory containing a `SKILL.md` (metadata plus instructions)
and, where it needs them, `references/` files that the agent loads only when the
task calls for them. Skills work in any skills-compatible agent — Claude Code,
Codex, Cursor, Copilot, Gemini CLI, and others.

## Installing

Use the [`skills` CLI](https://github.com/vercel-labs/skills), which reads GitHub
directly — no registry publish step, and it detects which agents you have
installed.

See what's on offer:

```bash
npx skills add Cloud-Peek/agent-skills --list
```

Install one skill, or the whole set:

```bash
npx skills add Cloud-Peek/agent-skills --skill nist-plan   # just this one
npx skills add Cloud-Peek/agent-skills                     # all of them
```

Skills install into the current project by default. Add `-g` to install to your
user directory instead, so they're available in every project:

```bash
npx skills add Cloud-Peek/agent-skills --skill nist-plan -g
```

The CLI installs to whichever agents it finds. Target specific ones with `-a`, and
remove a skill with `remove`:

```bash
npx skills add Cloud-Peek/agent-skills -a claude-code -a cursor
npx skills remove nist-plan
```

Each skill directory is self-contained — nothing is shared between them, so taking
one does not oblige you to take the rest. If you would rather not use the CLI, copy
the directory into your agent's skills folder by hand (`~/.claude/skills/` for
Claude Code, or `.claude/skills/` inside a project).

## Secure development (NIST SSDF)

Six skills covering the phases of the NIST Secure Software Development Framework
([SP 800-218](https://csrc.nist.gov/pubs/sp/800/218/final)). Each walks its phase,
files a GitHub epic per practice, and never creates an issue without asking first.
They file issues; they do not write code.

| Skill | Phase | Practices |
| --- | --- | --- |
| [`nist-plan`](skills/nist-plan) | 1 — Plan & Requirements | PO.1, PO.2, PO.3, PO.4, PO.5 |
| [`nist-design`](skills/nist-design) | 2 — Design | PW.1, PW.2, PW.4 |
| [`nist-build`](skills/nist-build) | 3 — Build | PS.1, PW.5, PW.6 |
| [`nist-test`](skills/nist-test) | 4 — Test | PW.7, PW.8 |
| [`nist-release`](skills/nist-release) | 5 — Release | PS.2, PS.3, PW.9 |
| [`nist-respond`](skills/nist-respond) | 6 — Operate & Respond | RV.1, RV.2, RV.3 |

The 19 SSDF practices live as reference files inside the phase that owns them —
`nist-plan/references/po1.md`, and so on. Practice text is quoted verbatim from
NIST; the implementation notes beneath each task are guidance, written to be
adapted to whatever repository the skill runs against.

Start with the phase that matches where you actually are. The phases are a cycle,
not a ladder: RV.3's root-cause findings are meant to flow back into PO.1's
requirements.

## Development workflow

| Skill | What it does |
| --- | --- |
| [`implement-issue`](skills/implement-issue) | Take a GitHub issue from claim to draft PR — branch, plan, implement, validate, push |
| [`feature-development-workflow`](skills/feature-development-workflow) | Deliver an issue from pickup to PR handoff, with review and regression tests |
| [`plan-to-issue`](skills/plan-to-issue) | Turn an approved plan into a self-contained issue another agent can pick up cold |
| [`pr-from-branch`](skills/pr-from-branch) | Draft a PR title, body, and labels from the current branch, then open it |
| [`release-candidate-review`](skills/release-candidate-review) | Compare competing release-candidate PRs against their design PR and report which implemented each section best |
| [`github-project-board`](skills/github-project-board) | Inspect and move cards on GitHub Projects v2 boards |
| [`gh-release-train-merge`](skills/gh-release-train-merge) | Merge multiple PRs into a release branch, recording each as properly merged |

## Utilities

| Skill | What it does |
| --- | --- |
| [`prettify`](skills/prettify) | Turn a document or message into a self-contained HTML infographic |

## License

Apache-2.0. See [LICENSE](LICENSE).
