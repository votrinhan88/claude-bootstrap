# Session 1 — Planning

> **Note:** This session is a placeholder — content is incomplete.

Explore spec, draft execution plan with role ownership and validation gates, refine through role review.

**Prerequisites:** Session 0 completed — `.claude/docs/spec.md`, all agents, orchestrator, and `.claude/state/` exist.

> **Agent:** Start in Plan Mode (read-only — no file modifications). Your job is to produce a reviewed, annotated execution plan. You do not implement anything.

## 1.1 Explore

> **Agent:** Read .claude/SPEC.md and the project structure. Identify dependencies, risks, unknowns. Present findings to the user.

## 1.2 Draft Plan

> **Agent:** Write .claude/plan.md. Each phase must have:
> - Clear deliverable
> - Validation step immediately after (test, review, or check)
> - Owning role-agent (from `.claude/agents/`)
> - Complexity estimate (low / med / high)
> - Explicit dependencies on other phases
>
> No phase implements more than one module without a passing checkpoint between them.

## 1.3 Plan Quality Pre-flight

> **Agent:** Before presenting the plan, self-validate:
> - Is every phase testable/verifiable?
> - Is every phase owned by exactly one role?
> - Are inter-phase dependencies explicit?
> - Does any phase touch more than 7 files? (quality degrades — split it)
> - Does every implementation phase have a validation phase immediately after?
>
> Fix violations before presenting to the user.

## 1.4 Annotate

> **Agent:** Present the plan to the user. The user may add `> NOTE:` annotations in plan.md. When they do:
>
> `Address all notes in .claude/plan.md. Don't implement yet.`
>
> Repeat until the user confirms the plan is clean.

## 1.5 Role Review of Plan

> **Agent:** For each role in .claude/agents/, spawn a subagent to review plan.md from that role's perspective against their acceptance criteria. Revise plan.md to address findings. Append review summary to .claude/state/logs/.

## 1.6 Planning Gate

> **Agent:** Present: final plan summary, role feedback incorporated, any open concerns.
> Ask: "Ready to begin implementation, or further adjustments?"

**Session 1 ends here. Close this session.**

---

