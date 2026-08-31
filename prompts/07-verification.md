# Phase 07: Verification & Testing

## Objective
Verify that the implementation meets all acceptance criteria and requirements. Run comprehensive tests and validation.

## Input
- Code in feature branch: `feature/<USER_STORY_ID>` (review-approved)
- `docs/artifacts/<USER_STORY_ID>/requirements.md` (read-only)
- `docs/artifacts/<USER_STORY_ID>/impl-plan.md` (read-only)

## Process

### 1. Verify Functional Requirements
For each functional requirement in requirements.md:
- Execute the feature or test that validates this requirement.
- Record the result (PASS / FAIL).
- If FAIL, document the exact failure and next steps.

### 2. Verify Non-Functional Requirements
- Performance testing (if applicable).
- Load testing or scalability validation.
- Security validation (no credentials leaked, proper encryption, etc.).
- Reliability testing (error recovery, failover, etc.).

### 3. Run Full Test Suite
- All unit tests pass.
- All integration tests pass.
- Any end-to-end tests pass.
- No regressions in existing tests.

### 4. Acceptance Criteria Validation
For each task in impl-plan.md:
- Verify all acceptance criteria are met.
- Document evidence (test results, screenshots, logs).

### 5. Verify Against User Story
- Feature delivers the value promised in the original User Story.
- All acceptance criteria from user-story.md are met.

## Output
Create `docs/artifacts/<USER_STORY_ID>/verification.md`:

```markdown
# Verification & Testing — <USER_STORY_ID>

**Verified:** <timestamp>
**Test Environment:** <e.g., local, staging, CI/CD>
**Branch:** `feature/<USER_STORY_ID>`

## Functional Requirements Verification

| Req ID | Requirement | Test/Evidence | Result | Notes |
|--------|-------------|---------------|--------|-------|
| FR-1 | <Requirement> | Test: `test_fr1()` | ✓ PASS | <Detail if any> |
| FR-2 | ... | ... | ✗ FAIL | <Failure reason> |

### Summary:
- Total requirements: X
- Passed: X
- Failed: 0 (or list failures)

## Non-Functional Requirements Verification

### Performance
- <Metric 1>: <Value> ✓ / ⚠ / ✗
- <Metric 2>: <Value>

### Security
- ✓ No hardcoded credentials found
- ✓ Input validation active
- ⚠ <Concern>
- ✗ <Issue>

### Reliability
- ✓ Error scenarios handled
- ✗ <Failure recovery not working>

## Test Results

### Unit Tests
- Total: X
- Passed: X
- Failed: 0 (or list failures)
- Coverage: XX%

### Integration Tests
- Total: X
- Passed: X
- Failed: 0

### End-to-End Tests
- Total: X
- Passed: X
- Failed: 0

### Regression Tests
- ✓ No regressions detected

## Acceptance Criteria Validation

### Task 1: <Task Name>
- AC1: <Criterion> ✓ PASS
- AC2: <Criterion> ✓ PASS
- AC3: <Criterion> ✗ FAIL — <Reason>

### Task 2: ...

## User Story Fulfillment

Does the feature deliver the value promised in SCRUM-XXXX?

- ✓ User goal is met
- ✓ All acceptance criteria from User Story are satisfied
- ⚠ <Caveat or limitation>
- ✗ <Unmet requirement>

## Issues Found

### Critical Issues (Blocker)
- <Issue 1: Must fix before release>
- <Issue 2>

### Major Issues (Should Fix)
- <Issue>

### Minor Issues (Nice to Fix)
- <Issue>

## Testing Environment & Configuration

- OS: <OS>
- Browser/Runtime: <Version>
- Database: <Type, version>
- External Dependencies: <List versions>

## Sign-Off

- **Verified By:** <Tester/Reviewer Name>
- **Ready for Release:** Yes / No
- **Caveats or Known Limitations:** <If any>

---

**Status:** Awaiting human approval.

If all requirements met: Proceed to Phase 08: PR.
If issues found: Return to implementation or file as known limitation.
```

## Approval Requirement
- **YES** — Verification results must be approved before Phase 08: PR (or deployment).

## Halt Conditions
**Stop and report if:**
- Test environment cannot be set up.
- Code fails to compile or run.
- Critical functional requirement fails.
- Security or reliability issue is found.

**Report:**
- Specific test/requirement that failed
- Error logs or evidence
- Severity (blocker vs. known limitation)
- Recommended action (fix, defer, escalate)

## Tester Responsibilities

The tester MUST:
- Execute all requirements.
- Validate all acceptance criteria.
- Test edge cases and error scenarios.
- Document results clearly.
- Not approve a release with critical issues.

The tester MUST NOT:
- Skip tests to speed up verification.
- Approve features that fail acceptance criteria.
- Assume functionality works without testing.

## Next Phase
Phase 08: Pull Request (awaiting human approval of verification)
