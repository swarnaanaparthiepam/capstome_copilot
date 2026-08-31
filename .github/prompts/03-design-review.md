# Phase 03: Design Review

## Objective
Conduct a senior design review of the proposed architecture. Validate that the architecture aligns with requirements, identify gaps, assess risks, and recommend changes. Update architecture.md if needed based on accepted review recommendations.

## Input
- `docs/artifacts/<USER_STORY_ID>/requirements.md` (read-only, approved)
- `docs/artifacts/<USER_STORY_ID>/architecture.md` (read-only, approved)
- `docs/artifacts/<USER_STORY_ID>/user-story.md` (read-only, for context)

## Process

### 1. Validate Requirement Alignment
- For each functional requirement, verify that the architecture includes a component/mechanism to fulfill it.
- Flag any requirements not explicitly addressed.

### 2. Review Architecture Quality
- **Modularity:** Are components properly separated? Can they be tested/deployed independently?
- **Clarity:** Is the design understandable? Are interfaces clear?
- **Completeness:** Are all necessary components present (e.g., logging, error handling, security)?
- **Technology Choices:** Are they justified and appropriate?

### 3. Security Review
- Authentication and authorization mechanisms
- Data encryption (in transit, at rest)
- Input validation
- Injection attack prevention
- Token/credential management
- Compliance with security standards

### 4. Reliability Review
- Failure modes: What happens if each component fails?
- Recovery mechanisms: How does the system recover?
- Data consistency: What consistency guarantees are maintained?
- Backup and restore: Are critical data backed up?

### 5. Scalability Review
- Can the system handle increasing load?
- Are there bottlenecks?
- How are state and sessions managed?
- Is the data model scalable?

### 6. Maintainability Review
- Is the code organized for easy maintenance?
- Are there clear boundaries between components?
- Can new developers understand the design?
- Is testing straightforward?

### 7. Error Handling Review
- Are all error scenarios covered?
- Is error messaging clear?
- Are critical errors logged and monitored?

## Output
Create `docs/artifacts/<USER_STORY_ID>/design-review.md`:

```markdown
# Design Review — <USER_STORY_ID>

**Reviewed:** <timestamp>
**Reviewer Role:** Senior Design Reviewer

## Review Summary

[Overall assessment: Is the architecture sound? Are major concerns? Proceed with caveats?]

## Requirement Alignment

| Requirement ID | Requirement | Architecture Component | Status |
|----------------|-------------|------------------------|--------|
| FR-1 | <Requirement> | <Component> | ✓ Aligned / ⚠ Gap / ✗ Missing |
| NFR-1 | <Requirement> | <Component> | ... |

### Findings:
- <Gap or concern 1>
- <Gap or concern 2>

## Architecture Quality Review

### Modularity
- ✓ <Positive finding>
- ⚠ <Concern>

### Clarity
- ✓ <Positive finding>

### Completeness
- ⚠ <Missing component or mechanism>

## Security Review

- ✓ Authentication mechanism is clearly defined
- ⚠ <Concern: e.g., "Token expiration not specified">
- ✗ <Issue: e.g., "No encryption for sensitive data at rest">

## Reliability Review

- ✓ <Positive finding>
- ⚠ <Concern: e.g., "Single point of failure in cache layer">

## Scalability Review

- ✓ <Positive finding>
- ⚠ <Concern: e.g., "Database indexing strategy not discussed">

## Maintainability Review

- ✓ <Positive finding>
- ⚠ <Concern>

## Error Handling Review

- ✓ <Positive finding>
- ⚠ <Concern: e.g., "Error recovery for network failures not detailed">

---

## Recommended Changes

### Recommended Change 1: <Title>
- **Rationale:** <Why this change improves the architecture>
- **Impact:** <What changes as a result>
- **Effort:** Low / Medium / High
- **Action:** Update `architecture.md` component: <Component Name>

### Recommended Change 2: ...

## Critical Issues (Must Fix Before Implementation)

- <Issue 1: e.g., "Add rate limiting to API">
- <Issue 2>

## Caveats and Notes

- <Note for implementation team>
- <Known limitation or deferred decision>

---

**Status:** Awaiting human approval.

If approved as-is: Proceed to Phase 04: Implementation Planning.
If approved with changes: Update `architecture.md` per recommendations and re-submit for approval.
If rejected: Return to Phase 02 for redesign.
```

## Architecture.md Update Process

If the design review recommends changes:

1. **Human reviews the recommended changes.**
2. **If approved:** Update `architecture.md` with the changes per the recommendations.
3. **Re-request approval** (human or automated re-review).

**Do NOT proceed to Phase 04 unless the architecture is approved.**

## Approval Requirement
- **YES** — Design review must be approved before Phase 04: Implementation Planning.

## Halt Conditions
**Stop and report if:**
- Required artifacts are missing or not approved.
- Critical security or reliability gaps cannot be resolved by the review.
- Architecture fundamentally misaligns with requirements.

**Report:**
- Specific gap or issue
- Severity (critical, major, minor)
- Recommended recovery

## Reviewer Responsibilities

The design reviewer MUST:
- Review against the approved requirements.
- Identify all gaps and risks.
- Provide actionable recommendations.
- Distinguish between critical (must fix) and advisory (should consider) feedback.

The reviewer MUST NOT:
- Reject the architecture without clear justification.
- Request changes outside the scope of the User Story.
- Add new requirements.

## Next Phase
Phase 04: Implementation Planning (awaiting human approval)
