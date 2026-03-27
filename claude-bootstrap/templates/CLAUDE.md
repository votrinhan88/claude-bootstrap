The only Claude file at root. Loads every session automatically.

```markdown
# [Project Name]

[One-line description]

## Stack

[Language, framework, key deps]

## Commands

- Build: `[cmd]`
- Test: `[cmd]`
- Lint: `[cmd]`

## Git (include only if git enabled)

- Branch: claude/[phase-name] per plan phase
- Commit: <type>(<scope>): <description>
- Never commit directly to main
- Squash-merge at phase boundaries
- IMPORTANT: Never force push or delete main

## Rules

- [Prevents the most common mistake]
- [Prevents the second most common mistake]
- IMPORTANT: [The one rule that must never be broken]

## Session Protocol

- Start: read .claude/CONTEXT.md, verify
  state against actual project (git log, tests, files). Reconcile.
  Read last 2-3 entries in .claude/state/logs/ if needed.
- End: curate CONTEXT.md (remove stale entries),
  append new entry to .claude/state/logs/
- Log entries: .claude/state/logs/{date}_{seq}_{type}.md
- On compaction: preserve modified file list, current plan step,
  unresolved blockers
- Operational signals: see `.claude/skills/bootstrap-runtime.md`
```

**Budget:** ~50 of ~150-200 instruction slots consumed by system prompt. Every line competes. Move domain knowledge to `.claude/skills/`, enforcement to `.claude/hooks/`.