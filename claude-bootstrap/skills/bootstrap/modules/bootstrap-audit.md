---
name: bootstrap-audit
description: Agent-only. Analyze existing project structure, tools, conventions, and gaps. Run during Bootstrap session, Step 2 for existing projects only.
user-invocable: false
metadata:
  reads: all
  writes: none
  authors: [votrinhan88]
  version: 0.1
---

# Bootstrap Audit
Audit an existing project to understand tools, environment, conventions, and gaps for framework integration.

## Steps
1. **Explore structure** — read project directory layout
  - Root files (README, package.json, Dockerfile, .gitignore, etc.)
  - Language & framework (infer from extensions, imports, package managers)
  - Directory conventions (src/, tests/, docs/, etc.)
  - Existing `.claude/` directory presence and contents
2. **Identify tools & environment** — document primary tools, build/run process, dependencies, automation, source control
3. **Audit existing Claude config** (if `.claude/` exists) — review agents/, skills/, hooks/, rules/, CLAUDE.md, state files (CONTEXT.md, PLAN.md, logs/)
4. **Identify conventions** — commit style, style conventions, documentation location, issue tracking patterns
5. **Assess gaps** — missing spec, prioritized todo list, failure modes, or build clarity
6. **Present findings to user** — show audit summary; ask for corrections
7. **Confirm understanding** — user verifies tools, conventions, `.claude/` status, and integration mode decision

## Examples

**Software dev (new project, no `.claude/`):**
- Tools: Node.js, React, Jest, GitHub
- Conventions: Conventional commits, GitHub Issues
- `.claude/`: does not exist
- Gaps: No formal spec, unclear roadmap
- Recommendation: New project mode

**AI/ML paper (existing project, local latex, no `.claude/`):**
- Tools: Python, LaTeX, Jupyter, local filesystem
- Conventions: Manual versioning, overleaf-style comments in .tex files
- `.claude/`: does not exist
- Gaps: No build automation, version control unclear, no issue tracking
- Recommendation: Existing + Migrate

**Personal knowledge management (existing with `.claude/`):**
- Tools: Markdown, Obsidian, local git, custom scripts
- Conventions: Atomic notes, backlinks, no formal commit style
- `.claude/`: exists with agents/, skills/, CONTEXT.md, PLAN.md
- Gaps: Outdated PLAN.md, no recent logs, custom conventions not formalized
- Recommendation: Existing + Extend

## Edge Cases

- **No `.claude/` directory:** Audit proceeds normally; integration mode deferred to bootstrap-integration
- **`.claude/` exists but incomplete:** Report what's present; note gaps in agents, skills, state structure
- **Conflicting conventions:** Document what's observed; user clarifies preference during bootstrap-interview
- **No git initialized:** Note; Bootstrap session bootstrap-git step handles initialization if user elects git
- **Unclear build process:** Report what's observable (scripts in package.json, Makefile, etc.); user fills in during interview

## Resources

### audit-summary schema

Return all findings to bootstrap skill as a structured dict:

```yaml
bootstrap_audit:
  project_exists: true | false
  tools:
    languages: [list of primary languages]
    frameworks: [list of frameworks]
    build_system: string
    package_manager: string
    automation: string | null
  conventions:
    git_present: true | false
    commit_style: string | null
    doc_location: string
    issue_tracking: string | null
  claude_config:
    exists: true | false
    agents: [list]
    skills: [list]
    has_context: true | false
    has_plan: true | false
    has_logs: true | false
  gaps: [list of gaps/unknowns]
  integration_mode_recommendation: [new | extend | migrate | replace]
```
