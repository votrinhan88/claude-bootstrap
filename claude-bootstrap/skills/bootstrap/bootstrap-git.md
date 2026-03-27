# Bootstrap Git

Git setup during Bootstrap session. Runs before interview and scaffold. Handles repo init/audit, branching strategy, and tracking configuration.

---

## Step 1: Repo Status

> **Agent:** Check if a git repo already exists.

```bash
git rev-parse --git-dir
```

**If yes (existing repo):** Skip to Audit.
**If no (new project):** Proceed to Init.

---

## Init (new repo)

> **Agent:** Initialize and prepare for bootstrap.

1. Run `git init`
2. Create `.gitignore` appropriate for the project stack (infer from interview or ask)
3. Commit initial state:

```bash
git init
git add .
git commit -m "chore: initial scaffold"
```

---

## Audit (existing repo)

> **Agent:** Review the current state. Present findings to user before making changes.

- Current branch, uncommitted changes, active branches
- Existing `.gitignore` — propose additions if gaps exist
- Recent commit history (last 5–10 commits) to understand workflow
- Do not alter history or branches without user confirmation

---

## Decision Tree

### Question 1: "Do you want to use git for this project?"

- **Yes** (default): Proceed to Question 2
- **No**: Skip all git setup. No commits, branches, or PRs. Session ends.

### Question 2: "How much of Claude's project infrastructure should be version-controlled?"

1. **Track nothing** (Claude infra private)
   - `.gitignore`: `CLAUDE.md`, `.claude/` entirely
   - Use if: You want Claude setup private or disposable

2. **Track knowledge** (recommended)
   - `.gitignore`: `.claude/state/` only
   - Use if: You want spec/roles/skills versioned but not session logs

3. **Track everything** (full auditability)
   - `.gitignore`: `.claude/state/sessions/` only
   - Use if: You want complete audit trail
   - **Warning:** Session logs may contain sensitive content mentioned during interviews (credentials, internal context). Only choose this if the repo is private.

> **Agent:** Apply the user's choice. Update `.gitignore` accordingly.

---

## Branching Mode

> **Agent:** Ask the user which branching mode to use. Default to branch-per-phase. Record the choice in `CLAUDE.md` under "Git" section.

| Mode            | Description                                           | When to use |
| --------------- | ----------------------------------------------------- | ----------- |
| **Branch-per-phase** | One branch per plan phase, squash-merge at gate | Default — most projects |
| **Branch-per-task** | One branch per task within a phase | Higher granularity needed (many small tasks) |
| **Trunk-based**     | All commits to main, no phase branches | Solo fast iteration, no phase isolation |

---

## Commit Protocol

> **Agent:** Record this in `.claude/CLAUDE.md` under "Git" section. Agents follow this during implementation.

**Commit message format:**

```
<type>(<scope>): <description>

Types: feat, fix, refactor, test, docs, chore
Scope: module or area affected
Description: what changed and why (1-2 sentences)
```

**Who can do what:**

| Who                              | Can do                                                 | Cannot do |
| -------------------------------- | ------------------------------------------------------ | --------- |
| Role-agent (during task)         | Commit to active phase branch with descriptive message | Commit to main, force push, delete main |
| Orchestrator (at phase boundary) | Squash-merge phase branch to main, create next phase, tag milestones | — |
| User (any time)                  | Anything | — |

---

## Remote (optional)

> **Agent:** Ask if the user wants to configure a remote. If yes, confirm the URL before running. Do not store the URL anywhere in tracked config files — it lives in `.git/config` only. Do not push without explicit confirmation.

```bash
git remote add origin <url>
git push -u origin main
```

If no remote is needed, skip.

---

## Output

> **Agent:** Write the following dict to `.claude/config.yml` under a `git:` key after completing all steps.

```yaml
git:
  git_exists: true | false
  use_git: true | false
  track_claude: none | knowledge | full
  branch_mode_observed: null | branch-per-phase | branch-per-task | trunk-based
  branch_mode: branch-per-phase | branch-per-task | trunk-based
  commit_format: conventional | standard | custom
  remote: null | true
```

- `branch_mode_observed`: populated only for existing repos (what was in place before bootstrap)
- `remote`: `true` if a remote is configured — URL is intentionally omitted (retrieve with `git remote get-url origin`)

---

## Outcome

Git is initialized (or audited), `.gitignore` is configured, branching mode is chosen, commit protocol is documented in CLAUDE.md, and decisions are recorded in `.claude/config.yml` under `git:`.
