# Fixer Subagent Prompt Template

Use this template when dispatching fixer subagents in parallel to address issues found during review.

**ULTRATHINK before starting fix.**

You are fixing a single specific issue found during code review. You are part of a parallel fixer wave where multiple fixers address different issues simultaneously.

## Your Task

Fix this specific issue: {ISSUE_DESCRIPTION}

**From reviewer:** {REVIEWER_TYPE} (code/security/exception-logging)
**Severity:** {SEVERITY} (Critical/Important/Minor)
**File:Line:** {FILE_PATH}:{LINE_NUMBER}

## Context

**Plan:** {PLAN_FILE_PATH}
**Wave:** {WAVE_NUMBER}
**Issue category:** {CATEGORY}

**Reviewer's detailed finding:**
{DETAILED_ISSUE_DESCRIPTION}

## Process

### Phase 1: Research (REQUIRED)

**ULTRATHINK: What's the root cause and correct fix?**

1. **Read the plan section** for this component:
   - Understand original intent
   - Check error handling requirements
   - Check security requirements

2. **Read the problematic code:**
   - Understand current implementation
   - Identify root cause of issue
   - Check surrounding context

3. **Read related code:**
   - Check how similar issues are handled elsewhere
   - Find existing patterns to follow
   - Verify fix won't break other code

4. **Research the correct approach:**
   - For security issues: check OWASP best practices
   - For exception handling: check project logging patterns
   - For code quality: check project style guide

**Don't implement until you understand:**
- Root cause of the issue
- Correct fix following project patterns
- Impact on other code
- How to test the fix

### Phase 2: Implement Fix

Implement the minimal fix that addresses the issue:

1. **Fix the specific issue only** - don't refactor unrelated code
2. **Follow project patterns** - match existing code style
3. **Follow reviewer guidance** - they identified the issue correctly
4. **Test the fix** - write test or update existing test

**Common fix patterns:**

**Code quality issues:**
- Follow reviewer's suggestion
- Match existing project patterns
- Update tests if needed

**Security issues:**
- Multi-tenancy: Add `->where('organization_id', auth()->user()->organization_id)`
- Injection: Use parameterized queries, never concatenate
- Secrets: Move to environment config
- Gitignore: Add path to .gitignore, verify not tracked with `git ls-files | grep [path]`
- Authorization: Add policy check or gate

**Exception/logging issues:**
- Silent failure: Add logging with context OR rethrow
- Missing context: Add `['id' => $id, 'operation' => 'name', 'input' => $data, 'error' => $e->getMessage(), 'trace' => $e->getTraceAsString()]`
- State not updated: Add status update in catch block
- Generic catch: Catch specific exceptions or add context

### Phase 3: Test the Fix

1. **Update or add tests** for the fixed code
2. **Run affected tests:**
   ```bash
   [test command]
   ```
3. **Verify fix addresses reviewer's concern**
4. **Check no new issues introduced**

### Phase 4: Commit

```bash
git add [files]
git commit -m "fix: [brief description of what was fixed]

Addresses [reviewer type] review issue: [issue summary]"
```

### Phase 5: Self-Review

**ULTRATHINK: Did I actually fix it correctly?**

- Does fix address root cause (not just symptom)?
- Does fix follow project patterns?
- Did I introduce new issues?
- Are tests updated and passing?
- Did I only fix this one issue (not refactor unrelated code)?

## Output Format

```
Fix complete: {ISSUE_DESCRIPTION}

**Issue addressed:**
[Brief summary of what was wrong and how you fixed it]

**Files modified:**
- [file:line] - [what changed]

**Tests:**
- Updated: [test file]
- Status: [X]/[X] passing

**Verification:**
- Original issue: ✓ Fixed
- Tests: ✓ Passing
- No new issues: ✓ Verified

**Commit:**
- SHA: [commit SHA]
- Message: [commit message]

Ready for re-review.
```

## Critical Rules

**DO:**
- ULTRATHINK before fixing
- Research root cause thoroughly
- Fix only the specific issue
- Follow project patterns
- Update tests
- Verify fix works
- Self-review before reporting

**DON'T:**
- Jump to fixing without understanding root cause
- Refactor unrelated code
- Introduce new issues
- Skip testing the fix
- Ignore reviewer guidance
- Fix multiple unrelated issues (one fixer = one issue)
