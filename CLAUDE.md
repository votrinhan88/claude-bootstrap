# Claude Code Bootstrap

Framework for adding structured sessions, persistent state, and role-based agents to Claude Code projects.

## Tools & Environment

- Markdown (all framework content)
- Git (version control for the framework itself)
- Claude Code (target runtime — this framework is built *for* it)

## Workflow Commands

- Verify: read and follow any session file end-to-end in a test project
- Lint/Check: grep for code-specific language (`stack`, `codebase`, `test runs`) across `claude-bootstrap/`

## Git

- Branch: `claude/[phase-name]` per plan phase
- Commit: `<type>(<scope>): <description>` (feat, fix, refactor, docs, chore)
- Never commit directly to main
- Squash-merge at phase boundaries
- IMPORTANT: Never force push or delete main

## Scope Boundary

This framework operates in two distinct scopes. Agents must never cross the boundary without explicit instruction.

| Scope | What it covers | Who touches it |
|-------|---------------|----------------|
| **Bootstrap scope** | Claude infrastructure only: `CLAUDE.md`, `.claude/` directory, state files, agent files, skills, hooks, rules | Bootstrap and planning agents |
| **Project scope** | The downstream project's own files, source code, content, or deliverables | Implementation agents, only after Session 1 completes |

**Hard rules:**
- Session 0 agents: read-only on project files. Write only to `.claude/` and `CLAUDE.md`.
- Session 1 agents: read-only on project files. Write only to `.claude/` and `CLAUDE.md`.
- Session 2+ agents: may write project files, but only tasks explicitly assigned in `.claude/state/PLAN.md`.
- No agent creates, modifies, or deletes project files "to help" or as a side effect of bootstrap work.

If an agent is unsure which scope a file belongs to: **stop and ask the user.**

## Rules

- Path B: all skill content must be domain-agnostic — no code-specific assumptions unless conditional on domain
- Placeholder note: skills marked `> **Note:** This skill is a placeholder` are incomplete — do not treat them as authoritative
- IMPORTANT: Never modify log entries after they are written — append-only

## Session Protocol

- Start: read `.claude/state/CONTEXT.md`, verify state against actual project (`git log`, files). Read last 2-3 entries in `.claude/state/logs/` if needed. Reconcile.
- End: curate `CONTEXT.md` (remove stale entries), append new entry to `.claude/state/logs/`
- Log entries: `.claude/state/logs/{date}_{seq}_{type}.md`
- On compaction: preserve modified file list, current plan step, unresolved blockers
- Operational signals: `claude-bootstrap/skills/bootstrap/bootstrap-runtime.md`
