---
name: planning-skills
description: Agent-only. Derive and define project-specific skills from SPEC and agents.
user-invocable: false
metadata:
  reads: [.claude/docs/SPEC.md, .claude/agents/, docs/preset-templates/skill.md]
  writes: [.claude/skills/, .claude/agents/, .claude/docs/templates/skill.md]
  authors: [votrinhan88]
  version: 0.1
---

# Planning Skills
Derive project-specific skills from the SPEC and agent needs. Skills encode reusable procedures that agents invoke; they keep agent files lean and allow shared workflows.

## Prerequisites

Agents must be defined in `.claude/agents/` (output from `planning-agents`). This module reads agent definitions to understand ownership context when deriving skills.

## Steps
1. **Verify prerequisites** — check that `.claude/agents/` directory exists and contains agent files; if missing, stop and prompt user to complete `planning-agents`
2. **Show template** — present skill template from `claude-bootstrap/docs/preset-templates/skill.md` to user; confirm they want to use it
3. **Identify skill needs** — from SPEC phases and agent definitions, list: recurring multi-step procedures; domain-specific workflows; cross-agent shared operations
4. **Scope each skill** — determine: which agent(s) invoke it; what it reads/writes; whether it needs sub-modules
5. **Draft skills** — for each skill, populate a SKILL.md using the template; scope tools to least privilege
6. **Present** — show skill list and definitions to user; iterate until approved
7. **Export template** — copy `claude-bootstrap/docs/preset-templates/skill.md` to `.claude/docs/templates/skill.md`
8. **Write** — create skill directories and SKILL.md files in `.claude/skills/`
9. **Update agents** — for each agent in `.claude/agents/`, populate `metadata.invokes.skills: [list of minimally invokable skills]` in their definition

## Examples
- **Web app:** `deploy`, `db-migrate`, `code-review` skills invoked by developer/ops agents
- **Book:** `outline`, `chapter-draft`, `edit-pass` skills invoked by writer/editor agents
- **ML pipeline:** `data-validate`, `train`, `evaluate` skills invoked by scientist/ops agents

## Edge Cases
- **Skill scope too broad:** split into modules or separate skills; SKILL.md should stay under 200 lines
- **Two agents need conflicting behavior for the same operation:** define two skills or add a mode parameter; do not write ambiguous instructions
- **SPEC phase maps to no skill:** acceptable for simple one-off tasks; only create skills for recurring or complex procedures
- **Agent doesn't invoke any skills:** leave `metadata.invokes.skills: []` in agent file; some roles may not need skills

## Resources
### Output schema
```yaml
planning_skills:
  [skill-name]:
    invoked_by: [list of agent-names]
  user_confirmed: true | false
```
