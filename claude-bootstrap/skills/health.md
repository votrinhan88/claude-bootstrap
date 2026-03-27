# Health Check

Project health dashboard. Run anytime to assess project state and identify issues.

---

## When to Run

- At phase boundaries
- Before escalations
- When context is tight (near compaction)
- Periodically during long implementation phases (after 3 or more phases)

---

## Procedure

> **Agent:** Read the following in order:
> - `.claude/state/CONTEXT.md`
> - `.claude/docs/SPEC.md`
> - `.claude/state/PLAN.md` (if exists)
> - Last 3–5 entries in `.claude/state/logs/`

Then report:

### Completion

- Tasks done / total
- Current phase completion percentage

### Open Blockers & Escalations

- List all entries in logs/ with type `issue` or `escalation`
- Age of each (how many phases old)
- For blockers: recommended resolution path
- For escalations: whether a user decision is still pending

### Spec Drift

- Any deviations from SPEC.md
- What changed and why
- Impact on remaining plan

### Agent Health

- Cross-reference agent roster (`.claude/agents/`) against CONTEXT.md and logs
- Agents never referenced in logs or context — flag as unused, recommend retirement
- Roles referenced in logs but missing from agent roster — flag as missing
- Read `**Tensions**` sections from all agent files
- **One-sided tensions**: agent A lists tension with B, but B does not list A — warn
- **Tensionless agents**: agents with no tensions — warn (may be legitimate utility role or may indicate redundancy)

### CONTEXT.md Hygiene

- Stale entries that should be removed
- Decisions/concerns no longer relevant
- Recommend removals for next session end

### Git Status (if enabled)

- Uncommitted changes (staged and unstaged)
- Active branches
- Open PRs, stale branches
- Commits since last phase merge

---

## Output Format

Present findings to user as:
- **Green**: healthy, on track
- **Yellow**: attention needed, monitor next phase
- **Red**: blocker or drift, action required

Suggest specific next actions.