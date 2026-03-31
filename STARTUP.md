# Startup

Agent bootstrap routine. Scaffold project infrastructure, define roles, and establish state management. Follow the flow below to completion.

**New to the framework?** See [diagrams.md](diagrams.md) for a visual overview of the three-session model (Bootstrap → Planning → Runtime).

---

## Flow

1. **Bootstrap session** → Read and follow [`bootstrap/SKILL.md`](claude-bootstrap/skills/bootstrap/SKILL.md) (or via skill `/bootstrap`)
2. **Optional: Compact context** → Trim session context if logs are verbose
3. **Planning session** → Read and follow [`planning/SKILL.md`](claude-bootstrap/skills/planning/SKILL.md) (or via skill `/planning`) through completion

---

## Resources

**Bootstrap skill modules:**
- Audit: [`bootstrap/modules/bootstrap-audit.md`](claude-bootstrap/skills/bootstrap/modules/bootstrap-audit.md)
- Git: [`bootstrap/modules/bootstrap-git.md`](claude-bootstrap/skills/bootstrap/modules/bootstrap-git.md)
- Integration: [`bootstrap/modules/bootstrap-integration.md`](claude-bootstrap/skills/bootstrap/modules/bootstrap-integration.md)

**Planning skill modules:**
- Interview: [`planning/modules/planning-interview.md`](claude-bootstrap/skills/planning/modules/planning-interview.md)
- Agents: [`planning/modules/planning-agents.md`](claude-bootstrap/skills/planning/modules/planning-agents.md)
- Skills: [`planning/modules/planning-skills.md`](claude-bootstrap/skills/planning/modules/planning-skills.md)
- Rules: [`planning/modules/planning-rules.md`](claude-bootstrap/skills/planning/modules/planning-rules.md)
- Plan: [`planning/modules/planning-plan.md`](claude-bootstrap/skills/planning/modules/planning-plan.md)
- State: [`planning/modules/planning-state.md`](claude-bootstrap/skills/planning/modules/planning-state.md)
- Claude: [`planning/modules/planning-claude.md`](claude-bootstrap/skills/planning/modules/planning-claude.md)

**User reference:** [`README.md`](README.md)

---

## Key Notes

**State & Logging:**
- CONTEXT.md is your working memory — curated at session boundaries to stay under 50 lines
- Logs in `.claude/state/logs/` are **append-only** (never edited) and structured by type:
  - `task` — agent writes per task completion
  - `blocker` — agent writes on escalation
  - `checkpoint` — orchestrator writes at phase gates (marks phase completion; triggers log archival)
  - `handoff` — human writes when pausing or ending a session
- Checkpoints mark natural phase boundaries; use `/health` to review state and capture context before advancing

**Scope Boundary:**
Bootstrap framework provides methodology only. Your project customizes agents, skills, and rules in `.claude/`.
