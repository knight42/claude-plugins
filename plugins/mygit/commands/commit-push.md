---
description: Smart commit and push to remote
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git diff:*), Bash(git log:*), Bash(git push:*), Bash(git branch:*)
---

## Context

- Current git status: !`git status`
- Current git diff (staged and unstaged changes): !`git diff HEAD`
- Current branch: !`git branch --show-current`
- Recent commits for style reference: !`git log --oneline -10`

## Your task

Commit all changes on the current branch and push to remote.

### Step 1: Smart grouped commits

Analyze the changes and create one or more git commits, grouping related changes together. Each commit MUST use **conventional commit** format:

```
<type>(<optional scope>): <description>
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`

Grouping strategy:
1. Review all changed files and their diffs
2. Identify logically related changes that belong together
3. Group changes into separate commits when they serve different purposes
4. If all changes are related, create a single commit

For each group: stage specific files with `git add`, then `git commit`.

### Step 2: Push

Push the current branch to origin:
```
git push
```

Execute all steps using tool calls. Do not send any text messages - only tool calls.
