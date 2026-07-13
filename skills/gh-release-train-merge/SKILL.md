---
name: gh-release-train-merge
description: Use when Codex needs to create or maintain a GitHub release/integration branch and draft PR by merging multiple existing PRs into it one at a time, especially when the user requires the original PRs to be commented on, retargeted if needed, merged through GitHub, and recorded as MERGED rather than merely having their commits manually merged into another branch.
---

# GitHub Release Train Merge

## Overview

Use this workflow for release-train branches such as `V0.7.0` where several
existing PRs must be integrated into one branch and the original PR pages must
end in GitHub's `MERGED` state.

The core rule: **manual merge commits on the release branch do not make GitHub
mark the original PRs as merged**. To close originals as merged, merge each
original PR through GitHub after its base is the release branch.

## Workflow

1. Resolve the repo, release branch, release PR, target base, PR list, and order.
   If the user says "lowest to highest", sort by numeric PR number. Process one
   PR fully before starting the next.

2. Inspect local state before writes:
   - `git status --short --branch`
   - current branch and upstream
   - any unrelated dirty files to avoid touching

3. Create the release branch and draft PR when requested:
   - branch from the requested target base, usually `origin/main`
   - push the branch
   - create a draft PR from release branch to target base

4. For each source PR, integrate through the PR object:
   - Fetch/view PR metadata: number, title, base, head branch, head SHA, state.
   - Prefer retargeting the original PR to the release branch and merging it via
     GitHub with the repository's expected merge method.
   - If the PR's code was already manually integrated into the release branch and
     GitHub rejects retargeting because there are no new commits, create an
     empty marker commit on the source branch:

     ```bash
     git fetch origin SOURCE_BRANCH
     git switch --detach origin/SOURCE_BRANCH
     git commit --allow-empty -m "chore: mark PR #NNNN integrated into RELEASE_BRANCH"
     git push origin HEAD:SOURCE_BRANCH
     ```

   - Retarget the original PR to the release branch.
   - Add a top-level PR comment explaining that the PR is being merged into the
     release integration branch and will be recorded as merged.
   - Merge the PR through GitHub, using `expected_head_sha` when available.
   - Fetch the release branch and fast-forward local checkout to the new remote
     tip before moving to the next PR.
   - Push immediately after every local commit made for the workflow, including
     source-branch marker commits and bookkeeping commits.

5. If a retargeted PR has real conflicts:
   - Stop the sequence at that PR.
   - Resolve conflicts on the PR head branch or a clearly named repair branch,
     preserving the original PR relationship when possible.
   - Push the conflict-resolution commit, rerun appropriate tests, then merge
     that PR through GitHub before continuing.

## Verification

After every PR, or at minimum before reporting done, verify each original PR:

```bash
gh pr view NNNN --repo OWNER/REPO --json number,state,mergedAt,baseRefName,headRefName,mergeCommit,url
```

Required final state for every source PR:

- `state` is `MERGED`
- `baseRefName` is the release branch
- `mergedAt` is non-empty

Also verify the release PR:

```bash
gh pr view RELEASE_PR --repo OWNER/REPO --json number,state,isDraft,baseRefName,headRefName,mergeable,url
gh pr checks RELEASE_PR --repo OWNER/REPO
```

Run project-required gates after code/conflict changes and after final
bookkeeping, for CloudPeek normally:

```bash
bun fmt
bun lint
bun typecheck
git diff --check
```

## Pitfalls

- Do not claim the originals are merged just because their commits are present
  on the release branch. Verify the PR objects.
- Do not close PRs manually when the user asked for "closed as merged".
- Do not process out of order when the user requested numeric ordering.
- Do not squash/rebase unless the user or repo policy explicitly calls for it;
  when the user says "as if merging to main", use the normal GitHub merge path.
- If GitHub says there are no commits between the release branch and PR head,
  add and push an empty marker commit to the source branch, then retarget and
  merge through GitHub.
