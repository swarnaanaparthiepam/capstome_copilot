# Framework Design Summary

## Overview

The **Agentic SDLC Framework** is a complete, autonomous software development lifecycle pipeline designed for the capstone project on "Automated Documentation Sync."

**Purpose:** Enable end-to-end execution of a User Story from Jira through to production merge, with human approval at critical gates, without hardcoding any specific Jira issues.

## Core Design Principles

### 1. Autonomous Phases
Each SDLC phase (00-08) is a **separate, independent agent** with:
- One clear responsibility
- Minimal required context (reads only what it needs)
- A single output artifact (or code)
- Clear success/failure criteria

### 2. Human-in-the-Loop
- Every phase (00-07) requires explicit human approval before the next phase starts.
- Agents halt at approval gates and never proceed without confirmation.
- Humans have full visibility into all artifacts and decisions.

### 3. Dynamic User Stories
- User Story IDs come from **external input** (Jira MCP or user command).
- No hardcoded Jira issues, URLs, or example content anywhere.
- Framework supports unlimited concurrent User Stories.
- Each User Story is isolated in its own artifact directory.

### 4. Safe Failure & Recovery
- If a phase fails, it reports the failure and halts.
- State is preserved in `status.md` for recovery.
- Orchestrator can resume from the exact point of interruption.
- No data loss or silent skipping of phases.

### 5. Traceability
- Every decision traces back to User Story ID, source evidence, and phase.
- Artifacts are immutable (approved artifacts cannot be changed without explicit re-review).
- Status tracker maintains complete history.

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                       ORCHESTRATOR                          │
│                  (State & Coordination)                     │
│                 .github/workflows/orchestrator.md           │
└──────────────┬──────────────────────────────────────────────┘
               │
    ┌──────────┴──────────┬──────────────┬───────────┬──────┐
    │                     │              │           │      │
    ▼                     ▼              ▼           ▼      ▼
┌──────────────────────────────────────────────────────────────┐
│ .github/copilot/                                             │
│ ├── 00-input-agent.instructions.md                           │
│ ├── 01-requirements-agent.instructions.md                    │
│ ├── 02-architecture-agent.instructions.md                    │
│ ├── 03-design-review-agent.instructions.md                   │
│ └── ... (8 agents total)                                     │
└──────────────┬───────────────────────────────────────────────┘
               │
    ┌──────────┴──────────┬──────────────┬───────────┬──────┐
    │                     │              │           │      │
    ▼                     ▼              ▼           ▼      ▼
┌──────────────────────────────────────────────────────────────┐
│ .github/prompts/                                             │
│ ├── 00-input.md                                              │
│ ├── 01-requirements.md                                       │
│ ├── 02-architecture.md                                       │
│ └── ... (9 prompts)                                          │
└──────────────────────────────────────────────────────────────┘
                             │
                             ▼
              ┌──────────────────────────────┐
              │  docs/artifacts/<ID>/        │
              │  ├── user-story.md           │
              │  ├── requirements.md         │
              │  ├── architecture.md         │
              │  ├── design-review.md        │
              │  ├── impl-plan.md            │
              │  ├── review.md               │
              │  ├── verification.md         │
              │  └── status.md               │
              │  (Central Status Tracker)    │
              └──────────────────────────────┘
```

## Phase Flow

```
Phase 00: Input
    ↓ (retrieve from Jira)
    ↓ (create user-story.md)
    ↓ [APPROVAL GATE]
    ↓
Phase 01: Requirements
    ↓ (extract & clarify)
    ↓ (create requirements.md)
    ↓ [APPROVAL GATE]
    ↓
Phase 02: Architecture
    ↓ (design solution)
    ↓ (create architecture.md)
    ↓ [APPROVAL GATE]
    ↓
Phase 03: Design Review
    ↓ (review architecture)
    ↓ (create design-review.md)
    ↓ [APPROVAL GATE]
    ↓
Phase 04: Planning
    ↓ (create implementation plan)
    ↓ (create impl-plan.md)
    ↓ [APPROVAL GATE]
    ↓
Phase 05: Implementation
    ↓ (write code, tests)
    ↓ (commit to feature branch)
    ↓ (no approval by agent)
    ↓
Phase 06: Code Review
    ↓ (review code quality)
    ↓ (create review.md)
    ↓ [APPROVAL GATE]
    ↓
Phase 07: Verification
    ↓ (run tests, verify requirements)
    ↓ (create verification.md)
    ↓ [APPROVAL GATE]
    ↓
Phase 08: PR & Merge
    ↓ (create PR, merge to main)
    ↓ (no approval by agent, maintainer approves on GitHub)
    ↓
[COMPLETE]
```

## Artifact Storage

**Location:** `docs/artifacts/<USER_STORY_ID>/`

**Structure:**
```
docs/artifacts/
├── SCRUM-31/                    # User Story 1
│   ├── user-story.md           # Phase 00 output
│   ├── requirements.md          # Phase 01 output
│   ├── architecture.md          # Phase 02 output
│   ├── design-review.md         # Phase 03 output
│   ├── impl-plan.md             # Phase 04 output
│   ├── review.md                # Phase 06 output
│   ├── verification.md          # Phase 07 output
│   └── status.md                # Shared status tracker
├── SCRUM-32/                    # User Story 2
│   ├── user-story.md
│   ├── requirements.md
│   ├── ...
│   └── status.md
└── SCRUM-N/                     # User Story N
    ├── user-story.md
    ├── ...
```

**No Code:**
- Implementation code lives in `feature/<USER_STORY_ID>` branches on Git.
- Artifacts are documentation only.
- Status tracker is the single source of truth.

## Key Files

| File | Purpose |
|------|---------|
| `.github/copilot/instructions.md` | Shared rules, principles, and conventions for all agents |
| `.github/workflows/orchestrator.md` | Orchestrator algorithm, state management, recovery |
| `docs/README.md` | Complete framework documentation |
| `docs/QUICKSTART.md` | Quick start guide and common commands |
| `docs/STATUS_TEMPLATE.md` | Template for User Story status trackers |
| `.github/prompts/<phase>.md` | Phase-specific prompt (9 files, one per phase) |
| `.github/copilot/<phase>-agent.instructions.md` | Agent-specific instructions (9 files) |

## Orchestrator Responsibilities

The Orchestrator is NOT an agent, but a state machine that:

1. **Accepts input:** User Story ID (dynamic, from Jira or user)
2. **Reads state:** Loads `status.md` to determine current phase
3. **Validates artifacts:** Ensures required artifacts exist before invoking next agent
4. **Invokes agents:** Calls the appropriate agent with read-only access to required artifacts
5. **Respects gates:** Halts at approval gates and never proceeds without human confirmation
6. **Updates state:** Records phase completion and approval status in `status.md`
7. **Recovers safely:** If interrupted, preserves partial progress and resumes from correct point
8. **Handles errors:** Detects and reports failures, marks phases as blocked

## Agent Responsibilities

Each agent:

1. **Reads required artifacts:** Only the inputs needed for its phase
2. **Executes phase logic:** Implements the phase-specific behavior from its prompt
3. **Produces phase artifact:** Creates the required output (e.g., requirements.md)
4. **Reports status:** Clearly states completion, approval requirement, and next phase
5. **Halts at gates:** Does NOT proceed to the next phase; reports "Awaiting approval"
6. **Fails safely:** Stops and reports failures, including User Story ID and root cause

## Multi-User Story Support

The framework handles unlimited concurrent User Stories:

```
ORCHESTRATOR input=SCRUM-31 action=start     → Phase 00 for SCRUM-31
ORCHESTRATOR input=SCRUM-32 action=start     → Phase 00 for SCRUM-32
ORCHESTRATOR input=SCRUM-31 action=resume    → Phase 01 for SCRUM-31
ORCHESTRATOR input=SCRUM-32 action=resume    → Phase 02 for SCRUM-32
```

Each User Story:
- Has its own artifact directory
- Has its own status tracker
- Has its own feature branch (if code is involved)
- Cannot interfere with others

## No Hardcoding

**User Story IDs are dynamic:**
- Input via `ORCHESTRATOR input=<ID>` command
- Queried from Jira MCP
- Never hardcoded in prompts, agents, or code

**No example artifacts:**
- No fake User Stories
- No hardcoded Jira issue examples
- All content is generated from real external input

## Security & Safety

1. **No Jira modifications:** Agents query Jira only; Phase 00 is read-only
2. **No secrets in artifacts:** All artifacts are documentation-only, no credentials
3. **No unintended changes:** Agents modify only their phase's artifact
4. **No approval bypass:** Approval gates are enforced by the Orchestrator
5. **No data loss:** Status tracker preserves complete history

## Capstone Application: Automated Documentation Sync

This framework will orchestrate the implementation of the "Automated Documentation Sync" capstone project:

1. **Phase 00:** Retrieve "Implement Documentation Sync Service" from Jira
2. **Phase 01:** Extract requirements (e.g., "Sync Confluence docs to Markdown", "Maintain version history")
3. **Phase 02:** Design architecture (e.g., MCP connectors, storage layer, sync scheduler)
4. **Phase 03:** Review design for reliability, security, scalability
5. **Phase 04:** Plan implementation tasks (e.g., build Confluence connector, design sync algorithm)
6. **Phase 05:** Implement the service with tests
7. **Phase 06:** Review code quality and design adherence
8. **Phase 07:** Verify functional and non-functional requirements
9. **Phase 08:** Merge to main, deploy

**Framework advantage:** The same pipeline works for ANY capstone project — no hardcoding needed.

## Testing & Validation

To validate the framework:

1. Start with a real Jira issue (e.g., SCRUM-31)
2. Run through all 8 phases
3. Verify:
   - Artifacts created correctly
   - Approval gates work
   - Orchestrator resumes from correct phase
   - Status tracker is accurate
   - No data loss or skipped phases
4. Run a second User Story concurrently
5. Verify multi-User Story isolation

---

**Framework Version:** 1.0
**Status:** Ready for deployment and capstone implementation
**Author:** Agentic SDLC Framework Design Team
**Date:** 2026-08-31
