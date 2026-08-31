# Agentic SDLC Framework — Quick Start Guide

## 1. Start a New User Story

**Command:**
```bash
ORCHESTRATOR input=<USER_STORY_ID> action=start
```

**Example:**
```bash
ORCHESTRATOR input=SCRUM-31 action=start
```

**What happens:**
- Orchestrator queries Jira for SCRUM-31.
- Phase 00 Agent retrieves the User Story.
- Creates `docs/artifacts/SCRUM-31/user-story.md` and `status.md`.
- Halts and requests human approval.

**Human Action:**
Review the User Story artifact at: `docs/artifacts/SCRUM-31/user-story.md`

If approved:
```bash
ORCHESTRATOR input=SCRUM-31 action=approve phase=00
```

---

## 2. Resume to Next Phase

**Command:**
```bash
ORCHESTRATOR input=SCRUM-31 action=resume
```

**What happens:**
- Orchestrator reads `status.md`.
- Determines next phase (01: Requirements).
- Invokes Phase 01 Agent.
- Phase 01 analyzes requirements and asks clarification questions.
- Creates `requirements.md`.
- Halts and requests human approval.

**Human Action:**
1. Review the Requirements artifact at: `docs/artifacts/SCRUM-31/requirements.md`
2. Answer any clarification questions in the artifact.
3. Approve with:
   ```bash
   ORCHESTRATOR input=SCRUM-31 action=approve phase=01
   ```

---

## 3. Continue Through All Phases

Repeat Step 2 for each phase:

| Phase | Artifact | Approval |
|-------|----------|----------|
| 02 | `architecture.md` | YES |
| 03 | `design-review.md` | YES |
| 04 | `impl-plan.md` | YES |
| 05 | Code (feature branch) | NO (reviewed by Phase 06) |
| 06 | `review.md` | YES |
| 07 | `verification.md` | YES |
| 08 | Pull Request | Merge automatically |

---

## 4. Query Status Anytime

**Command:**
```bash
ORCHESTRATOR input=SCRUM-31 action=status
```

**Output:**
- Current phase
- Completed phases
- Pending approval phase (if any)
- Blocked phase (if any)
- Artifact summary

---

## 5. Files to Review at Each Approval Gate

| Phase | File | What to Check |
|-------|------|---------------|
| 00 | `docs/artifacts/<ID>/user-story.md` | Is the User Story correctly retrieved from Jira? |
| 01 | `docs/artifacts/<ID>/requirements.md` | Are all requirements clear? Clarifications recorded? |
| 02 | `docs/artifacts/<ID>/architecture.md` | Does the architecture address all requirements? |
| 03 | `docs/artifacts/<ID>/design-review.md` | Are design review findings acceptable? Architecture updated? |
| 04 | `docs/artifacts/<ID>/impl-plan.md` | Is the implementation plan feasible? Dependencies clear? |
| 06 | `docs/artifacts/<ID>/review.md` | Is the code quality acceptable? All tasks completed? |
| 07 | `docs/artifacts/<ID>/verification.md` | Do all tests pass? Requirements verified? |
| 08 | GitHub PR | Is the PR merged to main? |

---

## 6. Example: Approving Phase 01 with Clarifications

**Scenario:** Phase 01 Agent has asked clarification questions.

**File:** `docs/artifacts/SCRUM-31/requirements.md`

**Content example:**
```markdown
# Requirements — SCRUM-31

## Clarifications Requested and Resolved

### Clarification 1
- **Question:** Should the empty state display when filtering returns 0 results?
- **Human Response:** Yes, always show "No results match your filter/search" when tasks exist but filter returns 0.
- **Resolution:** Added as FR-1.

### Clarification 2
- **Question:** What message should display when there are no tasks at all?
- **Human Response:** Show "Your list is empty. Add a new task to get started."
- **Resolution:** Added as FR-2.

## Functional Requirements
- FR-1: Display "No results match your filter/search" when filter returns 0 results (and tasks exist)
- FR-2: Display "Your list is empty" when no tasks exist
- FR-3: Update empty state immediately when filter changes
```

**Human Action:**
1. Review requirements.
2. Verify clarifications are correct.
3. Approve:
   ```bash
   ORCHESTRATOR input=SCRUM-31 action=approve phase=01
   ```

---

## 7. Handling Blocked Phases

**Scenario:** A phase cannot complete (e.g., missing data, unresolvable blocker).

**Status:** Check `status.md`
```markdown
Blocked Phase: 03: Design Review
Reason: Cannot determine authentication mechanism (requires Product Owner decision)
Recovery: Contact Product Owner, document decision, resume Phase 03
```

**Human Action:**
1. Understand the blocker.
2. Resolve the issue (e.g., get Product Owner decision).
3. Update `status.md` to clear the block.
4. Resume:
   ```bash
   ORCHESTRATOR input=SCRUM-31 action=resume
   ```

---

## 8. Running Multiple User Stories

The framework supports unlimited concurrent User Stories:

```bash
# Start User Story 1
ORCHESTRATOR input=SCRUM-31 action=start

# Start User Story 2 (different team, different phases)
ORCHESTRATOR input=SCRUM-32 action=start

# Check status of User Story 1
ORCHESTRATOR input=SCRUM-31 action=status

# Approve Phase 01 of User Story 1
ORCHESTRATOR input=SCRUM-31 action=approve phase=01

# Resume User Story 2 from where it left off
ORCHESTRATOR input=SCRUM-32 action=resume
```

Each User Story has its own directory and status tracker. No interference.

---

## 9. Framework Files Reference

| File | Purpose |
|------|---------|
| `.instructions.md` | Shared instructions for all agents |
| `ORCHESTRATOR.md` | Orchestrator algorithm and state management |
| `README.md` | Complete framework documentation |
| `STATUS_TEMPLATE.md` | Template for status.md |
| `prompts/00-input.md` | Phase 00 prompt |
| `prompts/01-requirements.md` | Phase 01 prompt |
| ... | (one prompt per phase) |
| `agents/00-input-agent.instructions.md` | Phase 00 agent instructions |
| `agents/01-requirements-agent.instructions.md` | Phase 01 agent instructions |
| ... | (one agent file per phase) |
| `docs/artifacts/<ID>/user-story.md` | Phase 00 output |
| `docs/artifacts/<ID>/requirements.md` | Phase 01 output |
| ... | (one artifact per phase) |
| `docs/artifacts/<ID>/status.md` | Status tracker for the User Story |

---

## 10. Common Commands

```bash
# Start a new User Story
ORCHESTRATOR input=<USER_STORY_ID> action=start

# Resume from the last phase
ORCHESTRATOR input=<USER_STORY_ID> action=resume

# Approve a phase
ORCHESTRATOR input=<USER_STORY_ID> action=approve phase=<PHASE_NUMBER>

# Query current status
ORCHESTRATOR input=<USER_STORY_ID> action=status

# Explicitly skip a phase (rare)
ORCHESTRATOR input=<USER_STORY_ID> action=skip phase=<PHASE_NUMBER> reason=<reason>

# Rollback to a previous phase (recover from error)
ORCHESTRATOR input=<USER_STORY_ID> action=rollback phase=<PHASE_NUMBER>
```

---

## 11. Success Checklist

When all phases are complete:

- [x] User Story retrieved from Jira (Phase 00)
- [x] Requirements analyzed and clarified (Phase 01)
- [x] Architecture designed and approved (Phase 02)
- [x] Design review completed (Phase 03)
- [x] Implementation plan created (Phase 04)
- [x] Feature implemented with tests (Phase 05)
- [x] Code reviewed and approved (Phase 06)
- [x] Feature verified and tested (Phase 07)
- [x] Pull request merged to main (Phase 08)

---

## 12. Support & Troubleshooting

**Question:** Where do I find the User Story for SCRUM-31?
**Answer:** `docs/artifacts/SCRUM-31/user-story.md`

**Question:** How do I approve a phase?
**Answer:** Review the artifact, then run:
```bash
ORCHESTRATOR input=<USER_STORY_ID> action=approve phase=<PHASE_NUMBER>
```

**Question:** What if a phase fails?
**Answer:** Check `status.md` for the block reason. Fix the issue, then resume.

**Question:** Can I run multiple User Stories in parallel?
**Answer:** Yes! Each has its own directory and status tracker.

**Question:** What if I need to modify an earlier phase?
**Answer:** The phase artifact can be updated, but you must manually reset `status.md` to that phase and resume from there.

---

**For detailed information, see `README.md` and `ORCHESTRATOR.md`**
