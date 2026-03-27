# Orchestrate

> **Note:** This skill is a placeholder — content is incomplete.

Entry point for Session 2+ implementation phases. Manages phase routing, execution, gates, and team scaling.

---

## Autonomy Modes

> **Agent:** Run with one of these modes:

### `/orchestrate` (manual)

Each task waits for user confirmation before proceeding. Full visibility and control. Slowest.

### `/orchestrate --auto` (phase gates)

Auto-execute tasks within a phase. User reviews and confirms at phase boundaries. Balanced.

### `/orchestrate --auto --skip-gates` (blockers only)

Auto-execute through all phases. User reviews only on blockers or escalations. Fastest.

---

## Phase Loop

1. **Read state** — CONTEXT.md, plan.md, last log entry
2. **Route task** — orchestrator selects next task per plan
3. **Invoke role** — spawn role-agent with task scope
4. **Capture result** — log entry written, CONTEXT.md updated
5. **Check gate** — if phase complete, pause for review (unless --auto or --skip-gates)
6. **Merge & tag** — if git enabled, squash-merge phase branch to main
7. **Repeat** — next phase

---

## Scaling Patterns

### Single Session

Default. All work in main session. Cost: 1x.

### Parallel Worktrees

Independent tasks in separate git worktrees:

```bash
git worktree add ../project-api feature/api-layer
git worktree add ../project-tests feature/test-coverage
```

Useful for parallel features. Cost: 3x+ (overhead per worktree).

### Subagent Delegation

Route peer-consult or specialized tasks to subagents instead of same agent. Cost: 1.5–2x.

### Agent Teams (experimental)

Back-and-forth multi-agent collaboration on complex tasks:

```bash
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

Cost: 2.5–3x + coordination overhead.

---

## Choosing a Pattern

| Situation | Pattern | Why |
|-----------|---------|-----|
| Solo, tight budget | Single session, manual | Low cost, full control |
| Solo, fast iteration | Single session, --auto | Balanced |
| Multi-developer | Parallel worktrees | True parallelism |
| Complex coordination needed | Agent teams | Deep back-and-forth |
| Peer review bottleneck | Subagent delegation | Parallel consults |

---

## Session End

At phase boundary or escalation:
1. Log phase summary to `.claude/state/logs/`
2. Update CONTEXT.md
3. Close session or proceed to next phase per mode