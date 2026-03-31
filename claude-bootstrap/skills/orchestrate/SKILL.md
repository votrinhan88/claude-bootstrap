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
2. **Execute task loop** — repeat until stop condition, controlled by `--auto` flag and gate/blocker checks
  a. Select next task from PLAN.md and identify owning role
  b. Spawn role-agent as subagent — pass invocation contract from `modules/contract.md` (task scope, read-only/write paths, escalation signal)
  c. Agent executes → returns structured report per `modules/contract.md` Report Template
  d. **Parse and validate agent report:**
     - Verify required fields: Task ID, Description, Acceptance criteria, Status, Summary, Changes
     - If status=blocked/escalated: confirm issue log path present; read and reference in CONTEXT.md Blocked
     - Update CONTEXT.md Tasks section (move to Completed or Blocked per status)
     - Append session log entry with agent summary (only orchestrator writes `.claude/state/`)
     - Present summary to user
  e. **Check stop conditions:**
     - If blocker or Tier 2 escalation → **hard stop**
     - Else if phase boundary → run phase gate (Step 3); if gate fails → **stop**; if gate passes and no `--auto` → **stop for user review**
     - Else if `--auto` → **loop to a** (select next task); else → **stop**
3. **Run phase gate** — at each phase boundary, before proceeding to next phase
  - See `modules/gates.md` for full gate and conflict resolution protocol
4. **Close session** — run the session end protocol using the `state-control` skill

## Examples

- **Input:** `/orchestrate` mid-project, task 3 of 5
- **Output:** Dispatches task 3 → presents summary → stops; re-invoke to continue
- **Input:** `/orchestrate --auto` at start of Phase 2, 4 tasks
- **Output:** Dispatches all tasks autonomously → stops at phase boundary for gate review

## Flags

- **`--auto`** (user-set, not agent-decided) — loop automatically through all remaining tasks in current phase (Step 2e); stop at phase boundaries for gate review or on blockers. Default: stop after each task.
- **`--skip-gates`** — skip phase gate execution (Step 3); proceed directly to next phase. Requires `--auto` for multi-task runs.

## Edge Cases

- PLAN.md does not exist: stop and prompt user — orchestrate requires an approved plan
- Tier 2 escalation mid-loop: stop immediately regardless of `--auto` flag
- Irreconcilable agent conflict at phase gate: human-escalate; see `modules/gates.md`
- Agent report missing required fields: log as Tier 2 escalation (agent contract violation); stop

## Multi-Task & Fresh Agent Model

- Agent executes **one task per spawn** — always returns structured report (Step 2c Report Template)
- Agent does not persist state between tasks — each spawn reads fresh CONTEXT.md
- Between tasks, orchestrator updates state (Step 2d) before spawning next agent
- Internal agent self-validation (tests, criteria checks, inline decisions) does not require resurfacing — only task completion triggers orchestrator involvement

## Resources

### `modules/`
- `contract.md` — invocation contract, report templates, permission enforcement
- `gates.md` — phase gate review, blockers, and conflict resolution
- `scaling.md` — worktrees, subagents, and agent teams

### `references/`
- `escalation.md` — escalation tier definitions (Tier 1: agent-resolvable; Tier 2: human decision required)
