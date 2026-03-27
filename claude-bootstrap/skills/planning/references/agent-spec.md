# Agent Specification for Bootstrap

## Directory structure

An agent is a single Markdown file placed in a designated agents directory:

```
.claude/agents/
├── orchestrator.md   # Orchestrator agent
├── planner.md        # Example role-agent
└── ...               # One file per role
```

Agents are discovered by filename. The filename (without `.md`) is the agent's identifier.

## Agent file format

Each agent file must contain YAML frontmatter followed by Markdown content.

### Frontmatter

| Field | Req | Details |
|-------|-----|---------|
| `name` | Yes | Matches the filename (without `.md`). Lowercase, numbers, hyphens only. |
| `description` | Yes | When to invoke — the orchestrator uses this for routing. |
| `tier` | Yes | `high`, `low`, or `orchestrator`. Resolved to a model via `CLAUDE_TIER_*` env vars in `.claude/settings.json`. |
| `tools` | Yes | Comma-separated list of tools scoped to this role's authority. |
| `metadata` | No | Key-value map. Supported keys: `reads`, `writes`, `invokes`, `authors`, `version`. |

### Minimal example

```yaml
---
name: planner
description: Decomposes goals into phases and tasks. Invoke when a new project or feature needs a plan.
tier: high
tools: Read, Glob, Grep, Write
---
```

### Example with optional fields

```yaml
---
name: planner
description: Decomposes goals into phases and tasks. Invoke when a new project or feature needs a plan.
tier: high
tools: Read, Glob, Grep, Write
metadata:
  reads: all
  writes: .claude/state/PLAN.md
  invokes:
    skills: [bootstrap]
    agents: []
  authors: [votrinhan88]
  version: "0.1"
---
```

### `name` field

* Must match the agent's filename (without `.md`)
* Lowercase alphanumeric and hyphens only; no leading/trailing or consecutive hyphens

### `description` field

* Used by the orchestrator to decide when to route to this agent
* Should describe the role's specialty and the conditions under which it should be invoked
* Include specific keywords that help the orchestrator match tasks

Good example:
```yaml
description: Reviews security posture and threat surface. Invoke when authentication, authorization, secrets handling, or external API calls are being added or changed.
```

Poor example:
```yaml
description: Security agent.
```

### `tier` field

Maps to a model via environment variables in `.claude/settings.json`:

| Value | Env var | Typical use |
|-------|---------|-------------|
| `orchestrator` | `CLAUDE_TIER_ORCHESTRATOR` | Top-level coordination |
| `high` | `CLAUDE_TIER_HIGH` | Complex reasoning, planning |
| `low` | `CLAUDE_TIER_LOW` | Focused, well-scoped tasks |

### `tools` field

* Comma-separated list of Claude Code tool names
* Scope narrowly to what the role actually needs — principle of least privilege
* Common sets: `Read, Glob, Grep` (read-only); add `Write, Edit` for roles that produce output; add `Bash` only when execution is required

### `metadata` field

All sub-fields are optional. Drop any that don't apply.

| Sub-field | Type | Description |
|-----------|------|-------------|
| `reads` | string | `none`, a path list, or `all` |
| `writes` | string | `none`, a path list, or `all` |
| `invokes.skills` | list | Skills this agent activates |
| `invokes.agents` | list | Sub-agents this agent delegates to |
| `authors` | list | Author handles |
| `version` | string | Semver or simple version string |

## Body content

The Markdown body defines the agent's behavior. Use the preset template as a guide:

### Required sections

| Section | Purpose |
|---------|---------|
| `## Identity` | What this role knows; what it owns |
| `## Priorities` | Ordered list; flag non-negotiables |
| `## Definition of Done` | Criteria the role checks before handing off |
| `## Watches For` | Patterns to detect + how to respond |
| `## Escalation` | Self-resolve / peer-consult / human-escalate thresholds |
| `## Tensions` | Named disagreements with peer roles — explains why the design is intentional |

### `## Watches For` — mandatory entry

Every role-agent **must** include this watch rule:

```markdown
- Do not write directly to `.claude/state/` — report back to the orchestrator
```

All state writes must route through the `state-control` skill, invoked by the orchestrator. Role-agents that attempt direct state writes must be rejected.

## Progressive disclosure

1. **Frontmatter** (~50 tokens): `name` and `description` are loaded at startup for routing
2. **Body** (< 3000 tokens recommended): loaded when the agent is activated
3. **Referenced skills/modules**: loaded on demand via `invokes`

Keep agent files concise. Detailed domain knowledge belongs in skills, not agent files.

## Preset template

See [agent.md](../../docs/preset-templates/agent.md) for the canonical blank template.

## References

- Preset agent example: [orchestrator.md](../../docs/preset-agents/orchestrator.md)
- Skill specification: [skill-spec.md](skill-spec.md)
- State-control skill: [../../skills/state-control/SKILL.md](../../skills/state-control/SKILL.md)
