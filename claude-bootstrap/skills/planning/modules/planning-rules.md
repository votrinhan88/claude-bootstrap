---
name: planning-rules
description: Agent-only. Derive and define project-specific rules from SPEC constraints and risk analysis. Creates both rule and hook files.
user-invocable: false
metadata:
  reads: [.claude/docs/SPEC.md, .claude/agents/, docs/preset-templates/rule.md, docs/preset-templates/hook.md]
  writes: [.claude/rules/, .claude/hooks/, .claude/docs/templates/rule.md, .claude/docs/templates/hook.md]
  authors: [votrinhan88]
  version: 0.1
---

# Planning Rules & Enforcement

Derive project-specific rules (policy statements, constraints) and define how they are enforced (hooks or manual). Rules define *what* to enforce; hooks define *how* to enforce it. Both are needed.

## Prerequisites

Agents and skills must be defined (outputs from `planning-agents` and `planning-skills`). Rules are derived from SPEC constraints, agent scope, and skill scope.

## Steps
1. **Verify prerequisites** — check that `.claude/agents/` and `.claude/skills/` directories exist with content; if missing, stop and prompt user to complete `planning-agents` and `planning-skills`
2. **Show templates** — present rule and hook templates from `claude-bootstrap/docs/preset-templates/` to user; confirm they want to use them
3. **Identify constraints** — from SPEC and risk analysis, extract:
  - Hard limits: platform/compliance/immutable decisions
  - Workflow boundaries: destructive ops, immutability, scope
  - Quality standards: format, review gates, testing
  - Team/process: approval chains, escalation paths
4. **Classify rules** — for each constraint, determine:
  - **Scope:** applies to bootstrap, project, both, or specific phase?
  - **Relevant agents:** who needs to know this rule? (developer, reviewer, ops, etc.)
5. **Determine enforcement method** — for each rule, answer:
  - **Can the violation be automatically detected?** (automatable, testable, deterministic)
    - Yes → **hook-based**. Will it block or warn? (blocking hook or warning hook)
    - No → proceed to next question
  - **Will a human gate enforce this?** (code review, approval, manual verification)
    - Yes → **manual enforcement**
    - No → **design-only**
6. **Plan enforcement (if hook-based)** — for rules requiring hooks:
  - **Identify trigger:** When should policy be checked? (SessionStart, PreToolUse, PostToolUse, Stop, etc.)
  - **Identify matcher & condition:** What pattern triggers this rule? (e.g., `Bash(git push --force)`)
  - **Choose handler type:** See Handler Type Decision Tree in Resources
  - **Note dependencies:** Does this hook depend on output from another hook?
7. **Draft rules** — for each policy, populate a rule file using `claude-bootstrap/docs/preset-templates/rule.md`; ensure:
  - Statement is clear and unambiguous (testable)
  - Scope and exceptions are explicit
  - Enforcement method is documented (design-only, manual, or hook-based)
  - If hook-based: Trigger, matcher/condition, handler type, and example implementation are in Enforcement Mechanism section
8. **Present** — show all rules and enforcement plan to user; iterate until approved
9. **Export templates** — copy rule and hook templates to `.claude/docs/templates/` for reference during agent invocation
10. **Write rule files** — create rule files in `.claude/rules/` for all approved rules
11. **Create hook files** — for each rule determined hook-based in Step 5, create corresponding `.claude/hooks/[name].md` file, populating trigger, matcher, handler type, and testing scenarios from the rule's Enforcement Mechanism section

## Examples
Each example shows SPEC requirement, derived rules, and enforcement method decisions.
- **Web app:** Derive "No direct production database access from dev agents" (hook blocks prod credentials), "No destructive schema ops without rollback plan" (design-only; reviewer checks), "All migrations must have up/down definitions" (hook validates migration structure)
- **Bootstrap framework:** Derive "Never modify project files from bootstrap phase" (design-only; documented scope boundary), "Never force-push main branch" (hook blocks command), "Commit message format" (hook warns if non-compliant)
- **Automation tool:** Derive "No credentials in logs" (hook detects and redacts), "All deployments logged" (design-only; ops team audit), "Breaking changes require approval" (manual; reviewer gate)

## Edge Cases
- **Rule with no hook:** Acceptable (design-only constraints, manual enforcement). Document in rule's Enforcement Mechanism section.
- **Multiple hooks enforce one rule:** OK if dependencies are clear; document which hook output feeds into which.
- **Rule conflicts with SPEC:** Surface to user; resolve conflict before proceeding.
- **Ambiguous constraint:** Clarify with user; rule statement must be testable (not vague).
- **Hook with no rule:** Acceptable for convenience (auto-format, logging). Document as "Design convenience — no policy enforced".
- **No rules needed:** Acceptable for simple projects; note in CLAUDE.md.

## Resources

### Handler Type Decision Tree

**Default to `command`.** Escalate to `prompt` → `agent` → `http` only as complexity increases.
- **`command`** — Local validation (git check, file validation, format check)
- **`prompt`** — Ambiguous policy decision (ask Claude to interpret rule)
- **`agent`** — Multi-step verification (run tests + parse results, verify state)
- **`http`** — Remote system (webhook, audit service, compliance check)

### Rule Derivation Coverage
1. From SPEC:
  - Platform/compliance constraints identified
  - Immutability requirements clear
  - Data safety boundaries defined
  - Team/process requirements captured
2. From agents:
  - Role boundaries are enforced
  - Scope constraints are explicit
  - Tensions are managed by rules
3. From plan phases:
  - Destructive operations are guarded
  - Validation gates are specified
  - Approval chains are defined

### Hook Specificity Validation
For each hook-based rule:
- Matcher is as narrow as possible (no wildcards unless intentional)
- Condition further narrows scope (e.g., `Bash(rm -rf)` not just `Bash`)
- False positive rate is <1% (test with benign examples)
- Handler exits cleanly with clear deny/allow decision
- Blocker hooks have meaningful error messages
- Testing scenarios cover both compliant and violation cases

### Output schema

```yaml
planning_rules:
  rules:
    [rule-name]:
      description: One-liner description
      scope: bootstrap | project | both | phase
      enforcement-method: hook | manual | design
  hooks:
    [hook-name]:
      rule: [which rule this implements]
      trigger: [event type]
      handler-type: [command | prompt | agent | http]
  user_confirmed: true | false
```
