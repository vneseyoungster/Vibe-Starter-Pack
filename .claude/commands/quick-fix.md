# Quick Fix

Quick fix for: $ARGUMENTS

## Overview

Streamlined fix workflow for known problems. Gathers minimal context,
implements fix directly, and validates. No planning phase - main agent
performs the fix directly.

**Best for:** Bug fixes, typo corrections, config changes, simple refactors
where the problem and solution are already known.

---

## Phase 1: Context Gathering

### Step 1.1: Understand the Problem

Parse $ARGUMENTS to identify:
- Affected files/modules
- Type of fix (bug, typo, config, refactor)
- Expected outcome

### Step 1.2: Gather Context (Conditional)

**Only if file locations are unclear:**

Invoke `codebase-explorer` sub-agent (haiku) with:
- Target: Files related to the problem
- Scope: Quick scan, not full analysis
- Output: List of relevant file paths

**Only if coding patterns are unclear:**

Invoke `pattern-researcher` sub-agent (haiku) with:
- Target: Patterns in affected area
- Scope: Minimal context for fix
- Output: Relevant conventions

### Step 1.3: Read Affected Files

- Read the files identified above
- Understand current implementation
- Identify exact change locations

---

## Phase 2: Direct Fix

### Step 2.1: Complexity Check

If fix touches 3+ files, show warning:

> "This fix affects X files. Consider using `/research` + `/execute` for complex changes. Continue anyway? (y/n)"

- If user confirms, proceed with quick-fix
- If user declines, suggest the full workflow and stop

### Step 2.2: Implement Fix

**Main agent performs the fix directly:**

- Make minimal changes to solve the problem
- Follow patterns identified in context gathering
- Do not over-engineer or add unrelated changes
- Do not delegate to implementation sub-agents

### Step 2.3: Quality Check

After changes:
- Run typecheck (if applicable): `npm run typecheck` or equivalent
- Run lint (if applicable): `npm run lint` or equivalent
- Fix any introduced errors immediately

---

## Phase 3: Validation (Lightweight)

### Step 3.1: Self-Review

Main agent reviews changes:
- [ ] Fix addresses the stated problem
- [ ] No unintended side effects
- [ ] Code follows existing patterns
- [ ] No security issues introduced

### Step 3.2: Run Tests

- Run tests related to changed files (if exist)
- Report test results
- If tests fail, report failures and ask user how to proceed

### Step 3.3: Auto-Commit

If all checks pass:
- Stage changes
- Auto-commit with descriptive message: `fix: [brief description]`
- No user confirmation needed

---

## Output

Report to user:
- Files modified
- Changes summary
- Test results (if any)
- Commit hash

---

## Error Handling

### If problem is unclear

Ask user for clarification before proceeding. Do not guess.

### If fix touches many files (3+)

Show warning but let user decide (see Step 2.1).

### If tests fail

Report failures and ask user:
- Proceed without tests passing?
- Attempt to fix the failing tests?
- Abort the quick-fix?

### If typecheck/lint fails

Attempt to fix the errors. If unable to resolve, report to user and ask how to proceed.
