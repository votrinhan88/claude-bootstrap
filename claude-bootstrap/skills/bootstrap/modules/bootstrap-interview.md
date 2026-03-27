# Bootstrap Interview

Collects everything needed to scaffold a new project: spec, stakeholders, failure modes, and handoff log. Run during Bootstrap session before scaffolding.

---

> **Agent:** Interview the user conversationally. Ask one topic at a time. Probe until answers are concrete — vague answers propagate to the spec, roles, and plan. Never recite the checklist — hold it internally.
>
> **After the first response, read the sophistication signal:**
> - **Focused** — user names stakeholders, constraints, and a success criterion unprompted → compress remaining gaps into 2–3 targeted follow-ups
> - **Exploratory** — user describes a general area or feeling without specifics → guide topic by topic with prompting questions

---

### What to extract

Cover all topics. Do not exit until all **required** items are concrete. If a required item is genuinely deferred (e.g., "formatting requirements TBD"), flag it in Confidence Notes and proceed — explicit deferral is acceptable; vagueness is not.

Topics to cover (hold internally, do not present as a list):

- **Domain** — What kind of project is this? (software, research, writing, design, operations, other) *(Required: shapes which sub-skills activate and which questions apply)*
- **Purpose** — What does this project do? What does success look like?
- **Success** — What does "done" look like? What would block completion? *(Required: at least one testable criterion)*
- **Scope** — What's in for v1? What's explicitly out?
- **Constraints** — Any hard limits? (platform, format, integration, compliance, deadline) *(Required: explicit "none" is acceptable)*
- **Quality** — What matters most? Rank top three. Where can you cut corners?
- **Failure modes** — What would a bad version of this look like? What's most likely to go wrong? For experienced users: what's kept you up at night on past projects?
- **Delivery** — What artifact or deliverable results? How will it be validated? Solo or collaborative? *(Required: delivery target must be named)*

---

### Confirmation

Summarize back before writing the handoff:

```
Project: [name] — [one-line description]
Domain: [software | research | writing | design | operations | other]
Success: [criteria]
In scope: [list]
Out of scope: [list]
Constraints: [list or "none"]
Quality priorities: [ranked]
Failure modes: [list]
Delivery: [target], validation: [approach], team: [solo/collaborative]
```

Ask: "Does this capture everything? Anything missing or wrong?"

Do not write the handoff until the user confirms. If any required field is still vague, probe further before confirming.

---

### Handoff to Scaffold

Write a log entry using the frontmatter format from `.claude/docs/templates/logs/handoff.md`. The body:

```
## Project
- Name:
- Domain: [software | research | writing | design | operations | other]
- Description (one line):
- Success criteria:

## Scope
- In scope (v1):
- Out of scope / deferred:
- Hard constraints:

## Quality
- Ranked priorities: (1) ... (2) ... (3) ...
- Definition of done:
- Acceptable tradeoffs:

## Failure Modes
- [failure mode]: [description]

## Delivery and Team
- Delivery target:
- Validation approach:
- Team: [solo | collaborative — who reviews]
- Git/CI: [if applicable, else omit]

## Confidence Notes
[Any inferred or low-confidence entries — flag for confirmation during scaffold]
```

This log entry is the input to Step 2 (Scaffold). Do not skip it.
