# Execute Full Workflow

Run complete workflow for: $ARGUMENTS

## Overview

This command orchestrates the full Research → Question → Plan → Execute → Validate workflow. It coordinates all sub-agents and ensures quality gates are enforced.

## Execution

### 1. Initialize Session

Create session directory structure:
```
docs/sessions/{date}-{slug}/
├── session.md        # Session tracking
├── research/         # Research artifacts
├── specs/            # Requirements
├── plans/            # Architecture & tasks
├── reviews/          # Validation reports
└── summary.md        # Final summary (created at end)
```

Initialize session tracking file with:
- Feature description
- Current phase (Research)
- Start timestamp
- Phase progress table

### 2. Research & Planning Phase

**Invoke:** `/research`

This combined phase handles:
1. Research codebase with appropriate sub-agents
2. Generate and present clarifying questions
3. Await user answers
4. Create architecture design (requires approval)
5. Create implementation plan (requires approval)

**Gates:**
- Research findings documented
- All blocking questions answered
- Architecture approved by user
- Implementation plan approved by user

### 3. Implementation Phase

**Invoke:** `/execute`

**Process:**
1. Load approved implementation plan
2. For each task:
   - Route to appropriate developer sub-agent
   - Execute task
   - Run verification
   - Commit changes
   - Update progress
3. Handle any deviations
4. Complete all tasks

**Sub-agent routing:**
| Task Type | Sub-Agent |
|-----------|-----------|
| API/Backend | backend-developer |
| UI/Frontend | frontend-developer |
| Database | database-specialist |

**Gate:** All tasks completed, code builds

### 4. Validation Phase

**Invoke:** `/code-check`

**Process:**
1. Run code review (code-reviewer)
2. If critical issues: STOP, fix, restart
3. Run tests (test-automator)
4. If tests fail: STOP, fix, restart
5. Run security audit (security-auditor)
6. If critical vulnerabilities: STOP, fix, restart
7. Update documentation (documentation-writer)
8. Generate final validation report

**Gate:** All validations pass

### 5. Completion

Generate final summary:
- What was built
- Key decisions made
- Files changed
- Validation results
- Known limitations
- Follow-up items

Update session status to COMPLETE.

Present results to user.

## User Interaction Points

| Phase | Interaction | Required |
|-------|-------------|----------|
| Research | Answer questions | Yes |
| Research | Approve architecture | Yes |
| Research | Approve implementation plan | Yes |
| Validate | Acknowledge results | Yes |

## Abort Handling

If user requests abort:
1. Save current state to session.md
2. Document last completed action
3. Create resume instructions in session directory
4. Provide command to resume later

## Resume Handling

If resuming interrupted session:
1. Load session.md to determine state
2. Identify last completed phase
3. Verify artifacts exist
4. Continue from appropriate point
5. Ask user to confirm resume point

## Phase Indicators

- 🔍 Research & Planning
- 🔨 Implementation
- ✅ Validation
- ✓ Complete

## Error Handling

| Error Type | Action |
|------------|--------|
| Research fails | Document gap, ask user for guidance |
| User declines plan | Revise based on feedback |
| Implementation fails | Document issue, attempt recovery |
| Validation fails | Return to implementation, fix issues |
| Critical blocker | Pause workflow, escalate to user |

## Session Artifacts

All workflow artifacts are preserved:

| Artifact | Location |
|----------|----------|
| Session tracking | `docs/sessions/{session}/session.md` |
| Research findings | `docs/sessions/{session}/research/` |
| Requirements | `docs/sessions/{session}/specs/` |
| Architecture | `docs/sessions/{session}/plans/` |
| Implementation plan | `docs/sessions/{session}/plans/` |
| Review reports | `docs/sessions/{session}/reviews/` |
| Final summary | `docs/sessions/{session}/summary.md` |
