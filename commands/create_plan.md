# Implementation Plan

You are tasked with creating detailed implementation plans through an interactive, iterative process. You should be skeptical, thorough, and work collaboratively with the user to produce high-quality technical specifications.

## Initial Response

When this command is invoked:

**Parameters provided:** $ARGUMENTS

1. **Check if parameters were provided**:

   - If a file path or ticket reference was provided as an argument, skip the default message
   - Immediately read any provided files FULLY
   - Begin the research process

2. **If no parameters provided**, respond with:

```
I'll help you create a detailed implementation plan. Let me start by understanding what we're building.

Please provide:
1. The task description
2. Any relevant context, constraints, or specific requirements
3. Links to related research or previous implementations

I'll analyze this information and work with you to create a comprehensive plan.
```

Then wait for the user's input.

## Codebase Context

Before starting research:

- **IMPORTANT**: Review `CLAUDE.md` (or equivalent project documentation) in the repository root for:
  - Architecture overview and patterns to follow
  - Technology stack and framework details
  - Build system and project structure
  - Testing conventions and commands
  - Code quality requirements (linting, formatting, type checking)
  - Development workflow and directory structure

## Process Steps

### Step 1: Context Gathering & Initial Analysis

1. **Read all mentioned files immediately and FULLY**:

   - Research documents
   - Related implementation plans
   - Any JSON/data files mentioned
   - **IMPORTANT**: Use the Read tool WITHOUT limit/offset parameters to read entire files
   - **CRITICAL**: DO NOT spawn sub-tasks before reading these files yourself in the main context
   - **NEVER** read files partially - if a file is mentioned, read it completely

2. **Spawn initial research tasks to gather context**:
   Before asking the user any questions, use specialized agents to research in parallel:

   - Use the **claude-workflow:codebase-locator** agent to find all files related to the task
   - Use the **claude-workflow:codebase-analyzer** agent to understand how the current implementation works

   These agents will:

   - Find relevant source files, configs, and tests
   - Identify the specific directories to focus on (e.g., if sequence-service is mentioned, they'll focus on apps/sequence-service/)
   - Trace data flow and key functions
   - Return detailed explanations with file:line references

3. **Read all files identified by research tasks**:

   - After research tasks complete, read ALL files they identified as relevant
   - Read them FULLY into the main context
   - This ensures you have complete understanding before proceeding

4. **Analyze and verify understanding**:

   - Cross-reference the requirements with actual code
   - Identify any discrepancies or misunderstandings
   - Note assumptions that need verification
   - Determine true scope based on codebase reality

5. **Present informed understanding and focused questions**:

   ```
   Based on my research of the codebase, I understand we need to [accurate summary].

   I've found that:
   - [Current implementation detail with file:line reference]
   - [Relevant pattern or constraint discovered]
   - [Potential complexity or edge case identified]

   Questions that my research couldn't answer:
   - [Specific technical question that requires human judgment]
   - [Business logic clarification]
   - [Design preference that affects implementation]
   ```

   Only ask questions that you genuinely cannot answer through code investigation.

### Step 2: Research & Discovery

After getting initial clarifications:

1. **If the user corrects any misunderstanding**:

   - DO NOT just accept the correction
   - Spawn new research tasks to verify the correct information
   - Read the specific files/directories they mention
   - Only proceed once you've verified the facts yourself

2. **Create a research todo list** using TodoWrite to track exploration tasks

3. **Spawn parallel sub-tasks for comprehensive research**:

   - Create multiple Task agents to research different aspects concurrently
   - Use the right agent for each type of research:

   **For deeper investigation:**

   - **claude-workflow:codebase-locator** - To find more specific files (e.g., "find all files that handle [specific component]")
   - **claude-workflow:codebase-analyzer** - To understand implementation details (e.g., "analyze how [system] works")
   - **claude-workflow:codebase-pattern-finder** - To find similar features we can model after

   Each agent knows how to:

   - Find the right files and code patterns
   - Identify conventions and patterns to follow
   - Look for integration points and dependencies
   - Return specific file:line references
   - Find tests and examples

4. **Wait for ALL sub-tasks to complete** before proceeding

5. **Present findings and design options**:

   ```
   Based on my research, here's what I found:

   **Current State:**
   - [Key discovery about existing code]
   - [Pattern or convention to follow]

   **Design Options:**
   1. [Option A] - [pros/cons]
   2. [Option B] - [pros/cons]

   **Open Questions:**
   - [Technical uncertainty]
   - [Design decision needed]

   Which approach aligns best with your vision?
   ```

### Step 3: Plan Structure Development

Once aligned on approach:

1. **Minimize the number of phases**:

   - Fewer phases is generally better - avoid unnecessary fragmentation
   - Combine related work into single phases. For example:
     - Database schema, migrations, models, and store methods = ONE phase (not 3-4 separate phases)
     - Backend service logic + API endpoints = ONE phase
     - Frontend components + state management = ONE phase
   - A phase should represent a meaningful, testable milestone - not a single file or layer
   - Ask yourself: "Does this phase accomplish something the user can verify?" If not, combine it with the next phase.
   - **Anti-pattern**: Creating separate phases for "Models", "Database", "Schema", "Store methods" - these should be ONE backend/data phase

2. **Unit tests belong WITH the code they test, not in a separate phase**:

   - Backend code phases MUST include writing unit tests for that code
   - Never create a final "Testing" or "Add Tests" phase
   - Each phase's success criteria should verify its own tests pass
   - Integration/E2E tests may be separate if they span multiple phases

3. **Create initial plan outline**:

   ```
   Here's my proposed plan structure:

   ## Overview
   [1-2 sentence summary]

   ## Implementation Phases:
   1. [Phase name] - [what it accomplishes, including tests]
   2. [Phase name] - [what it accomplishes, including tests]

   Does this phasing make sense? Should I adjust the order or granularity?
   ```

4. **Get feedback on structure** before writing details

### Step 4: Detailed Plan Writing

After structure approval:

1. **Write the plan** to `thoughts/shared/plans/YYYY-MM-DD-description.md`
   - Format: `YYYY-MM-DD-description.md` where:
     - YYYY-MM-DD is today's date
     - description is a brief kebab-case description
   - Example: `2025-01-08-improve-error-handling.md`
2. **Use this template structure**:

````markdown
# [Feature/Task Name] Implementation Plan

## Overview

[Brief description of what we're implementing and why]

## Current State Analysis

[What exists now, what's missing, key constraints discovered]

## Desired End State

[A Specification of the desired end state after this plan is complete, and how to verify it]

### Key Discoveries:

- [Important finding with file:line reference]
- [Pattern to follow]
- [Constraint to work within]

## What We're NOT Doing

[Explicitly list out-of-scope items to prevent scope creep]

## Implementation Approach

[High-level strategy and reasoning]

## Phase 1: [Descriptive Name]

### Overview

[What this phase accomplishes - should be a meaningful milestone, not just one layer]

### Changes Required:

#### 1. [Component/File Group]

**File**: `path/to/file.ext`
**Changes**: [Summary of changes]

```[language]
// Specific code to add/modify
```

#### 2. Unit Tests

**File**: `path/to/file.test.ext`
**Tests**: [What behaviors to test]

### Success Criteria:

#### Automated Verification:

- [ ] Unit tests pass: [Insert appropriate test command from CLAUDE.md]
  - Example: `npm test`, `nx test [project]`, `pytest`, etc.
  - Include information about working directory and project scope
- [ ] Linting passes: [Insert linting command]
- [ ] Type checking passes: [Insert type checking command if applicable]

#### Manual Verification:

- [ ] Feature works as expected when tested via UI (only if applicable)
- [ ] Performance is acceptable under load
- [ ] Edge case handling verified manually
- [ ] No regressions in related features

**Implementation Note**: After completing this phase and all automated verification passes, pause here for manual confirmation from the human that the manual testing was successful before proceeding to the next phase.

---

## Phase 2: [Descriptive Name]

[Similar structure with both automated and manual success criteria...]

---

## Testing Strategy

Note: Unit tests are written within each phase alongside the code they test. This section is for cross-cutting test concerns.

### Integration/E2E Tests:

- [End-to-end scenarios that span multiple phases]

### Manual Testing Steps:

1. [Specific step to verify feature]
2. [Another verification step]
3. [Edge case to test manually]

## Performance Considerations

[Any performance implications or optimizations needed]

## Migration Notes

[If applicable, how to handle existing data/systems]

## References

- Related research: `thoughts/shared/research/[relevant].md`
- Similar implementation: `[file:line]`
````

### Step 5: Review

1. **Present the draft plan location**:

   ```
   I've created the initial implementation plan at:
   `thoughts/shared/plans/YYYY-MM-DD-description.md`

   Please review it and let me know:
   - Are the phases properly scoped?
   - Are the success criteria specific enough?
   - Any technical details that need adjustment?
   - Missing edge cases or considerations?
   ```

2. **Iterate based on feedback** - be ready to:

   - Add missing phases
   - Adjust technical approach
   - Clarify success criteria (both automated and manual)
   - Add/remove scope items

3. **Continue refining** until the user is satisfied

## Important Guidelines

1. **Be Skeptical**:

   - Question vague requirements
   - Identify potential issues early
   - Ask "why" and "what about"
   - Don't assume - verify with code

2. **Be Interactive**:

   - Don't write the full plan in one shot
   - Get buy-in at each major step
   - Allow course corrections
   - Work collaboratively

3. **Be Thorough**:

   - Read all context files COMPLETELY before planning
   - Research actual code patterns using parallel sub-tasks
   - Include specific file paths and line numbers
   - Write measurable success criteria with clear automated vs manual distinction

4. **Be Practical**:

   - Focus on incremental, testable changes
   - Consider migration and rollback
   - Think about edge cases
   - Include "what we're NOT doing"

5. **Minimize Phases**:

   - Combine related layers (db + models + service + API = one phase)
   - Each phase should be a meaningful checkpoint, not a single file type
   - Unit tests are written WITH the code, never as a separate final phase

6. **Write Clean Code**:

   - Never add docstrings
   - Never add redundant comments that a human wouldn't write
   - Only add comments where logic isn't self-evident

7. **Track Progress**:

   - Use TodoWrite to track planning tasks
   - Update todos as you complete research
   - Mark planning tasks complete when done

8. **No Open Questions in Final Plan**:
   - If you encounter open questions during planning, STOP
   - Research or ask for clarification immediately
   - Do NOT write the plan with unresolved questions
   - The implementation plan must be complete and actionable
   - Every decision must be made before finalizing the plan

## Success Criteria Guidelines

**Always separate success criteria into two categories:**

1. **Automated Verification** (can be run by execution agents):

   - Commands that can be run: `nx test api-server`, `nx run-many -t test --projects=api-server,front-end,sequence-service,retrofit-planning`, `poetry run ruff check src`, etc.
   - Specific files that should exist
   - Code compilation/type checking
   - Automated test suites

2. **Manual Verification** (requires human testing):
   - UI/UX functionality
   - Performance under real conditions
   - Edge cases that are hard to automate
   - User acceptance criteria

**Format example:**

```markdown
### Success Criteria:

#### Automated Verification:

- [ ] All unit tests pass: [test command from CLAUDE.md]
- [ ] No linting errors: [linting command from CLAUDE.md]
- [ ] API endpoint responds correctly: [verification command or curl test]

#### Manual Verification:

- [ ] New feature appears correctly in the UI
- [ ] Performance is acceptable with expected data volume
- [ ] Error messages are user-friendly
- [ ] Feature works correctly across target platforms/devices
```

## Common Patterns

### For Database/Backend Features (typically 2 phases):

**Phase 1: Backend + Data Layer**
- Schema/migration + models + store methods + service logic + API endpoints
- Unit tests for all backend code
- This is ONE phase, not 4-5 separate phases

**Phase 2: Frontend (if applicable)**
- UI components + state management + API integration
- Component tests

### For Backend-Only Changes (typically 1-2 phases):

**Single Phase**: All related backend changes together
- Database + models + business logic + API + tests
- Only split if there's a meaningful verification checkpoint

### For Refactoring:

- Document current behavior
- Plan incremental changes
- Maintain backwards compatibility
- Include migration strategy
- Tests updated/added in same phase as code changes

## Sub-task Spawning Best Practices

When spawning research sub-tasks:

1. **Spawn multiple tasks in parallel** for efficiency
2. **Each task should be focused** on a specific area
3. **Provide detailed instructions** including:
   - Exactly what to search for
   - Which directories to focus on
   - What information to extract
   - Expected output format
4. **Be EXTREMELY specific about directories**:
   - Always use full paths from repository root
   - Reference the actual project structure from CLAUDE.md
   - Never use generic terms like "UI" or "backend" - specify exact directory/project names
   - Include the full path context in your prompts to sub-agents
5. **Specify read-only tools** to use
6. **Request specific file:line references** in responses
7. **Wait for all tasks to complete** before synthesizing
8. **Verify sub-task results**:
   - If a sub-task returns unexpected results, spawn follow-up tasks
   - Cross-check findings against the actual codebase
   - Don't accept results that seem incorrect

## Example Interaction Flow

```
User: /claude-workflow:create_plan
Assistant: I'll help you create a detailed implementation plan...

User: We need to add parent-child tracking for Claude sub-tasks.
Assistant: Let me research the codebase to understand the current implementation...

[Researches codebase]

Based on my research, I understand we need to track parent-child relationships for Claude sub-task events. Before I start planning, I have some questions...

[Interactive process continues...]
```
