# Copilot Atlas

**Multi-agent orchestration for VS Code Copilot that runs autonomously from planning to commit.**

> Built on [copilot-orchestra](https://github.com/ShepAlderson/copilot-orchestra) by ShepAlderson

## TL;DR

Drop these agent files into VS Code, then:

```
@Planner Add user authentication with JWT
```

The system will:
1. Research your codebase (parallel quadrant search)
2. Write a plan with failure analysis (Red Team Phase 0)
3. Implement each phase with TDD (tests first)
4. Review for security (OWASP Top 10) and performance (Big-O)
5. Auto-commit after each approved phase

You come back to a completed, tested, reviewed feature.

## Why This Exists

GitHub Copilot is powerful, but complex tasks require constant hand-holding. Copilot Atlas solves this:

| Problem | Solution |
|---------|----------|
| Stops after each step | **Zero-stop mode** - runs until complete |
| Loses context on crash | **State files** - resume from exact checkpoint |
| Sequential searches | **Quadrant parallelism** - 4 simultaneous searches |
| Manual debugging | **Self-healing** - auto-invokes researcher on failures |
| No quality gates | **Built-in audits** - OWASP + Big-O on every phase |

## Quick Start

### 1. Install

```bash
git clone https://github.com/huyhoang171106/Copilot-Atlas
```

Copy all `.agent.md` files to your VS Code prompts folder:

| OS | Path |
|----|------|
| Windows | `%APPDATA%\Code - Insiders\User\prompts\` |
| macOS | `~/Library/Application Support/Code - Insiders/User/prompts/` |
| Linux | `~/.config/Code - Insiders/User/prompts/` |

### 2. Configure VS Code

Add to `settings.json`:

```json
{
  "chat.customAgentInSubagent.enabled": true,
  "github.copilot.chat.responsesApiReasoningEffort": "high"
}
```

### 3. Use

**Full autonomous flow:**
```
@Planner Build a REST API for user management
```

**Resume after crash:**
```
@Conductor Continue from state
```

**Explore codebase:**
```
@ExplorerSubagent Find all files related to authentication
```

## Architecture

```
                         @Planner
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
      @ExplorerSubagent @ExplorerSubagent @ExplorerSubagent
        (frontend)        (backend)         (data)
              │              │              │
              └──────────────┼──────────────┘
                             ▼
                  @ResearcherSubagent (×N parallel)
                             │
                             ▼
                      Write Plan + Red Team
                             │
                             ▼
                        @Conductor
                             │
         ┌───────────────────┼───────────────────┐
         │                   │                   │
         ▼                   ▼                   ▼
@ImplementerSubagent  @FrontendSubagent  @TestWriterSubagent
      (backend)            (UI)              (tests)
         │                   │                   │
         └───────────────────┼───────────────────┘
                             ▼
                   @CodeReviewerSubagent
                      (OWASP + Big-O)
                             │
                             ▼
                    Auto-commit → Next phase
```

## Agents

### Primary Agents

| Agent | File | Role |
|-------|------|------|
| Planner | `Planner.agent.md` | Researches codebase, writes Red Team-validated plans, hands off to Conductor |
| Conductor | `Conductor.agent.md` | Orchestrates implementation phases, manages state, auto-commits |

### Subagents

| Agent | File | Role |
|-------|------|------|
| Explorer | `ExplorerSubagent.agent.md` | Quadrant-parallel codebase discovery (4 simultaneous searches) |
| Researcher | `ResearcherSubagent.agent.md` | Deep analysis: patterns, dependencies, risks |
| Implementer | `ImplementerSubagent.agent.md` | Backend implementation with strict TDD |
| Frontend | `FrontendSubagent.agent.md` | UI implementation with accessibility (WCAG 2.1 AA) |
| CodeReviewer | `CodeReviewerSubagent.agent.md` | Security (OWASP Top 10) + Performance (Big-O) audit |
| TestWriter | `TestWriterSubagent.agent.md` | Failing-first test generation |
| Dependency | `DependencySubagent.agent.md` | CVE audit + safe upgrade plans |
| CI | `CiSubagent.agent.md` | Pipeline design with caching |
| Docs | `DocsSubagent.agent.md` | README, changelog, API docs |
| Performance | `PerformanceSubagent.agent.md` | Profiling + hotspot remediation |

## Key Features

### Zero-Stop Mode

Conductor runs autonomously through all phases. Only stops on:
- Completion (success)
- Critical failure (2+ consecutive review failures)
- User interrupt

### State Recovery

Agents maintain state files (`.conductor_state.md`, `.planner_state.md`). On crash:
```
@Conductor Continue from state
```
Resumes from exact checkpoint.

### Self-Healing Debug

When tests fail twice:
1. Implementer auto-invokes `@ResearcherSubagent` with error context
2. Researcher analyzes root cause
3. Implementer applies fix
4. If still fails after 3 attempts → flags CRITICAL

### Red Team Phase 0

Every plan includes failure mode analysis:

| Failure Mode | Impact | Countermeasure |
|--------------|--------|----------------|
| SQL injection via search | Critical | Parameterized queries |
| Token expiry race condition | Medium | Refresh token rotation |
| Memory leak on long sessions | Low | Cleanup on unmount |

## File Structure

```
Copilot-Atlas/
├── Conductor.agent.md           # Main orchestrator
├── Planner.agent.md             # Autonomous planner
├── ExplorerSubagent.agent.md    # Codebase discovery
├── ResearcherSubagent.agent.md  # Deep analysis
├── ImplementerSubagent.agent.md # Backend TDD
├── FrontendSubagent.agent.md    # UI TDD
├── CodeReviewerSubagent.agent.md # Security + Performance
├── TestWriterSubagent.agent.md  # Test generation
├── DependencySubagent.agent.md  # CVE auditing
├── CiSubagent.agent.md          # CI/CD pipelines
├── DocsSubagent.agent.md        # Documentation
├── PerformanceSubagent.agent.md # Performance tuning
└── README.md
```

## Requirements

- **VS Code Insiders** (for latest agent features)
- GitHub Copilot subscription

## License

MIT
