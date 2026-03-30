---
name: bootstrap-integration
description: Agent-only. Choose and execute integration mode when an existing `.claude/` directory is present. Existing-project path only; runs after bootstrap-git.
user-invocable: false
metadata:
  reads: [.claude/]
  writes: [.claude/]
  authors: [votrinhan88]
  version: 0.1
---

# Bootstrap Integration
Choose how to integrate the Bootstrap framework with an existing `.claude/` directory.

## Steps
1. **Review audit findings** — read `bootstrap_audit` output; note existing agents, skills, hooks, rules, state files
2. **Present modes** — show the three options; ask "Which mode fits your situation?"
   - **Extend** (default) — add framework alongside existing config; nothing touched or overwritten
   - **Migrate** — restructure existing config into framework layout; agent proposes plan first, user approves before any change
   - **Replace** — archive existing `.claude/` to `.claude/.archive-{timestamp}/`; Planning session will create fresh infrastructure
3. **Execute mode workflow:**
   - *Extend:* record mode; no file changes at this step
   - *Migrate:* read `.claude/` recursively; draft mapping (existing files → target paths, conflicts, what framework adds); present plan; wait for user approval; backup to `.claude/.backup-{timestamp}/`; execute
   - *Replace:* confirm with user ("This will archive your existing `.claude/`. Ready?"); do not proceed until confirmed; archive existing `.claude/`; Planning session will create fresh infrastructure
4. **Confirm** — summarize what was done; confirm with user
5. **Return decisions** — pass `bootstrap_integration` output to bootstrap skill

## Examples
- **Input:** Existing project, one stale agent file, user chooses Replace
- **Output:** `.claude/` archived to `.claude/.archive-2026-03-30T14-00/`; Planning session will create fresh infrastructure; `mode: replace`
- **Input:** Existing project with structured agents and skills, user chooses Migrate
- **Output:** Backup created; existing files remapped to framework layout; `mode: migrate`
- **Input:** Existing project, user unsure, chooses Extend
- **Output:** No changes; Planning session will add framework files alongside existing config; `mode: extend`

## Edge Cases
- **`.claude/` exists but empty:** treat as new project; skip integration; `mode: skip`
- **User declines all modes:** stop; do not proceed until resolved — this step cannot be skipped
- **Migrate conflicts:** surface each with a recommendation; do not auto-resolve; ask user per conflict
- **Backup directory collision:** append `-2` suffix; never overwrite a prior backup

## Resources

### Output schema
```yaml
bootstrap_integration:
  mode: extend | migrate | replace | skip
  existing_files_found: [list of paths]
  backup_path: null | string        # Migrate and Replace only
  migration_plan: null | object     # Migrate only: moved[], conflicts[], added[]
  user_confirmed: true | false
```
