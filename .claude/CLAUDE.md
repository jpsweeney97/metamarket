# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A Claude Code plugin marketplace containing meta-level tools for Claude Code development. The primary plugin is **skillosophy** - a collaborative skill creation tool with multi-agent synthesis.

## Commands

```bash
# Validate marketplace
/plugin validate .

# Validate specific plugin
/plugin validate ./plugins/skillosophy

# Local development (install from working directory)
/plugin marketplace add /path/to/metamarket
/plugin install skillosophy@metamarket

# Distribution (install from GitHub)
/plugin marketplace add jpsweeney97/metamarket
```

## Architecture

### Marketplace Structure

```
.claude-plugin/marketplace.json    # Marketplace manifest (name, plugins array)
plugins/<plugin-name>/             # Individual plugins
```

### Plugin Structure (skillosophy)

```
.claude-plugin/plugin.json         # Plugin manifest
skills/skillosophy/SKILL.md        # Main skill definition
agents/                            # Synthesis panel (4 agents)
hooks/hooks.json                   # PostToolUse validation hook
scripts/validate_skill.sh          # SKILL.md structural validator
```

### Synthesis Panel Agents

Four agents provide independent quality review in Phase 4:
- `adversarial-reviewer.md` - Attack surface analysis
- `dialogue-auditor.md` - Requirement coverage verification
- `executability-auditor.md` - Feasibility assessment
- `semantic-coherence-checker.md` - Internal consistency

### Hook System

The `validate_skill.sh` hook runs on every Write/Edit but has early-exit optimization:
1. String check: exits immediately if "SKILL.md" not in input
2. Validates only files ending in `SKILL.md`
3. Checks: YAML frontmatter (name, description required), 11 H2 sections

Exit codes: 0=pass, 1=warning (non-blocking), 2=block

## Key Patterns

- **`${CLAUDE_PLUGIN_ROOT}`**: Use in hooks/MCP configs for plugin paths (plugins are cached when installed)
- **Local development**: Directory source bypasses caching for immediate edit feedback
- **Source paths**: Must start with `./` in marketplace.json (e.g., `"./plugins/skillosophy"`)

## Reference Documentation

Comprehensive Claude Code extension development guides in `.claude/rules/`:

| File | Covers |
|------|--------|
| `plugins.md` | Plugin structure, manifest schema, marketplace distribution |
| `skills.md` | SKILL.md format, progressive disclosure, hot-reload |
| `hooks.md` | Hook events, exit codes, input/output schemas |
| `agents.md` | Agent definitions, Task tool integration |
| `commands.md` | Slash command format, frontmatter options |
| `mcp-servers.md` | MCP server configuration, tool definitions |
| `settings.md` | Settings schema, precedence, permissions |

Consult these when developing or modifying plugin components.
