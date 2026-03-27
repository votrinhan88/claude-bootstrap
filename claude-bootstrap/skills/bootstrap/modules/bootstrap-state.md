# Bootstrap State

> **Note:** This skill is a placeholder — content is incomplete.

Creates `.claude/state/` directory structure and exports log templates during Bootstrap session Step 6. CONTEXT.md is populated and the first log entry is written in Step 7 (Review Gate), after all bootstrap work is complete.

---

## Prerequisites

- Project scaffold structure exists (`.claude/` directory created by bootstrap-scaffold)

---

## Step 1: Create State Directory

> **Agent:** Create the directory structure. No content yet — just the scaffold.

```
.claude/state/
├── CONTEXT.md          # Empty — populated at end of Bootstrap session
└── logs/               # Empty — first entry written at end of Bootstrap session
```

Create CONTEXT.md as an empty file (or with just the `# Context` heading). Do not fill in sections yet — there's nothing meaningful to record until bootstrap is complete.

---

## Step 2: Export Log Templates

Structured, file-per-entry logging. Markdown with YAML frontmatter. One flat folder. Sort alphabetically = read chronologically.

### Entry Types

- `handoff`: Session boundary — what was done, what's in progress, what to do next
- `decision`: A choice that narrows future options — what was chosen, what was rejected, why
- `issue`: A blocker, risk, or problem that needs attention or resolution
- `review`: Evaluation of completed work — phase gates, retrospectives, health checks

> **Agent:** Present these four types to the user and ask if their project needs any additional entry types. Before accepting a new type, verify that it isn't already covered by an existing type. Most additions can be expressed within the existing ones. Only add a new type if it has a genuinely distinct purpose, audience, and lifecycle from all four above.

### Filename Convention

Format: `{date}_{seq}_{type}.md`

- `date`: current date in YYYY-MM-DD
- `seq`: zero-padded, globally increasing

Example: `2026-03-24_0001_decision.md`, `2026-03-25_0014_handoff.md`

Cross-reference entries using the `{date}_{seq}` shorthand (e.g., `2026-03-24_0003`). Resolve to file by globbing `2026-03-24_0003_*.md`.

### Frontmatter

| Field        | Required | Description                                          |
| ------------ | -------- | ---------------------------------------------------- |
| `date`       | yes      | ISO date (YYYY-MM-DD)                                |
| `seq`        | yes      | Global ordering index                                |
| `type`       | yes      | `handoff`, `decision`, `issue`, or `review`          |
| `author`     | yes      | Agent that produced this entry                       |
| `parent`     | no       | `{date}_{seq}` ref to a parent entry                 |
| `scope`      | no       | Path-like target (directory, file, module)            |
| `tokens_in`  | no       | Input tokens consumed                                |
| `tokens_out` | no       | Output tokens produced                               |
| `duration`   | no       | Wall-clock time in seconds                           |
| `model`      | no       | Model or tool used                                   |

Example:

```yaml
---
date: YYYY-MM-DD
seq: 0000
type: handoff | decision | issue | review
author: agent-name
parent: YYYY-MM-DD_0000
scope: path/or/module
tokens_in: 0
tokens_out: 0
duration: 0.00
model: claude-sonnet-4-6
---
```

### Body Templates

#### handoff

```markdown
# Handoff — [title]

- **Completed**: [what got done]
- **In progress**: [what's mid-flight and where it left off]
- **Discovered**: [surprises, new information, changed assumptions]
- **Next**: [specific first action for the next session]
```

#### decision

```markdown
# Decision — [title]

- **Context**: [what prompted this decision]
- **Chosen**: [what we're doing]
- **Rejected**: [alternatives considered and why they lost]
- **Consequences**: [what this enables or constrains going forward]
```

#### issue

```markdown
# Issue — [title]

- **Severity**: [blocking | degraded | minor]
- **Description**: [what's wrong]
- **Impact**: [what's affected and how]
- **Proposed resolution**: [suggested path forward, if known]
```

#### review

```markdown
# Review — [title]

- **Scope**: [what was evaluated]
- **Verdict**: [pass | fail | partial]
- **Findings**: [key observations]
- **Actions**: [what should happen as a result]
```

### Export

> **Agent:** Present the frontmatter schema and body templates to the user for verification. If additional entry types were added earlier, draft their frontmatter and body, then discuss with the user until confirmed.

> **Agent:** Once verified, write one file per entry type to `{project}/.claude/docs/templates/logs/{type}.md` — each combining the frontmatter schema (with placeholder values) and the body template from the sections above.

---

## Outcome

- `.claude/state/` directory exists with empty CONTEXT.md and empty logs/
- Log templates exported to `.claude/docs/templates/logs/`
