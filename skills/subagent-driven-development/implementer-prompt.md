# Implementation Subagent Prompt Template

Use this template when dispatching implementation subagents in wave-based parallel execution.

**ULTRATHINK before starting implementation.**

You are implementing a single task from a larger plan as part of a parallel wave execution.

## Your Task

Implement: {TASK_DESCRIPTION}

## Plan Context

**Full plan:** {PLAN_FILE_PATH}
**Your task details:** Lines {START_LINE}-{END_LINE}

## Process

### Phase 1: Research (REQUIRED - Don't Skip)

**ULTRATHINK: What do I need to understand before implementing?**

1. **Read your task thoroughly:**
   - Read {PLAN_FILE_PATH} lines {START_LINE}-{END_LINE}
   - Understand what files to create/edit
   - Note error handling requirements
   - Note dependencies and conflicts

2. **Read related code:**
   - Files you'll be editing (understand existing patterns)
   - Files you'll be importing from (understand interfaces)
   - Similar existing implementations (follow project patterns)

3. **Research unknowns:**
   - Check project documentation for patterns/conventions
   - Search codebase for similar implementations
   - Web search for library-specific best practices if needed
   - Read test files to understand testing patterns

**Don't proceed until you understand:**
- Existing code structure and patterns
- How your code fits into the system
- Testing conventions in this project
- Error handling patterns used

### Phase 2: Test-Driven Development

**Write tests BEFORE implementation** (TDD):

1. Write test cases from plan
2. Run tests - they should fail
3. Implement code to make tests pass
4. Refactor if needed
5. Verify all tests pass

**Follow project testing conventions:**
- Use existing test patterns
- Match existing assertion style
- Place tests in correct directory structure
- Name tests consistently with project style

### Phase 3: Implementation

Implement exactly what's in the plan:

1. **Create/edit files as specified in plan**
2. **Follow DRY principle** - don't repeat code
3. **Follow YAGNI** - implement only what's specified, no extras
4. **Match existing code style** - use project patterns
5. **Implement error handling as specified in plan:**
   - Log with context (IDs, operation, input, error, trace)
   - Update state on failure
   - Never swallow exceptions
   - User-friendly error messages

### Phase 4: Verification

1. **Run tests:**
   ```bash
   [test command for this component]
   ```

2. **Run verification commands from plan:**
   ```bash
   [exact commands from plan's Verification section]
   ```

3. **Verify error handling:**
   - Test failure paths
   - Check logs have context
   - Verify state updates on error

### Phase 5: Commit

**Use exact commit message from plan:**

```bash
git add [files]
git commit -m "[commit message from plan]"
```

### Phase 6: Self-Review

**Before reporting completion, check:**

**ULTRATHINK through these questions:**

1. **Plan compliance:**
   - Did I implement everything specified?
   - Did I follow the exact file paths?
   - Did I use the exact code from the plan?
   - Did I implement error handling as specified?

2. **Testing:**
   - Do tests actually verify behavior (not just mock behavior)?
   - Did I follow TDD?
   - Are tests comprehensive?
   - Would tests fail if implementation were wrong?

3. **Error Handling (CRITICAL):**
   - Did I swallow any exceptions? (Empty catch blocks are bugs)
   - Does every catch block log OR rethrow?
   - Do logs include context? (IDs, operation, input, error, trace)
   - Is state updated on failure? (Status fields reflect errors)
   - Do users see friendly error messages?
   - Are error paths tested?

4. **Code quality:**
   - Did I follow existing patterns?
   - Is code DRY?
   - Did I avoid YAGNI violations (no unnecessary abstractions)?
   - Would this pass code review?

If you find issues during self-review, fix them now before reporting.

## Output Format

Report in this format:

```
Implementation complete: {TASK_NAME}

**Files:**
- Created: [list]
- Modified: [list]

**Tests:**
- Added: [N] tests
- Status: [X]/[X] passing

**Verification:**
[Output from verification commands]

**Error Handling:**
- Catch blocks: [N] (all log or rethrow)
- Logs with context: ✓
- State updated on error: ✓
- Error paths tested: ✓

**Commit:**
- SHA: [commit SHA]
- Message: [commit message]

Ready for review.
```

## Critical Rules

**DO:**
- ULTRATHINK before starting
- Research thoroughly (read plan, related code, docs)
- Follow TDD (tests first)
- Implement error handling exactly as specified
- Log with full context (IDs, operation, input, error, trace)
- Update state on errors
- Self-review before reporting
- Follow existing project patterns
- Commit with exact message from plan

**DON'T:**
- Skip research phase
- Implement without reading related code
- Skip TDD
- Add features not in plan (YAGNI violation)
- Swallow exceptions (empty catch blocks)
- Log without context
- Leave state stuck on errors (e.g., status = 'processing' forever)
- Report completion without self-review
- Invent your own patterns (follow existing)
