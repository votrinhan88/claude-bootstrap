---
name: planning
description: User-only. Entry point for Planning session — draft spec, define agents and skills, and produce an approved execution plan.
user-invocable: true
metadata:
  reads: [.claude/]
  writes: [.claude/]
  authors: [votrinhan88]
  version: 0.1
---

# Planning
Entry point for Planning session (spec-driven). Draft spec from Bootstrap findings, define role-agents and skills, derive and iterate the execution plan, enforce via hooks and rules.

## Steps
1. **Interview** — run `planning-interview`; fill gaps from Bootstrap/Audit findings; produce `.claude/docs/SPEC.md`: goals, phases, constraints, deliverables
2. **Define agents** — run `planning-agents`; derive role-agents from SPEC; assign ownership; validate coverage
3. **Define skills** — run `planning-skills`; derive project skills from SPEC + agents
4. **Rules & enforcement** — run `planning-rules`; derive rules and enforcement hooks from agents, skills, and SPEC
5. **Draft plan** — run `planning-plan`; draft, validate, and iterate PLAN.md until approved
6. **State init** — run `planning-state`; initialize `.claude/state/` structure; write CONTEXT.md
7. **Write CLAUDE.md** — run `planning-claude`; populate CLAUDE.md template with all planning outputs; write to project root
8. **Gate** — review and finalize all planning outputs; present to user for approval; commit if git tracking enabled
   - **Present for approval:** Completed CLAUDE.md (from Step 7), plus summary of all planning outputs:
     - `.claude/docs/SPEC.md` (project scope and constraints)
     - `.claude/agents/` (role-agents and responsibilities)
     - `.claude/skills/` (reusable workflows)
     - `.claude/hooks/` and `.claude/rules/` (enforcement boundaries)
     - `.claude/state/PLAN.md` (execution plan with phases and dependencies)
     - `.claude/state/CONTEXT.md` (initial session context)
   - **Approval gate:** User confirms all outputs are correct and complete; gather final feedback and revise any outputs as needed (return to relevant module if required)
   - **Checkpoint log:** Write `.claude/state/logs/{date}_{seq}_checkpoint.md` documenting: what was completed (all planning outputs), completion metrics, current status (planning phase complete, ready for implementation), next phase (first phase from PLAN.md)
   - **Git commit:** If git tracking enabled, commit all `.claude/` changes and `CLAUDE.md` with message: `feat(planning): complete planning session with spec, agents, skills, rules, plan, and state`

## Examples
- **Input:** Bootstrap session complete. Bootstrap/Audit findings available.
- **Output:** SPEC.md written. Agents, skills, hooks, and rules defined. PLAN.md approved by user. CLAUDE.md written. Ready for implementation.

## Edge Cases
- Planning session invoked without completed Bootstrap session: check for `.claude/` presence; if Bootstrap is not confirmed complete, stop and prompt user
- SPEC gaps surface during Define agents or Define skills: return to Draft SPEC; do not proceed until resolved
- PLAN.md iteration stalls: surface specific blocker to user; do not proceed to Gate until plan is approved

## Resources
### Modules
- `planning-interview.md` — gather project spec from user; fill gaps from Bootstrap/Audit findings
- `planning-agents.md` — define project-specific role-agents based on SPEC
- `planning-skills.md` — define project-specific skills based on SPEC + agents
- `planning-rules.md` — derive enforcement rules and hooks from SPEC, agents, and skills
- `planning-plan.md` — draft, validate, and iterate PLAN.md until approved
- `planning-state.md` — initialize `.claude/state/` structure; write CONTEXT.md
- `planning-claude.md` — draft and populate CLAUDE.md with planning outputs

### Previous Phase
Bootstrap phase must be complete before starting planning. See `/bootstrap` skill.
