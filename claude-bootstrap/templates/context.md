### context.md — Session State (curated)

Read every session start. Curated every session end — remove stale entries (preserved in logs/).
Updated every task completion. Orchestrator's primary routing input.

```markdown
# Context

## Status
[1-2 sentences: overall project state]

### Completed
- [task]: [result]

### In Progress
- [task]: [state, owner]

### Blocked
- **[task-id]**: [problem] — tried [X], needs [role], recommends [Y]

### Next
- [task]: [owner, complexity]

## Decisions
- [decision]: [rationale, date]

## Open Issues
- [issue]: [raised by, impact, status]
```
