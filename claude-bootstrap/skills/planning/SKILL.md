---
name: planning
description: User-only. Entry point for Planning session — define agents and skills, draft execution plan, and produce an approved implementation strategy.
user-invocable: true
metadata:
  reads: [.claude/docs/SPEC.md, .claude/state/logs/, .claude/agents/, .claude/skills/]
  writes: [CLAUDE.md, .claude/agents/, .claude/skills/, .claude/state/, .claude/rules/, .claude/hooks/]
  authors: [votrinhan88]
  version: 0.1
---

# Planning
Entry point for Planning session (agent + skill definition + execution planning). Engage the spec, define role-agents and project skills, draft execution plan, and enforce via hooks and rules.

## Steps

1. **Interview** — run `planning-interview` to gather project spec and populate `.claude/docs/SPEC.md`
2. **State init** — run `planning-state` to initialize `.claude/state/` and write CONTEXT.md with interview content
3. **Explore spec** — read project files and existing `.claude/` state; surface any gaps before proceeding
4. **Define agents** — run `bootstrap-agents` to define project-specific role-agents
5. **Define skills** — run `bootstrap-skills` to define project-specific skills
6. **Draft plan** — produce a phased PLAN.md with tasks assigned to roles
7. **Pre-flight** — verify plan completeness and scope coverage
8. **Annotate** — annotate plan tasks with role, complexity, and dependencies
9. **Role review** — review agent definitions against plan tasks; adjust as needed
10. **Hooks & rules** — run `bootstrap-hooks` to create hooks and rules files
11. **Write CLAUDE.md** — populate `docs/preset-templates/CLAUDE.md` template with all planning outputs (tools, git, rules, agents, workflow); write to project root
12. **Gate** — run Planning session gate per `sessions/session-planning.md` final step

## Examples

- **Input:** Bootstrap session complete. `.claude/` stub structure in place.
- **Output:** All agents and skills defined. PLAN.md written with phases, owners, and validation steps. Hooks and rules created. Ready for implementation.

## Edge Cases

- Planning session invoked without completed Bootstrap session: check for `.claude/` presence; if Bootstrap is not confirmed complete, stop and prompt user
- Spec gaps discovered during Explore: surface to user; do not proceed until resolved
- Agent-plan mismatch during role review: revise either plan or agent definition; loop until consistent

## Resources

### Modules
- `planning-interview.md` — gather project spec from user; populate SPEC.md
- `planning-state.md` — initialize `.claude/state/` and write CONTEXT.md with interview content
- `bootstrap-agents.md` — define project-specific role-agents based on spec and interview
- `bootstrap-skills.md` — define project-specific skills based on spec and agent needs
- `bootstrap-hooks.md` — create hooks and enforcement rules

### References
- `discipline.md` — complete Planning phase step-by-step execution guide
- `agent-spec.md` — agent file format, frontmatter fields, and body section guide
- `skill-spec.md` — skill directory structure, frontmatter fields, and body guide

### Previous Phase
Bootstrap phase must be complete before starting planning. See `/bootstrap` skill.
