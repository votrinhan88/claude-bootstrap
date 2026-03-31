---
name: planning-plan
description: Agent-only. Draft, validate, and iterate PLAN.md until user-approved.
user-invocable: false
metadata:
  reads: [.claude/docs/SPEC.md, .claude/]
  writes: [.claude/state/PLAN.md]
  authors: [votrinhan88]
  version: 0.1
---

# Planning Plan
Draft PLAN.md from all planning outputs, validate it, gather role feedback, and iterate with the user until approved.

## Prerequisites

Agents, skills, hooks, and rules must be defined (outputs from `planning-agents`, `planning-skills`, `planning-rules`). The plan assigns phases to agents and validates against hooks/rules.

## Steps

1. **Verify prerequisites** — check that `.claude/agents/`, `.claude/skills/`, `.claude/hooks/`, and `.claude/rules/` directories exist with content; if any missing, stop and prompt user to complete prior modules
2. **Draft** — produce `./.claude/state/PLAN.md`; each phase must have:
   - Clear, single deliverable
   - Appoint one role-agent and reviewing peer-agent(s) (from `.claude/agents/`)
   - Complexity estimate: low / med / high
   - Explicit dependencies on other phases
   - Validation step immediately after each implementation phase
   - No phase covers more than one deliverable without a checkpoint between them
3. **Identify parallelism** — for each phase pair, determine if they can run in parallel:
   - Does Phase A depend on Phase B's deliverable? (check "Depends on" field)
     - Yes → mark sequential (Phase A waits for Phase B)
     - No → proceed to next check
   - Does Phase B depend on Phase A's deliverable?
     - Yes → mark sequential (Phase B waits for Phase A)
     - No → mark parallelizable
   - Annotate in PLAN.md with `[Parallelizable: yes | no]` for each phase
4. **Pre-flight** — self-validate before presenting:
   - Every phase is verifiable (clear pass/fail criterion)
   - Every phase owned by exactly one role and has at least one reviewer
   - Inter-phase dependencies are explicit
   - Every implementation phase has a validation step after it
   - Fix any violations before proceeding
5. **Role review** — for each role in `.claude/agents/`, review PLAN.md from that role's perspective against their Definition of Done and Priorities (from agent file); surface findings ranked by severity; revise PLAN.md to address them
6. **Present + iterate** — present PLAN.md to user; user annotates with `> NOTE:` inline; address all notes; repeat until user confirms no further notes remain

## Examples
- **Input:** SPEC.md, agent files, skill files, hook/rule files all finalized
- **Output:** PLAN.md with phases, owners, complexity, dependencies, validation steps, parallelism annotations. All role feedback incorporated. User-approved.

## Edge Cases
- **Phase has no clear deliverable:** stop; surface to user; do not proceed until resolved
- **Role review surfaces conflicting feedback:** present conflict to user; do not auto-resolve
- **NOTE loop stalls (3+ iterations):** If user provides conflicting feedback across three or more iterations (e.g., wants both "reduce scope" and "add more features"), surface the conflict explicitly; ask user to clarify priority; do not proceed to State init until conflict is resolved
- **No agents defined:** skip role review; note absence in plan header

## Resources

### PLAN.md structure

```markdown
# Plan

## Phase 1: One-liner description
- **Owner:** [role-agent]
- **Reviewer:** [peer-agents]
- **Deliverable:**
- **Complexity:** [low | med | high]
- **Depends on:** [none | Phases ...]
- **Parallelizable:** [yes | no]
- **Definition of Done:**

## Phase 2: ...
```

### Output schema
```yaml
planning_plan:
  phases:
    - name: string
      owner: string
      complexity: low | med | high
  user_confirmed: true | false
```