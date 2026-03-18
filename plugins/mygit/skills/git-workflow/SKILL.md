---
name: git-workflow
description: Smart git workflow automation — grouped conventional commits, push, PR creation, and branch sync. Use this skill whenever the user wants to commit changes, push code, create a pull request, sync their main branch, or clean up stale branches. Triggers on phrases like "commit this", "push it", "create a PR", "sync main", "clean up branches", or any combination of these git operations.
---

# Git Workflow

Automates common git workflows. Determine which mode to use based on the user's intent:

| User intent | Mode |
|---|---|
| Commit changes only | **commit** |
| Commit and push | **commit-push** |
| Commit, push, and create a PR | **commit-push-pr** |
| Sync main and clean up branches | **sync** |

If the intent is ambiguous, default to **commit** — the safest option.

---

## Smart Grouped Commits (used by commit / commit-push / commit-push-pr)

Analyze all changes and create one or more git commits, grouping related changes together. Each commit uses **conventional commit** format:

```
<type>(<optional scope>): <description>
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`

### Grouping strategy

1. Review all changed files and their diffs
2. Identify logically related changes that belong together (e.g., a feature and its tests, a refactor across related files)
3. Group changes into separate commits when they serve different purposes
4. If all changes are related to a single purpose, create a single commit

For each group: stage specific files with `git add`, then `git commit`. Process groups sequentially.

---

## Mode: commit

1. Run `git status`, `git diff HEAD`, `git log --oneline -10` for context
2. Execute smart grouped commits
3. Done

## Mode: commit-push

1. Run `git status`, `git diff HEAD`, `git branch --show-current`, `git log --oneline -10` for context
2. Execute smart grouped commits
3. `git push`

## Mode: commit-push-pr

1. Run `git status`, `git diff HEAD`, `git branch --show-current`, `git log --oneline -10` for context
2. If on `main` or `master`, create a feature branch derived from the changes:
   - `feat/short-description`, `fix/short-description`, `chore/short-description`, `refactor/short-description`, `docs/short-description`
   - `git checkout -b <branch-name>`
3. Execute smart grouped commits
4. `git push -u origin <branch-name>`
5. Create PR via `gh pr create --title "<title>" --body "..."` with a summary of all commits

## Mode: sync

1. Run `git branch --show-current`, `git branch -v`, `git worktree list` for context
2. Determine default branch (`main` or `master` via `git rev-parse --verify main`)
3. `git checkout <main-branch> && git pull`
4. `git fetch --prune`
5. For each branch marked `[gone]` in `git branch -v`:
   - If it has a worktree, `git worktree remove --force <path>`
   - `git branch -D <branch-name>`
6. Report what was done

---

Execute all steps using tool calls with minimal text output. Only the **sync** mode's final report should include a text summary.
