# Claude Code Bootstrap

An opinionated, role-driven methodology layer for Claude Code.

Adds structured sessions, persistent state, and role-based agents on top of what Claude Code already provides — so your project stays organized and on track as it grows.

> **Requires:** [Claude Code](https://claude.ai/code) (Anthropic's CLI). Built specifically for it — not a generic framework.

## Get Started

Start a Claude Code session in your working directory and run:

```
Read STARTUP.md and follow the instructions.
```

The agent will interview you, scaffold your project infrastructure, and walk you through:

- **Session 0:** Bootstrap — capture intent, define agents, initialize state
- **Session 1:** Planning — draft and review a phased execution plan
- **Session 2+:** Implementation — orchestrate with configurable autonomy

## Commands

| Command                                | Purpose                            |
| -------------------------------------- | ---------------------------------- |
| `/bootstrap`                           | Run Session 0 on a new project     |
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
    ├── PLAN.md          # Execution plan (Session 1+)
    └── logs/            # Append-only session logs
```

For the framework source, see `claude-bootstrap/`:

```
claude-bootstrap/
├── sessions/            # Session workflows
├── skills/              # Reusable skill library
├── docs/                # Research and reports
└── eval/                # Evaluation framework
```
