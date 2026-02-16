---
description: Smart commit with grouped changes and conventional commit messages
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git diff:*), Bash(git log:*)
---

## Context

- Current git status: !`git status`
- Current git diff (staged and unstaged changes): !`git diff HEAD`
- Current branch: !`git branch --show-current`
- Recent commits for style reference: !`git log --oneline -10`

## Your task

Analyze the changes above and create one or more git commits, grouping related changes together. Each commit MUST use **conventional commit** format:

```
<type>(<optional scope>): <description>
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`

### Grouping strategy

1. Review all changed files and their diffs
2. Identify logically related changes that belong together (e.g., a feature and its tests, a refactor across related files, a docs update)
3. Group changes into separate commits when they serve different purposes (e.g., a bug fix and an unrelated formatting change should be separate commits)
4. If all changes are related to a single purpose, create a single commit

### Execution

For each logical group:
1. Stage only the files belonging to that group using `git add` with specific file paths
2. Create a commit with an appropriate conventional commit message

Stage and commit all groups using tool calls. Do not send any text messages - only tool calls. If there are multiple independent groups, process them sequentially (stage group 1, commit group 1, stage group 2, commit group 2, etc.).
