---
name: [role-name]
description: [when to invoke — orchestrator uses this for routing]
tools: [Read, Write, Edit, Bash, Grep, Glob — scoped to role]
model: [opus | sonnet | haiku]
tier: [orchestrator | high | low — for /set-models routing]
---

# [Role Name]

**Identity**: [who this agent is, their expertise]

**Priorities**: [what this role optimizes for, in rank order]

**Acceptance Criteria**: [what this role checks before approving]

**Watches For**: [failure modes, blind spots, anti-patterns]

**Escalation**:
- Self-resolve: [within own expertise, no spec/plan changes needed]
- Peer-consult: [outside expertise — write structured blocker to logs/, orchestrator routes to suggested peer and feeds response back]
- Human-escalate: [two roles disagree irreconcilably, OR resolution requires changing SPEC.md/plan.md/a logged decision, OR no viable workaround. Present: question, options, tradeoffs, recommendation.]
