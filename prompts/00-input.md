# Phase 00: Input — Retrieve User Story from Jira

## Objective
Retrieve a User Story from Jira MCP and create a normalized artifact that will serve as the input for all downstream phases.

## Input
- **User Story ID** (provided by orchestrator or user, e.g., `SCRUM-31`)
- **Access to Jira MCP** (pre-configured, no credentials needed)

## Process

### 1. Query Jira MCP
- Use the connected Jira MCP to fetch the User Story by ID.
- Retrieve:
  - Issue key
  - Summary
  - Description (full content)
  - Acceptance criteria (if available in description or custom field)
  - Any other relevant metadata (status, priority, assignee, etc.)
- **Do NOT modify** the Jira issue.

### 2. Create Normalized Artifact
Create `docs/artifacts/<USER_STORY_ID>/user-story.md` with:

```markdown
# User Story: <USER_STORY_ID>

**Source:** Jira
**Jira URL:** <link to the issue in Jira>
**Retrieved:** <timestamp>

## Title
<Summary from Jira>

## Description
<Full description from Jira, formatted as provided>

## Acceptance Criteria
<Acceptance criteria from Jira, or "Not specified" if not found>

## Additional Metadata
- **Status:** <Jira status>
- **Priority:** <Jira priority>
- **Assignee:** <Jira assignee, or "Unassigned">
- **Reporter:** <Jira reporter>
- **Created:** <Jira created date>

---

**Note:** This artifact is read-only. It serves as the source of truth for all downstream phases.
```

### 3. Create Status File
Create `docs/artifacts/<USER_STORY_ID>/status.md`:

```markdown
# User Story: <USER_STORY_ID>

**Title:** <Summary from user-story.md>

**Current Phase:** 00: Input (Complete)

**Completed Phases:**
- [X] 00: Input
- [ ] 01: Requirements
- [ ] 02: Architecture
- [ ] 03: Design Review
- [ ] 04: Planning
- [ ] 05: Implementation
- [ ] 06: Review
- [ ] 07: Verification
- [ ] 08: PR

**Pending Human Approval:** 00: Input

**Blocked Phase:** None

**Last Updated:** <TIMESTAMP>

**Notes:**
User Story successfully retrieved from Jira. Awaiting human approval to proceed to Phase 01: Requirements Analysis.
```

## Output
- `docs/artifacts/<USER_STORY_ID>/user-story.md` (created)
- `docs/artifacts/<USER_STORY_ID>/status.md` (created)
- **Approval Required:** YES

## Halt Condition
**Stop if:**
- User Story ID cannot be resolved in Jira.
- Jira MCP is unavailable.
- Retrieved content is invalid or incomplete.

**Report:**
- User Story ID that failed
- Error from Jira MCP
- Recommended recovery action (e.g., verify issue key, check Jira connectivity)

## No-Assumption Rule
- Do NOT infer missing details.
- If description or acceptance criteria are missing, note them as "Not specified" in the artifact.
- Do NOT generate placeholder acceptance criteria.

## Next Phase
Phase 01: Requirements Analysis (awaiting human approval)
