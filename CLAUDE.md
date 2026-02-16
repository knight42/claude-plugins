# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A collection of Claude Code plugins (`knight42-plugins`). Plugins extend Claude Code with custom functionality — skills, agents, hooks, MCP servers, and LSP servers.

## Repository Structure

```
.claude-plugin/marketplace.json   # Plugin collection registry (name, version, plugin list)
plugins/
  <plugin-name>/
    .claude-plugin/plugin.json    # Plugin metadata (name, version, description)
    commands/                     # Slash commands as markdown files
    skills/                       # Agent skills (folders with SKILL.md)
    agents/                       # Custom agent definitions
    hooks/                        # Event handlers (hooks.json)
    .mcp.json                     # MCP server configurations
    .lsp.json                     # LSP server configurations
    README.md
```

Not all directories are required — each plugin only includes what it needs. `commands/` and `skills/` directories must NOT be placed inside `.claude-plugin/`; only `plugin.json` goes there.

## Plugin Component Formats

**Commands** (`commands/<name>.md`): Slash commands with YAML frontmatter (`description`, `allowed-tools`) followed by context injection (`!`backtick-command``) and task instructions.

**Skills** (`skills/<name>/SKILL.md`): Model-invoked capabilities with frontmatter (`name`, `description`) — Claude uses them automatically based on task context.

## Important: Updating Plugins

When modifying a plugin (adding/removing components, changing metadata), always update **both**:
- `.claude-plugin/marketplace.json` (root-level collection registry)
- `plugins/<plugin-name>/.claude-plugin/plugin.json` (per-plugin metadata)

## Conventions

- Commits use conventional commit format: `<type>(<optional scope>): <description>`
- Command files specify explicit tool restrictions via `allowed-tools` frontmatter
- Commands should execute via tool calls with minimal text output
