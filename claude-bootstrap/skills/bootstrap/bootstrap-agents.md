# Bootstrap Agents

> **Note:** This skill is a placeholder — content is incomplete.

Derive and define project-specific agent roles during Session 0a, Step 4 (after scaffold, before state init).

---

## Prerequisites

- Interview handoff completed and confirmed
- Project scaffold structure exists (Step 3)
- `.claude/agents/` directory exists with placeholders

---

## Procedure

For each role placeholder, derive the full agent definition from:
1. The project's failure modes (identified in interview)
2. Quality dimensions that matter for this project
3. Stakeholder perspectives that conflict or complement each other

---

## Core Rules

Each role must:
- **Represent a distinct stakeholder perspective** with its own priorities and values
- **Create productive tension** with at least one other role (not every role needs to clash with every other, but no isolated roles)
- **Have a clear definition** in `.claude/agents/[role].md` (see template in `claude-bootstrap/templates/agent.md`)

---

## Tension Mapping

After drafting all roles, create a **tension map**:

For each pair of roles that conflicts, explicitly state:
- Where they disagree (which decision, quality metric, or approach)
- Why that tension is productive (what does it prevent? what does it enable?)

**Example tensions:**

| Roles | Disagree on | Productive because |
|-------|------------|-------------------|
| architect ↔ developer | speed vs. consistency | Forces thoughtful trade-offs; prevents reckless rewrites AND over-engineering |
| qa ↔ developer | coverage vs. velocity | Keeps shipping realistic while quality stays defensible |
| ops ↔ end-user advocate | stability vs. features | Prevents broken deployments AND feature starvation |

If a role creates no tension with any other role, **it is redundant** — remove it and delete its placeholder file.

---

## Example Derivations

These are illustrative, not prescriptive. Derive roles from *your* interview findings.

| Project type | Typical roles | Core tension |
|--------------|---------------|--------------|
| Web app | architect, developer, qa, end-user advocate | speed ↔ coverage ↔ consistency |
| Book | writer, editor, subject expert, reader advocate | expression ↔ clarity ↔ accessibility |
| ML pipeline | data engineer, scientist, ops, domain expert | complexity ↔ reproducibility ↔ interpretability |
| Automation | developer, ops, security, end-user | flexibility ↔ lockdown ↔ observability |

---

## Outcome

All agent files are fully defined (see `claude-bootstrap/templates/agent.md` for format). Tension map is created and approved by user before proceeding to Step 5 (State Definition).