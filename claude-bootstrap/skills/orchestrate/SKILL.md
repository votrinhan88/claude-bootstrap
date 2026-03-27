---
name: orchestrate
description: "User-only. Entry point for Runtime sessions — dispatch plan tasks to role-agents, run phase gates, and close sessions."
user-invocable: true
metadata:
  reads: all
  writes: [.claude/state/]
  invokes:
    skills: [state-control]
    agents: [role-agents]
  authors: [votrinhan88]
  version: 0.1
---

# Orchestrate
Dispatches plan tasks to role-agents, runs phase gates, and closes sessions.

## Steps

1. **Orient** — run the session start protocol using the `state-control` skill
2. **Execute the next task**
  a. Select next task from PLAN.md and identify owning role
  b. Spawn role-agent as subagent with task scope
  c. Agent executes → returns summary
  d. Advance task status in CONTEXT.md, append to `logs/`, present summary
  e. Hard stop check: if blocker or Tier 2 escalation → stop
  f. Gate stop check: else if phase boundary → run phase gate (Step 3); `--skip-gates` skips the gate; if `--auto` → loop to a, else stop
  g. Yield: else if `--auto` → loop to b (same role) or a (different role); else stop
3. **Run phase gate** — at each phase boundary, before proceeding
  - See `modules/gates.md` for full gate and conflict resolution protocol
4. **Close session** — run the session end protocol using the `state-control` skill

## Examples

- **Input:** `/orchestrate` mid-project, task 3 of 5
- **Output:** Dispatches task 3 → presents summary → stops; re-invoke to continue
- **Input:** `/orchestrate --auto` at start of Phase 2, 4 tasks
- **Output:** Dispatches all tasks autonomously → stops at phase boundary for gate review

## Edge Cases

- PLAN.md does not exist: stop and prompt user — orchestrate requires an approved plan
- Tier 2 escalation mid-loop: stop immediately regardless of autonomy flag
- Irreconcilable agent conflict: human-escalate immediately; see `modules/gates.md`

## Resources

### `modules/`
- `gates.md` — phase gate review, blockers, and conflict resolution
- `scaling.md` — worktrees, subagents, and agent teams

### `references/`
- `escalation.md` — escalation tier definitions (Tier 1: agent-resolvable; Tier 2: human decision required)
