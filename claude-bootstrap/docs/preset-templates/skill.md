---
name: skill-name
description: (User-only.?) What this skill does, when to invoke
compatibility: (Optional) Requires [environment, packages, MCP servers], etc.
user-invocable: true  # set false if agent-invocable
metadata:  # drop any fields if not applicable
  reads: [none | paths | all]
  writes: [none | paths | all]
  invokes:
    skills: []
    agents: []
  authors: [votrinhan88]
  version: 0.1
---

# [Skill Name]
One-sentence summary of the skill.

## Steps
1. **Step one** — describe what to do.
2. **Step two** — if / else / then.
3. **Step three** — apply `resources/`.
...

## Examples
- **Input:** describe a typical input
- **Output:** describe the expected result

## Edge Cases
- Edge case 1
- Edge case 2
- ...

## Resources (Optional - skip if unused)

### `modules/`, `scripts/`, `references/`, or `assets/`, etc.
List filetree and one-liner descriptions for each file.