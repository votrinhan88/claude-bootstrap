# Bootstrap Evaluation Metrics

Empirical measurement framework for bootstrap pipeline evaluations.

---

## Measurement Dimensions

### 1. Completion (Did the simulation reach expected endpoints?)

**What:** Track which session phases were reached and which steps within each phase completed.

**How to measure:**

```markdown
## Completion Checklist

Bootstrap session (Bootstrap):
- [ ] Step 1: Git Setup — PASS/FAIL
- [ ] Step 2: Audit — SKIP/PASS/FAIL (skip if new project)
- [ ] Step 3: Integration Mode — SKIP/PASS/FAIL (skip if no .claude/)
- [ ] Step 4: Interview — PASS/FAIL
- [ ] Step 5: Scaffold — PASS/FAIL
- [ ] Step 6: Agent Definition — PASS/FAIL
- [ ] Step 7: State Initialization — PASS/FAIL
- [ ] Step 8: Review Gate — PASS/FAIL

Planning session (Planning):
- [ ] 1.1 Explore — PASS/FAIL
- [ ] 1.2 Draft Plan — PASS/FAIL
- [ ] 1.3 Quality Pre-flight — PASS/FAIL
- [ ] 1.4 Annotate — PASS/FAIL
- [ ] 1.5 Role Review — PASS/FAIL
- [ ] 1.6 Planning Gate — PASS/FAIL

Runtime:
- [ ] Start state reconciliation — PASS/FAIL
- [ ] First task execution — PASS/FAIL
- [ ] Phase boundary review — PASS/FAIL
```

**Score:** `steps_passed / (steps_passed + steps_failed)` as percentage

Example:
- Reached: Bootstrap session complete, Planning session to step 1.4, runtime not started
- Score: 14 passed / 15 attempted = **93%**

---

### 2. Artifacts (Are generated files correct and complete?)

**What:** Validate each generated artifact against a checklist of required and optional fields.

**How to measure:**

For each artifact, score completeness and correctness:

#### CLAUDE.md (Root Config)

| Field | Required | Presence | Correctness | Score |
|-------|----------|----------|-------------|-------|
| Project name/description | Yes | ✅ | ✅ | 1 |
| Tools & environment section | Yes | ✅ | ⚠️ (vague) | 0.5 |
| Commands (build, test) | Yes | ✅ | ✅ | 1 |
| Git rules (if git enabled) | Yes | ✅ | ✅ | 1 |
| Session protocol | Yes | ✅ | ✅ | 1 |
| User-defined rules | No | ✅ | ✅ | 1 |
| **Total** | | | | **5.5/6** |

#### SPEC.md (Project Specification)

| Field | Required | Presence | Correctness | Score |
|-------|----------|----------|-------------|-------|
| Purpose/Goal | Yes | ✅ | ✅ | 1 |
| Success Criteria (testable) | Yes | ✅ | ⚠️ (1 vague) | 0.7 |
| Scope: In (v1) | Yes | ✅ | ✅ | 1 |
| Scope: Out/Deferred | Yes | ⚠️ (empty) | ✅ | 0.5 |
| Constraints | Yes | ✅ | ✅ | 1 |
| Quality Priorities (ranked) | Yes | ✅ | ✅ | 1 |
| Failure Modes | Yes | ✅ | ⚠️ (vague) | 0.7 |
| Delivery Target | Yes | ✅ | ✅ | 1 |
| **Total** | | | | **7.9/8** |

#### Agents (Orchestrator + Roles)

| Field | Required | Presence | Correctness | Score |
|-------|----------|----------|-------------|-------|
| Orchestrator defined | Yes | ✅ | ✅ | 1 |
| All roles from interview present | Yes | ✅ | ✅ | 1 |
| Responsibilities clear per role | Yes | ✅ | ⚠️ (some overlap) | 0.8 |
| Acceptance criteria per role | Yes | ✅ | ✅ | 1 |
| Tension map present | Yes | ✅ | ✅ | 1 |
| Tension map makes sense | Yes | ⚠️ (needs clarification) | ⚠️ | 0.5 |
| **Total** | | | | **5.3/6** |

#### CONTEXT.md (Initial State)

| Field | Required | Presence | Correctness | Score |
|-------|----------|----------|-------------|-------|
| Status summary | Yes | ✅ | ✅ | 1 |
| Active Decisions (git, branching, etc.) | Yes | ✅ | ✅ | 1 |
| Active Concerns/Unknowns | No | ✅ | ✅ | 1 |
| Constraints Discovered | Yes | ✅ | ✅ | 1 |
| **Total** | | | | **4/4** |

#### Interview Handoff (Log Entry)

| Field | Required | Presence | Correctness | Score |
|-------|----------|----------|-------------|-------|
| Frontmatter (date, seq, type) | Yes | ✅ | ✅ | 1 |
| Completed summary | Yes | ✅ | ✅ | 1 |
| In-progress work | Yes | ✅ (none) | ✅ | 1 |
| Discovered insights | No | ✅ | ✅ | 1 |
| Next action | Yes | ✅ | ✅ | 1 |
| **Total** | | | | **5/5** |

#### PLAN.md (Execution Plan, if Planning session reached)

| Field | Required | Presence | Correctness | Score |
|-------|----------|----------|-------------|-------|
| All phases have deliverables | Yes | ✅ | ⚠️ (vague on 2) | 0.7 |
| All phases have validation | Yes | ✅ | ✅ | 1 |
| All phases have owning role | Yes | ✅ | ✅ | 1 |
| Dependencies explicit | Yes | ⚠️ (some implicit) | ⚠️ | 0.6 |
| Complexity estimates | No | ✅ | ✅ | 1 |
| No phase >7 files | Yes | ✅ | ✅ | 1 |
| **Total** | | | | **5.3/6** |

**Overall Artifacts Score:** Average of all artifacts' scores

Example: `(5.5 + 7.9 + 5.3 + 4 + 5 + 5.3) / 6 = **5.5/6 = 92%**`

---

### 3. Clarity (Did the user/agent encounter confusion?)

**What:** Track ambiguities, re-reads, clarification loops, and vague answers.

**How to measure:**

```markdown
## Clarity Analysis

AMBIGUITY INCIDENTS:
1. Interview step — "What does success look like?"
   - Severity: Low (easily clarified)
   - Resolution: Agent re-prompted, user gave clear criteria
   - Recovery loops: 1
   - Root cause: Interview instructions could be clearer

2. Integration mode choice — "Extend vs Migrate — which should I pick?"
   - Severity: Medium (user picked wrong mode initially, had to backtrack)
   - Resolution: Agent provided detailed comparison
   - Recovery loops: 2
   - Root cause: Mode descriptions in skill are dense

3. Scaffold step — "Where do my config files go?"
   - Severity: High (user escalated, required manual intervention)
   - Resolution: Agent couldn't resolve; user clarified
   - Recovery loops: 3
   - Root cause: Documentation missing (placeholder content)

CLARITY SCORE CALCULATION:
- Total decision points in flow: ~20
- Clear decision points (0 loops): 17
- Ambiguous but recoverable (1-2 loops): 2
- Ambiguous, required escalation (3+ loops): 1

Score: (17 + 2*0.5 + 1*0) / 20 = **85%**
```

**Factors that reduce clarity:**
- Vague wording ("do X as needed" without criteria)
- Missing context (reference to undefined term)
- Placeholder content (e.g., "TODO: add details here")
- Long/dense instructions (user needs multiple re-reads)
- Ambiguous branching logic (user unsure which path to take)

---

### 4. Blockers (What prevented progress?)

**What:** Categorize obstacles that stopped forward progress.

**How to measure:**

```markdown
## Blocker Inventory

CRITICAL (prevents completion, requires external fix or escalation):
- [ ] Missing file: bootstrap-audit.md — User couldn't audit existing project
  - Status: FIXED in Phase 1
  - Impact: Blocked existing-project path entirely

- [ ] Ambiguous gate: "How do I know interview is done?"
  - Status: UNFIXED
  - Impact: User unsure when to move to next step
  - Recommendation: Add explicit done-criteria to interview skill

DEGRADED (slows progress but can be worked around):
- [ ] Unclear instruction in bootstrap-agents.md
  - Status: UNFIXED
  - Impact: Agent needed clarification from user; added 5 min
  - Recommendation: Rewrite with examples

- [ ] Vague acceptance criteria examples
  - Status: UNFIXED
  - Impact: User questioned if their agent definitions were "good enough"
  - Recommendation: Add 2-3 concrete examples

MINOR (self-resolving or doesn't impact user):
- [ ] Typo in CONTEXT.md template reference
  - Status: Self-resolved by agent
  - Impact: None (agent corrected automatically)

BLOCKER SCORE:
- Critical blockers: 1 FIXED, 1 UNFIXED
- Degraded blockers: 2 UNFIXED
- Minor blockers: 1 SELF-RESOLVED

Unresolved: 3 / Total: 4 = **75% resolved**
```

---

## Overall Scoring Formula

```
OVERALL SCORE = (Completion + Artifacts + Clarity + Blocker-Free) / 4

Where:
- Completion: phases_reached / phases_available (0-100%)
- Artifacts: avg artifact scores (0-100%)
- Clarity: clear_decisions / total_decisions (0-100%)
- Blocker-Free: (1 - critical_unresolved / total_critical) (0-100%, capped)

Example:
- Completion: 90%
- Artifacts: 92%
- Clarity: 85%
- Blocker-Free: 50% (1 critical unfixed out of 2)

Overall = (90 + 92 + 85 + 50) / 4 = **79.25% → 🟡 PARTIAL PASS**
```

---

## Score Interpretation

| Range | Status | Interpretation |
|-------|--------|-----------------|
| 90-100% | 🟢 PASS | Framework flows smoothly; minor issues only |
| 75-89% | 🟡 PARTIAL | Framework works but has friction; needs fixes |
| 60-74% | 🔴 FAIL | Major blockers; framework not production-ready |
| <60% | 🔴 CRITICAL | Cannot complete; requires significant rework |

---

## Recording Metrics

When documenting a simulation result, include:

```markdown
# Simulation Results: {testcase-name} — Run {number}

**Date:** YYYY-MM-DD
**Tester:** [name or "sub-agent"]
**Duration:** HH:MM

## Completion
- Phases reached: [X/Y]
- Last completed step: [step-name]
- Score: **[%]**

## Artifacts
| Artifact | Score | Notes |
|----------|-------|-------|
| CLAUDE.md | 92% | Tools & environment section vague |
| SPEC.md | 95% | Success criteria clear |
| Agents | 87% | Tension map unclear |
| CONTEXT.md | 100% | Complete |
| Total | **93%** | |

## Clarity
- Ambiguities: [X] (Y critical, Z recoverable)
- Longest loop: [step name] — [N] clarifications
- Score: **[%]**

## Blockers
- Critical: [X] ([Y] fixed, [Z] unfixed)
- Degraded: [X] (unfixed)
- Minor: [X] (self-resolved)
- Unresolved: **[Z]**

## Overall Score: **[%]** → 🟢/🟡/🔴

## Key Findings
- [Major blocker or success]
- [Pattern observed]
- [Recommendation for fix]
```

---

## Updating summary.md

After each batch of simulations, aggregate results:

```markdown
# Evaluation Summary

**Last updated:** YYYY-MM-DD
**Batches run:** [X]
**Total simulations:** [N]

## Results by Test Case

| Test Case | Runs | Avg Completion | Avg Artifacts | Avg Clarity | Avg Score | Status |
|-----------|------|---|---|---|---|---|
| new-simple-exp | 2 | 100% | 95% | 90% | **95%** | 🟢 |
| new-complex-new | 2 | 75% | 78% | 70% | **74%** | 🔴 |
| existing-complex-exp | 2 | 90% | 92% | 88% | **90%** | 🟡 |

## Top Blockers (Across All Runs)

1. **[blocker name]** — Appeared in X runs, severity [Critical/Degraded]
   - Root cause: [...]
   - Fix: [...]
   - Priority: [High/Medium/Low]

2. ...

## Patterns

- New users struggle most with: [area]
- Existing projects hit blockers at: [step]
- Integration modes: [which works best]

## Recommendations

1. Fix [critical blocker]
2. Rewrite [unclear section]
3. Add [missing content]
```

