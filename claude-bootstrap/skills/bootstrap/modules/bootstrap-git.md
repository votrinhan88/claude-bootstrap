---
name: bootstrap-git
description: Agent-only. Git setup during Bootstrap session — repo init/audit, branching strategy, tracking configuration, and commit protocol. Runs after bootstrap-audit.
user-invocable: false
metadata:
  reads: [.git/, .gitignore, CLAUDE.md]
  writes: none
  authors: [votrinhan88]
  version: 0.1
---

# Bootstrap Git
Git setup during Bootstrap session. Runs after audit and before interview. Handles repo init/audit, branching strategy, and tracking configuration.

## Steps
1. **Check repo status** — run `git rev-parse --git-dir`
   - If repo exists: proceed to Audit
   - If no repo: proceed to Ask Git
2. **Audit (existing repo)** — if repo exists
   - Present current branch, uncommitted changes, active branches
   - Review existing `.gitignore` and propose additions if gaps exist
   - Show recent commit history (last 5–10 commits)
   - Do not alter history or branches without user confirmation
3. **Ask: Do you want to use git for this project?**
   - **Yes** (default): proceed to Step 4
   - **No**: skip all git setup; document in CONTEXT.md, proceed to interview
4. **Init (new repo)** — if repo doesn't exist AND user answered Yes
   - Run `git init`
   - Create `.gitignore` appropriate for project tools/environment (from bootstrap-audit findings)
   - Commit: `git commit -m "chore: initial scaffold"`
5. **Choose tracking mode** — "How much Claude infrastructure should be version-controlled?"
   - **Track nothing**: `.gitignore: CLAUDE.md, .claude/` (use if: Claude setup is private/disposable)
   - **Track knowledge** (recommended): `.gitignore: .claude/state/` only (use if: spec/roles/skills versioned, not session logs)
   - **Track everything**: `.gitignore: .claude/state/sessions/` only (use if: full audit trail needed; **warning** — repo must be private, logs may contain sensitive content)
   - Apply user's choice to `.gitignore`
6. **Choose branching mode**
   - **Branch-per-phase** (default): one branch per plan phase, squash-merge at gate
   - **Branch-per-task**: one branch per task within a phase
   - **Trunk-based**: all commits to main, no phase branches
7. **Confirm commit protocol**
   - Format: `<type>(<scope>): <description>` (types: feat, fix, refactor, test, docs, chore)
   - Permissions: role-agents can commit to phase branch; orchestrator squash-merges; user can do anything
8. **Configure remote** (optional) — ask if user wants a remote
   - If yes: confirm URL, run `git remote add origin <url>` and `git push -u origin main`
   - If no: skip
9. **Return decisions** — report all git decisions back to the main bootstrap skill (use_git, track_claude, branch_mode, remote, commit_format)

## Examples

**New project, track knowledge, branch-per-phase:**
```yaml
bootstrap_git:
  git_exists: false
  use_git: true
  track_claude: knowledge
  branch_mode_observed: null
  branch_mode: branch-per-phase
  commit_format: conventional
  remote: false
```

**Existing repo, track everything, trunk-based:**
```yaml
bootstrap_git:
  git_exists: true
  use_git: true
  track_claude: full
  branch_mode_observed: branch-per-feature
  branch_mode: trunk-based
  commit_format: conventional
  remote: true
```

**User declines git:**
```yaml
bootstrap_git:
  git_exists: false
  use_git: false
  track_claude: null
  branch_mode: null
  commit_format: null
  remote: null
```

## Edge Cases
- **No git initialized:** Step 2 initializes it; if user declines git at Step 4, skip all git operations and record `use_git: false`
- **Existing repo with uncommitted changes:** Audit (Step 3) surfaces this; do not commit without user confirmation
- **Existing `.gitignore` conflicts:** Audit proposes additions; merge user's choice with existing rules
- **Remote setup fails:** Catch error, surface to user, allow retry or skip; record `remote: false` if skipped
- **User changes tracking mode mid-session:** `.gitignore` may need refresh; re-apply user's final choice before returning decisions

## Resources

### bootstrap-git-schema

Return all decisions to bootstrap skill as a structured dict:
```yaml
bootstrap_git:
  git_exists: true | false                          # was repo present before bootstrap?
  use_git: true | false                             # user elected to use git
  track_claude: none | knowledge | full             # what to version-control
  branch_mode_observed: null | branch-per-phase | branch-per-task | trunk-based  # existing mode (existing repos only)
  branch_mode: branch-per-phase | branch-per-task | trunk-based  # elected mode
  commit_format: conventional | standard | custom   # commit message style
  remote: null | true                               # remote configured? (URL in .git/config only)
```

The bootstrap skill collects these and threads them to `bootstrap-interview` for incorporation into the handoff log.
