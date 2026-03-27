---
name: bootstrap
description: User-only. Entry point for Bootstrap session — initialize project infrastructure, establish Claude config, and capture initial spec.
user-invocable: true
metadata:
  reads: all
  writes: [.claude/, CLAUDE.md]
  authors: [votrinhan88]
  version: 0.1
---

# Bootstrap
Entry point for Bootstrap session (infrastructure + spec). Initialize git, audit existing work (if any), establish Claude infrastructure, and gather project spec.

## Steps

**New project:**
1. **Git setup** — run `bootstrap-git`
2. **Interview** — run `bootstrap-interview` to gather project context and preferences
3. **Scaffold** — run `bootstrap-scaffold` to create `.claude/` directory structure
4. **State init** — run `bootstrap-state` to initialize CONTEXT.md, log templates, and state system
5. **Gate** — run Bootstrap session gate per `sessions/session-bootstrap.md` Step 7

**Existing project:**
1. **Audit** — run `bootstrap-audit` to assess current state
2. **Git setup** — run `bootstrap-git`
3. **Mode choice** — run `bootstrap-integration` to determine integration mode
4. **Interview** — run `bootstrap-interview`
5. **Scaffold** — run `bootstrap-scaffold`
6. **State init** — run `bootstrap-state`
7. **Gate** — run Bootstrap session gate per `sessions/session-bootstrap.md` Step 7

## Examples

- **Input:** `/bootstrap` on a fresh directory with no git history
- **Output:** Runs new-project Bootstrap session flow → `.claude/` scaffolded, CONTEXT.md initialized, gate passed; ready for Planning session

- **Input:** `/bootstrap` on an existing project with a `.claude/` directory already present
- **Output:** Runs audit → git setup → integration mode selected → interview fills gaps → scaffold merges → gate passed

## Edge Cases

- No git initialized: `bootstrap-git` handles setup; if user declines git, document in CONTEXT.md Constraints and skip git-dependent steps
- `.claude/` already exists: `bootstrap-audit` detects it; `bootstrap-integration` determines merge vs. reset vs. skip
- Bootstrap session gate fails: stop and surface blockers to user — do not proceed to Planning session until gate passes

## Resources

### Modules
- `bootstrap-git.md` — git configuration and workflow setup
- `bootstrap-audit.md` — assess existing project state (existing-project path)
- `bootstrap-integration.md` — determine integration mode (existing-project path)
- `bootstrap-interview.md` — gather project context and preferences
- `bootstrap-scaffold.md` — create `.claude/` directory structure
- `bootstrap-state.md` — initialize CONTEXT.md, state system, and log templates

### References
- `discipline.md` — complete Bootstrap phase step-by-step execution guide

### Next Phase
See `/planning` skill for Planning phase (agent definition and execution planning).
