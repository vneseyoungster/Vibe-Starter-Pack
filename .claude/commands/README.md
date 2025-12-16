# Slash Commands

This directory contains custom slash commands for the development workflow.

## Available Commands

| Command | Purpose |
|---------|---------|
| `/start` | Execute full workflow (research → execute → validate) |
| `/research` | Research codebase, ask questions, create approved plan |
| `/execute` | Execute implementation from approved plan |
| `/code-check` | Run code review, tests, security audit |
| `/quick-fix` | Quick fix for known problems (no planning phase) |

## Usage

Slash commands are invoked by typing `/command-name` in Claude Code.

---

### `/start [feature description]`

**Recommended for most tasks.** Executes the complete workflow automatically:

1. **Research & Plan** - Analyzes codebase, asks questions, creates plan
2. **Execute** - Implements tasks with appropriate developers
3. **Validate** - Runs all quality checks

**User Interaction Points:**
- After questions generated (answers needed)
- After architecture created (approval needed)
- After plan created (approval needed)
- After validation (acknowledge results)

**Example:**
```
/start Add user authentication with JWT tokens
```

**Session Artifacts Created:**
```
docs/sessions/{date}-{slug}/
├── session.md        # Session tracking
├── research/         # Research findings
├── specs/            # Requirements
├── plans/            # Architecture & tasks
├── reviews/          # Validation reports
└── summary.md        # Final summary
```

---

### `/research [task description]`

Combines research, questioning, and planning into one phase:

1. Creates session directory structure
2. Invokes appropriate research sub-agents
3. Generates clarifying questions (prioritized)
4. Presents questions to user and records answers
5. Creates architecture design (requires approval)
6. Creates implementation plan (requires approval)

**Example:**
```
/research Add user authentication
```

**Output:**
- `docs/sessions/{session}/research/` - Research findings
- `docs/sessions/{session}/specs/requirements.md` - Requirements
- `docs/sessions/{session}/plans/architecture.md` - Architecture
- `docs/sessions/{session}/plans/implementation.md` - Task breakdown

**Quality Gates:**
- All blocking questions answered
- Architecture approval required
- Implementation plan approval required

---

### `/execute [task description]`

Executes the implementation phase:

1. Loads implementation plan from `/research` phase
2. Routes tasks to appropriate developer sub-agents
3. Executes tasks in order with verification
4. Commits after each task completion
5. Tracks progress in session file
6. Handles deviations appropriately

**Prerequisites:**
- `/research` phase completed
- Architecture and implementation plan approved

**Example:**
```
/execute Add user authentication
```

**Sub-Agent Routing:**
| Task Type | Sub-Agent |
|-----------|-----------|
| API/Backend | `backend-developer` |
| UI/Frontend | `frontend-developer` |
| Database | `database-specialist` |

**Quality Gates:**
- All tasks must complete before validation
- All verifications must pass

---

### `/code-check [task description]`

Executes the validation phase:

1. Invokes code-reviewer sub-agent for code review
2. Checks for critical issues (stops if found)
3. Invokes test-automator sub-agent for test execution
4. Checks test results (stops if failing)
5. Invokes security-auditor sub-agent for security audit
6. Checks for vulnerabilities (stops if critical)
7. Invokes documentation-writer sub-agent for doc updates
8. Generates final validation report
9. Determines merge readiness

**Prerequisites:**
- `/execute` phase completed
- All tasks done, no pending deviations

**Example:**
```
/code-check Add user authentication
```

**Sub-Agent Invocation:**
| Check | Sub-Agent |
|-------|-----------|
| Code Review | `code-reviewer` |
| Test Execution | `test-automator` |
| Security Audit | `security-auditor` |
| Documentation | `documentation-writer` |

**Output:**
- `docs/sessions/{session}/reviews/code-review.md`
- `docs/sessions/{session}/reviews/test-report.md`
- `docs/sessions/{session}/reviews/security-audit.md`
- `docs/sessions/{session}/reviews/documentation.md`
- `docs/sessions/{session}/reviews/final-validation.md`

**Final Status:**
- READY FOR MERGE - All validations pass
- READY WITH NOTES - Warnings only
- NEEDS REMEDIATION - Failures found

---

### `/quick-fix [problem description]`

Streamlined fix workflow for known problems. Skips planning phase entirely -
main agent gathers minimal context and implements the fix directly.

**Best for:**
- Bug fixes where cause is already identified
- Typo/config corrections
- Simple refactors with clear scope
- Small code changes with known solutions

**Workflow:**
1. **Context Gathering** - Quick scan for relevant files and patterns (only if needed)
2. **Direct Fix** - Main agent implements fix directly (no sub-agents)
3. **Validation** - Self-review + run tests + auto-commit

**Example:**
```
/quick-fix Fix the null pointer exception in UserService.getProfile()
```

**Key Differences from Full Workflow:**
| Aspect | Full RQPIV | Quick Fix |
|--------|-----------|-----------|
| Research | Full analysis | Minimal context |
| Questions | Comprehensive | Only if unclear |
| Planning | Full architecture | None |
| Implementation | Sub-agents | Main agent |
| Validation | Full suite | Self-review + tests |
| Session | Creates docs | No session |

**Sub-Agent Usage (Optional):**
| Agent | When Used |
|-------|-----------|
| `codebase-explorer` | Only if file locations unclear |
| `pattern-researcher` | Only if coding patterns unclear |

**Complexity Warning:**
If fix touches 3+ files, a warning is shown but user decides whether to proceed
or switch to `/research` + `/execute` workflow.

**Auto-Commit:**
Changes are automatically committed if tests pass and self-review is clean.

---

## Command File Format

Each command is a Markdown file named after the command (without `/`):

```markdown
# Command Title

Description of what the command does: $ARGUMENTS

## Steps
1. Step one
2. Step two
...
```

The `$ARGUMENTS` placeholder is replaced with any text following the command.

## Adding New Commands

1. Create a new `.md` file named after the command (e.g., `my-command.md`)
2. The command will be available as `/my-command`
3. Use `$ARGUMENTS` to capture user input
4. Document the command in this README
