# Health Report — [date]

## 1. Orient
- State files: CONTEXT.md, SPEC.md, PLAN.md, last 3–5 log entries [✅/⚠️/❌]
- Git [✅/⚠️/❌] *(or "disabled")*
  - Uncommitted changes: [none / list]
  - Active branches: [list]
  - Open PRs: [none / list]
  - Stale branches: [none / list]
  - Commits since last phase merge: [N]

## 2. Validate
- CONTEXT.md hygiene [✅/⚠️/❌]
  - Stale entries: [none / list entries — curate immediately if found]
  - Decisions no longer relevant: [none / list]

## 3. Operational
- Completion [✅/⚠️/❌] — X/Y tasks (Z%), Phase N
- Blockers [✅/⚠️/❌]
  - [type: blocker] [name] — Phase N, N phases old — [resolution path or "awaiting user decision"]
  - ...

## 4. Strategic
- Spec drift [✅/⚠️/❌]
  - [deviation] [what changed and why] — [impact on remaining plan]
  - ...
- Agent health [✅/⚠️/❌]
  - Unused: [agent name] — never referenced in logs or context → recommend retirement
  - Missing: [role name] — referenced in logs but no agent file
  - One-sided tension: [agent A] lists [agent B] but [agent B] does not list [agent A]
  - Tensionless: [agent name] — no tensions declared
- Role adequacy *(periodic retrospectives only)* [✅/⚠️/❌]
  - Roles to add: [none / list]
  - Roles to merge or retire: [none / list]
  - Plan changes warranted: [none / list]

## Next Actions
- [⚠️ or ❌ finding] → [PLAN.md task reference, or flag: plan needs revision]
- ...
