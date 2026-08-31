# Capstone Project: Agentic SDLC Framework

This repository implements a complete **Agentic Software Development Lifecycle** pipeline using GitHub Copilot Agent Mode.

## Quick Navigation

### 📚 Documentation
- **[Framework Guide](docs/README.md)** — Complete framework documentation
- **[Quick Start](docs/QUICKSTART.md)** — Get started with 5 commands
- **[Framework Design](docs/FRAMEWORK_DESIGN.md)** — Architecture and design details
- **[Deployment Checklist](docs/DEPLOYMENT_CHECKLIST.md)** — Verification and deployment
- **[Status Template](docs/STATUS_TEMPLATE.md)** — Track User Story progress

### 🤖 Copilot Configuration
- **[Shared Instructions](.github/copilot/instructions.md)** — Core principles and rules
- **[Agents](.github/copilot/)** — 8 independent agents (one per SDLC phase)
- **[Prompts](.github/prompts/)** — Phase-specific prompts
- **[Orchestrator](.github/workflows/orchestrator.md)** — State machine and coordination

### 📦 Project Artifacts
- **[Artifacts](docs/artifacts/)** — User Story artifacts (created dynamically)
  - Each User Story gets its own `docs/artifacts/<USER_STORY_ID>/` directory
  - Contains phase outputs: user-story.md, requirements.md, architecture.md, etc.

## Framework Overview

The framework orchestrates 8 SDLC phases:

| Phase | Agent | Purpose |
|-------|-------|---------|
| 00 | Input | Retrieve User Story from Jira |
| 01 | Requirements | Extract and clarify requirements |
| 02 | Architecture | Design high-level solution |
| 03 | Design Review | Validate architecture |
| 04 | Planning | Create implementation plan |
| 05 | Implementation | Write code and tests |
| 06 | Code Review | Review code quality |
| 07 | Verification | Test and validate |
| 08 | PR & Merge | Create PR and merge |

## Getting Started

### 1. Start a New User Story
```bash
ORCHESTRATOR input=<USER_STORY_ID> action=start
```

Example:
```bash
ORCHESTRATOR input=SCRUM-31 action=start
```

### 2. Review and Approve
The framework will create artifacts and halt at approval gates. Review the artifact and approve:
```bash
ORCHESTRATOR input=SCRUM-31 action=approve phase=<PHASE_NUMBER>
```

### 3. Resume to Next Phase
```bash
ORCHESTRATOR input=SCRUM-31 action=resume
```

### 4. Query Status
```bash
ORCHESTRATOR input=SCRUM-31 action=status
```

## Folder Structure

```
project-root/
│
├── .github/
│   ├── copilot/                         # Copilot agent definitions
│   │   ├── instructions.md              # Shared instructions
│   │   └── *-agent.instructions.md      # 8 agent instruction files
│   ├── prompts/                         # Phase-specific prompts
│   │   ├── 00-input.md
│   │   ├── 01-requirements.md
│   │   └── ... (one per phase)
│   └── workflows/
│       └── orchestrator.md              # Orchestrator state machine
│
├── docs/
│   ├── README.md                        # Framework documentation
│   ├── QUICKSTART.md                    # Quick start guide
│   ├── FRAMEWORK_DESIGN.md              # Design and architecture
│   ├── DEPLOYMENT_CHECKLIST.md          # Deployment verification
│   ├── STATUS_TEMPLATE.md               # Status template
│   └── artifacts/                       # User Story artifacts
│       └── <USER_STORY_ID>/             # (created dynamically)
│           ├── user-story.md
│           ├── requirements.md
│           ├── architecture.md
│           ├── design-review.md
│           ├── impl-plan.md
│           ├── review.md
│           ├── verification.md
│           └── status.md
│
├── src/                                 # Application source code
├── tests/                               # Test files
├── scripts/                             # Utility scripts
│
└── README.md                            # This file
```

## Key Features

✅ **8 Independent Agents** — Autonomous execution of each SDLC phase  
✅ **Human-in-the-Loop** — Approval gates at critical phases  
✅ **Dynamic User Stories** — No hardcoded Jira issues  
✅ **Multi-Story Support** — Run unlimited concurrent User Stories  
✅ **Safe Failure & Recovery** — State preserved, safe interruption handling  
✅ **Complete Traceability** — Every decision links to User Story and phase  
✅ **Jira MCP Integration** — Dynamic User Story retrieval  

## Requirements

- Jira MCP configured and connected
- Git/GitHub configured
- GitHub Copilot Agent Mode available
- VS Code or compatible editor

## Documentation

Start with **[docs/README.md](docs/README.md)** for comprehensive framework documentation.

Quick reference:
- **[Quick Start Guide](docs/QUICKSTART.md)** — 5 commands to get started
- **[Framework Design](docs/FRAMEWORK_DESIGN.md)** — Architecture and design principles
- **[Orchestrator Details](.github/workflows/orchestrator.md)** — State machine and recovery
- **[Shared Instructions](.github/copilot/instructions.md)** — Rules and principles

## Next Steps

1. Review [docs/README.md](docs/README.md)
2. Choose a Jira User Story (e.g., SCRUM-31)
3. Run: `ORCHESTRATOR input=SCRUM-31 action=start`
4. Proceed through phases with human approvals
5. Deploy framework for your capstone project

## Support

For questions or issues:
1. Check the documentation in `docs/`
2. Review the framework design in [docs/FRAMEWORK_DESIGN.md](docs/FRAMEWORK_DESIGN.md)
3. Consult the orchestrator algorithm in [.github/workflows/orchestrator.md](.github/workflows/orchestrator.md)

---

**Framework Version:** 1.0  
**Status:** Ready for Production  
**Last Updated:** 2026-08-31
