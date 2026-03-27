# Test Case Template

Copy this file and fill in the details. Name it descriptively (e.g., `new-simple-experienced.md`).

---

## Test Case Metadata

**Name:** {descriptive-name}
**ID:** {short-id: new-simple-exp, existing-complex-new, etc.}
**Purpose:** {What question does this test case answer?}
**Expected Outcome:** {What phases/artifacts should be completed?}

---

## Project Definition

### Project Type

- [ ] New project
- [ ] Existing project

### Complexity Level

- [ ] Simple (single-file script, clear purpose)
- [ ] Medium (monolith, 5-10 files, some patterns)
- [ ] Complex (multi-file, multiple stakeholders, existing patterns)

### Project Details

**Name:** {project name}

**Description:** {One-line description of what the project does}

**Stack:** {Language, framework, key dependencies}

**Initial State:**
- Files/directories to pre-create: {list}
- Existing `.claude/` directory: Yes/No
  - If yes, what's in it: {describe existing config}

---

## User Persona Definition

### Experience Level

- [ ] New to Claude Code (first time using this framework)
- [ ] Experienced with Claude (knows frameworks, has used Claude before)
- [ ] Expert (deep knowledge of framework, has built custom agents)

### Team Context

- [ ] Solo developer
- [ ] Small team (2-5 people)
- [ ] Larger team (5+ people)

### User Goals & Context

**Primary goal:** {What is the user trying to accomplish?}

**Existing knowledge:** {What do they already know? What will be new?}

**Constraints:** {Time pressure, team constraints, skill gaps, etc.}

**Success definition (user's perspective):** {How will they know if bootstrap worked?}

---

## Setup Instructions

### Environment Preparation

Step-by-step instructions to create the project for testing:

```bash
# Example:
mkdir /tmp/eval-test-{test-name}
cd /tmp/eval-test-{test-name}

# Create project structure
mkdir src tests docs

# Create initial files
touch src/main.py
echo "# My Project" > README.md

# If testing existing project path:
# Create .claude/ with some existing config
mkdir -p .claude/agents .claude/skills
touch .claude/CLAUDE.md
echo "# Existing Config" > .claude/CLAUDE.md

# Optionally: create .gitignore
echo "*.pyc" > .gitignore
```

### User Context Setup

Instructions for what the user should keep in mind:

```
USER CONTEXT TO ESTABLISH:
- You are a {persona-type} developer
- Your goal is to {user-goal}
- You've {prior-experience} with this framework
- Constraints: {constraints}
- Start by reading STARTUP.md in the bootstrap framework
```

---

## Expected Flow

**Expected phases to reach:**
- [ ] Session 0, Step 1-8 (Bootstrap) — YES/NO
- [ ] Session 1, Steps 1-6 (Planning) — YES/NO
- [ ] Session 2+ (Implementation) — YES/NO

**Expected decision points:**
1. {Decision point}: {Expected choice}
2. {Decision point}: {Expected choice}

**Expected artifacts:**
- [ ] `.claude/CLAUDE.md`
- [ ] `.claude/docs/SPEC.md`
- [ ] `.claude/agents/` (orchestrator + [list roles])
- [ ] `.claude/state/CONTEXT.md`
- [ ] `.claude/state/logs/` (interview handoff)
- [ ] `.claude/state/PLAN.md` (if Session 1 reached)

---

## Known Unknowns

**What might surprise us in this test case:**
- {Potential ambiguity or blocker}
- {Edge case we're testing}
- {Integration path we want to validate}

**What we're specifically looking for:**
- Does the user get stuck at [step]?
- Can the agent handle [scenario]?
- Is [instruction] clear enough for [persona]?

---

## Notes

Additional context or observations:

```
- [Note 1]
- [Note 2]
- [To test: specific skill or decision point]
```

