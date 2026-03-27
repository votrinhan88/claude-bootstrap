# Bootstrap State

> **Note:** This skill is a placeholder — content is incomplete.

Initializes `.claude/state/` directory and CONTEXT.md during Session 0a, Step 5 (after agent definition).

---

## Prerequisites

- Interview handoff completed and confirmed
- Project scaffold structure exists
- Agent definitions complete with tension map confirmed

---

## Initialize State Directory

> **Agent:** Create `.claude/state/` directory structure with initial files.

### Directory structure

```
.claude/state/
├── CONTEXT.md          # Working memory (curated each session)
└── logs/               # Append-only log entries (one file per entry)
```

### CONTEXT.md — Initial Content

Create from interview handoff. Schema:

```markdown
# Context

## Summary

[One sentence: core challenge or opportunity right now]

## Active Decisions

- [decision]: [rationale, date]

## Active Concerns

- [concern]: [raised by role/issue, status]

## Constraints Discovered

- [constraint]: [when found, impact]
```

Populate from interview and agent definitions:
- **Summary**: One-line project purpose or current phase goal
- **Active Decisions**: Git tracking choice, branching mode, delivery target (from interview), any role-specific priority trade-offs (from agent tension map)
- **Active Concerns**: Any low-confidence areas flagged in interview handoff or discovered during agent definition
- **Constraints Discovered**: Hard limits from interview (platform, deadline, compliance, etc.), and role-specific constraints (e.g., "architect must approve any breaking changes")

### logs/ Directory

Create empty. First entry (interview handoff) already written by bootstrap-interview skill in Step 2.

---

## Log Entry System

See `claude-bootstrap/skills/bootstrap-template-logs.md` for full schema and templates.

**In brief:**
- Filename: `.claude/state/logs/{date}_{seq}_{type}.md`
- Frontmatter: date, seq, type, author, optional (parent, scope, tokens_in, tokens_out, duration, model)
- Entry types: handoff, decision, issue, review
- Append-only — never edit old entries
- Read last 2–3 entries at session start to understand project state

---

## Outcome

`.claude/state/` is initialized with CONTEXT.md populated and logs/ directory ready for entries.