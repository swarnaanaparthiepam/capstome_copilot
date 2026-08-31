# Phase 04: Implementation Planning

## Objective
Create a detailed, prioritized, and dependency-ordered implementation plan. Break down the approved architecture into actionable tasks with clear priorities and dependencies.

## Input
- `docs/artifacts/<USER_STORY_ID>/requirements.md` (read-only, approved)
- `docs/artifacts/<USER_STORY_ID>/architecture.md` (read-only, approved)
- `docs/artifacts/<USER_STORY_ID>/design-review.md` (read-only, for reference on critical changes)

## Process

### 1. Identify Implementation Tasks
For each component in the approved architecture:
- Break down into logical implementation tasks.
- Define what "done" means for each task (acceptance criteria).

### 2. Identify Dependencies
- Which tasks must complete before others can start?
- Are there any external dependencies?
- What is the critical path?

### 3. Estimate Effort
- Assign rough effort estimates (S, M, L, XL) to each task.
- Note tasks with high uncertainty.

### 4. Prioritize Tasks
- **Priority 1 (Critical):** Must complete before any feature testing.
- **Priority 2 (High):** Required for the feature to be functional.
- **Priority 3 (Medium):** Enhances functionality or performance.
- **Priority 4 (Low):** Nice-to-have, can be deferred.

### 5. Order Tasks
- Respect dependencies.
- Start with foundational tasks (e.g., data models, core components).
- Group related tasks.
- Create a logical sequence for parallel work where possible.

### 6. Identify Component/File Impact
- Which files or components will this task touch?
- Are there any shared components that could conflict?

## Output
Create `docs/artifacts/<USER_STORY_ID>/impl-plan.md`:

```markdown
# Implementation Plan — <USER_STORY_ID>

**Based on:** 
- docs/artifacts/<USER_STORY_ID>/architecture.md
- docs/artifacts/<USER_STORY_ID>/requirements.md

**Planned:** <timestamp>

## Task Breakdown

### Task 1: <Task Title>
- **Component:** <Architecture component this task implements>
- **Priority:** 1 (Critical) / 2 (High) / 3 (Medium) / 4 (Low)
- **Effort:** S / M / L / XL
- **Dependencies:** <Task IDs this depends on, or "None">
- **Blocked:** Yes / No
- **Description:** <Clear description of work>
- **Acceptance Criteria:**
  - AC1: <Verification criterion>
  - AC2: <Verification criterion>
- **Files/Components to Create/Modify:**
  - `src/components/ComponentName.ts`
  - `src/utils/helperFunction.ts`
  - `tests/ComponentName.test.ts`

### Task 2: ...

## Dependency Graph

[ASCII representation or clear textual description of task dependencies]

Example:
```
Task 1 (Data Model) → Task 2 (Repository) → Task 3 (API)
Task 1 (Data Model) → Task 4 (UI Component) → Task 5 (Integration Test)
Task 3 (API) → Task 5 (Integration Test)
```

## Critical Path

Tasks on the critical path (no slack, any delay delays the whole feature):
1. Task 1: Data Model
2. Task 2: Repository
3. Task 3: API
4. Task 5: Integration Test

**Estimated Total Duration:** <Rough estimate, e.g., "4 sprints", "2 weeks">

## Task Summary Table

| Task ID | Task | Component | Priority | Effort | Dependencies | Files |
|---------|------|-----------|----------|--------|--------------|-------|
| 1 | Data Model | Backend | 1 | M | None | src/models.ts |
| 2 | Repository | Backend | 1 | M | 1 | src/repository.ts |
| 3 | API | Backend | 1 | L | 2 | src/api.ts |
| ... | ... | ... | ... | ... | ... | ... |

## Implementation Strategy

### Phase A: Foundational
- Tasks: 1, 2
- Rationale: These are prerequisites for everything else.

### Phase B: Core Features
- Tasks: 3, 4, 6
- Rationale: Implement the main functionality.

### Phase C: Integration & Testing
- Tasks: 5, 7, 8
- Rationale: Verify that components work together.

### Phase D: Polish & Optimization
- Tasks: 9, 10
- Rationale: Performance, error handling, UX refinements.

## Risks and Mitigations

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|-----------|
| <Risk 1> | High/Medium/Low | High/Medium/Low | <Mitigation> |
| <Risk 2> | ... | ... | ... |

## Out-of-Scope Tasks

- <Task that was considered but deferred>
- <Task deferred to a future User Story>

## Notes for Implementation Team

- <Any special considerations>
- <Known gotchas or patterns to follow>
- <Integration points or data contracts>

---

**Status:** Ready for human approval.
```

## Approval Requirement
- **YES** — Implementation plan must be approved before Phase 05: Implementation begins.

## Halt Conditions
**Stop and report if:**
- Architecture or requirements artifacts are missing or not approved.
- Cannot identify tasks that fulfill all approved requirements.
- Critical dependencies are circular or unresolvable.

**Report:**
- Missing approval or artifact
- Blocked task or circular dependency
- Requirement gap (task for which no architectural component exists)

## Planner Responsibilities

The planner MUST:
- Trace every requirement to at least one task.
- Identify all dependencies accurately.
- Create a feasible sequence.
- Flag any risks.

The planner MUST NOT:
- Add tasks that exceed the approved architecture.
- Create tasks for requirements not in requirements.md.
- Skip tasks required for the architecture.

## Next Phase
Phase 05: Implementation (awaiting human approval)
