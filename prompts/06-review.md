# Phase 06: Code Review

## Objective
Conduct a peer/senior code review of the implemented feature. Verify that the code matches the approved plan, meets quality standards, and is ready for testing.

## Input
- Code in feature branch: `feature/<USER_STORY_ID>`
- `docs/artifacts/<USER_STORY_ID>/impl-plan.md` (read-only, to verify plan completion)
- `docs/artifacts/<USER_STORY_ID>/architecture.md` (read-only, for design reference)

## Process

### 1. Verify Plan Completion
- Cross-reference all tasks in impl-plan.md.
- Confirm all tasks are implemented.
- Verify files and components match the plan.

### 2. Code Quality Review
- **Readability:** Code is clear and well-commented.
- **Standards:** Follows project conventions and style.
- **Maintainability:** Code is organized for easy future changes.
- **DRY:** No unnecessary duplication.
- **Error Handling:** Errors are caught and handled gracefully.

### 3. Test Coverage Review
- Tests exist for all acceptance criteria.
- Test coverage is adequate (target: >80%).
- Tests are clear and maintainable.
- Edge cases are covered.

### 4. Architecture Alignment
- Code structure matches the approved architecture.
- Components have clear responsibilities.
- Interfaces and contracts are well-defined.

### 5. Security Review
- No hardcoded secrets or credentials.
- Input validation is present.
- Authentication/authorization checks are in place.
- No injection vulnerabilities.

### 6. Performance Review
- No obvious performance bottlenecks.
- Database queries are optimized (if applicable).
- N+1 query patterns avoided.

## Output
Create `docs/artifacts/<USER_STORY_ID>/review.md`:

```markdown
# Code Review — <USER_STORY_ID>

**Reviewed:** <timestamp>
**Reviewer Role:** Senior Code Reviewer
**Feature Branch:** `feature/<USER_STORY_ID>`

## Review Summary

[Overall assessment: Code is ready / Needs changes / Blocked]

## Plan Completion Verification

| Task ID | Task | Status | Notes |
|---------|------|--------|-------|
| 1 | Task Name | ✓ Complete | <Notes if any> |
| 2 | ... | ⚠ Partial | <What's missing> |
| 3 | ... | ✗ Missing | <Task not implemented> |

### Findings:
- <All tasks completed / Incomplete tasks listed>

## Code Quality Review

### Readability
- ✓ Code is well-commented
- ⚠ <Section needs clearer naming>
- <Positive finding>

### Standards & Style
- ✓ Follows project conventions
- ⚠ <Minor style inconsistency in file X>

### Maintainability
- ✓ Good separation of concerns
- ⚠ <Component Y has high coupling>

### Error Handling
- ✓ Network errors handled
- ⚠ <Database errors not caught in function Z>

## Test Coverage Review

- Total Coverage: <XX%>
- ✓ Acceptance criteria verified
- ⚠ <Edge case not covered: ...>
- Files with coverage:
  - `src/components/Component1.ts` — 85%
  - `src/utils/helper.ts` — 92%

## Architecture Alignment

- ✓ Code structure matches architecture.md
- ✓ Components have clear responsibilities
- ⚠ <Concern: Component A violates layer separation>

## Security Review

- ✓ No hardcoded credentials
- ✓ Input validation present
- ⚠ <Missing validation in endpoint X>
- ✓ Auth checks in place

## Performance Review

- ✓ No obvious bottlenecks
- ✓ Database queries use indexes
- ⚠ <Concern: Loop in function Y could be optimized>

---

## Requested Changes

### Change 1: <Title>
- **Reason:** <Why this change is needed>
- **Severity:** Critical / Major / Minor
- **Location:** `src/file.ts`, line X
- **Action:** <Specific change to make>

### Change 2: ...

## Critical Issues (Blocker)

- <Issue 1: Must fix before approval>
- <Issue 2>

## Advisory Notes

- <Style note or pattern suggestion>
- <Future optimization opportunity>

---

**Status:** Awaiting human approval.

If approved: Proceed to Phase 07: Verification.
If changes requested: Address changes, re-commit, request re-review.
If rejected: Return to implementation.
```

## Approval Requirement
- **YES** — Code review must be approved before Phase 07: Verification.

## Halt Conditions
**Stop and report if:**
- Feature branch is missing or empty.
- Approved plan tasks are not implemented.
- Critical security or quality issues are found.
- Tests fail or coverage is inadequate.

**Report:**
- Specific issue or gap
- Severity
- Files/commits affected
- Required action

## Reviewer Responsibilities

The reviewer MUST:
- Verify the plan was implemented completely.
- Apply consistent quality standards.
- Identify all security issues.
- Distinguish between critical (blocker) and advisory feedback.

The reviewer MUST NOT:
- Reject based on stylistic preferences alone.
- Request features beyond the approved plan.
- Approve code that fails tests or has critical issues.

## Next Phase
Phase 07: Verification (awaiting human approval of code review)
