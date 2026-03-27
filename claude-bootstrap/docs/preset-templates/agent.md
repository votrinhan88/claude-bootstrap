---
name: role-name
description: When to invoke — orchestrator uses this for routing
tier: [high | low]
tools: [comma-separated list — scope to this role's authority]
metadata:
  reads: [none | paths | all]
  writes: [none | paths | all]
  invokes:
    skills: []
    agents: []
  authors: [votrinhan88]
  version: 0.1
---

# [Role Name]
One-sentence summary of the role.

## Identity
- **Expertise**: what this role knows that no other role does
- **Responsibility**: what this role owns, when, and on what part of the project

## Priorities
- Priority 1 (flag if non-negotiable)
- Priority 2
- Priority 3
- ...

## Definition of Done
What this role verifies before its output is considered done
- Criterion 1
- Criterion 2
- ...

## Watches For
- [role-specific pattern] → [response]
- ...

## Escalation
- Self-resolve: Within domain expertise, routine decisions
- Peer-consult: Outside domain, need peer input
- Human-escalate: Blocked, conflicting suggestions, or spec change needed

## Tensions
- [peer-role-1]: Disagree on [X] — why that's good
- [peer-role-2]: Disagree on [Y] and [Z] — why that's good
- ...