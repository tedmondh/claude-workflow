# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Claude Code plugin that provides structured workflows for code exploration, planning, and implementation. It includes specialized agents and slash commands.

## Structure

- `agents/` - Specialized agents for codebase research (markdown files with frontmatter)
- `commands/` - Slash command definitions (markdown files with prompt templates)
- `.claude-plugin/` - Plugin configuration (plugin.json, marketplace.json)

## Plugin Components

### Commands (user-invocable)

- `/claude-workflow:create_plan` - Interactive planning workflow that researches codebase, asks questions, and writes plans to `thoughts/shared/plans/YYYY-MM-DD-description.md`
- `/claude-workflow:implement_plan <path>` - Executes approved plans phase-by-phase with automated verification
- `/claude-workflow:validate_plan <path>` - Validates implementation against plan specifications, calls `/claude-workflow:finalize_plan` on success
- `/claude-workflow:finalize_plan <path>` - Archives a completed plan: adds completion status, removes code examples, adds summary

### Agents (used by commands internally)

- `codebase-locator` - Finds WHERE code lives (files, directories)
- `codebase-analyzer` - Understands HOW code works (implementation details, data flow)
- `codebase-pattern-finder` - Finds similar implementations to model after

## Agent/Command File Format

Agent and command files use YAML frontmatter:

```yaml
---
name: agent-name
description: Brief description shown in tool descriptions
tools: Read, Grep, Glob, LS # Available tools for this agent
model: inherit # Use parent model
---
[Prompt content with instructions]
```

Commands can include `argument-hint:` for file path autocomplete and `$ARGUMENTS` placeholder for passed arguments.

## Design Philosophy

Agents follow a "documentarian, not critic" approach - they describe what exists without suggesting improvements unless explicitly asked. This keeps research focused and prevents unwanted refactoring suggestions.
