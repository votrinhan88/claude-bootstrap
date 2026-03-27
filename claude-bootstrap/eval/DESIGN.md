# Evaluation Framework — Design

Design rationale for the bootstrap evaluation framework. Covers what we're measuring, why those dimensions, how domain-agnosticism shapes coverage, and where gaps remain.

---

## Purpose

The evaluation framework exists to answer one question:

> Does the bootstrap framework reliably guide a user from zero to a working `.claude/` infrastructure — for any project type?

"Reliably" means: across project types (new/existing), complexity levels (simple/complex), user experience levels (new/expert), and domains (software, writing, research, operations, other). A framework that only works for software projects is not domain-agnostic.

---

## What We're Measuring

Four dimensions, each capturing a distinct failure mode:

| Dimension | What it catches |
|-----------|----------------|
| **Completion** | Dead ends — steps that can't be reached or skipped |
| **Artifacts** | Garbage output — files created but incorrect or incomplete |
| **Clarity** | Friction — user or agent confusion requiring re-prompting |
| **Blockers** | Hard failures — things that prevented forward progress |

Each dimension is independent. A simulation can score 100% completion but low artifact quality (everything ran but outputs were wrong). Tracking all four prevents masking problems behind aggregates.

See `docs/METRICS.md` for scoring methodology.

---

## Domain-Agnosticism as a Test Constraint

The framework claims to work for any project domain. This claim must be testable.

**The risk:** Skills written by software developers tend to embed software assumptions — git as mandatory, "tests" as the validation method, "stack" as the project descriptor. These assumptions are invisible until someone tries the framework on a writing project or an ML pipeline.

**The discipline:** Designing eval scenarios *before* all skills are complete forces those assumptions to surface. A test case for a research paper project will immediately reveal any skill that asks "what's your test framework?" or assumes a `src/` directory.

**The rule for test case design:** Every test case must be runnable without modification against any project type. Skills that break non-software scenarios are Path B violations, not eval failures.

---

## Test Coverage Matrix

Scenarios should cover the following axes:

### Project axis
| Starting state | Complexity | Has `.claude/` |
|---------------|------------|----------------|
| New | Simple | No |
| New | Complex | No |
| Existing | Simple | No |
| Existing | Simple | Yes |
| Existing | Complex | Yes |

### Domain axis
At least one non-software scenario per batch:
- Software (web app, CLI, library)
- Research / writing (paper, book, analysis)
- Operations (runbook, workflow, process)
- Data / ML (pipeline, experiment, dataset)

### User persona axis
| Experience | Team |
|-----------|------|
| New to Claude | Solo |
| Experienced | Solo |
| Expert | Small team |

Full coverage of all axes is not required per batch. Minimum viable coverage:
- 1 new + software scenario (baseline best-case)
- 1 existing + software scenario (integration stress test)
- 1 non-software scenario (domain-agnosticism check)

---

## Artifact Validation Criteria

Artifacts are evaluated on two dimensions: **presence** (was it created?) and **correctness** (does it capture the right content?).

Correctness is domain-sensitive. For a research project, "success criteria" in SPEC.md should look like "paper accepted to venue X" — not "tests pass". An eval that only validates presence misses this.

**Rule:** Each artifact checklist must include at least one correctness criterion that is domain-sensitive (i.e., would look different for a writing project vs a software project).

Current artifacts to validate:
- `CLAUDE.md` — tools/environment, commands, session protocol
- `SPEC.md` — purpose, success criteria, scope, constraints, failure modes, delivery
- `agents/` — roles defined, tensions mapped
- `state/CONTEXT.md` — status, decisions, constraints
- `state/logs/` — interview handoff present, correct format
- `state/PLAN.md` — phases, owners, validation steps (Planning session+)

---

## What Good Looks Like

A passing simulation (≥90%) means:
- The user reached all expected phases without dead ends
- Generated files contain correct, domain-appropriate content
- The agent did not require re-prompting more than once per decision point
- No critical blockers remained unresolved at session end

A partial pass (75-89%) means the framework is functional but has friction — typically: placeholder content in skills, vague instructions at decision points, or domain assumptions surfacing under non-software scenarios.

Below 75% means something structural is broken.

---

## Known Gaps (as of 2026-03-26)

| Gap | Impact | Priority |
|-----|--------|----------|
| No non-software test scenarios exist yet | Domain-agnosticism is untested | High |
| Session 3 (closure) doesn't exist — lifecycle has no "done" state | Can't test end-to-end | High |
| Several skills are placeholders — bootstrap-scaffold, bootstrap-agents | Bootstrap and Planning session completion rate will be low | High |
| Duplicate file structure (`eval/` vs `eval/docs/`) — unclear which is canonical | Confusion during evaluation runs | Medium |
| `eval/docs/METRICS.md` duplicates `eval/METRICS.md` | Drift risk | Medium |

---

## Implementation Order

Evaluation should be implemented after Sessions 1–2 are content-complete:

1. **Now:** Design (this file) — disciplines remaining skill authoring
2. **After Bootstrap and Planning session content complete:** Write 3 baseline test scenarios (software new, software existing, non-software new)
3. **After runtime content complete:** Run first simulation batch; record results
4. **Before release:** Full matrix coverage; ≥90% on all baseline scenarios

---

## Open Questions

- **Duplicate eval structure:** `eval/METRICS.md` and `eval/docs/METRICS.md` appear to be duplicates. Which is canonical? The `eval/docs/` tree seems more complete — consider removing the top-level duplicates.
- **Simulation execution:** Currently manual (human runs through STARTUP.md). Could be automated with a subagent running a scripted persona. Deferred until manual baseline is established.
- **Score thresholds:** The 75%/90% thresholds are provisional. Adjust after first batch.
