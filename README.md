# Claude Code Bootstrap

An opinionated, role-driven methodology layer for Claude Code.

Adds **three-session workflow** (Bootstrap → Planning → Runtime), **role-based agents**, **persistent state with phase-boundary checkpoints**, and **auto-logging with structured log types** (task, blocker, checkpoint, handoff). So your project stays organized, auditable, and on track as it grows.

> **Requires:** [Claude Code](https://claude.ai/code) (Anthropic's CLI). Built specifically for it — not a generic framework.

## Get Started

Start a Claude Code session in your working directory and run:

```
Read STARTUP.md and follow the instructions.
```

The agent will interview you, scaffold your project infrastructure, and walk you through:

- **Bootstrap session:** capture intent, define agents, initialize state
- **Planning session:** draft and review a phased execution plan
- **Runtime sessions:** orchestrate with configurable autonomy; use `/health` to inspect state and capture context at any point

## Commands

| Command                                | Purpose                            |
| -------------------------------------- | ---------------------------------- |
| `/bootstrap`                           | Run Bootstrap session on a new project |
| `/orchestrate [--auto [--skip-gates]]` | Task execution with autonomy modes |
| `/set-models max \| default \| min`    | Switch model tiers                 |
| `/health`                              | Project status dashboard           |
| `Esc`                                  | Interrupt                          |

## Structure

`.claude/` is the product — a fully built project infrastructure that grows across all sessions:

```
your-project/.claude/
├── docs/
│   └── SPEC.md          # What you're building
├── agents/
│   ├── orchestrator.md  # Routes work
│   └── [role].md        # Project roles
├── skills/
├── rules/
├── hooks/
└── state/
    ├── CONTEXT.md       # Working memory (curated each session)
    ├── PLAN.md          # Execution plan (Planning session+)
    └── logs/            # Append-only structured logs: task, blocker, checkpoint (phase end), handoff (user pause)
```

For the framework source, see `claude-bootstrap/`:

```
claude-bootstrap/
├── skills/              # Bootstrap methodology: bootstrap, planning, health, orchestrate, state-control
├── docs/                # Research, templates, reference architecture
└── eval/                # Evaluation framework
```

---

## Scope Boundary

- **Bootstrap framework** (`claude-bootstrap/`) — provides methodology templates, skills, and patterns
- **Project scope** (`.claude/` in your project) — customized agents, skills, rules, and execution state

Bootstrap agents operate in read-only mode on project files; they only write to `.claude/` and CLAUDE.md. Downstream agents own project execution.
