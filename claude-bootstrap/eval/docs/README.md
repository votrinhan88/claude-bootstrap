# Bootstrap Evaluation Framework

Empirical testing of the claude-bootstrap pipeline across diverse project and user scenarios.

---

## Purpose

Validate that the bootstrap framework can guide users from initial project state → fully instrumented `.claude/` infrastructure, across:
- Project types (new, existing, simple, complex)
- User personas (new to Claude, experienced, expert)
- Integration modes (Extend, Migrate, Replace)

---

## Quick Start

### 1. Choose a Test Case

Browse `scenarios/` and pick one (or create a new one):
- `scenarios/new-simple-experienced.md` — New project, solo experienced user (baseline best-case)
- `scenarios/new-complex-new.md` — New project, new-to-Claude user (friction test)
- `scenarios/existing-complex-experienced.md` — Existing project with `.claude/`, experienced user (integration stress test)

### 2. Run a Simulation

```bash
# Read the test case
cat docs/eval/scenarios/new-simple-experienced.md

# Follow the instructions to set up a fresh project matching the scenario
# (Spin up a temp directory with the project structure, user persona context, etc.)

# Start a Claude Code session pointing to STARTUP.md
# Let the simulation run end-to-end

# Record results using templates/result-template.md
# Save to eval/results/{date}_{testcase-name}_{run-number}.md
```

### 3. Analyze Results

All simulation results go in `eval/results/` with timestamp:
- `eval/results/{date}_{testcase-name}_{run-number}.md`

Review against `METRICS.md` to score completion, artifacts, clarity, blockers.

Aggregate results in `eval/summary.md` to identify patterns.

---

## Directory Structure

```
docs/eval/
├── README.md (this file)
├── METRICS.md (measurement framework and scoring)
├── scenarios/ (hand-crafted test scenarios)
│   ├── new-simple-experienced.md
│   ├── new-complex-new.md
│   └── existing-complex-experienced.md
│
└── templates/ (templates for defining and recording tests)
    ├── testcase-template.md (how to define a new test case)
    └── result-template.md (how to write up results)

eval/
├── results/ (simulation output — one file per run)
│   ├── 2026-03-25_new-simple-experienced_run-1.md
│   └── ...
└── summary.md (aggregate results and trends — updated after each batch)
```

---

## Evaluation Workflow

### Run a Batch (3-5 test cases, 2 runs each)

1. Pick 3-5 test cases from `docs/eval/scenarios/`
2. For each, run 2 separate simulations (account for variance)
3. Record each in `eval/results/{date}_{name}_{run-num}.md`
4. Use `docs/eval/templates/result-template.md` to structure findings

### Analyze & Aggregate

1. Read all results from the batch
2. Extract metrics (completion %, blockers, time, artifacts score)
3. Update `eval/summary.md` with:
   - Results table (test case × metrics)
   - Top blockers (what failed most often?)
   - Patterns (e.g., "new users always struggle with interview step")
   - Recommendations for Phase 2 fixes

---

## Key Artifacts to Validate

When reviewing a simulation result, check these were created correctly:

- [ ] `.claude/CLAUDE.md` — root config with stack, commands, rules
- [ ] `.claude/docs/SPEC.md` — spec from interview (purpose, scope, constraints, etc.)
- [ ] `.claude/agents/` — orchestrator + role agents with clear responsibilities
- [ ] `.claude/agents/*/acceptance-criteria` — each role has decision criteria
- [ ] `.claude/state/CONTEXT.md` — initial state with decisions, constraints, concerns
- [ ] `.claude/state/logs/` — interview handoff + any other session logs
- [ ] `.claude/skills/`, `.claude/rules/`, `.claude/hooks/` — placeholder infrastructure
- [ ] `.claude/state/PLAN.md` — execution plan (if Session 1 reached)

---

## Metrics Summary

Three main dimensions:

1. **Completion** (did it finish?) → % of phases reached
2. **Artifacts** (are outputs correct?) → validity checklist
3. **Clarity** (did user/agent get confused?) → ambiguity count + severity
4. **Blockers** (what broke?) → categorized by root cause

See `METRICS.md` for detailed scoring framework.

---

## Examples

### Running a Single Test Case

```bash
# Read the scenario
cat docs/eval/scenarios/new-simple-experienced.md

# Set up environment (create temp dir, files, context)
mkdir /tmp/eval-test-1
cd /tmp/eval-test-1

# Initialize project matching scenario
# (Spin up a minimal project structure)

# Start Claude Code session pointing to /path/to/claude-bootstrap/STARTUP.md
# Run simulation end-to-end
# Record in eval/results/

# After simulation completes:
cat > eval/results/2026-03-25_new-simple-experienced_run-1.md << 'EOF'
[Use docs/eval/templates/result-template.md as guide]
EOF
```

### Reviewing Results

```bash
# Read results from a run
cat eval/results/2026-03-25_new-simple-experienced_run-1.md

# Check against METRICS.md scoring criteria
# Extract: completion %, artifact scores, clarity issues, blockers
# Update eval/summary.md with findings
```

---

## When to Run Evaluations

- **After Phase 1 completes** (now!) — baseline with current state
- **After Phase 2 completes** — measure improvements in content/clarity
- **After Phase 3 completes** — final polish validation
- **Periodically during development** — catch regressions

---

## Contributing New Test Cases

Copy `templates/testcase-template.md`, define:
1. Project type and complexity
2. User persona (experience level, team size)
3. Project structure (files, existing `.claude/` if applicable)
4. User context (what they know, what they're trying to do)

Add to `docs/eval/scenarios/` with a descriptive name.
