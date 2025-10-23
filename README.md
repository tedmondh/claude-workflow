# Claude Code Workflow Tools

A collection of custom agents and slash commands for Claude Code that streamline code exploration, planning, and implementation.

## Quick Start

```bash
# 1. Create a detailed implementation plan
/create_plan add user authentication feature
# or just: /create_plan (it will prompt you)

# 2. Execute the plan
/implement_plan path/to/plan.md

# 3. Validate the implementation
/validate_plan path/to/plan.md
```

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

Specialized agents used during planning (primarily by `/create_plan`):

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
