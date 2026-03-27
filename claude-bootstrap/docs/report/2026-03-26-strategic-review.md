# Strategic Review — Claude Bootstrap Framework

_Date: 2026-03-26_

---

## 1. What the Framework Is

Claude Bootstrap is a methodology layer for Claude Code that adds discipline on top of raw capability. It provides structured sessions, persistent state, spec-accountable agents, and a governance loop that detects drift before it compounds.

The framework is organized into three sessions:

- **Session 0 — Bootstrap**: git setup, project audit, interview, scaffold, agent definition, state initialization
- **Session 1 — Planning**: spec exploration, phased plan drafting, quality pre-flight, role review, planning gate
- **Session 2+ — Implementation**: orchestrated phase execution, spec drift detection, phase gates, blocker escalation, pivot protocol

---

## 2. Ecosystem Overlap

### Real Competitor: Unstructured Claude Code Usage

The framework does not compete with CrewAI, AutoGen, or LangGraph. Those are generic agent infrastructure tools for any LLM. The actual competitor is the default, unstructured way people use Claude Code — ad hoc prompts, no persistent state, no plan, no recovery protocol.

| Without the framework                  | With the framework                                    |
| -------------------------------------- | ----------------------------------------------------- |
| Context lost between sessions          | `CONTEXT.md` persists working memory                  |
| No plan — just prompts                 | `SPEC.md` as a durable contract                       |
| Implementation drifts from intent      | Spec drift detection at every phase gate              |
| Ad hoc roles and inconsistent behavior | Defined agents with tensions and escalation           |
| Pivots are crises                      | Freeze → re-spec → re-plan → resume                   |
| Solo blind spots                       | Multi-role review catches what one perspective misses |

### FOSS Tool Overlap by Session

**Session 0** — Mostly original. No FOSS tool runs an audit → interview → role-derivation pipeline as a structured setup ceremony. MetaGPT is closest but its interview is shallower and produces no tension map.

**Session 1** — Medium overlap. Phased planning with role ownership exists in MetaGPT. The pre-flight self-validation (is every phase testable? owned? scoped?) and multi-role plan review as a formal pre-implementation gate are original.

**Session 2** — Most original. Spec drift enforcement, the pivot protocol, model tier routing by role, and multi-perspective phase gates after every phase do not exist in any FOSS tool.

### Genuinely Original Features

- **Spec as a contract**: original intent written down and checked at every phase gate
- **Pivot protocol**: structured recovery when a fundamental problem emerges mid-project
- **Model tier switching**: routing agents to different models by role tier
- **Tension integrity**: symmetric agent tension maps as a governance mechanism
- **Plan quality pre-flight**: agent self-validates the plan before presenting it

---

## 3. Positioning

### Tagline

> _Claude Code gives you capability. This gives you accountability._

### One-Liner

> A methodology layer for Claude Code — structured sessions, persistent state, spec-accountable agents, and governed execution so your AI development stays on track.

### Elevator Pitch

Most Claude Code projects start strong and drift. You lose context between sessions. Your implementation diverges from what you actually wanted to build. When something breaks fundamentally, there's no recovery path — just a fresh start.

This framework adds the discipline layer Claude Code doesn't ship with: a setup ceremony that captures your intent, a plan that persists as a contract, agents that stay in role, and a governance loop that detects drift before it compounds.

### The Three Differentiators (for GitHub)

1. **Spec as a contract** — your original intent is written down and checked against at every phase gate, not just remembered
2. **Structured pivot recovery** — when a fundamental problem emerges mid-project, there's a protocol: freeze, re-spec, re-plan, resume
3. **Multi-role accountability** — agents with defined tensions surface trade-offs a solo perspective misses, without needing a real team

### What to Downplay

- The evaluation framework — not a focus yet
- Tension mechanics in detail — introduce as a benefit ("catches blind spots"), not a mechanism
- Session 0's 8-step structure — frame it as "a one-time setup that takes one session"

### Honest Constraint

The framework requires Claude Code (paid). State this plainly in the README. Frame it as intentional: _"Built specifically for Claude Code — not a generic framework bolted onto the wrong tool."_

---

## 4. Scope Decisions

### Target User

Solo developers using Claude Code, from any background or domain. Expand to teams if opportunities arise when the framework is mature.

### Distribution

Free and open-source on GitHub. Leave room for productization when mature.

### Domain Scope: Path B — Domain-Agnostic from the Start

The framework's core concepts (spec, plan, agents, phases, state) are already domain-agnostic. Code-specific assumptions are concentrated in specific touchpoints and must be removed or made optional.

**Required changes:**

| Location              | Assumption                                                  | Replacement                                             |
| --------------------- | ----------------------------------------------------------- | ------------------------------------------------------- |
| Session 0, Step 1     | Git is the versioning tool                                  | "Version control" — optional, git is one option         |
| Session 0, Step 4     | Interview asks about "stack"                                | Ask about "tools and environment"                       |
| Session 0 throughout  | "project files", "codebase"                                 | "project artifacts"                                     |
| Session 1, Step 3     | "Does any phase touch more than 7 files?"                   | Remove or make domain-configurable                      |
| Session 1, Step 3     | "test, review, or check"                                    | "verify, review, or check"                              |
| Session 2             | "git log, file system, test results" for state verification | "artifact inventory, logs, and checks"                  |
| Session 2, Phase gate | Implicitly assumes tests as validation                      | "Validation method defined per phase"                   |
| Health skill          | "Git Status" section                                        | Make optional, gated on git being in use                |
| bootstrap-git.md      | Entire skill assumes git                                    | Make pluggable — one of several version control options |

**Addition to Session 0 interview**: add **domain** as a first-class question — _"What kind of project is this? (software, research, writing, design, other)"_ — and let the answer shape which bootstrap sub-skills activate.

---

## 5. Vision

### Frame

> **The project discipline layer for AI-assisted development.**

Not an agent framework. Not a project management tool. The discipline layer — the thing that keeps AI-assisted development accountable, continuous, and recoverable across any domain.

### Missing: Session 3 — Closure

The framework takes a project from zero to ongoing execution but has no concept of _done_. Without a closure protocol, projects trail off. A lightweight Session 3 should:

- Run a final health check
- Close all open state (CONTEXT.md, logs)
- Write a project retrospective
- Archive or tag the final state in version control (if applicable)

### Evaluation Framework

Evaluation is a future differentiator, not a current focus. A framework that can measure its own quality — bootstrap completeness, artifact health, role coverage — is rare and builds trust with users.

**Approach**: design the evaluation framework now, implement later.

- **Now**: write `eval/DESIGN.md` — intended metrics, rubric structure, what passing vs failing looks like across multiple domain types (not just software). Designing for multiple domains while building Sessions 1–2 will surface any remaining code-specific assumptions.
- **Later**: implement against the completed framework.

---

## 6. Priority Order

1. Audit and patch all code-specific assumptions (Path B changes)
2. Complete Session 1
3. Complete Session 2
4. Add Session 3 (closure)
5. Write `eval/DESIGN.md` in parallel with steps 2–4
6. Implement evaluation against the completed framework
7. GitHub presence: README, positioning copy, examples
