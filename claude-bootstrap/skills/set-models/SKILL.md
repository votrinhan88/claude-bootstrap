# Model Tiers

`/set-models` controls which models power each agent tier.

| Tier | Use for | Examples |
|------|---------|----------|
| **Orchestrator** | Always the orchestrator | routing, state management, escalation |
| **High** | Deep reasoning, nuance, or judgment | analysis, strategy, review, writing |
| **Low** | Structured, well-scoped execution | implementation, formatting, data processing |

| Preset | Orchestrator | High | Low |
|--------|-------------|------|-----|
| `max` | opus | opus | sonnet |
| `default` | opus | sonnet | haiku |
| `min` | sonnet | haiku | haiku |

> **Note:** Model names above are defaults — check current Claude model IDs when configuring.

Usage: `/set-models <preset>` or `/set-models <orchestrator> <high> <low>`
