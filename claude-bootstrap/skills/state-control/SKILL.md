---
name: state-control
description: Manage .claude/state/ — session start verification, CONTEXT.md curation, and log entry writing. Invoked by orchestrate and health at session boundaries.
user-invocable: false
metadata:
  reads: [.claude/state/]
  writes: [.claude/state/]
  invokes:
    skills: []
    agents: []
  authors: [votrinhan88]
  version: 0.1
---

# State Control

Runtime skill for managing `.claude/state/` — both CONTEXT.md (working memory) and logs/ (append-only history). These are two views of the same state: CONTEXT.md is what's true *now*; logs record *how we got here*.

**Context template:** `claude-bootstrap/templates/context.md`
**Log schema:** `claude-bootstrap/skills/bootstrap/bootstrap-state.md`

---

## Steps

### Session Start

> **Agent:** Before doing any work:

1. **Read** CONTEXT.md and the last 2-3 log entries
2. **Verify** — compare Understanding entries against actual project state (`git log`, file reads, output/artifact checks). Mark any invalidated beliefs.
3. **Detect drift** — `git log --oneline` since last log entry date (if git is enabled); scan for modified files not captured in What Changed.
4. **Write What Changed** — populate the section with anything new (commits, human edits, external events).
5. **Reconcile** — if drift invalidates Understanding entries, Decisions, or task states, update them before proceeding.

### Session End

> **Agent:** At the end of every session or at phase boundaries:

1. **Curate CONTEXT.md**
   1. **Update Status** — rewrite to reflect current reality in 1-2 sentences
   2. **Curate Understanding** — add confirmed learnings, remove invalidated beliefs
   3. **Clear What Changed** — it served its purpose
   4. **Advance Tasks**:
      - Move finished work to Completed (keep only the 5 most recent; older → log ref)
      - Remove resolved blockers from Blocked
      - Promote queued work as appropriate
   5. **Review Decisions** — any decision older than 5 sessions: still valid? Downgrade confidence or move to Open Issues if uncertain
   6. **Budget** — CONTEXT.md must stay under **50 lines of content**. Else move detail to a log entry, leave a reference
2. **Write log entry** — append a new file to `.claude/state/logs/` using the project's log templates in `.claude/docs/templates/logs/`
   - Most session ends produce a `handoff` entry; use `decision`, `issue`, or `review` as events warrant
   - Append-only — never edit old log entries
   - One file per entry, one entry per file

---

## CONTEXT.md Section Guide

### Status
The single most important line. An agent resuming cold reads this first. Write it as ground truth, not aspiration.

### Understanding
The agent's mental model — beliefs with evidence. Each entry needs a source (file, test, commit, session). Unsubstantiated beliefs belong in Open Issues, not here.

**Anti-pattern:** Don't store facts derivable from code unless they were non-obvious or hard-won.

### What Changed
Ephemeral. Populated at session start, cleared at session end. Forces the agent to notice drift before acting on stale assumptions.

### Tasks
Lightweight state tracking. The orchestrator owns task *routing*; this section tracks *status*.

- **In Progress**: active work and current state
- **Blocked**: always include what was tried, what's needed, and a recommendation. An agent should be able to unblock itself from this entry alone.
- **Next**: queued work with complexity hints
- **Completed**: recent results only — the log is the archive

For multi-agent setups, add `[owner]` to In Progress and Next entries.

### Open Issues
Where uncertainty lives — low-confidence areas, unresolved questions, things that might bite later. Better here than hidden inside an overconfident Understanding entry.

### Decisions
Load-bearing — they shape all downstream work. Track confidence:
- **high**: verified, tested, or externally mandated
- **medium**: reasonable choice, alternatives exist
- **low**: best guess, revisit soon

### Constraints
- **Hard**: non-negotiable (platform, compliance, deadlines)
- **Soft**: preferences that can be overridden with justification

Hard constraints eliminate options; soft constraints weight them.
