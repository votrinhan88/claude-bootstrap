---
name: planning-claude
description: Agent-only. Draft and populate CLAUDE.md from all planning outputs.
user-invocable: false
metadata:
  reads: [.claude/docs/SPEC.md, .claude/agents/, .claude/skills/, .claude/hooks/, .claude/rules/, .claude/state/PLAN.md, docs/preset-templates/CLAUDE.md]
  writes: [.claude/CLAUDE.md, .claude/docs/templates/CLAUDE.md]
  authors: [votrinhan88]
  version: 0.1
---

# Planning CLAUDE.md
Draft and populate CLAUDE.md from all planning outputs: spec, agents, skills, hooks, plan. This becomes the project's primary documentation source for Claude Code automation.

## Prerequisites

All prior planning modules must be complete: SPEC.md, agents, skills, hooks, rules, PLAN.md, and state initialization (outputs from all previous planning modules). CLAUDE.md aggregates all planning outputs into project documentation.

## Steps
1. **Verify prerequisites** — check that all prior modules are complete: `.claude/docs/SPEC.md`, `.claude/agents/`, `.claude/skills/`, `.claude/hooks/`, `.claude/rules/`, and `.claude/state/PLAN.md`; if any missing, stop and prompt user to complete prior modules

2. **Show template** — present CLAUDE.md template from `claude-bootstrap/docs/preset-templates/CLAUDE.md` to user; confirm they want to use it

3. **Gather planning outputs** — collect:
  - `.claude/docs/SPEC.md` — project purpose, scope, constraints
  - `.claude/agents/` — agent definitions and roles
  - `.claude/skills/` — skill definitions and invocations
  - `.claude/hooks/` and `.claude/rules/` — enforcement rules
  - `.claude/state/PLAN.md` — execution plan and phases

4. **Draft CLAUDE.md** — populate template:
  - **name & description** — from SPEC (project name, one-liner)
  - **metadata** — stack/maintainer from SPEC; git_workflow from rules; version 0.1
  - **Overview** — from SPEC purpose, scope, deliverables
  - **Architecture** — from SPEC scope/in-scope items; folder tree if project-specific
  - **Workflow** — phases from PLAN.md with owners and validation criteria
  - **Agents** — table of agents and tensions from `.claude/agents/`
  - **Skills** — list of skills and their invocations from `.claude/skills/`
  - **Hooks & Rules** — enforcement boundaries from `.claude/hooks/` and `.claude/rules/`
  - **Rules & Constraints** — from SPEC constraints and any project-specific rules
  - **Git Workflow** — from `.claude/rules/git.md` and SPEC constraints
  - **State & Logging** — reference to state structure and log types

5. **Present** — show completed CLAUDE.md to user; iterate until approved

6. **Export template** — copy `claude-bootstrap/docs/preset-templates/CLAUDE.md` to `.claude/docs/templates/CLAUDE.md`

7. **Write** — write CLAUDE.md to `./.claude/CLAUDE.md`

## Examples
- **Input:** All planning outputs finalized; template populated
- **Output:** CLAUDE.md written to project root; user approved; ready for downstream reference

## Edge Cases
- **Existing CLAUDE.md:** if file exists, ask user whether to overwrite or merge; preserve any project-specific sections
- **Missing SPEC fields:** note in Workflow section that details available in PLAN.md
- **No agents or skills defined:** acceptable for simple projects; note in those sections and move on
- **Project-specific workflow notes:** add to Workflow section; reference `.claude/state/PLAN.md` for detail

## Resources

### Output schema
```yaml
planning_claude:
  file_path: CLAUDE.md (project root)
  sections_populated: [list of completed sections]
  user_confirmed: true | false
```
