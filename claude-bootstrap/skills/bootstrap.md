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
| [bootstrap-state](bootstrap/bootstrap-state.md) | Initialize CONTEXT.md, state system, and log templates |
| [bootstrap-hooks](bootstrap/bootstrap-hooks.md) | Create hook placeholder infrastructure |
| [bootstrap-agents](bootstrap/bootstrap-agents.md) | Define project-specific agents |
| [bootstrap-runtime](bootstrap/bootstrap-runtime.md) | Session discipline and operational signals |

---

## Session 0 Gate

> **Agent:** After all bootstrap skills have run (interview, git, scaffold, state, hooks, agents, runtime):

1. **Populate CONTEXT.md** from the template (`claude-bootstrap/templates/context.md`), filling in:
   - Status: one sentence summarizing the bootstrapped project
   - Decisions: all choices made during bootstrap (git, branching, agent roles, etc.)
   - Constraints: hard limits discovered during interview
   - Open Issues: any low-confidence areas flagged during bootstrap
   - Leave Understanding, What Changed, and Tasks sections empty — they activate in Session 1+
2. **Write the first log entry** — a `handoff` entry summarizing the full Session 0 bootstrap
3. **Present to user** for review and confirmation

---

## Next Steps

After Session 0 gate passes:
- Proceed to Session 1 (Planning) in `session-01-PLAN.md`
