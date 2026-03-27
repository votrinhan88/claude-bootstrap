# Bootstrap Hooks

> **Note:** This skill is a placeholder — content is incomplete.

Offloaded from playbook Hooks section. Creates hook placeholders in `.claude/hooks/` during Session 0a, Step 3 (Scaffold).

> **NOTE:** Hook implementations are detailed and filled in Session 1. This skill only scaffolds the structure and purpose definitions.

---

## Prerequisites

- Project scaffold structure exists (`.claude/hooks/` directory)

---

## Create Hook Placeholders

> **Agent:** Create one placeholder file per hook type. Frontmatter documents purpose; body is a placeholder for implementation.

### Hook Types

| Hook | Purpose | When it runs |
|------|---------|--------------|
| `auto-format.md` | Auto-format after writes | PostToolUse (after Read, Write, Edit) |
| `guard.md` | Block destructive operations | PreToolUse (before Bash, Edit, Glob, etc.) |
| `commit.md` | Validate commit messages | PostToolUse (after Bash git commit) |
| `stop.md` | End-of-turn verification | Stop (before agent turn ends) |

### Placeholder Structure

Each file:

```markdown
---
name: [hook-name]
type: [PreToolUse | PostToolUse | Stop]
trigger: [when it runs]
purpose: [what it does]
---

# [Hook Name]

[Placeholder: implementation filled in Session 1]
```

---

## Outcome

`.claude/hooks/` contains placeholder files for auto-format, guard, commit, and stop hooks. Bodies left empty for Session 1 population.

---

## Next

Session 1 fills in hook implementations based on project stack and role needs.