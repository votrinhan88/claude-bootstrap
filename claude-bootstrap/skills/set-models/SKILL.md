---
name: set-models
description: "User-only. Configure which Claude models power each agent tier (orchestrator, high, low). Writes tier aliases to .claude/settings.json as env vars; orchestrator resolves aliases to current model IDs at invocation time."
user-invocable: true
metadata:
  reads: [.claude/settings.json]
  writes: [.claude/settings.json]
  invokes:
    skills: []
    agents: []
  authors: [votrinhan88]
  version: 0.1
---

# Set Models
Configure the Claude model assigned to each agent tier. Aliases (`opus`, `sonnet`, `haiku`) are stored as-is; the orchestrator resolves them to current model IDs at invocation time.

## Tiers

| Tier | Use for | Env var |
|------|---------|---------|
| **orchestrator** | Routing, state management, escalation | `CLAUDE_TIER_ORCHESTRATOR` |
| **high** | Deep reasoning, judgment, nuance — analysis, strategy, review, writing | `CLAUDE_TIER_HIGH` |
| **low** | Structured, well-scoped execution — implementation, formatting, data processing | `CLAUDE_TIER_LOW` |

## Presets

| Preset | orchestrator | high | low |
|--------|-------------|------|-----|
| `default` | sonnet | sonnet | haiku |
| `performance` | opus | opus | sonnet |
| `budget` | sonnet | haiku | haiku |
| `opus` | opus | opus | opus |
| `sonnet` | sonnet | sonnet | sonnet |
| `haiku` | haiku | haiku | haiku |

## Steps

1. **Parse input** — determine invocation form:
   - `/set-models <preset>` → expand preset to tier aliases using the table above
   - `/set-models <orchestrator> <high> <low>` → use explicit values directly
   - `/set-models` (no args) → read and display current tier env vars from `settings.json`; stop
2. **Write to settings.json** — update the `env` block in `.claude/settings.json` with the three tier env vars; create the file if it does not exist; preserve all other existing fields
3. **Confirm** — print a summary of what was set:
   - Show tier → env var → alias for all three tiers
   - Note the preset name if one was used, or `(custom)` if explicit values were given

## Examples

- **Input:** `/set-models default`
- **Output:** Sets `CLAUDE_TIER_ORCHESTRATOR`=sonnet, `CLAUDE_TIER_HIGH`=sonnet, `CLAUDE_TIER_LOW`=haiku in `settings.json`.
- **Input:** `/set-models sonnet`
- **Output:** Sets all three tier env vars to `sonnet` in `settings.json`.
- **Input:** `/set-models opus sonnet haiku`
- **Output:** Sets tiers explicitly; confirms with `(custom)` label.
- **Input:** `/set-models` (no args)
- **Output:** Reads and displays current tier env vars from `settings.json` without writing.

## Edge Cases

- Unknown preset name: list valid presets and stop; do not write
- Unrecognized alias: write as-is with a warning — the orchestrator will attempt to resolve it at invocation time
- `settings.json` does not exist: create it with only the `env` block; do not treat as an error
- Partial args (e.g. only two values): prompt for missing tier; do not write partial config
- Other fields in `settings.json` (permissions, hooks, etc.): preserve them; only modify the three tier env vars

## Resources

### `.claude/`
- `settings.json` — project-level Claude Code config; `env` block holds tier aliases as `CLAUDE_TIER_*` env vars; loaded automatically at session start; orchestrator reads and resolves aliases at invocation time
