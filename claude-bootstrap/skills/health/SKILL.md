---
name: health
description: "User-only. Project health dashboard — assess completion, blockers, spec drift, and agent health. Run at phase boundaries, before escalations, near context compaction, or every 3 completed plan phases."
user-invocable: true
metadata:
  reads: all
  writes: [.claude/state/]
  invokes:
    skills: [state-control]
    agents: []
  authors: [votrinhan88]
  version: 0.1
---

# Health Check
Project health dashboard. Assess project state, spec drift, and role adequacy.

## Steps
1. **Orient** — gather raw facts before evaluating anything
   - Read in order: CONTEXT.md, SPEC.md, PLAN.md (if exists), last 3–5 log entries
   - Git status (if enabled): uncommitted changes, active branches, open PRs, stale branches, commits since last phase merge
2. **Validate** — is the state picture trustworthy?
   - CONTEXT.md hygiene: flag stale entries, decisions or concerns no longer relevant; if found, curate immediately using the state-control curation protocol before proceeding
3. **Operational assessment** — where are we, and what's in the way?
   - Completion: tasks done / total; current phase completion %
   - Blockers: list all log entries with type `issue` or `escalation`; note age of each (how many phases old); for blockers note recommended resolution path; for escalations note whether a user decision is still pending
4. **Strategic assessment** — are we heading in the right direction?
   - Spec drift: deviations from SPEC.md; what changed and why; impact on remaining plan
   - Agent health: cross-reference agent roster (`.claude/agents/`) against CONTEXT.md and logs; flag unused agents (recommend retirement) and missing roles; read `**Tensions**` sections from all agent files; warn on one-sided tensions (A lists B but B does not list A) and tensionless agents (may be legitimate utility role or may indicate redundancy)
   - For periodic retrospectives (every 3 phases): are current roles still right? Any to add, merge, or retire? Anything learned that should change the remaining plan?
5. **Present findings** — print a report using `references/report-template.md`; ✅ healthy / ⚠️ at risk / ❌ blocked per section; next actions must reference PLAN.md — either advance the next planned task or flag that the plan needs revision

## Examples
- **Input:** `/health` mid-project at a phase boundary
- **Output:** Completion 4/9 (44%) ✅. Two open escalations from Phase 1 ⚠️. No spec drift ✅. One tensionless agent flagged ⚠️.
- **Input:** `/health` near context compaction
- **Output:** Abbreviated report focused on CONTEXT.md hygiene and any ❌ blockers

## Edge Cases
- No PLAN.md exists: skip completion reporting in Step 3
- Periodic trigger (every 3 phases): extend Step 4 with role adequacy retrospective
- Git status disabled: omit from Step 1 without flagging as an issue

## Resources

### `references/`
- `report-template.md` — output format for Step 5
