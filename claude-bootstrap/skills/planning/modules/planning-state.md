---
name: planning-state
description: Agent-only. Initialize `.claude/state/` with CONTEXT.md and log templates.
user-invocable: false
metadata:
  reads: [.claude/, docs/preset-templates/]
  writes: [.claude/state/, .claude/docs/templates/logs/]
  authors: [votrinhan88]
  version: 0.1
---

# Planning State
Initialize the state system that `state-control` will operate on at runtime: CONTEXT.md with real initial content and log templates.

## Prerequisites

`PLAN.md` must be approved and written to `.claude/state/PLAN.md` (output from `planning-plan`). State initialization uses plan phases and user preferences to set up CONTEXT.md.

## Steps
1. **Verify prerequisites** — check that `.claude/state/PLAN.md` exists and has been approved; if missing, stop and prompt user to complete `planning-plan`
2. **Create state directory** — create `.claude/state/logs/` if not already present
3. **Initialize CONTEXT.md** — write `.claude/state/CONTEXT.md` with section headings populated from `planning_interview` output:
  - Status: "Planning complete — ready for implementation"
  - Understanding: project purpose and domain from interview
  - What Changed: "Initial state — Planning session complete"
  - Tasks: Next = first implementation phase from PLAN.md (if available); others empty
  - Open Issues: any `confidence_notes` deferred items from interview
  - Decisions: record integration mode if existing project
  - Constraints: constraints from interview
4. **Present log types** — show the four standard types to the user; ask if any additional types are needed:
  - `handoff` — ad-hoc context transfer: human-initiated pause/end of working session; what was done, in progress, discovered, next
  - `task` — task outcome: completed, failed, or escalated; changes made and references
  - `blocker` — blocker or risk: severity, description, impact, proposed resolution
  - `checkpoint` — phase boundary: completion metrics, archive signal, insights
5. **For each custom log type requested by user:**
  - Evaluate: Is it genuinely distinct from the four standard types?
    - Yes → proceed to Step 5b
    - No → recommend using an existing type; return to Step 4
  - **(5b) Draft template:** Create a new template based on existing ones; present to user for approval
  - **(5b) Write approved template:** If approved, write template to `.claude/docs/templates/logs/`
6. **Export log templates**
  - Copy the four preset templates from `claude-bootstrap/docs/preset-templates/logs/` to `.claude/docs/templates/logs/` (handoff, task, blocker, checkpoint)
  - Include any user-approved custom templates from Step 5b
7. **Confirm** — list created paths; confirm with user before proceeding

## Examples
- **Input:** Planning interview complete, new project, no custom log types requested
- **Output:** `CONTEXT.md` written with real initial content from interview; four template files in `.claude/docs/templates/logs/`; user confirmed

- **Input:** Existing project, `state-control` already running, `.claude/state/` exists
- **Output:** Skip paths that already exist; only add missing templates; report what was skipped

## Edge Cases
- **`.claude/state/` already exists:** do not overwrite CONTEXT.md if it has content; report to user and skip
- **Custom log type requested:** draft frontmatter + body template; present to user for approval before writing

## Resources

### Output schema
```yaml
planning_state:
  log_types: [list]
  user_confirmed: true | false
```
