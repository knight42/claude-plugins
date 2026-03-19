---
name: babysit-pr
description: REQUIRED when two conditions are met: (1) an existing pull request is referenced — via "my PR", "a PR", "PR #N", a GitHub PR URL, or "opened a PR", AND (2) the user wants it fixed, unblocked, or monitored. Problem signals: CI red, checks failing, test/lint/type/build errors, merge conflicts, "blocked", "not merging", "make it mergeable". Monitoring signals: babysit, watch, keep an eye on, auto-merge, "until it lands". Reads CI logs, pushes fixes, rebases conflicts, enables auto-merge. SKIP when: no existing PR is involved, user wants to create a new PR, cherry-pick, or do git operations on a branch without a PR.
---

# Babysit PR

Monitors a pull request and autonomously maintains it: fixes failed CI checks, rebases to resolve merge conflicts, and enables auto-merge. Designed for repeated invocation via `/loop` — each call is one check-and-fix pass.

## PR Identification

Determine the target PR from (in priority order):

1. Explicit PR number or URL in the user's message
2. Current branch → `gh pr view --json number,title`
3. Ask the user

## Workflow

Each invocation runs through these steps in order. After each push, wait for checks to complete before continuing.

### 1. Check PR status

```bash
gh pr view <PR> --json number,title,state,mergeable,baseRefName,headRefName,autoMergeRequest
gh pr checks <PR>
```

If the PR is merged or closed, report final state and stop.

### 2. Fix failed checks

If any checks are failing:

1. Find failed runs:
   ```bash
   gh run list --branch <head-branch> --status failure --limit 5 --json databaseId,name,conclusion
   ```
2. Read failure logs:
   ```bash
   gh run view <run-id> --log-failed
   ```
3. Diagnose the failure from the logs. Common categories:
   - **Test failures**: read the failing test and code under test, fix the root cause
   - **Lint/format errors**: run the linter/formatter locally, apply fixes
   - **Type errors**: fix type issues
   - **Build failures**: fix compilation errors
4. If the failure is ambiguous or requires design decisions, report it and ask the user instead of guessing.
5. Stage, commit, and push:
   ```bash
   git add <specific-files>
   git commit -m "fix(ci): <description>"
   git push
   ```
6. Wait for checks to complete:
   ```bash
   gh pr checks <PR> --watch
   ```
7. If checks pass, continue to step 3. If checks fail again, loop back to step 2.1 to diagnose the new failure.

### 3. Resolve merge conflicts

If `mergeable` is `CONFLICTING`:

1. Fetch and rebase:
   ```bash
   git fetch origin
   git rebase origin/<base-branch>
   ```
2. Resolve each conflict — preserve the feature branch's intent while incorporating base branch changes.
3. Complete the rebase:
   ```bash
   git add <resolved-files>
   git rebase --continue
   ```
4. Push safely:
   ```bash
   git push --force-with-lease
   ```
5. Wait for checks to complete:
   ```bash
   gh pr checks <PR> --watch
   ```
6. If checks fail after rebase, go to step 2 to fix them.

### 4. Enable auto-merge

If all checks are passing and auto-merge is not yet enabled (`autoMergeRequest` is null):

```bash
gh pr merge <PR> --auto --squash
```

### 5. Report

Output a single status line:

| Status | Meaning |
|---|---|
| All checks passing, auto-merge enabled | Nothing to do — PR will merge when ready |
| Fixed \<description\>, checks green, auto-merge enabled | PR is ready to merge |
| Rebased onto \<base\>, checks green, auto-merge enabled | PR is ready to merge |
| Could not fix: \<reason\> | Needs user intervention |

---

## Key Behaviors

- **Wait after push**: always run `gh pr checks <PR> --watch` after pushing, then continue the workflow based on the result.
- **Safe force-push**: always `--force-with-lease`, never `--force`.
- **Ask when unsure**: if a CI failure is ambiguous or a conflict resolution requires a judgment call, report the issue and ask the user rather than guessing.
- **Idempotent**: each invocation reads current PR state fresh. Safe to run on a timer via `/loop`. When there's nothing to do, report status and exit quickly.
