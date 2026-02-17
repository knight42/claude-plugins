---
description: Smart commit, push, and create a pull request
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git diff:*), Bash(git log:*), Bash(git checkout:*), Bash(git push:*), Bash(gh pr create:*)
---

## Context

- Current git status: !`git status`
- Current git diff (staged and unstaged changes): !`git diff HEAD`
- Current branch: !`git branch --show-current`
- Recent commits for style reference: !`git log --oneline -10`

## Your task

Complete the full workflow: smart commit, push, and create a PR.

### Step 1: Create branch if on main

If the current branch is `main` or `master`, create and switch to a new feature branch. Derive the branch name from the changes using conventional format:

- `feat/short-description` for new features
- `fix/short-description` for bug fixes
- `chore/short-description` for maintenance
- `refactor/short-description` for refactoring
- `docs/short-description` for documentation

Use `git checkout -b <branch-name>`.

### Step 2: Smart grouped commits

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

### Step 3: Push

Push the branch to origin:
```
git push -u origin <branch-name>
```

### Step 4: Create PR

Create a pull request using `gh pr create`. The PR title should be concise (under 70 characters). The body should summarize all commits:

```
gh pr create --title "<title>" --body "$(cat <<'EOF'
## Summary
<1-3 bullet points summarizing the changes>
EOF
)"
```

Execute all steps using tool calls. Do not send any text messages - only tool calls.
