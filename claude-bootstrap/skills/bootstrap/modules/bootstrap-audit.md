# Bootstrap Audit

Analyze an existing project to understand tools and environment, conventions, infrastructure, and gaps. Run during Session 0, Step 2 for existing projects only.

---

## Prerequisites

- Project directory exists and is accessible
- User is available to confirm/correct findings

---

## Procedure

> **Agent:** Follow this workflow to audit the project. Do not modify any files — this is read-only analysis.

### 1. Explore Structure

Read the project directory layout. Identify:
- **Root files:** README, package.json, Dockerfile, .gitignore, etc.
- **Language & framework:** Infer from file extensions, imports, package managers
- **Directory convention:** src/, tests/, docs/, etc.
- **Existing `.claude/` directory?** Note if present and describe its contents

### 2. Identify Tools & Environment

Document:
- **Primary tools:** Main tools, languages, frameworks, or platforms in use
- **Build/run process:** How work is executed or validated
- **Dependencies:** Package manager, external services, runtimes
- **Automation:** CI/CD, scheduled tasks, pipelines, or equivalent
- **Source control:** Git workflow (if applicable)

### 3. Audit Existing Claude Config (if `.claude/` found)

If `.claude/` exists, read its structure and report:
- What's in `.claude/agents/`, `.claude/skills/`, `.claude/hooks/`, `.claude/rules/`?
- Is there a CLAUDE.md or SPEC.md?
- Any state files (CONTEXT.md, PLAN.md, logs/)?
- Any existing hooks or rules?

### 4. Identify Conventions

Look for patterns in existing work:
- **Commit style:** Conventional commits? Squash or merge? Branching pattern? (if git is used)
- **Style conventions:** Formatting, linting, naming patterns?
- **Documentation:** Where do users/contributors go first? Wiki? README?
- **Issue tracking:** GitHub Issues, Linear, Jira, or other?

### 5. Assess Gaps

Note what's missing or unclear:
- Is there a spec document or clear vision?
- Is there a prioritized todo list or roadmap?
- Are there documented failure modes or known issues?
- Is the build/validation process clear?

---

## Presentation

Present findings to user in this structure:

```markdown
## Project Audit Summary

### Tools & Environment
- Primary tools: [X]
- Build/run process: [X]
- Dependencies: [X]
- Automation: [X]
- Source control: [git | other | none]

### Existing Claude Config
- `.claude/` exists: [yes | no]
- [if yes, describe contents]

### Conventions
- Commit style: [convention | squash | other | n/a]
- Style conventions: [config found | inferred | none]
- Docs location: [README | Wiki | other]
- Issue tracking: [GitHub | Linear | other | none]

### Gaps & Unknowns
- [gap 1]
- [gap 2]
- ...

### Recommendation
Proceed with:
- **New project mode** (scaffold from scratch)
- **Existing project + Extend** (add framework alongside)
- **Existing project + Migrate** (integrate into framework)
- **Existing project + Replace** (archive old, start fresh)
```

Ask: "Does this match your understanding? Anything incorrect or missing?"

Let the user correct and fill in gaps before proceeding.

---

## Outcome

Project is fully understood. User confirms:
- Tools, environment, and conventions are documented
- Whether `.claude/` exists and what's in it
- Decision on integration mode (if applicable)

Pass to Step 3 (bootstrap-integration.md if `.claude/` found) or Step 4 (bootstrap-interview.md).
