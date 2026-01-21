# Claude Code Workflow Tools

A collection of slash commands for Claude Code that streamline code exploration, planning, and implementation.

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
/claude-workflow:create_plan ADP-123
# or just: /claude-workflow:create_plan (it will prompt you for requirements)

# 2. Execute the plan (provide the file path as an argument)
/claude-workflow:implement_plan thoughts/shared/plans/your-plan.md

# 3. Validate the implementation
/claude-workflow:validate_plan thoughts/shared/plans/your-plan.md
```

## Linear Integration (Optional)

The `/create_plan` command can automatically fetch ticket details from Linear when you provide a ticket ID:

```bash
/claude-workflow:create_plan ADP-123
```

### Setup

To enable Linear integration, configure the Linear MCP server:

```bash
claude mcp add --transport sse linear https://mcp.linear.app/sse
```

Then authenticate by running `/mcp` in Claude Code and following the prompts.

### Usage

Once configured, simply pass a ticket ID to create_plan:

```bash
/claude-workflow:create_plan ENG-456
```

The command will fetch the ticket's title and description, then ask if you want to add additional context (files, directories, constraints) before planning begins.

If Linear MCP isn't configured, you'll be prompted to either set it up or paste the ticket details manually.

## Structure

```
.
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

## File Conventions

- Plans: `thoughts/shared/plans/YYYY-MM-DD-description.md`
- Research: `thoughts/shared/research/`

## Design Philosophy

- **Iterative collaboration**: Work with users, not in isolation
- **Thoroughness**: Complete file reads, parallel research, precise references
- **Clear verification**: Separate automated (tests, linting) from manual (UI, performance) checks
- **Adaptability**: Follow plan intent while adapting to code reality
