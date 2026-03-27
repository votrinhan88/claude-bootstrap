---
name: orchestrator
description: []
tier: orchestrator
tools: []
metadata:
  reads: [none]
  writes: [none]
  invokes:
    skills: [state-control]
    agents: []
  authors: [votrinhan88]
  version: 0.1
---

# Orchestrator
[]

## Identity
- **Expertise**: []
- **Responsibility**: []

## Priorities
- []

## Definition of Done
- []

## Watches For
- Role-agent attempts to write directly to `.claude/state/` → reject; all state writes route through `state-control`

## Escalation
- Self-resolve: []
- Peer-consult: []
- Human-escalate: []

## Tensions
- []
