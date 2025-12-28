---
argument-hint: thoughts/shared/plans/plan.md
---

# Finalize Plan

You are tasked with finalizing a completed implementation plan - marking it as complete, cleaning up verbose content, and creating a concise summary for future reference.

This command is typically called automatically by `/claude-workflow:validate_plan` after successful validation, but can also be run standalone when you're confident the implementation is complete.

**Plan file path:** $ARGUMENTS

## Prerequisites

Before finalizing, ensure:

- The implementation is complete
- All automated tests pass
- The plan has been validated (via `/claude-workflow:validate_plan` or manual review)

If you're unsure whether the implementation is complete, run `/claude-workflow:validate_plan` first.

## Finalization Process

### Step 1: Read and Understand the Plan

1. Read the plan file completely
2. Identify all phases and what was implemented
3. Note any code examples, detailed instructions, or research notes

### Step 2: Add Completion Status

Add a status block at the very top of the plan (after any YAML frontmatter):

```markdown
> **Status:** Completed
> **Completed:** YYYY-MM-DD
> **Summary:** [One sentence describing what was accomplished]
```

Example:
```markdown
> **Status:** Completed
> **Completed:** 2025-01-15
> **Summary:** Added user authentication with JWT tokens and session management
```

### Step 3: Clean Up Verbose Content

Transform the plan from an implementation guide into a historical record.

**Remove:**
- Inline code examples (the real code is now in the codebase)
- Detailed step-by-step instructions
- Research notes and alternatives that weren't chosen
- Lengthy explanations of how to implement things
- Code snippets showing what to write

**Keep:**
- Phase names and descriptions
- File paths that were created/modified
- Key architectural decisions and why they were made
- Success criteria (mark as completed)
- Any gotchas or important notes for future reference

**Before cleanup:**
```markdown
### Phase 1: Add User Model

Create the user model with the following fields:

```typescript
interface User {
  id: string;
  email: string;
  createdAt: Date;
}
```

Implementation steps:
1. Create src/models/user.ts
2. Add validation using zod schema
3. Export from src/models/index.ts

Make sure to handle edge cases like:
- Empty email strings
- Invalid email formats
```

**After cleanup:**
```markdown
### Phase 1: Add User Model

Added `User` interface with id, email, and createdAt fields.

**Files:** `src/models/user.ts`, `src/models/index.ts`

**Notes:** Includes zod validation for email format.
```

### Step 4: Add Implementation Summary

Add a summary section at the end of the plan:

```markdown
---

## Implementation Summary

### What Was Built
- [Component/feature 1]: Brief description of what it does
- [Component/feature 2]: Brief description of what it does

### Key Files
- `path/to/file1.ts` - [purpose]
- `path/to/file2.ts` - [purpose]

### Design Decisions
- [Decision 1]: [Brief rationale]
- [Decision 2]: [Brief rationale]

### Testing
- [How to test / what tests were added]
```

### Step 5: Write the Finalized Plan

Use the Edit tool to update the plan file with all changes:

1. Add the status block at the top
2. Replace verbose sections with concise summaries
3. Add the implementation summary at the end

### Step 6: Confirm Completion

Report to the user:

```
Plan finalized: [plan path]

Changes made:
- Added completion status (YYYY-MM-DD)
- Condensed [N] phases from detailed instructions to summaries
- Removed [N] code examples
- Added implementation summary

The plan is now archived for future reference.
```

## Guidelines

1. **Preserve important context** - Don't remove information that explains *why* decisions were made
2. **Keep file references** - These help locate the implementation later
3. **Be concise but complete** - Someone reading this later should understand what was built
4. **Don't lose gotchas** - If there were tricky parts, keep notes about them

## Example: Full Before/After

**Before (implementation plan):**
```markdown
# Add Search Feature

## Phase 1: Backend API

Create a search endpoint at `/api/search`.

```typescript
app.get('/api/search', async (req, res) => {
  const { query } = req.query;
  const results = await searchService.search(query);
  res.json(results);
});
```

Steps:
1. Create src/routes/search.ts
2. Implement SearchService class
3. Add to router in src/app.ts

## Phase 2: Frontend Component

Create SearchBar component...
[more detailed instructions]
```

**After (finalized):**
```markdown
> **Status:** Completed
> **Completed:** 2025-01-15
> **Summary:** Added full-text search with backend API and frontend SearchBar component

# Add Search Feature

## Phase 1: Backend API

Added `/api/search` endpoint with SearchService for query handling.

**Files:** `src/routes/search.ts`, `src/services/searchService.ts`

## Phase 2: Frontend Component

Added SearchBar component with debounced input and result highlighting.

**Files:** `src/components/SearchBar.tsx`, `src/components/SearchResults.tsx`

---

## Implementation Summary

### What Was Built
- Search API endpoint with fuzzy matching
- Reusable SearchBar component with keyboard navigation

### Key Files
- `src/routes/search.ts` - API endpoint
- `src/services/searchService.ts` - Search logic
- `src/components/SearchBar.tsx` - UI component

### Design Decisions
- Used debouncing (300ms) to reduce API calls
- Implemented fuzzy matching for better UX

### Testing
- Added tests in `src/__tests__/search.test.ts`
- Manual testing: type in search bar, verify results appear
```

## Relationship to Other Commands

- `/claude-workflow:create_plan` - Creates the initial implementation plan
- `/claude-workflow:implement_plan` - Executes the plan
- `/claude-workflow:validate_plan` - Verifies implementation, then calls `/claude-workflow:finalize_plan` on success
- `/claude-workflow:finalize_plan` - Archives the plan (this command)
