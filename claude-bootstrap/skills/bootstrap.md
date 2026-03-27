# Bootstrap

Entry point for Session 0. Guides users through project initialization.

---

## How to Use

1. Start a new Claude Code session in your project directory
2. Reference `claude-code-bootstrap.md` or this skill
3. Agent invokes the interview to determine project context
4. Routes to appropriate bootstrap flow (new or existing project)

---

## Agent Flow

1. Interview (new or existing project?)
2. If new: git-setup → scaffold → agents → gate
3. If existing: audit → mode-choice → scaffold → agents → gate
4. Proceed to Session 1

---

## Skills

| Skill | Purpose |
|-------|---------|
| [bootstrap-interview](bootstrap/bootstrap-interview.md) | Gather project context and preferences |
| [bootstrap-git](bootstrap/bootstrap-git.md) | Git configuration and workflow setup |
| [bootstrap-scaffold](bootstrap/bootstrap-scaffold.md) | Create `.claude/` directory structure |
| [bootstrap-state](bootstrap/bootstrap-state.md) | Initialize CONTEXT.md and state system |
| [bootstrap-hooks](bootstrap/bootstrap-hooks.md) | Create hook placeholder infrastructure |
| [bootstrap-agents](bootstrap/bootstrap-agents.md) | Define project-specific agents |
| [bootstrap-template-logs](bootstrap/bootstrap-template-logs.md) | Log entry templates and format |
| [bootstrap-runtime](bootstrap/bootstrap-runtime.md) | Session discipline and operational signals |

---

## Next Steps

After Session 0 completes:
- Read `.claude/CONTEXT.md` to verify project state
- Proceed to Session 1 (Planning) in `session-01-plan.md`
