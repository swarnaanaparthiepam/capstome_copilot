# Agentic SDLC Framework for Capstone Project

## Overview

This framework implements a complete Agentic Software Development Lifecycle (SDLC) process using GitHub Copilot Agent Mode. It enables autonomous execution of each SDLC phase while maintaining human control at critical approval gates.

**Key Feature:** The framework uses dynamic User Story IDs from Jira, supports multiple User Stories in parallel, and requires NO hardcoding of specific issues.

## Framework Structure

```
.
├── .instructions.md                    # Shared instructions for all agents
├── ORCHESTRATOR.md                     # Orchestrator design and algorithm
├── STATUS_TEMPLATE.md                  # Template for User Story status tracking
├── prompts/
│   ├── 00-input.md                    # Phase 00: Input prompt
│   ├── 01-requirements.md             # Phase 01: Requirements prompt
│   ├── 02-architecture.md             # Phase 02: Architecture prompt
│   ├── 03-design-review.md            # Phase 03: Design Review prompt
│   ├── 04-planning.md                 # Phase 04: Planning prompt
│   ├── 05-implementation.md           # Phase 05: Implementation prompt
│   ├── 06-review.md                   # Phase 06: Code Review prompt
│   ├── 07-verification.md             # Phase 07: Verification prompt
│   └── 08-pr.md                       # Phase 08: PR prompt
├── agents/
│   ├── 00-input-agent.instructions.md
│   ├── 01-requirements-agent.instructions.md
│   ├── 02-architecture-agent.instructions.md
│   ├── 03-design-review-agent.instructions.md
│   ├── 04-planning-agent.instructions.md
│   ├── 05-implementation-agent.instructions.md
│   ├── 06-review-agent.instructions.md
│   ├── 07-verification-agent.instructions.md
│   └── 08-pr-agent.instructions.md
└── docs/artifacts/                     # (Created dynamically per User Story)
    └── <USER_STORY_ID>/               # One directory per User Story
        ├── user-story.md              # Phase 00 output
        ├── requirements.md            # Phase 01 output
        ├── architecture.md            # Phase 02 output
        ├── design-review.md           # Phase 03 output
        ├── impl-plan.md               # Phase 04 output
        ├── review.md                  # Phase 06 output
        ├── verification.md            # Phase 07 output
        └── status.md                  # Status tracker (all phases)
```

## How It Works

### 1. Start a New User Story

**Orchestrator Entry:**
```
ORCHESTRATOR input=<USER_STORY_ID> action=start
```

**What happens:**
1. Orchestrator queries Jira MCP for the User Story ID.
2. Invokes **Phase 00 Agent** (Input).
3. Phase 00 creates `docs/artifacts/<USER_STORY_ID>/user-story.md` and `status.md`.
4. Halts and requests human approval.

### 2. Human Approves Phase 00

Human reviews the User Story artifact and approves.

**Orchestrator Command:**
```
ORCHESTRATOR input=<USER_STORY_ID> action=approve phase=00
```

### 3. Proceed Through Phases

**Orchestrator Entry:**
```
ORCHESTRATOR input=<USER_STORY_ID> action=resume
```

**What happens:**
1. Orchestrator reads `status.md`.
2. Determines next phase (01: Requirements).
3. Verifies required artifacts exist (user-story.md).
4. Invokes **Phase 01 Agent**.
5. Phase 01 extracts requirements, asks clarification questions, creates `requirements.md`.
6. Halts and requests human approval.

**Cycle repeats:**
- Human approves → Orchestrator resumes → Next agent runs → Approval gate → Repeat

### 4. Implementation Phase (No Approval by Agent)

**Phase 05 (Implementation)** does NOT set an approval gate. The code is automatically reviewed by Phase 06 (Code Review).

**Phase 06 (Code Review)** sets an approval gate for code quality.

### 5. Merge to Main

After **Phase 07 (Verification)** approval:
1. **Phase 08 Agent** (PR) creates a pull request.
2. Monitors CI/CD.
3. Merges to main.
4. Updates `status.md` with completion.

## Phase Details

### Phase 00: Input
- **Agent:** `00-input-agent`
- **Input:** User Story ID (from Jira MCP)
- **Output:** `user-story.md`
- **Approval Required:** YES
- **Prompt:** `prompts/00-input.md`

### Phase 01: Requirements
- **Agent:** `01-requirements-agent`
- **Input:** `user-story.md` (approved)
- **Output:** `requirements.md`
- **Approval Required:** YES
- **Prompt:** `prompts/01-requirements.md`
- **Special:** Asks clarification questions ONE AT A TIME

### Phase 02: Architecture
- **Agent:** `02-architecture-agent`
- **Input:** `requirements.md` (approved)
- **Output:** `architecture.md`
- **Approval Required:** YES
- **Prompt:** `prompts/02-architecture.md`

### Phase 03: Design Review
- **Agent:** `03-design-review-agent`
- **Input:** `architecture.md` (approved)
- **Output:** `design-review.md` (may update `architecture.md`)
- **Approval Required:** YES
- **Prompt:** `prompts/03-design-review.md`

### Phase 04: Planning
- **Agent:** `04-planning-agent`
- **Input:** `requirements.md` (approved), `architecture.md` (approved)
- **Output:** `impl-plan.md`
- **Approval Required:** YES
- **Prompt:** `prompts/04-planning.md`

### Phase 05: Implementation
- **Agent:** `05-implementation-agent`
- **Input:** `impl-plan.md` (approved)
- **Output:** Code in `feature/<USER_STORY_ID>` branch
- **Approval Required:** NO (reviewed by Phase 06)
- **Prompt:** `prompts/05-implementation.md`

### Phase 06: Code Review
- **Agent:** `06-review-agent`
- **Input:** `feature/<USER_STORY_ID>` code
- **Output:** `review.md`
- **Approval Required:** YES
- **Prompt:** `prompts/06-review.md`

### Phase 07: Verification
- **Agent:** `07-verification-agent`
- **Input:** Reviewed code
- **Output:** `verification.md`
- **Approval Required:** YES
- **Prompt:** `prompts/07-verification.md`

### Phase 08: PR
- **Agent:** `08-pr-agent`
- **Input:** Verified code
- **Output:** Pull request, merged to main
- **Approval Required:** NO (maintainer approval via GitHub)
- **Prompt:** `prompts/08-pr.md`

## Key Principles

### 1. Human-in-the-Loop
Every phase (00-07) requires human approval before proceeding to the next phase.

### 2. No-Assumption Rule
Agents MUST NOT make assumptions about unclear requirements. They ask clarification questions ONE AT A TIME and wait for human responses.

### 3. Phase Isolation
Each agent reads ONLY the artifacts required by its phase. Agents do NOT modify artifacts from other phases (except Phase 03, which may update architecture.md per design review recommendations).

### 4. Safe Failure
If a phase cannot complete:
- Agent reports the specific issue and User Story ID.
- Updates `status.md` with a block reason.
- Halts and awaits human intervention.

### 5. No Hardcoding
User Story IDs are always dynamic (from Jira MCP or user input). No hardcoded Jira issues, URLs, or example content.

### 6. Traceability
Every decision is traced back to:
- User Story ID
- Source evidence (requirements, design, etc.)
- Phase where the decision was made

## Using the Framework

### Prerequisites
- Jira MCP configured and connected.
- Git/GitHub configured.
- Copilot Agent Mode available.

### Step-by-Step

1. **Start with a User Story ID:**
   ```
   ORCHESTRATOR input=SCRUM-31 action=start
   ```

2. **Human reviews and approves Phase 00:**
   ```
   ORCHESTRATOR input=SCRUM-31 action=approve phase=00
   ```

3. **Resume and proceed through phases:**
   ```
   ORCHESTRATOR input=SCRUM-31 action=resume
   ```

4. **At each approval gate, human reviews artifacts:**
   - Read the artifact at `docs/artifacts/<USER_STORY_ID>/<phase>.md`
   - Approve or request changes
   - Approve with:
     ```
     ORCHESTRATOR input=<USER_STORY_ID> action=approve phase=<PHASE_NUMBER>
     ```

5. **Query status at any time:**
   ```
   ORCHESTRATOR input=SCRUM-31 action=status
   ```

### Multiple User Stories

The framework supports unlimited concurrent User Stories:
- Each has its own directory: `docs/artifacts/<USER_STORY_ID>/`
- Each has its own status tracker.
- Agents are invoked per User Story ID.
- No interference between stories.

## Customization

### Adding Skills

If a phase requires reusable capability (e.g., code review analysis, requirements mining), create a skill:
- **Naming:** `skills/<skill-name>.md`
- **Content:** Reusable instructions for a specific capability.
- **Usage:** Reference the skill from the agent's prompt.

### Adjusting Phase Sequence

All phases MUST run in order (00 → 01 → ... → 08). Do NOT skip phases.

To defer a phase:
1. Document the deferral in `status.md` (e.g., "Phase 05 deferred until sprint 2").
2. Update the orchestrator state.
3. Resume from the correct phase when ready.

### Integrating Other Systems

Agents can integrate with other systems via:
- **Jira MCP:** Already connected (Phase 00 uses it).
- **GitHub API:** Phase 08 creates PRs.
- **CI/CD Pipelines:** Phase 08 monitors CI/CD checks.
- **Confluence:** Agents can document decisions in Confluence (optional).

## Troubleshooting

### Artifact Not Found
- **Cause:** Previous phase did not complete or artifacts were deleted.
- **Fix:** Check `status.md` to determine the current phase. Re-run the missing phase or restore the artifact.

### Approval Pending
- **Cause:** Human approval is required for a phase.
- **Fix:** Review the artifact, approve or request changes, then resume the orchestrator.

### Circular Dependencies
- **Cause:** Implementation plan has circular task dependencies.
- **Fix:** Phase 04 agent should detect this. Review impl-plan.md and identify the circular dependency. Request replanning.

### Code Merge Conflict
- **Cause:** Feature branch has conflicts with main.
- **Fix:** Phase 08 agent resolves conflicts via rebase. Resolve manually if needed and retry merge.

## Success Criteria

The framework is successfully implemented when:

✓ All agents can be invoked independently.
✓ Agents read only required artifacts.
✓ Agents produce their phase artifacts.
✓ Human approval gates work correctly.
✓ Orchestrator resumes from correct phase.
✓ Multiple User Stories can run in parallel.
✓ No hardcoded User Story IDs anywhere.
✓ Framework supports the "Automated Documentation Sync" capstone project.

## Next Steps

1. **Review the framework:** Read `.instructions.md`, `ORCHESTRATOR.md`, and each phase prompt.
2. **Test with a sample User Story:** Run Phase 00 with a real Jira issue (SCRUM-31 works well).
3. **Iterate phases:** Proceed through each phase, approving at each gate.
4. **Document results:** Track status, issues, and improvements.
5. **Implement the capstone:** Once the framework is proven, implement the "Automated Documentation Sync" application using this pipeline.

---

**Framework Version:** 1.0
**Last Updated:** 2026-08-31
**Status:** Ready for deployment
