# Phase 05: Implementation

## Objective
Implement the approved implementation plan. Write code, create tests, and commit to the repository.

## Input
- `docs/artifacts/<USER_STORY_ID>/requirements.md` (read-only, approved)
- `docs/artifacts/<USER_STORY_ID>/architecture.md` (read-only, approved)
- `docs/artifacts/<USER_STORY_ID>/impl-plan.md` (read-only, approved)

## Process

### 1. Set Up Development Environment
- Create feature branch: `feature/<USER_STORY_ID>`
- Ensure all dependencies and build tools are available.

### 2. Implement Tasks in Order
- Follow the priority and dependency order from impl-plan.md.
- Implement only tasks in the approved plan.
- Create/modify only the files specified in the plan.

### 3. Write Tests
- Unit tests for each component.
- Integration tests for component interactions.
- Tests must verify acceptance criteria from impl-plan.md.
- Target: >80% code coverage.

### 4. Code Quality
- Follow project conventions and style guides.
- Write clear, maintainable code.
- Add comments for complex logic.
- Handle errors gracefully.

### 5. Commit Regularly
- Commit after each task or logical chunk.
- Use descriptive commit messages: `TASK-<task_id>: <description>`
- Example: `TASK-1: Implement data model for user stories`

### 6. Update Status
- Record completion of each task.
- Flag any blockers or deviations from the plan.

## Output
- **Code:** Implemented and committed to `feature/<USER_STORY_ID>` branch.
- **Tests:** All tests passing locally.
- **Commits:** Clear commit history in the feature branch.

No approval required by the agent. The next phase (Phase 06: Review) will review the code.

## Halt Conditions
**Stop and report if:**
- An approved plan task cannot be implemented as specified.
- A hidden requirement emerges (not in requirements.md).
- Code cannot meet acceptance criteria.
- Build fails or tests fail with unclear cause.

**Report:**
- Task ID and description
- Specific blocker or issue
- Code commit(s) relevant to the issue
- Recommendation (fix, adapt plan, escalate)

## Implementation Guidelines

### DO:
- Implement exactly what the plan specifies.
- Write tests for acceptance criteria.
- Commit frequently with clear messages.
- Add error handling and logging.
- Follow existing project patterns.

### DO NOT:
- Add features not in the approved plan.
- Skip tasks.
- Commit without clear messages.
- Implement without tests.
- Modify code from other User Stories.

## Success Criteria
- All tasks in the plan are implemented.
- All tests pass.
- Code is committed to `feature/<USER_STORY_ID>`.
- No blocker issues remain unresolved.

## Next Phase
Phase 06: Code Review (no approval step, awaiting review)
