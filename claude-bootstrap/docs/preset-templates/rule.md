---
name: [rule-name]
description: One-line summary of the constraint
scope: [bootstrap | project | both | specific-phase]
enforcement: [hard-stop | warning | logging | design-constraint]
related-hooks: [list of hook names that implement this rule, if any]
metadata:
  authors: [votrinhan88]
  version: 0.1
---

# [Rule Name]

## Purpose
[2-3 sentences: Why does this rule exist? What risk or failure mode does it prevent?]

**Example:** Prevent force-pushing to main branch, which can destroy shared work and break CI/CD pipelines. Once code is merged to main, history should be immutable.

## The Rule (Statement)
[Clear, unambiguous constraint. Use imperative form.]

**Example:**
- **MUST:** Never force-push to `main` branch
- **MUST:** Commit messages follow `<type>(<scope>): <message>` format
- **MUST NOT:** Run destructive operations (schema drops, migrations) without user confirmation
- **SHOULD:** Tag breaking changes with `BREAKING:` in commit message

## Scope

### Applies To
- **When:** [which phases, workflows, or contexts]
- **Who:** [bootstrap agents, project agents, users, specific roles]
- **Where:** [which files, directories, or domains]

**Example:**
- **When:** Anytime code is about to be pushed to main
- **Who:** All agents and users
- **Where:** Git operations on the main branch

### Does NOT Apply To
- [scenarios where rule is suspended or doesn't matter]

**Example:**
- Local feature branches (pre-merge)
- Squash-merge commits (allowed to rewrite if not yet merged)

## Consequences of Violation

### Severity
[critical | high | medium | low]

### Impact
[What goes wrong if this rule is broken?]

**Example (critical):**
- Loss of git history and previous commits
- Breaks CI/CD pipelines for downstream consumers
- Requires manual intervention to recover code
- Undermines code review and audit trail

### Detection
[How will this violation be discovered?]

**Example:**
- Automated hook blocks the `git push --force` command before it happens
- Git server-side hooks reject force-push
- Post-mortem audit of git log shows force-push occurred

## Enforcement Mechanism

### How This Rule is Enforced
[Design constraint | documentation | hook | pre-commit check | permissions | code review | other]

**Example:** Hook (automated block) + code review (manual verification)

### If Hook-Based Enforcement

**When to use a hook:** Rule violations are deterministic (can be automated) and need blocking or immediate warning.

#### Trigger & Scope
- **Trigger:** [SessionStart | UserPromptSubmit | PreToolUse | PermissionRequest | PostToolUse | PostToolUseFailure | Stop | FileChanged | ConfigChange | CwdChanged]
- **Tool/Matcher:** [e.g., Bash, Edit, Glob — what triggers this rule violation?]
- **Condition:** [Optional: narrow scope with pattern, e.g., `Bash(git push --force)`]
- **Blocking:** [true | false — should hook prevent execution?]

**Example:**
- **Trigger:** PreToolUse
- **Matcher:** Bash
- **Condition:** `Bash(git push --force|git reset --hard)` on main branch
- **Blocking:** true

#### Handler Type
- **Type:** [command | prompt | agent | http]
- **Description:** [Brief explanation of handler]

**Examples by complexity:**
- **command:** Local git check, file validation, format check
- **prompt:** Ambiguous policy decision (ask Claude to interpret rule)
- **agent:** Multi-step verification (run tests, parse results, verify state)
- **http:** Remote audit service, webhook notification

#### Implementation Sketch
[Pseudo-code or bash snippet showing enforcement logic. Keep under 20 lines.]

**Example:**
```bash
# Check if git command violates immutability rule
COMMAND="$1"
if echo "$COMMAND" | grep -qE 'git push --force|git reset --hard'; then
  # Check if on main branch
  BRANCH=$(git rev-parse --abbrev-ref HEAD 2>/dev/null || echo "")
  if [[ "$BRANCH" == "main" ]]; then
    echo "Rule violation: force operations on main blocked"
    exit 2  # Deny
  fi
fi
exit 0  # Allow
```

### Manual Enforcement
[If not fully automated: how should agents/users verify compliance?]

**Example:**
- Code reviewer checks commit message format in PR diff
- CI/CD linter validates commit message on every push
- Oncall engineer spots-checks git log during incident

## Examples

### ✓ Compliant

**Example 1:** Regular merge to main
```bash
git checkout main
git merge feature/my-feature
git push origin main
```
Status: ✓ Allowed. No force-push, history intact.

**Example 2:** Commit message with proper format
```
feat(auth): add OAuth2 support

Adds OAuth2 provider integration with Google and GitHub.
Closes #123.
```
Status: ✓ Compliant. Format is `feat(scope): message`.

### ✗ Violations

**Example 1:** Force-push to main
```bash
git push --force origin main
```
Status: ✗ Blocked. Hook rejects; Claude sees policy explanation.

**Example 2:** Commit message missing scope
```
Add OAuth support
```
Status: ✗ Non-compliant. Missing `<scope>` field.

## Exceptions & Edge Cases

### When Can This Rule Be Suspended?
[Describe legitimate exceptions]

**Example:**
- **During emergency:** If a critical bug is deployed to prod, rule can be suspended temporarily with explicit user confirmation
- **During migration:** Bulk schema changes require one-time exception; logged with incident ticket
- **At project end:** If sunsetting a repo, can use force-push to clean up history

### How to Request an Exception
[Process for overriding the rule]

**Example:**
- Surface blocker to user explicitly: "Rule 'No force-push to main' would block this. Override? [Yes/No]"
- Log exception with timestamp, reason, and user confirmation in `.claude/state/CONTEXT.md`
- Do not proceed without explicit confirmation

## Related Rules & Hooks

- **Rules:** [Other rules this interacts with]
- **Hooks:** [Hooks that enforce or support this rule]
- **SPEC:** [Reference to spec section that justified this rule]
- **PLAN:** [Reference to plan phases that depend on this rule]

**Example:**
- **Related Rules:** "No destructive operations without confirmation" (complements this rule)
- **Hook:** `.claude/hooks/guard-git-main.md`
- **SPEC:** Scope § "Immutable main branch required for CI/CD integrity"
- **PLAN:** Phase 5 "Deploy to production" requires this rule active

## Implementation Checklist

- [ ] Rule is documented and approved
- [ ] Hook(s) implementing this rule are written and tested
- [ ] Team is aware of rule and consequences
- [ ] Exception process is clear
- [ ] Monitoring/audit is in place to detect violations

---

**Scope:** Keep rule files under 200 lines. Rule now documents both *why* and *how* to enforce. For complex hook implementations, reference `.claude/hooks/[name].md` for full details.