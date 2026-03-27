# Gates, Blockers & Conflict Resolution

---

## Phase Gate: Multi-Perspective Review

After each plan phase:

> **Agent:** For each role in `.claude/agents/`, spawn a parallel subagent to review the completed phase against their acceptance criteria. Return findings ranked by severity.
>
> Verdict: **Ready to Proceed** / **Needs Attention** / **Needs Rework**
>
> For critical phases, use a different model for review than implementation.

Include in every phase gate:

> **Agent:** Compare current work against `.claude/docs/SPEC.md`. Flag deviations. If intentional, log to `.claude/state/CONTEXT.md` and `.claude/state/logs/`. If accidental, add to `.claude/state/CONTEXT.md` under Blocked.

---

## Conflict Resolution

When two role-agents disagree, read each agent's `**Tensions**` section to decide:

| Tension type | Orchestrator action |
|---|---|
| Known productive tension (in agent file) | Let both roles make their case — surface trade-off to user at next gate |
| Known tension, clear winner per context | Apply the context rule from the agent's tension entry; log the decision |
| Unknown tension (not in any agent file) | Escalate to user, add tension to relevant agent files if a new pattern is identified |
| Irreconcilable disagreement | Human-escalate immediately regardless of autonomy mode |

> Agent tension entries are not tiebreakers — they're a map. Use them to recognize *expected* friction and keep it productive rather than treating every disagreement as a blocker.
