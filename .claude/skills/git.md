# Git Skill

Actionable git workflow for agents. Follow these rules exactly. Do not improvise git operations outside this skill.

---

## When to Commit

- After completing a task and confirming it works (tests pass, no regressions)
- At phase gate — squash-merge into `main`
- Before switching to a different task or branch
- One concern per commit — do not batch unrelated changes

---

## Branching

**Default: branch-per-phase.**

Each plan phase gets its own branch off `main`. Work stays on that branch until the phase gate passes, then squash-merges into `main`.

```bash
# Start a phase
git checkout main && git pull
git checkout -b phase/<name>

# During the phase
git add <files>
git commit -m "<type>(<scope>): <desc>"

# At the phase gate
git checkout main
git merge --squash phase/<name>
git commit -m "<type>(<scope>): <phase summary>"
git branch -d phase/<name>
```

**Alternatives** (only if project config specifies):

| Mode            | Branch pattern                | When                                          |
| --------------- | ----------------------------- | --------------------------------------------- |
| Branch-per-task | `task/<name>`                 | Higher granularity needed                     |
| Trunk-based     | Commit directly to `main`     | Solo fast iteration, no phase isolation needed |

---

## Commit Format

`<type>(<scope>): <description>`

- **type** (required): `feat` | `fix` | `chore` | `docs` | `refactor` | `test` | `style` | `perf`
- **scope** (optional): area of codebase, e.g. `auth`, `api`, `db`
- **description**: lowercase, imperative mood, no period, under 72 chars
- **body** (optional): blank line after subject, explain _why_ — not what

---

## Recovery

**Undo last commit (keep changes staged):**

```bash
git reset --soft HEAD~1
```

**Resolve merge conflict:**

1. Open conflicted files, resolve manually
2. `git add <resolved-files>`
3. `git commit`

