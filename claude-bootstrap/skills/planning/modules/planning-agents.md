---
name: planning-agents
description: Agent-only. Derive and define project-specific role-agents from the SPEC.
user-invocable: false
metadata:
  reads: [.claude/docs/SPEC.md, docs/preset-templates/agent.md]
  writes: [.claude/agents/, .claude/docs/templates/agent.md]
  authors: [votrinhan88]
  version: 0.1
---

# Planning Agents
Derive role-agents from SPEC. Each agent represents a distinct stakeholder perspective; together they create productive tension that surfaces tradeoffs during implementation.

## Prerequisites

`.claude/docs/SPEC.md` must be written and contain project scope, goals, and constraints (output from `planning-interview`).

## Steps
1. **Verify prerequisites** — check that `.claude/docs/SPEC.md` exists and contains project scope, goals, and constraints; if missing or empty, stop and prompt user to complete `planning-interview`
2. **Show template** — present agent template from `claude-bootstrap/docs/preset-templates/agent.md` to user; confirm they want to use it
3. **Derive roles**
  - From the SPEC, identify: failure modes the project must guard against; quality dimensions that matter; stakeholder perspectives that conflict or complement each other
  - Define a realistic, spec-driven combination of agent roles to fill those gaps
4. **Map tensions** — identify key productive tensions between role pairs; warn if any role has no tension (may be redundant or under-defined)
5. **Draft agents** — for each role, populate an agent file using the template; scope tools to least privilege; leave `metadata.invokes.skills: []` empty for now (will be populated by planning-skills)
6. **Present** — show all agent definitions and tension map to user; iterate until approved
7. **Export template** — copy `claude-bootstrap/docs/preset-templates/agent.md` to `.claude/docs/templates/agent.md`
8. **Write** — write agent definitions to `.claude/agents/`

## Examples
- Web app: architect, developer, qa, advocate — speed/coverage/consistency
- Book: writer, editor, expert, advocate — expression/clarity/accessibility
- ML pipeline: researcher, engineer, scientist, ops, expert — novelty/complexity/reproducibility/interpretability
- Automation: developer, ops, security, user — flexibility/lockdown/observability

## Edge Cases
- **Role has no tensions:** warn user — may be a utility/observer role (acceptable) or redundant/under-defined (needs revision)
- **Roles overlap in scope:** surface conflict; let user merge or redefine; do not write overlapping agent files
- **SPEC is ambiguous:** surface specific gap; return to Draft SPEC before proceeding

## Resources

### Output schema
```yaml
planning_agents:
  agents:
    [role-name]:
      description: One-liner description
      tensions: [peer-agents]
  user_confirmed: true | false
```

**Note:** `planning-skills` will add `metadata.invokes.skills: [list]` to agent files after skills are defined.
