---
argument-hint: thoughts/shared/plans/plan.md
---

# Implement Plan

You are tasked with implementing an approved technical plan from `thoughts/shared/plans/`. These plans contain phases with specific changes and success criteria.

## Getting Started

**Plan file path:** $ARGUMENTS

If a plan path was provided:
- Read the plan completely and check for any existing checkmarks (- [x])
- Read the original ticket and all files mentioned in the plan
- **Read files fully** - never use limit/offset parameters, you need complete context
- Think deeply about how the pieces fit together
- Create a todo list to track your progress
- Start implementing if you understand what needs to be done

If no plan path provided:
- Ask the user to provide the path to the plan file in `thoughts/shared/plans/`
- Example: `thoughts/shared/plans/2025-01-08-feature-name.md`

## Codebase Context

Before implementing:
- **IMPORTANT**: Review `CLAUDE.md` (or equivalent project documentation) in the repository root for:
  - Architecture patterns and conventions to follow
  - Technology stack and framework-specific requirements
  - Build system and project structure
  - Testing commands and patterns
  - Code quality requirements (linting, formatting, type checking)
  - Development workflow and where to run commands

## Implementation Philosophy
  
Plans are carefully designed, but reality can be messy. Your job is to:
- Follow the plan's intent while adapting to what you find
- Implement each phase fully before moving to the next
- Verify your work makes sense in the broader codebase context
- Update checkboxes in the plan as you complete sections

When things don't match the plan exactly, think about why and communicate clearly. The plan is your guide, but your judgment matters too.

If you encounter a mismatch:
- STOP and think deeply about why the plan can't be followed
- Present the issue clearly:
  ```
  Issue in Phase [N]:
  Expected: [what the plan says]
  Found: [actual situation]
  Why this matters: [explanation]

  How should I proceed?
  ```

## Verification Approach

After implementing a phase, run these checks (refer to CLAUDE.md for specific commands):

### Code Quality Checks:
1. **Linting**: Run the project's linting command - Must pass per code quality requirements
2. **Type checking**: Run type checking if applicable (TypeScript, mypy, etc.)
3. **Tests**: Run the appropriate test command for affected projects
   - Use build system test runners when available
   - For more verbose output or specific tests, use framework-specific commands directly

Fix any issues before proceeding. Then:
- Update your progress in both the plan and your todos
- Check off completed items in the plan file itself using Edit
- **Pause for human verification**: After completing all automated verification for a phase, pause and inform the human that the phase is ready for manual testing. Use this format:
  ```
  Phase [N] Complete - Ready for Manual Verification

  Automated verification passed:
  - [List automated checks that passed]

  Please perform the manual verification steps listed in the plan:
  - [List manual verification items from the plan]

  Let me know when manual testing is complete so I can proceed to Phase [N+1].
  ```

If instructed to execute multiple phases consecutively, skip the pause until the last phase. Otherwise, assume you are just doing one phase.

do not check off items in the manual testing steps until confirmed by the user.


## If You Get Stuck

When something isn't working as expected:
- First, make sure you've read and understood all the relevant code
- Consider if the codebase has evolved since the plan was written
- Present the mismatch clearly and ask for guidance

Use sub-tasks sparingly - mainly for targeted debugging or exploring unfamiliar territory.

## Resuming Work

If the plan has existing checkmarks:
- Trust that completed work is done
- Pick up from the first unchecked item
- Verify previous work only if something seems off

Remember: You're implementing a solution, not just checking boxes. Keep the end goal in mind and maintain forward momentum.
