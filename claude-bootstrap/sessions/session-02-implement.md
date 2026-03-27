# Session 2+ — Implementation

> **Note:** This session is a placeholder — content is incomplete.

Execute plan phases with orchestrated role agents, intervene at phases and blockers, manage state and logs.

**Prerequisites:** Session 1 completed — `.claude/state/PLAN.md` exists and is approved.

> **Agent:** Fresh session. Normal Mode. Read `.claude/state/CONTEXT.md` first. Verify state against actual project (git log, file system, outputs/artifacts). Reconcile any discrepancies before proceeding.

## Autonomy Modes

| Command                            | Pauses at                   | Best for                                  |
| ---------------------------------- | --------------------------- | ----------------------------------------- |
| `/orchestrate`                     | Every task completion       | Unfamiliar territory, early phases        |
| `/orchestrate --auto`              | Phase boundaries + blockers | Trusted plan, mid-project                 |
| `/orchestrate --auto --skip-gates` | Blockers only               | Well-validated plan, late mechanical work |

At any autonomy level: **Esc always interrupts immediately**, and **Tier 2 escalations always pause for the human**.

## Model Tier Switching

`/set-models` controls which models power each agent. Agents are grouped into three tiers:

| Tier             | Use for                                             | Examples                                    |
| ---------------- | --------------------------------------------------- | ------------------------------------------- |
| **Orchestrator** | Always the orchestrator                             | routing, state management, escalation       |
| **High**         | Roles requiring deep reasoning, nuance, or judgment | analysis, strategy, review, writing, theory |
| **Low**          | Roles doing structured, well-scoped execution       | implementation, formatting, data processing |

| Preset    | Orchestrator | High   | Low    |
| --------- | ------------ | ------ | ------ |
| `max`     | opus         | opus   | sonnet |
| `default` | opus         | sonnet | haiku  |
| `min`     | sonnet       | haiku  | haiku  |

> **Note:** Model names above are defaults — check current Claude model IDs when configuring.

Usage: `/set-models <preset>` or `/set-models <orchestrator> <high> <low>`

## The Orchestration Loop

```
1. Orchestrator selects next task + owning role
2. Spawns role-agent as subagent with task scope
3. Agent executes → returns summary
4. If next task has same owning role and no phase boundary
   → same-role continuation (skip orchestrator re-dispatch)
5. Otherwise → curate CONTEXT.md,
   append to .claude/state/logs/, present summary
6. At phase boundary → run phase gate review
```

## Intervention Points

At every stage of the loop, you have fine-grained control:

**1. Task Selection** (orchestrator picks next task)

| Action        | How                           |
| ------------- | ----------------------------- |
| Accept        | Proceed silently or say "go"  |
| Redirect      | "Do [X] instead"              |
| Reorder       | "Move [X] before [Y]"         |
| Insert        | "Before that, do [X]"         |
| Skip          | "Skip, move to next"          |
| Change owner  | "Have [role] do this instead" |
| Change effort | `/effort high` for this phase |

**2. During Execution** (role-agent working)

| Action        | How                                       |
| ------------- | ----------------------------------------- |
| Watch         | Let it run                                |
| Interrupt     | Esc — stops mid-task, review partial work |
| Status check  | `/btw where are you on this?`             |
| Adjust effort | `/effort` or `ultrathink`                 |

**3. Task Completion** (agent returns summary)

| Action        | How                                         |
| ------------- | ------------------------------------------- |
| Accept        | "Continue"                                  |
| Reject        | "Redo this, because [reason]"               |
| Annotate      | "Close, but fix [specific issues]"          |
| Peer review   | "Have [role] review this before continuing" |
| Redirect next | Override orchestrator's next pick           |
| Pause         | "Stop here, I need to think"                |

**4. Phase Boundary** (all tasks in phase complete)

| Action                | How                                     |
| --------------------- | --------------------------------------- |
| Phase gate review     | Multi-perspective role review (default) |
| Skip review           | "I trust this, continue"                |
| Adversarial pass      | "Stress-test this phase"                |
| Adjust plan           | Edit `.claude/state/PLAN.md` before continuing          |
| Adjust roles          | "Add [X] perspective" / "Retire [Y]"    |
| Trigger retrospective | Full health check + role adequacy       |
| End session           | Handoff and fresh start                 |

**5. Blocker Escalation** (agent stuck, escalated to you)

| Action                 | How                                  |
| ---------------------- | ------------------------------------ |
| Resolve directly       | "The answer is [X], proceed"         |
| Suggest different peer | "Ask [role] instead"                 |
| Override               | "Going with [X], log it"             |
| Defer                  | "Park this, move to next task"       |
| Change scope           | "Remove this from spec"              |
| Trigger pivot          | "This reveals a fundamental problem" |

**6. Retrospective** (periodic or `/health`)

| Action          | How                                            |
| --------------- | ---------------------------------------------- |
| Continue as-is  | "Looks good, proceed"                          |
| Adjust roles    | Add, merge, retire, redefine                   |
| Revise spec     | Update `.claude/docs/SPEC.md`                  |
| Re-plan         | Rewrite upcoming phases                        |
| Pivot           | Fundamental course correction                  |
| Change autonomy | Switch between manual / auto / auto-skip-gates |

## When Agents Get Stuck

```markdown
# Blocked

## [task-id] — [brief description]

- **Agent**: [role]
- **Problem**: [what's stuck]
- **Tried**: [what was attempted]
- **Needs**: [suggested peer role]
- **Recommendation**: [best guess]
```

> **Orchestrator:**
>
> 1. Read the blocker
> 2. Spawn the suggested peer agent with blocker context
> 3. Feed peer's response back to original agent
> 4. If unresolved or contradictory: escalate to human with both perspectives

## Phase Gate: Multi-Perspective Review

After each plan phase:

> **Agent:** For each role in .claude/agents/, spawn a parallel subagent to review the completed phase against their acceptance criteria. Return findings ranked by severity.
>
> Verdict: **Ready to Proceed** / **Needs Attention** / **Needs Rework**
>
> For critical phases, use a different model for review than implementation.

## Spec Drift Check

Include in every phase gate:

> **Agent:** Compare current implementation against `.claude/docs/SPEC.md`. Flag deviations. If intentional, log to `.claude/state/CONTEXT.md` and `.claude/state/logs/`. If accidental, add to `.claude/state/CONTEXT.md` under Blocked section.

## Periodic Retrospective

> **Agent:** Periodically — every 3 completed plan phases by default, or when user runs `/health`:
>
> - Are the current roles still right? Add, merge, retire?
> - Has the spec drifted enough to warrant revision?
> - Any roles consistently not creating useful tension?
> - Anything learned that should change the remaining plan?
>
> Present findings. This is a checkpoint, not a blocker — user decides whether to act.

## Pivot Protocol

When a Tier 2 escalation reveals a fundamental problem — wrong approach, wrong scope, spec error.

> **Agent:**
>
> 1. Freeze implementation
> 2. Log the pivot trigger to .claude/state/logs/ with full context
> 3. Update `.claude/docs/SPEC.md` with the new understanding
> 4. Re-evaluate roles: still valid?
> 5. Re-plan from current state (preserve completed work)
> 6. Update `.claude/state/CONTEXT.md` and logs
> 7. Present the pivot to user for confirmation before resuming
