# Session 1 — Planning

Engage the spec, define agents and enforcement, draft execution plan, refine through role review.

**Prerequisites:** Session 0 completed — `.claude/docs/SPEC.md`, scaffold, and `.claude/state/` exist.

> **Agent:** Start in Plan Mode (read-only — no file modifications). Your job is to produce a reviewed, annotated execution plan with fully defined roles. Exit Plan Mode only to write files in Steps 1.2, 1.3, and 1.7. You do not implement anything.
>
> **Scope:** Write only to `CLAUDE.md` and `.claude/`. Do not create, modify, or delete any downstream project files — not even as a convenience, preview, or proof-of-concept.

## 1.1 Explore

> **Agent:** Read `.claude/docs/SPEC.md` and the project structure. Also read the Session 0 interview handoff from `.claude/state/logs/`. Surface:
> - Key dependencies and sequencing constraints
> - Risks and unknowns that could block phases
> - Gaps or ambiguities in the spec
> - Domain context that will shape role design
>
> Present findings to the user before proceeding. If any gaps are significant, ask the user to resolve them now.

**Outcome:** Agent understands the spec, interview context, and constraints. User has confirmed or corrected the exploration findings.

## 1.2 Agent Definition

> **Agent:** Exit Plan Mode. Run the agents skill: `claude-bootstrap/skills/bootstrap/bootstrap-agents.md`
>
> Derive roles from the spec, interview handoff, and exploration findings from 1.1. Create a tension map. Present to user for approval before writing agent files.

**Outcome:** All agent files fully defined in `.claude/agents/`. Tension map approved.

## 1.3 Draft Plan

> **Agent:** Write `.claude/state/PLAN.md`. Each phase must have:
> - Clear deliverable
> - Validation step immediately after (verify, review, or check)
> - Owning role-agent (from `.claude/agents/`)
> - Complexity estimate (low / med / high)
> - Explicit dependencies on other phases
>
> No phase covers more than one deliverable without a passing checkpoint between them.
>
> For any unknowns surfaced in 1.1, add a discovery phase before the phase that depends on it.

**Outcome:** `.claude/state/PLAN.md` written with all phases, owners, validation steps, and dependencies.

## 1.4 Plan Quality Pre-flight

> **Agent:** Before presenting the plan, self-validate:
> - Is every phase verifiable? (Is there a clear pass/fail criterion?)
> - Is every phase owned by exactly one role?
> - Are inter-phase dependencies explicit?
> - Does every implementation phase have a validation step immediately after?
>
> Fix violations before presenting to the user.

**Outcome:** PLAN.md passes all pre-flight checks.

## 1.5 Annotate

> **Agent:** Present the plan to the user. Ask them to add `> NOTE:` annotations inline in `.claude/state/PLAN.md` for anything that needs adjustment.
>
> When the user has finished annotating, address all notes:
> `Address all notes in .claude/state/PLAN.md. Don't implement yet.`
>
> Repeat until the user confirms no further notes remain.

**Outcome:** PLAN.md reflects all user feedback. No unresolved `> NOTE:` annotations remain.

## 1.6 Role Review

> **Agent:** For each role in `.claude/agents/`, spawn a subagent to review `.claude/state/PLAN.md` from that role's perspective against their acceptance criteria (defined in each agent file). Each subagent returns findings ranked by severity.
>
> Revise `.claude/state/PLAN.md` to address findings. Write a review summary log entry to `.claude/state/logs/` using the log format from `.claude/docs/templates/logs/`.

**Outcome:** PLAN.md incorporates role feedback. Review summary logged.

## 1.7 Hooks & Rules

> **Agent:** Run the hooks skill: `claude-bootstrap/skills/bootstrap/bootstrap-hooks.md`
>
> Derive hook and rule needs from the approved plan and agent roles — what behaviors need automated enforcement, what conventions need guarding. Present to user before writing any files.

**Outcome:** Hook and rule files created in `.claude/hooks/` and `.claude/rules/`. Enforcement boundaries established.

## 1.8 Planning Gate

> **Agent:** Present to the user:
> - Final plan summary (phases, owners, total complexity)
> - Role feedback incorporated and any open concerns
> - Any items deferred to discovery phases
> - Hooks and rules established
>
> Ask: "Ready to begin implementation, or further adjustments?"
>
> Do not proceed until the user confirms. Once confirmed, write a handoff log entry to `.claude/state/logs/`. If git tracking is enabled, commit the plan now.

**Outcome:** User has approved the plan. Handoff log written. If git enabled, plan committed.

---

**Session 1 ends here. Close this session.**

---
