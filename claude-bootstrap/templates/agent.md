---
name: [role-name]
description: [when to invoke — orchestrator uses this for routing]
model: [opus | sonnet | haiku]
tier: [orchestrator | high | low — for /set-models routing]
tools: [Read, Write, Edit, Bash, Grep, Glob — scoped to role]
skills: [skill files this agent may invoke]
---

# [Role Name]

**Identity**: [who this agent is, their expertise]

**Priorities**: [what this role optimizes for, in rank order]

**Acceptance Criteria**: [what this role checks before approving]

**Watches For**: [failure modes, blind spots, anti-patterns]

**Tensions**:
- [peer-role]: [what we disagree on] — [why that tension is productive]

**Escalation**:
- Self-resolve: [within own expertise, no spec/plan changes needed]
- Peer-consult: [outside expertise — write structured blocker to logs/, orchestrator routes to suggested peer and feeds response back]
- Human-escalate: [two roles disagree irreconcilably, OR resolution requires changing SPEC.md/PLAN.md/a logged decision, OR no viable workaround. Present: question, options, tradeoffs, recommendation.]
