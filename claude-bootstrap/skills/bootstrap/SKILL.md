# Bootstrap

Entry point for Session 0. Guides users through project initialization.

---

## How to Use

1. Start a new Claude Code session in your project directory
2. Run this skill (`claude-bootstrap/skills/bootstrap.md`)
3. Agent invokes the interview to determine project context
4. Routes to appropriate bootstrap flow (new or existing project)

---

## Agent Flow

**Session 0** (infrastructure + spec):
- New project: git-setup → interview → scaffold → state → gate
- Existing project: git-setup → audit → mode-choice → interview → scaffold → state → gate

**Session 1** (roles + plan):
- Explore spec → define agents → draft plan → pre-flight → annotate → role review → hooks & rules → gate

---

## Skills

| Skill | Purpose |
|-------|---------|
| [bootstrap-interview](bootstrap/bootstrap-interview.md) | Gather project context and preferences |
| [bootstrap-git](bootstrap/bootstrap-git.md) | Git configuration and workflow setup |
| [bootstrap-scaffold](bootstrap/bootstrap-scaffold.md) | Create `.claude/` directory structure |
| [bootstrap-state](bootstrap/bootstrap-state.md) | Initialize CONTEXT.md, state system, and log templates |
| [bootstrap-agents](bootstrap/bootstrap-agents.md) | Define project-specific agents (Session 1, Step 1.2) |
| [bootstrap-hooks](bootstrap/bootstrap-hooks.md) | Create hooks and rules (Session 1, Step 1.7) |
| [bootstrap-runtime](bootstrap/bootstrap-runtime.md) | Session discipline and operational signals |

---

## Session 0 Gate

> **Agent:** Run `session-00-bootstrap.md` Step 7. State population, handoff log, and user confirmation are all handled there.

---

## Next Steps

After Session 0 gate passes:
- Proceed to Session 1 (Planning) in `session-01-plan.md`
