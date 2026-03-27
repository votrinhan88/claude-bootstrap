# Bootstrap Integration

Choose how to integrate the Bootstrap framework with an existing `.claude/` directory. Run during Bootstrap session, Step 3 for existing projects with existing Claude config.

---

## Prerequisites

- Audit completed (Bootstrap session, Step 2)
- `.claude/` directory exists
- User is available to choose and confirm

---

## The Three Modes

### Extend (Default)

Add Bootstrap framework infrastructure **alongside** existing config. Nothing is touched or overwritten.

**Use when:**
- You want to keep existing custom rules, hooks, skills, agents
- You're not ready to migrate yet
- You want to pilot the framework without disrupting current workflow

**What happens:**
- Framework creates `.claude/docs/SPEC.md` (interview spec)
- Framework creates `.claude/agents/orchestrator.md` + role agents
- Framework creates `.claude/state/` (CONTEXT.md, logs/)
- Existing files remain untouched
- Both old and new agents can coexist

**Outcome:** New `.claude/` directories exist; old configs are preserved.

---

### Migrate (Structured Integration)

Restructure existing Claude config into the Bootstrap framework. Agent proposes a plan; user approves before any change is made.

**Use when:**
- You want a clean integration with existing infrastructure
- You have existing agents/skills that map to framework roles
- You're committed to the framework long-term

**What happens:**
1. Agent reads existing `.claude/` structure
2. Agent proposes mapping:
   - Existing agents → framework role agents (with renames, if needed)
   - Existing skills → framework domain skills
   - Existing rules → framework rules (may need reorganization)
   - Existing hooks → framework hooks (may need reorganization)
3. Agent backs up existing files (`.claude/.backup-{timestamp}/`)
4. Agent restructures into framework layout
5. Framework adds missing pieces (orchestrator, state/, SPEC.md)

**Outcome:** Existing work is preserved and integrated; framework fills gaps.

---

### Replace (Clean Start)

Archive existing Claude config. Start fresh from framework defaults.

**Use when:**
- Existing config is outdated or minimal
- You're starting a new phase and want a clean slate
- You're willing to lose the old config (after confirming with user)

**What happens:**
1. Agent archives existing `.claude/` as `.claude/.archive-{timestamp}/`
2. Framework scaffolds fresh `.claude/` from defaults
3. User can reference archived version if needed

**Outcome:** Clean `.claude/` directory with framework structure; old config archived but available.

---

## User Choice

Present the three options clearly:

```markdown
## Choose Integration Mode

Your project has an existing `.claude/` directory.

1. **Extend** (recommended if unsure)
   - Keep existing config, add framework alongside
   - No modifications, zero risk
   - Best for: piloting the framework

2. **Migrate** (recommended if ready to commit)
   - Integrate existing config into framework
   - Agent proposes a plan first
   - Best for: existing agents/skills you want to preserve

3. **Replace** (recommended if config is old/minimal)
   - Archive existing, start fresh
   - Fastest path to clean framework structure
   - Best for: abandoned or minimal existing config
```

Ask: "Which mode fits your situation?"

---

## Mode-Specific Workflows

### Extend: Proceed Immediately

No additional steps. User confirms mode. Pass to Step 4 (bootstrap-interview.md).

### Migrate: Propose & Approve Plan

> **Agent:** Before making any changes:
>
> 1. Read existing `.claude/` recursively
> 2. Draft a migration plan showing:
>    - Each existing file → target location (with any renames)
>    - Conflicts or ambiguities (with recommendations)
>    - What framework pieces will be added
> 3. Present plan to user
> 4. Ask: "Ready to proceed, or adjust anything?"
> 5. Do not modify files until user confirms

Once user approves, execute the plan (back up, move, restructure, add).

### Replace: Confirm Loss & Archive

> **Agent:** Confirm with user:
> "This will move your existing `.claude/` to `.claude/.archive-{timestamp}/` for safekeeping. You can still reference it. Ready?"
>
> Do not proceed until user confirms.
>
> Once confirmed, archive and scaffold fresh.

---

## Outcome

Integration mode is chosen. If Migrate, the migration plan is approved and executed. Existing infrastructure is preserved or integrated.

Pass to Step 4 (bootstrap-interview.md).
