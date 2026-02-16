# mygit

A Claude Code plugin for smart git workflows: grouped conventional commits, PR creation, and branch sync.

## Commands

### `/mygit:commit`

Analyzes all staged and unstaged changes, groups related changes into separate commits, and uses [conventional commit](https://www.conventionalcommits.org/) format (`feat:`, `fix:`, `docs:`, etc.).

### `/mygit:commit-push-pr`

Full workflow: smart grouped commits, push to remote, and create a pull request via `gh`. Auto-creates a feature branch if on main.

### `/mygit:sync`

Switches to main, pulls latest from remote, prunes stale remote tracking branches, and deletes local branches whose remotes have been merged/deleted. Handles worktree cleanup.

## Installation

```bash
claude --plugin-dir /path/to/this/repo
```

## Prerequisites

- Git
- [GitHub CLI](https://cli.github.com/) (`gh`) - for `/mygit:commit-push-pr`
