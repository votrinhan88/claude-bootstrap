# Simulation Results Template

Use this template to record the outcome of a bootstrap simulation. Save to `eval/results/{date}_{testcase-id}_{run-num}.md`.

---

## Header

**Test Case:** {testcase-name}
**Run Number:** {N} of {total-runs-for-this-case}
**Date:** {YYYY-MM-DD}
**Duration:** {HH:MM}
**Tester:** {name or "sub-agent"}

**Test Case Link:** [testcase-name](../testcases/{testcase-name}.md)

---

## Execution Summary

Brief narrative of what happened during the simulation:

```
{1-2 paragraph summary of how the simulation went. Did it flow smoothly?
Where did it get stuck? Any surprises?}
```

---

## Completion Analysis

### Phase Progression

Track which steps were reached and whether they passed, failed, or were skipped.

#### Session 0 (Bootstrap)

| Step | Action | Status | Time | Notes |
|------|--------|--------|------|-------|
| 1 | Git Setup | ✅ PASS | 2m | User chose "track knowledge" |
| 2 | Audit | ⏭️ SKIP | — | New project, skipped |
| 3 | Integration | ⏭️ SKIP | — | No .claude/, skipped |
| 4 | Interview | ✅ PASS | 8m | 2 clarifications on success criteria |
| 5 | Scaffold | ✅ PASS | 5m | All files created correctly |
| 6 | Agents | ✅ PASS | 6m | 3 roles defined, tension map clear |
| 7 | State | ✅ PASS | 2m | CONTEXT.md initialized |
| 8 | Gate | ✅ PASS | 3m | User confirmed ready to plan |

**Session 0 Score: 8/8 steps = 100%**

#### Session 1 (Planning) — [if reached]

| Step | Action | Status | Time | Notes |
|------|--------|--------|------|-------|
| 1.1 | Explore | ✅ PASS | 3m | — |
| 1.2 | Draft Plan | ✅ PASS | 8m | — |
| 1.3 | Quality Pre-flight | ⚠️ PARTIAL | 4m | Found 1 phase > 7 files, split |
| 1.4 | Annotate | ❌ FAIL | 2m | User annotations unclear, loop 2x |
| 1.5 | Role Review | — | — | Not reached |
| 1.6 | Gate | — | — | Not reached |

**Session 1 Score: 2.5/6 steps = 42%**

#### Session 2+ (Implementation) — [if reached]

| Milestone | Status | Time | Notes |
|-----------|--------|------|-------|
| State reconciliation | ✅ PASS | 2m | CONTEXT.md verified against reality |
| First task execution | ✅ PASS | 5m | Role agent spawned, task completed |
| Phase 1 boundary | ✅ PASS | 3m | Gate review passed |
| Phase 2 start | ⏸️ STOPPED | — | Simulation ended; could continue |

**Session 2+ Score: 3/4 checkpoints = 75%**

### Overall Completion

**Total phases reached:** Session 0 + partial Session 1
**Last completed step:** Session 1, Step 1.2 (Draft Plan)
**Completion score:** `(8 + 2.5 + 0) / 14 = **75%**`

---

## Artifact Validation

For each artifact created, score completeness and correctness using the scoring rubric in `METRICS.md`.

### CLAUDE.md

| Field | Status | Score | Notes |
|-------|--------|-------|-------|
| Project name/description | ✅ | 1/1 | Clear: "Blog Platform" |
| Tools & environment section | ✅ | 1/1 | Python, Flask, PostgreSQL |
| Commands | ✅ | 1/1 | build, test, lint all present |
| Git rules | ✅ | 1/1 | Conventional commits, branching rules |
| Session protocol | ✅ | 1/1 | Complete, matches bootstrap template |
| **Total** | | **5/5** | Excellent |

### SPEC.md

| Field | Status | Score | Notes |
|-------|--------|-------|-------|
| Purpose | ✅ | 1/1 | Clear goal: personal blog platform |
| Success Criteria | ⚠️ | 0.7/1 | 2 testable, 1 vague ("user-friendly") |
| Scope: In | ✅ | 1/1 | Blog posts, comments, search |
| Scope: Out | ✅ | 1/1 | Social sharing, analytics deferred |
| Constraints | ✅ | 1/1 | 3-month deadline, free hosting |
| Quality Priorities | ✅ | 1/1 | Performance > Completeness > Aesthetics |
| Failure Modes | ⚠️ | 0.8/1 | 3 modes, 1 is vague ("scale issues") |
| Delivery | ✅ | 1/1 | GitHub repo + deployed site |
| **Total** | | **7.5/8** | Good, minor clarity issues |

### Agent Definitions

| Field | Status | Score | Notes |
|-------|--------|-------|-------|
| Orchestrator | ✅ | 1/1 | Clear routing responsibilities |
| Roles present | ✅ | 1/1 | Developer, Reviewer, Validator (3 roles) |
| Responsibilities clear | ✅ | 1/1 | No overlap; distinct ownership |
| Acceptance criteria | ⚠️ | 0.8/1 | Present but criteria for Reviewer is vague |
| Tension map | ✅ | 1/1 | Shows Developer-Reviewer tension well |
| **Total** | | **4.8/5** | Strong |

### CONTEXT.md

| Field | Status | Score | Notes |
|-------|--------|-------|-------|
| Status | ✅ | 1/1 | Clear: "Beginning design phase" |
| Decisions | ✅ | 1/1 | Git tracking, branching, delivery target |
| Constraints | ✅ | 1/1 | Deadline, platform limits, role authority |
| Concerns | ✅ | 1/1 | Scale questions, integration unknowns |
| **Total** | | **4/4** | Complete |

### PLAN.md (if created)

[Not created in this simulation — Session 1 incomplete]

### Interview Handoff Log

| Field | Status | Score | Notes |
|-------|--------|-------|-------|
| Frontmatter | ✅ | 1/1 | date, seq, type, author correct |
| Completed | ✅ | 1/1 | Clear summary of interview |
| Discovered | ✅ | 1/1 | User mentioned scale concerns |
| Next | ✅ | 1/1 | "Move to plan drafting" |
| **Total** | | **4/4** | Complete |

### Artifact Summary

| Artifact | Score | Status |
|----------|-------|--------|
| CLAUDE.md | 5/5 (100%) | ✅ |
| SPEC.md | 7.5/8 (94%) | ✅ |
| Agents | 4.8/5 (96%) | ✅ |
| CONTEXT.md | 4/4 (100%) | ✅ |
| Interview Handoff | 4/4 (100%) | ✅ |
| **Overall Artifacts Score** | **25.3/26 (97%)** | ✅ |

---

## Clarity Analysis

### Ambiguities Encountered

Track where the user or agent got confused and needed clarification.

#### Ambiguity 1: Interview Success Criteria

**Step:** Session 0, Step 4 (Interview)
**Severity:** Low
**What happened:**
User asked "What does success look like?" — agent's response was too abstract.
User rephrased: "Give me specific things we could measure."
Agent clarified with testable criteria.

**Recovery:** 1 clarification loop (< 1 min)
**Root cause:** Agent instructions could mention "make criteria testable/measurable"

#### Ambiguity 2: Tension Map Interpretation

**Step:** Session 0, Step 6 (Agent Definition)
**Severity:** Low
**What happened:**
User asked "Is the Developer-Reviewer tension healthy?"
Agent explained the dynamic.
User accepted.

**Recovery:** 1 clarification loop (< 1 min)
**Root cause:** Tension map template could include examples

#### Ambiguity 3: Plan Quality Pre-flight (Step 1.3)

**Step:** Session 1, Step 1.3 (Quality Pre-flight)
**Severity:** Medium
**What happened:**
Agent detected: "Phase X touches 8 files (exceeds limit of 7)"
Needed to split the phase.
User approved split but asked "Why 7?" — agent explained quality reasoning.

**Recovery:** 1 clarification loop (2 min)
**Root cause:** Quality rules documented but reasoning not in interview

### Clarity Metrics

| Metric | Value | Notes |
|--------|-------|-------|
| Total decision points | 18 | Interview, Scaffold, Agent def, etc. |
| Clear decisions (0 loops) | 15 | User/agent aligned immediately |
| Ambiguous but recoverable (1-2 loops) | 3 | Above 3 ambiguities |
| Escalations (3+ loops) | 0 | None needed user manual help |

**Clarity Score: 15/18 + (3 × 0.5)/18 = `16.5/18 = 92%`**

---

## Blocker Inventory

### Critical Blockers

These prevented forward progress and required external intervention or fix.

- **[None encountered]**

### Degraded Blockers

These slowed progress but were worked around.

**Blocker 1: Step 1.3 Phase-splitting logic**
- **Impact:** Took extra 2 min; user asked clarifying question
- **Severity:** Degraded (low impact, easily resolved)
- **Root cause:** Quality pre-flight rules not pre-explained in interview
- **Resolution:** Agent explained; user understood
- **Recommendation:** Add brief explanation of "7-file limit" to interview output or plan template

### Minor Blockers

Self-resolving or no impact.

- **[None encountered]**

### Blocker Summary

| Category | Count | Status | Unresolved |
|----------|-------|--------|-----------|
| Critical | 0 | — | 0 |
| Degraded | 1 | Resolved | 0 |
| Minor | 0 | — | 0 |
| **Total** | **1** | **100% Resolved** | **0** |

---

## Overall Score

Using the formula from `METRICS.md`:

```
OVERALL SCORE = (Completion + Artifacts + Clarity + Blocker-Free) / 4

Completion: 75%
Artifacts: 97%
Clarity: 92%
Blocker-Free: 100% (0 unresolved critical blockers)

Overall = (75 + 97 + 92 + 100) / 4 = 91% → 🟢 PASS
```

---

## Key Findings

### What Worked Well

1. **Artifact quality** — All generated files were correct and complete (97% score)
2. **Agent clarity** — Minimal ambiguities; agent explained well when asked
3. **Interview flow** — User provided clear, specific answers to most questions
4. **Blockers resolved** — No critical blockers; all degraded issues quickly resolved

### What Could Improve

1. **Completion depth** — Simulation stopped at Session 1 Step 1.2; didn't reach full planning or implementation
   - Root cause: Time/scope constraints of test, not framework issue

2. **Plan quality rules** — Phase-splitting logic took extra explanation
   - Recommendation: Document "7-file guideline" earlier (in interview output or SPEC)

3. **Tension map examples** — User asked for examples of "healthy tension"
   - Recommendation: Add 1-2 examples to agent template

### Patterns Observed

- **Experienced user flows smoothly:** Minimal clarifications needed
- **Clear interview output → clear SPEC:** Tight interview → high-quality downstream artifacts
- **Artifact chaining:** SPEC quality directly impacted agent definitions quality

---

## Recommendations for Framework Fixes

**Priority: Medium**
1. Add phase-splitting guideline explanation to interview output or plan template
2. Add 2-3 examples of healthy vs. unhealthy agent tension to agent template

**Priority: Low**
1. Add note about making success criteria "testable/measurable" to interview guide

---

## Simulation Log (Optional)

For reference, include timestamps and key moments:

```
10:30 — Session 0, Step 1 started (Git setup)
10:32 — User chose "track knowledge"
10:34 — Step 2 skipped (new project)
10:35 — Step 4 started (Interview)
10:37 — Ambiguity 1: success criteria clarification
10:38 — Interview completed
...
10:58 — Session 1, Step 1.3: Phase-splitting decision
11:00 — Simulation paused (for purposes of this test)
```

---

## Attachments

- [ ] Captured outputs (CLAUDE.md, SPEC.md, agents/, CONTEXT.md, PLAN.md if created)
- [ ] Session logs from Claude Code (if available)
- [ ] Photos/screenshots of decision points (if relevant)

