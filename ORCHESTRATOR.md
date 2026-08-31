# Orchestrator Configuration

The Orchestrator is the central coordinator for the Agentic SDLC pipeline. It is NOT an agent itself, but rather a configuration and decision-making guide for running agents in sequence.

## Orchestrator Entry Point

**Input:** User Story ID (obtained from Jira MCP query, user input, or environment variable)

**Location:** `docs/artifacts/<USER_STORY_ID>/`

## Orchestrator Algorithm

### 1. Initialize

```
USER_STORY_ID = <input from user or Jira MCP>
ARTIFACT_DIR = docs/artifacts/{USER_STORY_ID}
STATUS_FILE = {ARTIFACT_DIR}/status.md
```

### 2. Determine Current State

**If `status.md` exists:**
- Parse the file.
- Read `Current Phase`.
- Read `Pending Human Approval`.
- Read `Blocked Phase`.
- Validate artifact consistency.

**If `status.md` does NOT exist:**
- Scan `ARTIFACT_DIR` for existing artifacts.
- Infer the state from the highest-numbered artifact present.
  - If `user-story.md` only → phase 00 complete, phase 01 next
  - If `requirements.md` → phase 01 complete, phase 02 next
  - If `architecture.md` → phase 02 complete, phase 03 next
  - ... etc.
- If no artifacts exist, start at phase 00.
- Create `status.md` with inferred state.

**If state is ambiguous:**
- Stop and report: "Artifact state ambiguous. Manual intervention required."
- List conflicting/missing artifacts.
- Halt orchestration.

### 3. Check Approval Gates

**If `Pending Human Approval` is set:**
- Display the approval gate.
- Show the artifact requiring approval.
- Do NOT proceed to the next phase.
- Halt and wait for human approval action.

**If `Blocked Phase` is set:**
- Display the block reason.
- Do NOT proceed.
- Halt and await manual recovery.

### 4. Determine Next Phase

**Current Phase Mapping:**
- 00: Input → Next: 01 (Requirements)
- 01: Requirements → Next: 02 (Architecture)
- 02: Architecture → Next: 03 (Design Review)
- 03: Design Review → Next: 04 (Planning)
- 04: Planning → Next: 05 (Implementation)
- 05: Implementation → Next: 06 (Review)
- 06: Review → Next: 07 (Verification)
- 07: Verification → Next: 08 (PR)
- 08: PR → **Complete**

### 5. Validate Required Artifacts

Before running the next agent, verify that all artifacts required by that phase are present:

**Phase 01 requires:** `user-story.md`
**Phase 02 requires:** `requirements.md` (approved)
**Phase 03 requires:** `requirements.md` (approved), `architecture.md`
**Phase 04 requires:** `requirements.md` (approved), `architecture.md` (approved)
**Phase 05 requires:** `requirements.md` (approved), `architecture.md` (approved), `impl-plan.md` (approved)
**Phase 06 requires:** Implementation artifacts (generated code, tests)
**Phase 07 requires:** Implementation artifacts + `review.md` (approved)
**Phase 08 requires:** All approved artifacts

If required artifacts are missing, halt and report.

### 6. Run the Agent

Invoke the appropriate agent for the next phase:

```
INVOKE AGENT <PHASE_NUMBER>-<PHASE_NAME>-agent
WITH:
  - USER_STORY_ID
  - Required artifacts (read-only)
  - Artifact directory path
  - Phase prompt (if applicable)
```

### 7. Receive Agent Output

The agent will return:
- **Success:** Artifact created/updated, approval required (YES/NO)
- **Error:** Block reason, state, recovery action
- **Status:** Phase name, User Story ID, next phase

### 8. Update Status

Update `status.md`:

```
Current Phase: <NEXT_PHASE_NUMBER>: <NEXT_PHASE_NAME>
Completed Phases: Mark current phase as complete [X]
Pending Human Approval: <PHASE_NUMBER>: <PHASE_NAME> (if required)
Blocked Phase: None (or phase number and reason if error)
Last Updated: <TIMESTAMP>
```

### 9. Halt or Resume

**If approval required:**
- Halt orchestration.
- Display approval gate to user.
- Wait for explicit human approval.
- On approval, resume from step 3 (re-check approval gates).

**If no approval required:**
- Return to step 3.
- Re-check approval gates.
- Proceed to next phase if no gates are set.

**If error/blocked:**
- Halt orchestration.
- Display error message and recovery options.
- Await human intervention.

## Artifact Dependencies

```
Phase 00 (Input) → user-story.md
                 ↓ (approval)
Phase 01 (Requirements) → requirements.md
                        ↓ (approval)
Phase 02 (Architecture) → architecture.md
                        ↓ (approval)
Phase 03 (Design Review) → design-review.md (may update architecture.md)
                         ↓ (approval)
Phase 04 (Planning) → impl-plan.md
                    ↓ (approval)
Phase 05 (Implementation) → code, tests
                          ↓ (no approval by agent)
Phase 06 (Review) → review.md
                  ↓ (approval)
Phase 07 (Verification) → verification.md
                        ↓ (approval)
Phase 08 (PR) → pull request
```

## Recovery Scenarios

### Scenario A: Interrupted Mid-Phase
- `status.md` shows phase in progress.
- Agent was killed/timed out.
- On resume: Orchestrator detects partial state, requests human decision:
  - Retry the phase? (re-invoke agent)
  - Skip to next? (mark as complete, move on)
  - Rollback? (delete artifact, restart phase)

### Scenario B: Approval Not Yet Provided
- `Pending Human Approval: 02: Architecture`
- Human has not yet reviewed/approved `architecture.md`.
- On resume: Orchestrator halts at approval gate.
- Displays: "Awaiting approval for Phase 02: Architecture. Review docs/artifacts/<USER_STORY_ID>/architecture.md"

### Scenario C: Artifact Missing
- Phase 05 requires `impl-plan.md`.
- `impl-plan.md` is missing or deleted.
- Orchestrator halts and reports: "Required artifact missing: impl-plan.md"
- Recommends: Restart from phase 04 or manually restore artifact.

### Scenario D: Ambiguous State
- `status.md` says phase 02, but phase 04 artifact exists.
- No clear progression path.
- Orchestrator halts: "Artifact state is ambiguous. Manual review required."
- Lists all artifacts with their phases.

## Orchestrator Entry Commands

### Start a New User Story
```
ORCHESTRATOR input=<USER_STORY_ID> action=start
```
- Queries Jira MCP for User Story ID.
- Invokes Phase 00 agent.
- Initializes artifacts directory and status.md.

### Resume a User Story
```
ORCHESTRATOR input=<USER_STORY_ID> action=resume
```
- Reads existing status.md and artifacts.
- Determines next phase.
- Checks approval gates.
- Resumes from the correct phase.

### Approve a Pending Phase
```
ORCHESTRATOR input=<USER_STORY_ID> action=approve phase=<PHASE_NUMBER>
```
- Validates that phase has completed.
- Updates status.md: `Pending Human Approval: None`
- Resumes orchestration.

### Query Status
```
ORCHESTRATOR input=<USER_STORY_ID> action=status
```
- Displays current phase, completed phases, pending approvals, blocks.

---

**Note:** The Orchestrator is a decision engine and state tracker. It does NOT implement agent logic. Each agent is independent and invoked by the Orchestrator with only the artifacts and context it requires.
