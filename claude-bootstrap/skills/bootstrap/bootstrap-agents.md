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

After drafting all roles, identify productive tensions between them:

- Each agent's `**Tensions**` section is the source of truth
- Each entry names the conflicting peer, what they disagree on, and why it's productive
- Present tensions to the user for approval before writing them into agent files

If a role has no tensions with any other role, **warn the user** — it may be a legitimate utility/observer role, or it may indicate the role is redundant or under-defined. Let the user decide.

> The health skill can assemble a full tension matrix on demand by reading all agent files.

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

- All agent files fully defined (see `claude-bootstrap/templates/agent.md` for format)
- Tensions defined in each agent file's `**Tensions**` section
- Proceed to Step 5 (State Definition)