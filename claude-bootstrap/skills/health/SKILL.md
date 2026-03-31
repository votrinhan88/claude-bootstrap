---
name: health
description: "User-only. Project health dashboard with three escalation levels — assessment (inspect), curation (update context), and handoff (capture context). User-invoked at any point to pause and reflect."
user-invocable: true
metadata:
  reads: all
  writes: [.claude/state/]
  invokes:
    skills: [state-control]
  authors: [votrinhan88]
  version: 0.2
---

# Health
User-invoked health check: inspect project state, update working memory, and capture context via three escalation levels (assessment → curation → handoff).

## Steps

### Level 1: Assessment
Pure inspection — no writes. Gather facts, validate state picture, assess operational and strategic health.
1. **Orient** — gather raw facts before evaluating anything
  - Read in order: CONTEXT.md, SPEC.md, PLAN.md (if exists), last 3–5 log entries
  - Git status (if enabled): uncommitted changes, active branches, open PRs, stale branches, commits since last phase merge
2. **Validate** — is the state picture trustworthy?
  - CONTEXT.md hygiene: identify stale entries, invalidated decisions or concerns (flag, do not modify)
3. **Operational assessment** — where are we, and what's in the way?
  - Completion: tasks done / total; current phase completion %
  - Blockers: list all log entries with type `blocker`; note age of each (how many phases old); for blockers note recommended resolution path; check CONTEXT.md Blocked section for any user decisions still pending
4. **Strategic assessment** — are we heading in the right direction?
  - Spec drift: what deviations from SPEC.md exist? What changed and why? What's the impact on remaining plan?
  - Agent health: cross-reference agent roster (`.claude/agents/`) against CONTEXT.md and logs; flag unused agents (recommend retirement) and missing roles; read `**Tensions**` sections to verify no one-sided relationships (A lists B but B does not list A) and no tensionless agents (may indicate utility role or redundancy)
  - Periodic check (every 3 phases): extend agent health assessment — are current roles still right? Any to add, merge, or retire? Anything learned that should reshape the remaining plan?
5. **Present findings** — print a report using `references/report-template.md`; ✅ healthy / ⚠️ at risk / ❌ blocked per section
6. **Prompt user** — "Proceed to curation?" (Level 2) or stop here

### Level 2: Curation
Update working memory via state-control session-end protocol. Clears stale entries, archives tasks, enforces 50-line budget.
1. **Invoke state-control** — run Session End curation (see state-control SKILL.md for full protocol details)
2. **Prompt user** — "Proceed to handoff?" (Level 3) or stop here

### Level 3: Handoff
Capture working session context via detailed log entry. User pauses to reflect; log records what was learned, decisions made, unresolved escalations, and next priorities.

1. **Compose log entry** — draft a `handoff` entry for `.claude/state/logs/` (see `.claude/docs/templates/logs/handoff.md` for structure):
  - Phase summary: what was completed, key milestones
  - Tasks resolved: list completed work with brief notes
  - Blockers resolved: any escalations now clear
  - Key decisions made: load-bearing choices for next phase
  - Unresolved escalations: any decisions still pending, open blockers
  - Next phase priorities: what comes next, recommended entry points
2. **Present to user** — show draft and await confirmation
3. **Write log entry** — append new handoff entry to `.claude/state/logs/` (append-only; never modify past entries)

## Examples
- **Input:** `/health` at phase boundary; stop after assessment
- **Output:** Report: Completion 4/9 (44%) ✅. Two open escalations ⚠️. No spec drift ✅.
- **Input:** `/health` → Level 2 curation; CONTEXT.md has stale entries
- **Output:** Understanding curated, 3 completed tasks archived, 2 blockers resolved. CONTEXT.md: 48 lines ✅.
- **Input:** `/health` → Level 2 → Level 3 handoff; ending Phase 2
- **Output:** Draft handoff: Phase 2 (7 tasks done, 1 escalation pending). Key decisions: [list]. Next: planning agents.

## Edge Cases
- No PLAN.md: skip completion reporting in Level 1
- Periodic (every 3 phases): extend Level 1 with role adequacy retrospective
- Git disabled: omit git status from Level 1 without flagging
- Blockers identified during assessment: embed in report with severity and recommended next step; if action needed, user promotes to CONTEXT.md during L2 curation, then captured in L3 handoff

## Resources
- `references/report-template.md` — format for Level 1 assessment report
- `state-control` skill — invoked by Level 2; CONTEXT.md curation and session-end protocol
- `.claude/docs/templates/logs/handoff.md` — template for Level 3 log entry
