# Health Check

> **Note:** This skill is a placeholder — content is incomplete.

Project health dashboard. Run anytime to assess project state and identify stale or drifting artifacts.

---

## When to Run

- At phase boundaries
- Before escalations
- When context is tight (near compaction)
- Periodically during long implementation phases (every 3 phases)

---

## Procedure

> **Agent:** Read the following in order:
> - `.claude/state/CONTEXT.md`
> - `.claude/docs/SPEC.md`
> - `.claude/state/plan.md` (if exists)
> - Last 3–5 entries in `.claude/state/logs/`

Then report:

### Completion

- Tasks done / total
- Current phase completion percentage

### Open Blockers

- List all blockers in logs/ (type: `issue`)
- Age of each (how many phases old)
- Recommended resolution path

### Spec Drift

- Any deviations from SPEC.md
- What changed and why
- Impact on remaining plan

### Role Adequacy

- Are all defined roles being used?
- Are any critical roles missing?
- Unused roles — recommend retirement

### Stale Artifacts

- Any files in `.claude/` not updated in 3+ sessions
- Skill/hook/rule placeholders still empty
- Agent definitions that haven't been invoked

### CONTEXT.md Hygiene

- Stale entries that should be removed
- Decisions/concerns no longer relevant
- Recommend removals for next session end

### Git Status (if enabled)

- Uncommitted changes (staged and unstaged)
- Active branches
- Open PRs, stale branches
- Commits since last phase merge

### Unresolved Escalations

- Any escalations logged in logs/ awaiting resolution
- Age and status
- Recommendation

---

## Output Format

Present findings to user as:
- **Green**: healthy, on track
- **Yellow**: attention needed, monitor next phase
- **Red**: blocker or drift, action required

Suggest specific next actions.