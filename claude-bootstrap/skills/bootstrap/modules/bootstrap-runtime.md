# Bootstrap Runtime

> **Note:** This skill is a placeholder — content is incomplete.

Operational signals and feedback patterns. Reference during any session.

---

## Pivot Protocol

When a Tier 2 escalation reveals a fundamental problem — wrong architecture, wrong technology, spec error.

**Agent:**

1. Freeze implementation
2. Log the pivot trigger to .claude/state/logs/ with full context
3. Update SPEC.md with the new understanding
4. Re-evaluate roles: still valid?
5. Re-plan from current state (preserve completed work)
6. Update CONTEXT.md
7. Present the pivot to user for confirmation before resuming

---

## Session Discipline

When you encounter these signals, act as follows:

| Signal                      | Action                                                                                                                                                    |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Starting a session          | Read CONTEXT.md. Read last 2-3 entries in .claude/state/logs/ if needed. Verify against actual project state (git log, outputs/artifacts, files). Reconcile. |
| Ending a session            | Curate CONTEXT.md (remove stale entries). Append handoff entry to .claude/state/logs/.                                                     |
| Completing a plan phase     | Phase gate review. If git: squash-merge phase branch. Then fresh session.                                                                                 |
| Resolving Tier 2 escalation | Fresh session.                                                                                                                                            |
| Context at ~50%             | `/compact` — preserve modified files, plan step, blockers.                                                                                                |
| Already compacted once      | Fresh session.                                                                                                                                            |
| Claude ignoring rules       | Context pressure — fresh session.                                                                                                                         |
| Switching tasks mid-session | `/clear`                                                                                                                                                  |
| Quick throwaway question    | `/btw [question]`                                                                                                                                         |
| Tuning reasoning depth      | `/effort low\|med\|high` or `ultrathink` for one-off                                                                                                      |
| Switching model tiers       | `/set-models default` or `/set-models opus sonnet sonnet`                                                                                                 |

---

## Iteration Patterns

Feedback loops for refining work in progress.

### Annotation cycle

Output is close but not right:

1. Add `> NOTE:` inline in the file
2. `Address all notes. Verify outputs.`
3. Repeat until clean.

### Scrap and redo

Refinement is the wrong move:

```
Knowing everything you know now, scrap this and implement the
elegant solution from scratch.
```

### Adversarial pass

Stress-test completed work:

```
Grill me on these changes. What breaks in production? What
edge cases did we miss? Don't proceed until I pass your test.
```