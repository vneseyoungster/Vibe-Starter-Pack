# Project Context for Claude Code

## RQPIV Workflow System

This project uses the RQPIV (Research, Question, Plan, Implement, Validate)
workflow system for all development tasks.

### Quick Start

```
/start [feature description]
```

This command orchestrates the full workflow automatically.

### Manual Phase Execution

```
/research [task]         # Research, question, and plan
/execute [task]          # Implementation phase
/code-check [task]       # Validation phase
```

## Sub-Agent Reference

### Research Agents (Phase R)

| Agent | Purpose | Model |
|-------|---------|-------|
| `codebase-explorer` | Project structure mapping | haiku |
| `module-researcher` | Deep module analysis | sonnet |
| `frontend-researcher` | Frontend architecture | sonnet |
| `backend-researcher` | Backend architecture | sonnet |
| `dependency-researcher` | Package analysis | haiku |
| `pattern-researcher` | Convention detection | sonnet |

### Questioning Agents (Phase Q)

| Agent | Purpose | Model |
|-------|---------|-------|
| `requirement-analyst` | Requirement validation | opus |

### Planning Agents (Phase P)

| Agent | Purpose | Model |
|-------|---------|-------|
| `solution-architect` | Architecture design | opus |
| `task-planner` | Task breakdown | sonnet |

### Implementation Agents (Phase I)

| Agent | Purpose | Model |
|-------|---------|-------|
| `backend-developer` | Server-side code | sonnet |
| `frontend-developer` | Client-side code | sonnet |
| `database-specialist` | Database operations | sonnet |

### Validation Agents (Phase V)

| Agent | Purpose | Model |
|-------|---------|-------|
| `code-reviewer` | Code quality | inherit |
| `test-automator` | Test creation | sonnet |
| `security-auditor` | Security review | sonnet |
| `documentation-writer` | Documentation | haiku |

### Orchestration

| Agent | Purpose | Model |
|-------|---------|-------|
| `workflow-orchestrator` | Phase coordination | opus |

**IMPORTANT:** NEVER do extensive research in main context - always delegate to sub-agents to preserve context window.

## Artifact Locations

| Type | Location |
|------|----------|
| Research findings | `docs/research/` |
| Requirements | `docs/specs/` |
| Architecture | `docs/plans/` |
| Implementation plans | `docs/plans/` |
| Review reports | `docs/reviews/` |
| Session data | `docs/sessions/` |

## Quality Gates

### Research → Execute
- All research artifacts created
- All blocking questions answered
- Requirements document approved
- Architecture approved by user
- Implementation plan approved by user

### Execute → Validate
- All tasks completed
- Deviations documented

### Validate → Complete
- Code review passed (no critical issues)
- Tests passed
- Security audit passed (no critical vulnerabilities)
- Documentation updated

## Slash Commands

| Command | Purpose |
|---------|---------|
| `/start` | Execute full workflow (research → execute → validate) |
| `/research` | Research codebase, ask questions, create approved plan |
| `/execute` | Execute implementation from approved plan |
| `/code-check` | Run code review, tests, security audit |

## Best Practices

1. **Context Preservation**: Use sub-agents for research to keep main context clean
2. **Documentation**: Store all findings in appropriate `docs/` directories
3. **User Confirmation**: Never skip questioning phase - always clarify requirements
4. **Plan Adherence**: Follow approved plans exactly, document any deviations
5. **Quality First**: Never skip validation phase
6. **Session Tracking**: Use session directories for complex features

## Project-Specific Information

### Tech Stack
- Language: [Define your language]
- Framework: [Define your framework]
- Database: [Define your database]
- Testing: [Define your test framework]

### Key Directories
- Source code: [Define source directory]
- Tests: [Define test directory]
- Configuration: [Define config directory]

### Build Commands
```bash
npm install        # Install dependencies
npm run dev        # Start development
npm run build      # Build for production
npm test           # Run tests
npm run lint       # Run linting
```

### Code Conventions
- See `docs/research/patterns-*.md` for detected patterns
- Follow existing codebase conventions
- Use git-workflow skill for commits and branches

### Environment Setup
[Environment-specific instructions]
