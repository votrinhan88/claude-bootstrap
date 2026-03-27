# Bootstrap Audit

Analyze an existing project to understand stack, conventions, infrastructure, and gaps. Run during Session 0, Step 2 for existing projects only.

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

### 2. Identify Stack

Document:
- **Language:** Primary language(s)
- **Framework/Runtime:** Web framework, build tool, runtime
- **Package Manager:** npm, cargo, pip, etc.
- **Testing:** Test framework and coverage
- **CI/CD:** GitHub Actions, GitLab CI, other?
- **Source control:** Git workflow (trunk-based, git-flow, other)

### 3. Audit Existing Claude Config (if `.claude/` found)

If `.claude/` exists, read its structure and report:
- What's in `.claude/agents/`, `.claude/skills/`, `.claude/hooks/`, `.claude/rules/`?
- Is there a CLAUDE.md or SPEC.md?
- Any state files (CONTEXT.md, PLAN.md, logs/)?
- Any existing hooks or rules?

### 4. Identify Conventions

Look for patterns in existing code:
- **Commit style:** Conventional commits? Squash or merge? Branching pattern?
- **Code style:** Linting config (eslint, black, clippy)?
- **Documentation:** Where do users/devs go first? Wiki? README?
- **Issue tracking:** GitHub Issues, Linear, Jira?

### 5. Assess Gaps

Note what's missing or unclear:
- Is there a spec document or clear vision?
- Is there a prioritized todo list or roadmap?
- Are there documented failure modes or known issues?
- Is the build/test process clear?

---

## Presentation

Present findings to user in this structure:

```markdown
## Project Audit Summary

### Stack
- Language: [X]
- Framework: [X]
- Package Manager: [X]
- Testing: [X]
- CI/CD: [X]

### Existing Claude Config
- `.claude/` exists: [yes | no]
- [if yes, describe contents]

### Conventions
- Commit style: [convention | squash | other]
- Code style: [config found | inferred | none]
- Docs location: [README | Wiki | other]
- Issue tracking: [GitHub | Linear | other]

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
- Stack and conventions are documented
- Whether `.claude/` exists and what's in it
- Decision on integration mode (if applicable)

Pass to Step 3 (bootstrap-integration.md if `.claude/` found) or Step 4 (bootstrap-interview.md).
