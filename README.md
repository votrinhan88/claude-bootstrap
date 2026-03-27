# Claude Code Bootstrap

A framework to turn your project into an organized, multi-agent workspace with structured state and quality gates.

## Get Started

Start a Claude Code session in your project directory and point the agent to `STARTUP.md`:

```
Read STARTUP.md and follow the instructions.
```

The agent will interview you, scaffold your project infrastructure, and walk you through:

- **Session 0:** Bootstrap (set up `.claude/` structure and agents)
- **Session 1:** Planning (create execution plan with multi-role review)
- **Session 2+:** Implementation (orchestrate with configurable autonomy)

## Commands

| Command                                | Purpose                            |
| -------------------------------------- | ---------------------------------- |
| `/orchestrate [--auto [--skip-gates]]` | Task execution with autonomy modes |
| `/set-models max \| default \| min`    | Switch model tiers                 |
| `/health`                              | Project status dashboard           |
| `Esc`                                  | Interrupt                          |

## Project Structure

Both `CLAUDE.md` (thin config) and `.claude/` (infrastructure) are created and managed locally.

```
project/
├── CLAUDE.md                           # The ONLY Claude file at root
└── .claude/
    ├── docs/
    │   └── SPEC.md                     # What we're building
    ├── agents/
    │   ├── orchestrator.md             # Routes work
    │   └── [role].md                   # Project roles
    ├── skills/
    │   ├── bootstrap/                  # Bootstrap procedures
    │   ├── health.md                   # Status dashboard
    │   └── [domain].md                 # Domain knowledge
    ├── rules/
    ├── hooks/
    └── state/
        ├── CONTEXT.md                  # Working memory (curated each session)
        ├── plan.md                     # Execution plan (Session 1+)
        └── logs/                       # Append-only session logs
```

Explore `claude-bootstrap/` for full documentation.
