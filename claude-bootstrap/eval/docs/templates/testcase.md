# Test Case Template

---
## Test Case Metadata

**Name:** {descriptive-name}
**ID:** {short-id: new-simple-exp, existing-complex-new, etc.}
**Purpose:** {What question does this test case answer?}
**Expected Outcome:** {What phases/artifacts should be completed?}

---


---

## Expected Flow

**Expected phases to reach:**
- [ ] Bootstrap session, Step 1-8 (Bootstrap) — YES/NO
- [ ] Planning session, Steps 1-6 (Planning) — YES/NO
- [ ] Runtime — YES/NO

**Expected decision points:**
1. {Decision point}: {Expected choice}
2. {Decision point}: {Expected choice}

**Expected artifacts:**
- [ ] `.claude/CLAUDE.md`
- [ ] `.claude/docs/SPEC.md`
- [ ] `.claude/agents/` (orchestrator + [list roles])
- [ ] `.claude/state/CONTEXT.md`
- [ ] `.claude/state/logs/` (interview handoff)
- [ ] `.claude/state/PLAN.md` (if Planning session reached)

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