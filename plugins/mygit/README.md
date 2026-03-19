# mygit

A Claude Code plugin for smart git workflows: grouped conventional commits, PR creation, branch sync, and autonomous PR babysitting.

## Skills

### git-workflow

Unified skill with 4 modes — Claude picks the right one based on your intent:

- **commit**: Analyzes changes, groups related changes into separate [conventional commits](https://www.conventionalcommits.org/)
- **commit-push**: Smart grouped commits + push to remote
- **commit-push-pr**: Full workflow — commits, push, and PR creation via `gh`. Auto-creates a feature branch if on main
- **sync**: Switches to main, pulls latest, prunes stale branches, cleans up merged local branches

### babysit-pr

Autonomous PR maintenance loop. Monitors a pull request and keeps it healthy:

- Fixes failed CI checks (tests, lint, type errors, build failures) by reading logs, diagnosing, and pushing fixes
- Rebases to resolve merge conflicts
- Enables auto-merge (`--squash`) when the PR is ready

Designed for use with `/loop` for continuous monitoring. Each invocation is one check-and-fix pass.

## Installation

```bash
claude --plugin-dir /path/to/this/repo
```

## Prerequisites

- Git
- [GitHub CLI](https://cli.github.com/) (`gh`) — required for PR-related workflows
