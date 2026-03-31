---
name: [hook-name]
description: One-line description of what this hook enforces
trigger: [SessionStart | UserPromptSubmit | PreToolUse | PermissionRequest | PostToolUse | PostToolUseFailure | Stop | FileChanged | ConfigChange | CwdChanged]
type: [command | prompt | agent | http]
matcher: [Tool name or regex pattern, e.g., "Bash", "Edit", "Bash.*rm.*"]
condition: [Optional: narrow scope with if-condition, e.g., "Bash(rm -rf *)"]
blocking: [true | false]  # can this hook prevent execution?
metadata:
  authors: [votrinhan88]
  version: 0.1
---

# [Hook Name]

## Purpose
[1-2 sentences: What behavior are we enforcing or automating? Why does this matter?]

**Example:** Block destructive `rm` commands before execution to prevent accidental data loss during rapid iteration.

## Trigger Point
- **Event:** [which trigger from above]
- **Fires when:** [specific condition in English]
- **Blocking behavior:** [what happens if hook denies — does execution stop, or just log?]

**Example:**
- **Event:** PreToolUse
- **Fires when:** User is about to execute a Bash command
- **Blocking behavior:** If hook returns `deny`, Bash command is blocked and Claude sees error message

## Scope (Matcher)
- **Applies to:** [which tool(s), paths, or patterns]
- **Ignores:** [what's explicitly out of scope]
- **Specificity:** [broad vs narrow — document choice to avoid side effects]

**Example:**
```
matcher: "Bash"
condition: "Bash(rm -rf *)" or "Bash(rm.*--force)"
```
This narrows from *all* Bash commands to only dangerous destructive patterns.

## Handler Type & Implementation

### Option 1: Command (Shell Script)
For: Local validation, checking files, running scripts
```bash
#!/bin/bash
# Read input from stdin as JSON
COMMAND=$(jq -r '.tool_input.command' <<< "$1")

if echo "$COMMAND" | grep -qE 'rm -rf|drop table'; then
  jq -n '{
    hookSpecificOutput: {
      hookEventName: "PreToolUse",
      permissionDecision: "deny",
      permissionDecisionReason: "Destructive command blocked by guard hook"
    }
  }'
  exit 2  # Blocking error
else
  exit 0  # Allow
fi
```

### Option 2: Prompt
For: Complex decisions requiring Claude's judgment
```json
{
  "type": "prompt",
  "prompt": "User is running: $COMMAND. Does this violate our destructive operation policy? Approve only if safe.",
  "model": "claude-opus-4-1"
}
```

### Option 3: Agent
For: Multi-step verification (e.g., check build status, git state)
```json
{
  "type": "agent",
  "prompt": "Before this task completes, verify the branch is clean and tests pass. Use Bash and Grep to check."
}
```

## Exit Code Semantics
- **Exit 0:** Success. Parse JSON from stdout for structured decision.
- **Exit 2:** Blocking error. Return reason in JSON; execution stops.
- **Other:** Non-blocking error. Log and continue.

## JSON Response Format
```json
{
  "hookSpecificOutput": {
    "hookEventName": "[EventName]",
    "permissionDecision": "allow" | "deny" | "modify",
    "permissionDecisionReason": "Why this decision was made",
    "additionalContext": "Optional: extra info for Claude"
  }
}
```

## Testing & Verification

### Scenario 1: [Normal case]
- **Input:** [example input]
- **Expected:** [hook allows/logs]
- **Verify:** [how to test]

### Scenario 2: [Violation case]
- **Input:** [example that triggers hook]
- **Expected:** [hook blocks/warns]
- **Verify:** [how to test]

**Example:**
### Scenario 1: Allowed Bash command
- **Input:** `ls -la`
- **Expected:** Hook allows (not a destructive pattern)
- **Verify:** Run in terminal; no hook blocking message appears

### Scenario 2: Blocked destructive command
- **Input:** `rm -rf /home/user/*`
- **Expected:** Hook blocks with reason
- **Verify:** Claude's response shows "Destructive command blocked" message

## Common Pitfalls

- **Too broad matcher:** `matcher: "Bash"` triggers on *every* Bash command → high noise, false positives
  - **Fix:** Narrow with `condition` field to specific patterns
- **Wrong handler type:** Using `prompt` for every decision → slow, expensive
  - **Fix:** Reserve `prompt` for genuinely ambiguous cases; use `command` for deterministic checks
- **Unclear reason:** User sees "Permission denied" with no context
  - **Fix:** Always include `permissionDecisionReason` that explains the policy
- **Async handler blocking:** Using `async: true` on a blocking hook defeats purpose
  - **Fix:** Only use async for logging/audit hooks that don't need to block

## Related

- **Rule:** [Link to `.claude/rules/[rule-name].md` that this hook implements]

---

**Scope:** Keep hook files under 100 lines. Complex logic belongs in agent handlers or external scripts, not inline.