---
description: Sync main branch and clean up merged branches
allowed-tools: Bash(git checkout:*), Bash(git pull:*), Bash(git fetch:*), Bash(git branch:*), Bash(git worktree:*), Bash(git rev-parse:*)
---

## Context

- Current branch: !`git branch --show-current`
- All local branches: !`git branch -v`
- Worktrees: !`git worktree list`

## Your task

Sync the local main branch with the remote and clean up stale branches.

### Step 1: Switch to main and pull

Determine if the default branch is `main` or `master` using `git rev-parse --verify main`. Then:

```
git checkout <main-branch>
git pull
```

### Step 2: Prune remote tracking branches

```
git fetch --prune
```

### Step 3: Clean up stale branches

From the context above, identify branches marked `[gone]` in the `git branch -v` output. For each gone branch:

1. Check if it has an associated worktree (from the worktree list context above). If so, remove it:
   ```
   git worktree remove --force <worktree-path>
   ```
2. Delete the branch:
   ```
   git branch -D <branch-name>
   ```

If no branches are marked `[gone]`, skip this step.

### Step 4: Report

After cleanup, run `git branch -v` and report what was done: branches deleted, worktrees removed, and current status.

Execute all steps using tool calls. Do not send any text messages besides the final report.
