---
name: [Project Name]
description: [One-sentence summary]
compatibility: Claude Code
metadata:  # drop any fields if not applicable
  authors: [votrinhan88]
  version: 0.1
---

# [Project Name]
[Brief overview of what this project does and its main purpose]

## Overview
[1-2 sentences on purpose, scope, and key deliverables]

## Architecture
- **Stack:** [tech/languages]
[Folder tree at maximum 2 levels with one-liner descriptions]

## Workflow
- git_workflow: [trunk-based | feature-branch | other]
[How agents coordinate and phases of execution; reference `.claude/state/PLAN.md` for detail]

## Agents
[Role-agents and tensions; format: list or table of (Agent | Role | Tensions)]

## Skills
[Reusable workflows agents invoke; format: list of (skill-name: description; invoked by [agents])]

## Hooks & Rules
[Automated enforcement boundaries; list of (hook/rule-name: what it enforces; trigger/scope)]

## Rules & Constraints
[Project-specific enforcements and hard limits; see `.claude/rules/` for detail]

## Git Workflow
[Branching strategy, commit conventions, PR expectations]
- Branch naming: `[pattern]`
- Commit format: `[format]`
- Main branch: `[branch-name]`; [restrictions on direct commits]

## State & Logging
[Session tracking — current context: `.claude/state/CONTEXT.md`; logs: `.claude/state/logs/`]

---

**Scope:** Keep CLAUDE.md under 200 lines total. For detail, reference `.claude/state/PLAN.md`, `.claude/rules/`, and `.claude/state/logs/`.