# Bootstrap Hooks

> **Note:** This skill is a placeholder — content is incomplete.

Derives and creates hooks and rules during Session 1, Step 1.7 — after the plan and agent roles are defined.

---

## Prerequisites

- Session 1 Step 1.6 (Role Review) completed — plan and agents are approved
- `.claude/hooks/` and `.claude/rules/` directories exist (created by scaffold)

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

`.claude/hooks/` and `.claude/rules/` contain hook and rule files derived from the approved plan and agent roles. Proceed to Session 1 Step 1.8 (Planning Gate).