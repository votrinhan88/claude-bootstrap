# Interview Evaluation Rubric

> **Agent:** You are the auditor. You have two jobs:
> 1. Score interview quality using the checks in this file.
> 2. Check isolation using `eval/skills/check-isolation.md`.
>
> Run both, then produce a single combined results file. Every check is binary (PASS/FAIL). For each, cite the exact evidence from the transcript or handoff log.

---

## Inputs

You receive these from the orchestrator:

- `TRANSCRIPT` — array of `{ role, message }` turns
- `handoff_log` — string or null
- `turn_count` — integer
- `exit_reason` — `"complete"`, `"timeout"`, or `"stall"`
- `PERSONA_CONTENTS` — full text of the persona file used
- `TURN_MESSAGE_TEMPLATE` — the framing string sent to the interviewee each turn

---

## Dimension 1: Extraction

**Question:** Did the handoff log capture all required information concretely?

If `handoff_log` is null: all 8 checks → FAIL.

For each field, apply two checks:

| # | Field | Present? | Concrete? |
|---|-------|----------|-----------|
| 1 | Purpose / Description | Search handoff_log for a "Project" or "Description" section. PASS if found. | Read the content. PASS if it contains at least one falsifiable statement (someone could verify it as true or false). FAIL if it only contains subjective language ("good", "nice", "user-friendly", "scalable") with no measurable qualifier. |
| 2 | Success criteria | Search for "Success criteria". | PASS if at least one criterion is testable (could write a pass/fail check for it). |
| 3 | Scope: in | Search for "In scope". | PASS if it lists specific items (features, components, deliverables), not just a category ("frontend stuff"). |
| 4 | Scope: out | Search for "Out of scope" or "Deferred". | PASS if it names specific exclusions, or explicitly states "none". FAIL if missing entirely. |
| 5 | Constraints | Search for "Constraints" or "Hard constraints". | PASS if it names specific limits (deadline, platform, budget, team size) or explicitly states "none". |
| 6 | Quality priorities | Search for "Ranked priorities" or "Quality". | PASS if priorities are listed in explicit rank order (1, 2, 3). FAIL if listed without ranking. |
| 7 | Failure modes | Search for "Failure Modes". | PASS if at least one failure mode describes a specific bad outcome (not just "things could go wrong"). |
| 8 | Delivery target | Search for "Delivery target" or "Delivery". | PASS if it names a concrete artifact or endpoint (repo, URL, package, document). |

**Result:** Count of fields where BOTH present and concrete passed, out of 8.

---

## Dimension 2: Technique

**Question:** Did the interviewer follow the skill's instructions?

Apply each check to the TRANSCRIPT. Each is binary.

### T1. One topic at a time

Read each interviewer message (excluding the confirmation summary). Count how many introduce **2 or more unrelated questions** (questions about fields not discussed in the preceding 2 turns).

- **PASS** if 0-1 messages violate this.
- **FAIL** if 2+ messages violate this.
- **Evidence:** Quote the worst-offending message, or state "no violations".

### T2. Did not dump checklist

Read the interviewer's **first message** only.

- **PASS** if it asks about one topic (may contain 1-2 related questions on that topic).
- **FAIL** if it lists 3+ unrelated topics or asks about 3+ distinct fields at once.
- **Evidence:** Quote the first message.

### T3. Probed vague answers

Scan interviewee messages for vague responses: answers that use hedging language ("something like", "maybe", "I think", "not sure", "TBD", "kind of") or that respond to an open question with fewer than 10 words without providing specifics.

For each vague answer found, check if the next interviewer message follows up on the vague part specifically.

- **PASS** if no vague answers exist, OR if ≥ half of vague answers received a targeted follow-up.
- **FAIL** if < half of vague answers received a follow-up (interviewer accepted vagueness).
- **Evidence:** List each vague answer and whether it was probed. Quote both sides.

### T4. Confirmed before handoff

Search the TRANSCRIPT for a message from the interviewer that summarizes collected information AND asks the interviewee to confirm (look for: "does this capture", "anything missing", "is this correct", "look right", "confirm").

Then check if the interviewee responded before the interview ended.

- **PASS** if summary was presented AND interviewee responded to it.
- **FAIL** if no summary was presented, OR summary was presented but interviewer proceeded without waiting for a response.
- **Evidence:** Quote the summary message and the interviewee's response (or note their absence).

### T5. Adapted to persona

Read `PERSONA_CONTENTS`. Look for a `## How you communicate` section. If it doesn't exist: **SKIP** (neither pass nor fail).

If it exists, determine the persona type from the behavioral description, then apply the matching check:

| Persona trait | PASS if | FAIL if |
|---------------|---------|---------|
| Direct / expert / gives info upfront | Interviewer used ≤ 8 question turns total (compressed) | Interviewer used ≥ 12 question turns (didn't compress) |
| Vague / exploratory / uncertain | Interviewer probed ≥ 3 vague answers with targeted follow-ups | Interviewer accepted ≥ 3 vague answers without probing |
| Terse / short answers | Interviewer used specific prompts ("Can you give an example of...", "What specifically...") after short answers | Interviewer repeated the same question verbatim, or gave up and moved on after a short answer |
| Scope-expanding / tangential | Interviewer explicitly deferred or excluded ≥ 1 item the interviewee tried to add | Interviewer never pushed back on any scope addition |
| Resistant / hostile | Interviewer remained neutral (no defensive language) AND extracted ≥ 6/8 fields | Interviewer became defensive, OR abandoned ≥ 3 required fields |

If the persona doesn't clearly match any row: apply your best judgment on whether the interviewer adapted to the described communication style. State which trait you mapped to and why.

- **Evidence:** Quote the persona's communication style, then quote transcript turns showing adaptation or failure.

### T6. No redundant questions

For each interviewer question, check if the interviewee already clearly answered that topic in a previous turn.

- **PASS** if 0-1 redundant questions.
- **FAIL** if 2+ redundant questions.
- **Evidence:** List each redundant question with the turn where it was already answered.

**Result:** Count of checks passed out of 5 (or 6 if T5 was not skipped).

---

## Dimension 3: Isolation

Run all 4 tests from `eval/skills/check-isolation.md` using the inputs provided by the orchestrator.

**Result:** `ISOLATED` or `COMPROMISED`, plus per-test detail as specified in that skill.

---

## Verdict

Count total checks:

```
extraction_passed = count of fields where both Present and Concrete passed (max 8)
technique_passed  = count of technique checks passed (max 5 or 6)
isolation         = ISOLATED or COMPROMISED
```

| Condition | Verdict |
|-----------|---------|
| extraction ≥ 7 AND technique ≥ 4 AND ISOLATED | `PASS` |
| extraction ≥ 5 AND technique ≥ 3 AND ISOLATED | `PARTIAL` |
| extraction ≥ 4 AND technique ≥ 2 | `WEAK` |
| Anything worse | `FAIL` |

Append modifiers:
- If `exit_reason == "timeout"`: append `(TIMEOUT)`
- If `exit_reason == "stall"`: append `(STALL)`
- If isolation is `COMPROMISED`: append `(COMPROMISED)` — scores are unreliable, flag but still report

---

## Output Format

> **Agent:** Write your evaluation in exactly this format. Do not add or remove sections.

```markdown
# Interview Eval: {persona_name}

**Date:** {YYYY-MM-DD}
**Persona:** {persona_name}
**Turns:** {turn_count} / 20
**Exit:** {exit_reason}
**Verdict: {PASS / PARTIAL / WEAK / FAIL}**

---

## Extraction ({X}/8 passed)

| # | Field | Present | Concrete | Evidence |
|---|-------|---------|----------|----------|
| 1 | Purpose | PASS/FAIL | PASS/FAIL | {quote from handoff_log, or "MISSING"} |
| 2 | Success criteria | ... | ... | ... |
| 3 | Scope: in | ... | ... | ... |
| 4 | Scope: out | ... | ... | ... |
| 5 | Constraints | ... | ... | ... |
| 6 | Quality priorities | ... | ... | ... |
| 7 | Failure modes | ... | ... | ... |
| 8 | Delivery target | ... | ... | ... |

## Technique ({Y}/5 or {Y}/6 passed)

| Check | Result | Evidence |
|-------|--------|----------|
| T1. One topic at a time | PASS/FAIL | {quote or "no violations"} |
| T2. No checklist dump | PASS/FAIL | {first message quote} |
| T3. Probed vague answers | PASS/FAIL | {vague count, probed count, quotes} |
| T4. Confirmed before handoff | PASS/FAIL | {summary + response quotes} |
| T5. Adapted to persona | PASS/FAIL/SKIP | {persona trait, evidence quotes} |
| T6. No redundant questions | PASS/FAIL | {list or "none found"} |

## Isolation

{Output from check-isolation.md — full section as specified in that skill}

## Transcript

{Full TRANSCRIPT, verbatim}

## Handoff Log

{handoff_log verbatim, or "NULL — interview timed out"}
```
