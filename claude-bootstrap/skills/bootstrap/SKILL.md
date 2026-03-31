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

1. **Audit** — run `bootstrap-audit` to assess project state (handles new and existing projects)
2. **Git setup** — run `bootstrap-git` to initialize and configure git
3. **Integration** — run `bootstrap-integration` to determine integration mode (skipped for new projects)
4. **Wrap up** — summarize findings and next steps; prompt user to run `/planning`

## Examples
- **Input:** `/bootstrap` on a fresh/empty directory
- **Output:** Audit reports new project → git initialized → wrap up; user prompted to run `/planning`
- **Input:** `/bootstrap` on an existing project with `.claude/` directory
- **Output:** Audit detects existing project → integration mode determined → git configured; user prompted to run `/planning`

## Edge Cases

- **Empty project (Audit finds no tools/conventions):** Report as new; skip integration step; proceed to git setup and wrap up
- **Git setup fails or user declines:** Surface blocker at Wrap up; do not prompt for `/planning` until resolved
- **`.claude/` already exists:** `bootstrap-audit` detects it; `bootstrap-integration` determines whether to extend, migrate, or replace

## Resources

### Modules
- `bootstrap-audit.md` — assess project state (new and existing projects)
- `bootstrap-git.md` — git configuration and workflow setup
- `bootstrap-integration.md` — determine integration mode for existing projects

### Next Phase
See `/planning` skill for Planning phase (interview, agent definition, execution planning).
