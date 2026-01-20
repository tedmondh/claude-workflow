# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Claude Code plugin that provides structured workflows for code exploration, planning, and implementation.

## Structure

- `commands/` - Slash command definitions (markdown files with prompt templates)
- `.claude-plugin/` - Plugin configuration (plugin.json, marketplace.json)

## Commands (user-invocable)

- `/claude-workflow:create_plan` - Interactive planning workflow that researches codebase, asks questions, and writes plans to `thoughts/shared/plans/YYYY-MM-DD-description.md`
- `/claude-workflow:implement_plan <path>` - Executes approved plans phase-by-phase with automated verification
- `/claude-workflow:validate_plan <path>` - Validates implementation against plan specifications, calls `/claude-workflow:finalize_plan` on success
- `/claude-workflow:finalize_plan <path>` - Archives a completed plan: adds completion status, removes code examples, adds summary

## Search Tools

When exploring beyond built-in tools, prefer:
- `fd <pattern>` - Fast filename search (over `find`)
- `rg "pattern"` - Fast text search (over `grep`)
- `ast-grep --lang <lang> -p '<pattern>'` - AST-aware code search (over `rg` for code patterns)
  - Wildcards: `$NAME` = single node, `$$$` = rest/multiple

## Command File Format

Command files use YAML frontmatter:

```yaml
---
name: command-name
description: Brief description shown in tool descriptions
argument-hint: <file-path>  # Optional, for autocomplete
---
[Prompt content with $ARGUMENTS placeholder for passed arguments]
```
