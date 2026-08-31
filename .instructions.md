# Agentic SDLC Framework — Shared Instructions

## Core Principles

### 1. Human-in-the-Loop
- Every phase requires human approval before proceeding to the next phase.
- Agents MUST halt at approval gates and report the state clearly.
- Agents MUST NOT proceed past a pending approval without explicit consent.

### 2. No-Assumption Rule
- Agents MUST NOT make assumptions about unclear requirements.
- Agents MUST identify gaps, ask clarification questions ONE AT A TIME, and wait for human responses.
- Ambiguities MUST be recorded in the artifact with explicit flags.

### 3. Phase Isolation
- Each agent reads ONLY artifacts required by its phase.
- Agents MUST NOT access or modify artifacts from other phases unless explicitly required.
- Each agent produces a single, well-defined artifact for its phase.

### 4. Traceability
- Every decision in an artifact MUST trace back to:
  - The User Story ID
  - Source evidence (e.g., Jira content, requirements, design review notes)
  - Phase context where the decision was made
- User Story ID MUST appear in all artifacts and status tracking.

### 5. Safe Failure Behavior
- If an artifact cannot be found, agents MUST stop and report: artifact name, expected location, User Story ID.
- If state is ambiguous (e.g., conflicting artifacts), agents MUST stop and request human intervention.
- If an approval is pending, agents MUST skip their phase and halt.
- Agents MUST NEVER silently skip a phase or assume approval.

### 6. No Hardcoding
- User Story IDs MUST be dynamically obtained from Jira MCP or user input.
- Agents MUST NOT hardcode any Jira issue ID, User Story URL, title, or example content.
- Artifacts MUST NOT reference specific hardcoded Jira issues as examples.

### 7. Artifact Location and Naming
- All artifacts MUST be stored in: `docs/artifacts/<USER_STORY_ID>/`
- Artifact naming follows the phase name (e.g., `user-story.md`, `requirements.md`, `architecture.md`)
- Status MUST always be tracked in: `docs/artifacts/<USER_STORY_ID>/status.md`

### 8. Phase Ordering
- Phases MUST run in order: 00 → 01 → 02 → 03 → 04 → 05 → 06 → 07 → 08.
- Earlier phases MUST complete and receive approval before later phases can start.
- Approved phases MUST NOT be re-executed unless explicitly requested.

### 9. Context Minimization
- Agents MUST read only the minimum context required to complete their phase.
- Agents MUST NOT load entire codebases or all artifacts at once.
- Agents MUST NOT reproduce content from previous phases unnecessarily.

### 10. Security and Jira Policy
- Agents MUST NOT modify Jira issues.
- Agents MUST NOT delete artifacts from completed phases.
- Agents MUST treat approved artifacts as immutable.
- Only the orchestrator MUST update status.md.

## Orchestrator Responsibilities

The orchestrator MUST:

1. **Accept Dynamic Input**: Receive a User Story ID (from Jira MCP query result or user input).
2. **Locate Artifacts**: Identify `docs/artifacts/<USER_STORY_ID>/`.
3. **Read Status**: Load `status.md` to determine:
   - Current phase
   - Completed phases
   - Pending human approval (if any)
   - Blocked phase (if any)
   - Last updated timestamp
4. **Resume Correctly**: Determine which phase to run next:
   - If status.md exists and is valid, resume from the next unfinished phase.
   - If status.md is missing, inspect existing artifacts to infer state.
   - If state is ambiguous, stop and request human intervention.
5. **Skip Completed Phases**: Never re-execute a completed phase unless explicitly requested.
6. **Respect Approval Gates**: If human approval is pending, halt and display the approval gate.
7. **Read Only Required Artifacts**: Pass to each agent ONLY the artifacts it needs.
8. **Record Phase Completion**: After an agent completes, update status.md with:
   - Phase number and name
   - Completion timestamp
   - Approval status (pending)
9. **Recover Safely**: If interrupted, preserve all partial progress in artifacts and status.md.
10. **Fail Safely**: If any phase produces an error or blocks, update status.md and halt with a clear error message.

## Agent Responsibilities

Each agent MUST:

1. **Read Required Artifacts Only**: Accept ONLY the artifacts needed for its phase.
2. **Execute Phase Logic**: Implement the phase-specific behavior per its prompt.
3. **Produce Phase Artifact**: Create or update the required artifact (e.g., `requirements.md`).
4. **Report Status**: Clearly state:
   - Phase name and number
   - Artifact created/updated
   - Pending approval (if required)
   - Next expected phase
5. **Halt at Approval Gates**: Do NOT proceed to the next phase. Report "Awaiting human approval."
6. **Report Failures**: If blocked, provide:
   - Root cause
   - Affected artifact or phase
   - User Story ID
   - Recommended recovery action

## User Story Lifecycle

1. **Phase 00 (Input)**: Retrieve User Story from Jira MCP. Create `user-story.md`.
2. **Phase 01 (Requirements)**: Analyze requirements. Create `requirements.md`. Human approval.
3. **Phase 02 (Architecture)**: Design high-level architecture. Create `architecture.md`. Human approval.
4. **Phase 03 (Design Review)**: Review architecture against requirements. Create `design-review.md`. Update `architecture.md` if needed. Human approval.
5. **Phase 04 (Planning)**: Create implementation plan. Create `impl-plan.md`. Human approval.
6. **Phase 05 (Implementation)**: Implement features. Add/update tests.
7. **Phase 06 (Review)**: Peer review code. Create `review.md`. Human approval.
8. **Phase 07 (Verification)**: Run tests and verification. Create `verification.md`. Human approval.
9. **Phase 08 (PR)**: Create/update pull request. Await merge.

## Status Tracking

`status.md` template:

```
# User Story: <USER_STORY_ID>

**Title:** (from user-story.md)

**Current Phase:** <PHASE_NUMBER>: <PHASE_NAME>

**Completed Phases:**
- [ ] 00: Input
- [ ] 01: Requirements
- [ ] 02: Architecture
- [ ] 03: Design Review
- [ ] 04: Planning
- [ ] 05: Implementation
- [ ] 06: Review
- [ ] 07: Verification
- [ ] 08: PR

**Pending Human Approval:** <PHASE_NUMBER>: <PHASE_NAME> (or None)

**Blocked Phase:** None (or phase number and reason)

**Last Updated:** <TIMESTAMP>

**Notes:**
(Any relevant blockers, decisions, or context)
```

## Key Reminders

- **Do NOT hardcode Jira issues.** Every User Story ID must come from external input.
- **Do NOT assume approval.** Always halt at approval gates.
- **Do NOT modify Jira.** Only read.
- **Do NOT skip phases silently.** Always report phase completion.
- **Do NOT merge artifacts.** Each phase produces its own artifact.
- **Communicate clearly.** Report status, approval requirements, and blockers explicitly.
