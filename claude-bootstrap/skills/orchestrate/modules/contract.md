# Agent Invocation Contract

---

## Overview

When the orchestrator spawns a role-agent to execute a task, it establishes a **contract** that defines inputs, outputs, constraints, and exception flows. This ensures agents do not mutate shared state (CONTEXT.md, logs) directly and that the orchestrator can reliably parse results and update state.

---

## Contract Template (Orchestrator → Agent)

The orchestrator passes this context to the role-agent subagent:

```
**Task Assignment:**
- Task ID: [from PLAN.md]
- Description: [task narrative]
- Acceptance criteria: [what done means]

**Scope:**
- Read-only paths: [PLAN.md, CONTEXT.md, project files as scoped to task]
- Write paths: [project files only, scoped to task boundaries — NO .claude/ writes]
- Model tier: [high | low, determined by agent role definition]

**Escalation Signal:**
If you discover a blocker or conflict:
1. Write a blocker log to .claude/state/logs/{date}_{seq}_blocker.md (see template in claude-bootstrap/docs/preset-templates/logs/blocker.md)
2. Return status=blocked or status=escalated in your report (see Report Template below)
3. Include the blocker log path in the Blockers field
```

---

## Report Template (Agent → Orchestrator)

Agent returns a structured report with these required fields:

```
**Task Identity:**
- Task ID: [from contract]
- Description: [from contract]
- Acceptance criteria met? [yes | no | partial — explain]

**Execution Outcome:**
- Status: completed | failed | escalated | blocked
- Summary: 1–2 sentences on what was done
- Changes: [list of modified files, or "none"]

**Findings (if status != completed):**
- Blockers: [if any — type (Tier 1 recoverable | Tier 2 human required), issue log path]
- New decisions: [facts or learnings to add to CONTEXT.md]
```

---

## State Update Rules

### Orchestrator Authority
- **Only the orchestrator** writes to `.claude/state/CONTEXT.md` and appends session logs to `.claude/state/logs/`
- Agent reports are parsed and validated before any write

### Blocker Log Exception
- Agents **MAY** write an `.claude/state/logs/{date}_{seq}_blocker.md` entry if they discover a blocker (one blocker log per blocker type/instance)
- Blocker log must include Severity, Description, Impact, and Proposed Resolution (see template)
- Orchestrator reads the log path from agent report and references it in CONTEXT.md Blocked section

### Post-Invocation Validation
Orchestrator validates agent report before updating state:
1. Parse required fields: Task ID, Description, Acceptance criteria, Status, Summary, Changes
2. If any required field missing: log as Tier 2 escalation (agent contract violation); stop
3. If status=blocked/escalated and no issue log path: prompt agent for details or proceed with best-effort summary; update CONTEXT.md Blocked
4. Otherwise: update state per status (completed → Tasks/Completed; failed/escalated → Tasks/Blocked)

---

## Autonomy & Resurfacing

### Task Execution
- Agent executes ONE task per invocation
- Internal self-validation (e.g., test passes, requirements verified) does not require resurfacing
- Task completion triggers resurfacing: agent returns structured report to orchestrator

### Multi-Task Workflows
Even with `--auto` flag:
1. Agent spawns for Task N, returns report
2. Orchestrator parses report, updates CONTEXT.md, logs outcome
3. Orchestrator spawns **fresh agent** for Task N+1 with new session state read
4. Each agent gets a clean context — no inherited assumptions from prior task

### `--auto` Behavior
- Controls orchestrator loop, not agent behavior
- `--auto`: orchestrator automatically proceeds to next task after validating report
- No `--auto`: orchestrator pauses after each task for user review (can continue, skip, or edit before next spawn)

---

## Permission Enforcement

### Agent Metadata
All role-agent definitions must include:
```yaml
metadata:
  reads: [PLAN.md, CONTEXT.md, project files as scoped to task]
  writes: [project files only — no .claude/, no state/]
```

### Scope Boundaries (from CLAUDE.md)
- **Project scope:** downstream agents may write project files per PLAN.md task assignments
- **Bootstrap scope:** `.claude/` and `CLAUDE.md` — only orchestrator and planning agents write here
- **Agent constraint:** role-agents never cross bootstrap scope boundary

