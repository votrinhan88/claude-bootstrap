---
name: planning-interview
description: Agent-only. Collect project spec, constraints, and failure modes from the user.
user-invocable: false
metadata:
  reads: [.claude/docs/SPEC.md]
  writes: [.claude/docs/SPEC.md]
  authors: [votrinhan88]
  version: 0.1
---

# Planning Interview
Gather everything needed to define agents and build the execution plan: spec, domain, constraints, and failure modes.

## Steps
1. **Read existing spec** — check `.claude/docs/SPEC.md`; if it has content, use it as a starting point and probe only for gaps; if it is a stub, interview from scratch
2. **Read sophistication signal** — after the first user response, assess depth and adapt:
   - Focused: user named stakeholders, constraints, and a success criterion unprompted → compress remaining gaps into 2–3 targeted follow-ups
   - Exploratory: user describes a general area without specifics → guide topic by topic with prompting questions
3. **Extract required topics** — interview conversationally; hold the checklist internally, never recite it; ask one topic at a time; probe until answers are concrete — vague answers propagate to the spec and plan
   - Domain (required) — software, research, writing, design, operations, other; shapes which sub-skills activate
   - Purpose — what does this project do?
   - Success (required) — at least one testable criterion; what would block completion?
   - Scope — what's in for v1? what's explicitly out?
   - Constraints (required) — hard limits on platform, format, integration, compliance, deadline; explicit "none" is acceptable
   - Quality — what matters most? rank top three; where can corners be cut?
   - Failure modes — what would a bad version look like? what's most likely to go wrong?
   - Delivery (required) — what artifact results? how validated? solo or collaborative?
4. **Confirm** — present the confirmation template (see Resources) filled with extracted values; ask "Does this capture everything? Anything missing or wrong?"
5. **Write output** — write confirmed values to `planning_interview`; populate `.claude/docs/SPEC.md` with confirmed content; flag deferred required items in Confidence Notes; do not proceed to agent definition until user confirms

## Examples
- **Input:** SPEC.md is an empty stub; user says "I want to build a CLI tool for syncing config files"
- **Output:** Domain: software; Success: sync completes without conflict in <5s; Constraints: Linux/macOS only; Delivery: binary + README; SPEC.md populated; user confirmed

- **Input:** SPEC.md already has content from a prior session
- **Output:** Gaps probed only; existing content preserved; SPEC.md updated with any new confirmations

## Edge Cases
- **Required field still vague after probing:** flag in Confidence Notes and proceed — explicit deferral is acceptable, vagueness is not
- **User gives unfocused or meandering answers:** ask one concrete clarifying question before moving to the next topic
- **Domain is ambiguous:** pick the closest match and note uncertainty; can be refined during agent definition
- **SPEC.md has conflicting content:** surface the conflict to the user before overwriting

## Resources

### Confirmation template
Presented to the user at Step 4 for human review — plain text, not logged.
```
Project: [name] — [one-line description]
Domain: [type]
Success: [criteria]
In scope: [list]
Out of scope: [list]
Constraints: [list or "none"]
Quality priorities: [ranked]
Failure modes: [list]
Delivery: [target], validation: [approach], team: [solo/collaborative]
```

### Output schema
```yaml
planning_interview:
  project_name: string
  domain: software | research | writing | design | operations | other
  purpose: string
  success_criteria: [list]
  in_scope: [list]
  out_of_scope: [list]
  constraints: [list]
  quality_priorities: [ranked list]
  failure_modes: [list]
  delivery_target: string
  validation_approach: string
  team: solo | collaborative
  confidence_notes: [list of deferred items]
  user_confirmed: true | false
```
