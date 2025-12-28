# Claude Code Workflow Tools

A collection of custom agents and slash commands for Claude Code that streamline code exploration, planning, and implementation.

## Installation

Install this plugin using Claude Code's plugin system:

```bash
# Add the marketplace
/plugin marketplace add tedmondh/claude-workflow

# Install the plugin
/plugin install claude-workflow@tedmondh
```

After installation, run `/help` to see the newly available commands.

## Quick Start

```bash
# 1. Create a detailed implementation plan
/claude-workflow:create_plan thoughts/shared/tickets/your-ticket.md
# or just: /claude-workflow:create_plan (it will prompt you)

# 2. Execute the plan (provide the file path as an argument)
/claude-workflow:implement_plan thoughts/shared/plans/your-plan.md

# 3. Validate the implementation
/claude-workflow:validate_plan thoughts/shared/plans/your-plan.md
```

**Note**: File paths must be provided as command arguments. While you can use `@` for file autocomplete in regular messages, slash commands require the path as a text argument.

## Structure

```
.
├── agents/           # Specialized agents for code exploration
└── commands/         # Slash commands for structured workflows
```

## Commands

### 📋 /create_plan

Creates detailed implementation plans through interactive research and collaboration.

**Process**: Understand requirements → Research codebase → Ask clarifying questions → Write plan

**Output**: `thoughts/shared/plans/YYYY-MM-DD-description.md`

### 🛠️ /implement_plan

Executes approved plans phase-by-phase with automated verification.

**Process**: Read plan → Implement phase → Run tests/linting → Update checkboxes → Pause for manual testing

### ✅ /validate_plan

Validates implementation completeness and generates verification report.

**Process**: Review commits → Run all checks → Compare to plan → Document deviations → Report status

## Agents

Specialized agents used during planning (primarily by `/claude-workflow:create_plan`):

- **codebase-locator**: Finds WHERE code lives (files, directories, components)
- **codebase-analyzer**: Understands HOW code works (implementation details, data flow)
- **codebase-pattern-finder**: Locates similar implementations to model after

All agents follow a "documentarian, not critic" approach - they describe what exists without suggesting improvements.

## File Conventions

- Plans: `thoughts/shared/plans/YYYY-MM-DD-description.md`
- Research: `thoughts/shared/research/`

## Design Philosophy

- **Iterative collaboration**: Work with users, not in isolation
- **Thoroughness**: Complete file reads, parallel research, precise references
- **Clear verification**: Separate automated (tests, linting) from manual (UI, performance) checks
- **Adaptability**: Follow plan intent while adapting to code reality
