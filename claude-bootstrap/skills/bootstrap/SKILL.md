---
name: bootstrap
description: User-only. Entry point for Bootstrap session — initialize git and assess existing state. Fully project-agnostic.
user-invocable: true
metadata:
  reads: all
  writes: none
  authors: [votrinhan88]
  version: 0.1
---

# Bootstrap
Entry point for Bootstrap session (git only). Initialize git and assess existing state. Fully project-agnostic — no files written, no spec collected.

## Steps

**New project:**
1. **Git setup** — run `bootstrap-git`
2. **Wrap up** — confirm git is initialized and configured; prompt user to run `/planning`

**Existing project:**
1. **Audit** — run `bootstrap-audit` to assess current state
2. **Git setup** — run `bootstrap-git`
3. **Integration** — run `bootstrap-integration` to determine integration mode
4. **Wrap up** — summarize git setup and integration mode; prompt user to run `/planning`

## Examples
- **Input:** `/bootstrap` on a fresh directory with no git history
- **Output:** Git initialized and configured; user prompted to run `/planning`
- **Input:** `/bootstrap` on an existing project with a `.claude/` directory already present
- **Output:** Audit complete → integration mode selected → git configured; user prompted to run `/planning`

## Edge Cases

- No git initialized: `bootstrap-git` handles setup; if user declines git, note in session and skip git-dependent steps
- `.claude/` already exists: `bootstrap-audit` detects it; `bootstrap-integration` determines integration mode
- Git setup fails or user declines: surface blocker at Wrap up; do not prompt for `/planning` until resolved

## Resources

### Modules
- `bootstrap-git.md` — git configuration and workflow setup
- `bootstrap-audit.md` — assess existing project state (existing-project path)
- `bootstrap-integration.md` — determine integration mode (existing-project path)

### Next Phase
See `/planning` skill for Planning phase (interview, agent definition, execution planning).
