# Session 0 — Bootstrap

Initialize or onboard a project: set up git, audit existing work, establish Claude infrastructure, define agents, and initialize state.

> **Agent:** This is a disposable session. Your job is to set up infrastructure and get confirmation. You do not implement anything. You do not modify existing project files unless the user explicitly approves.

---

## Step 1: Git Setup

> **Agent:** Run the git skill: `claude-bootstrap/skills/bootstrap/bootstrap-git.md`
>
> The skill will ask: use git? If yes, how much to track?

**Outcome:** Git is initialized or confirmed. `.gitignore` is configured. Branching mode is decided. Decisions recorded in `.claude/config.yml` under `git:`.

---

## Step 2: Audit

> **Agent:** Skip this step if the project is new. Run only for existing projects.
>
> Run the audit skill: `claude-bootstrap/skills/bootstrap/bootstrap-audit.md`
>
> Present findings to the user and let them correct and fill gaps. Note explicitly whether `.claude/` exists.

**Outcome:** Project is fully understood. Stack, conventions, existing infrastructure, and gaps are documented. Whether `.claude/` exists is confirmed.

---

## Step 3: Claude Integration Mode

> **Agent:** Skip this step if: (a) the project is new, or (b) no `.claude/` directory was found in Step 2.
>
> Run the integration-mode skill: `claude-bootstrap/skills/bootstrap/bootstrap-integration.md`
>
> Present three options:
> - **Extend** (default) — Add framework infrastructure alongside existing config. Nothing is touched or overwritten.
> - **Migrate** — Restructure existing Claude config into the framework. Agent proposes a plan; user approves before any change is made.
> - **Replace** — Archive existing Claude config, start fresh from framework defaults.

**Outcome:** Integration mode is chosen. If Migrate, the migration plan is approved.

---

## Step 4: Interview

> **Agent:** Run the interview skill: `claude-bootstrap/skills/bootstrap/bootstrap-interview.md`
>
> - **New project:** Full interview — goals, failure modes, stack, constraints.
> - **Existing project:** Gap-filling only — cover what the audit couldn't reveal: technical debt, implementation patterns, team context, current phase, and success criteria.
>
> A log entry is written to state logs as the interview handoff record.

**Outcome:** A structured understanding of goals, failure modes, stack, and constraints. Interview log recorded.

---

## Step 5: Scaffold

> **Agent:** Run the scaffold skill: `claude-bootstrap/skills/bootstrap/bootstrap-scaffold.md`
>
> - **New project:** Create all directories and placeholder files from scratch.
> - **Existing project, Extend:** Add framework infrastructure alongside. Never overwrite existing files. Adapt to existing conventions and git workflow.
> - **Existing project, Migrate:** Execute the approved migration plan. Back up existing files first.
> - **Existing project, Replace:** Archive existing `.claude/` config. Scaffold fresh from framework defaults.

**Outcome:** All directories and placeholder files exist. Infrastructure is structurally complete.

---

## Step 6: Agent Definition

> **Agent:** Run the agents skill: `claude-bootstrap/skills/bootstrap/bootstrap-agents.md`
>
> Derive roles from interview findings, failure modes, and (if existing project) audit findings and any prior role definitions. Create a tension map. Present to user for approval before proceeding.

**Outcome:** All agent files are fully defined and confirmed by the user. The tension map is approved.

---

## Step 7: State Initialization

> **Agent:** Run the state skill: `claude-bootstrap/skills/bootstrap/bootstrap-state.md`
>
> Initialize context state with role-aware decisions and concerns from the interview, audit (if run), and agent definitions.

**Outcome:** Context state initialized. The system is now role-aware.

---

## Step 8: Review Gate

> **Agent:** Present a summary to the user:
> - Git configuration chosen
> - Integration mode chosen (if existing project) and what was done
> - Roles and tension map
> - CLAUDE.md and SPEC.md outline
> - Configuration and context state contents
> - Hooks and skills included (as placeholders — content filled in Session 1)
>
> Ask: "Ready to move to planning, or adjust anything?"
>
> Do not proceed until the user confirms.

**Outcome:** User has approved the scaffold. If git tracking is enabled, commit the scaffold now.

---

**Session 0 ends here. Close this session.**

---
