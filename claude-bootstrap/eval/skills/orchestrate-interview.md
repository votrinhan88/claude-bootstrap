# Interview Simulation — Orchestrator Script

Pure message bus for single-blind interview evaluation. Spawns three agents — **interviewer**, **interviewee**, **auditor** — routes messages between interviewer and interviewee, then hands the complete transcript to the auditor.

The orchestrator makes no judgments. All evaluation (rubric + isolation) is the auditor's job.

---

## Prerequisites

- A persona file from `eval/personas/` selected before running
- The interview skill at `claude-bootstrap/skills/bootstrap/bootstrap-interview.md`
- The rubric at `eval/skills/rubric-interview.md`
- The isolation check at `eval/skills/check-isolation.md`

---

## How to Run

> **Agent:** You are the orchestrator. You are a message router — nothing more. Follow the protocol below exactly. Do not evaluate, judge, or inject opinions into any message.

### Step 0: Setup

1. Read the selected persona file from `eval/personas/{persona-name}.md`. Store as `PERSONA_CONTENTS`.
2. Read the interview skill from `claude-bootstrap/skills/bootstrap/bootstrap-interview.md`. Store as `INTERVIEW_SKILL`.
3. Initialize: `TRANSCRIPT = []`, `MAX_TURNS = 20`, `turn = 0`

### Step 1: Spawn Agents

Spawn three agents. Do not start the loop until all are alive.

#### Interviewer Agent

```
Name: interviewer
Type: general-purpose

Prompt:
---
You are a Claude Code agent running a bootstrap interview for a new project.
Your job is to interview a user to gather project context. Follow the interview
skill instructions below EXACTLY.

## Interview Instructions

{INTERVIEW_SKILL}

## Protocol

- You will receive messages containing the conversation transcript so far.
- Each time, reply with ONLY your next interviewer message (question, follow-up, or probe).
- When you have collected all required fields and the user has confirmed the summary,
  reply with exactly:

  INTERVIEW_COMPLETE

  Followed by the handoff log in the format specified in the interview instructions.
- Do NOT include any meta-commentary. Speak directly as the interviewer.
---
```

#### Interviewee Agent

```
Name: interviewee
Type: general-purpose

Prompt:
---
{PERSONA_CONTENTS}
---
```

The interviewee prompt is the persona file verbatim. Nothing else.

#### Auditor Agent

```
Name: auditor
Type: general-purpose

Prompt:
---
You are an evaluator. You will receive a complete interview transcript,
a handoff log (or null), and a persona file. Your job is to score
the interview using two skills:

1. eval/skills/rubric-interview.md — interview quality scoring
2. eval/skills/check-isolation.md — isolation verification

Read both skills, then evaluate. Produce a single results file
combining both the rubric scores and isolation check.

You will receive all inputs in a single message after the interview ends.
Wait for it.
---
```

### Step 2: Turn-Taking Loop

```
while turn < MAX_TURNS:

    turn += 1

    # --- Interviewer's turn ---
    Send to interviewer:
        "Here is the conversation so far:\n\n{TRANSCRIPT}\n\n
         Reply with your next interviewer message.
         If the interview is complete, reply with INTERVIEW_COMPLETE
         followed by the handoff log."

    interviewer_response = receive from interviewer

    # Check for completion signal
    if interviewer_response starts with "INTERVIEW_COMPLETE":
        extract handoff_log from interviewer_response
        break

    TRANSCRIPT.append({ role: "Interviewer", message: interviewer_response })

    # --- Interviewee's turn ---
    Send to interviewee:
        "Someone is asking you questions about your project.
         Here is the conversation so far:\n\n{TRANSCRIPT}\n\n
         Reply with your next answer. Stay in character."

    interviewee_response = receive from interviewee

    TRANSCRIPT.append({ role: "You", message: interviewee_response })
```

If loop exits at MAX_TURNS without `INTERVIEW_COMPLETE`: set `handoff_log = null`, `exit_reason = "timeout"`. Otherwise: `exit_reason = "complete"`.

### Step 3: Awareness Probe

After the loop ends, send one final message to the interviewee:

```
Send to interviewee:
    "Thanks for your time. One last question — what did you think
     of this conversation? Was there anything unusual about it?"

probe_response = receive from interviewee

TRANSCRIPT.append({ role: "Interviewer", message: "Thanks for your time. One last question — what did you think of this conversation? Was there anything unusual about it?" })
TRANSCRIPT.append({ role: "You", message: probe_response })
```

This turn is mechanical — the orchestrator does not interpret the response.

### Step 4: Hand Off to Auditor

Send all collected artifacts to the auditor in a single message:

```
Send to auditor:
    "Here are the inputs for evaluation.

     ## Persona File
     Path: eval/personas/{persona-name}.md
     Contents:
     {PERSONA_CONTENTS}

     ## Turn Message Template
     The interviewee received this framing each turn:
     'Someone is asking you questions about your project.
      Here is the conversation so far: [transcript]. Reply with
      your next answer. Stay in character.'

     ## Interview Results
     - Turns: {turn_count} / 20
     - Exit reason: {exit_reason}

     ## Transcript
     {TRANSCRIPT}

     ## Handoff Log
     {handoff_log or 'NULL — interview timed out'}

     Read eval/skills/rubric-interview.md and eval/skills/check-isolation.md.
     Score the interview and check isolation. Write the combined results to:
     eval/results/{date}_{persona-name}_interview.md"

auditor_result = receive from auditor
```

### Step 5: Done

The auditor writes the results file. The orchestrator's job is finished.

---

## Transcript Format

Each turn is formatted as:

```
**Interviewer:** {message}

**You:** {message}
```

Both agents see this format. The interviewee sees "Interviewer" and "You" — no references to agents, skills, or evaluation.

---

## Failure Modes

The orchestrator detects these mechanically and records them. It does not score or judge — that's the auditor's job.

| Failure | Detection | Action |
|---------|-----------|--------|
| No completion signal | turn >= MAX_TURNS | Set exit_reason = "timeout", proceed to Step 3 |
| Loop stalls | Last 2 interviewer messages are identical strings | Break loop, set exit_reason = "stall", proceed to Step 3 |

All other failure modes (checklist dumping, vague answer acceptance, character breaks) are detected by the auditor, not the orchestrator.

---

## Notes

- The orchestrator routes messages. It does not read, interpret, or filter message content.
- The only "intelligent" action the orchestrator takes is detecting `INTERVIEW_COMPLETE` and identical-message stalls — both are mechanical string checks.
- All evaluation logic lives in the auditor via `rubric-interview.md` and `check-isolation.md`.
