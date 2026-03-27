Here's a comprehensive breakdown of how Claude skills work and the battle-tested template to use:

---

## What is a Claude Skill?

A **skill** is a Markdown file (`SKILL.md`) that gives Claude specialized knowledge, step-by-step workflows, and bundled resources for a specific task. Claude reads it at runtime to guide its behavior — think of it as a reusable instruction manual Claude loads on demand.

---

## The Battle-Tested Skill Template

```
my-skill/
├── SKILL.md              ← Required: the brain of the skill
└── (optional extras)
    ├── scripts/          ← Executable code for repetitive tasks
    ├── references/       ← Docs loaded into context as needed
    └── assets/           ← Templates, fonts, icons
```

Here's the proven `SKILL.md` structure:

```markdown
---
name: my-skill
description: >
  What this skill does AND when to trigger it. Be specific about
  trigger phrases. Make it slightly "pushy" — e.g., "Use this skill
  whenever the user mentions X, Y, or Z, even if they don't say it explicitly."
---

# Skill Name

One-liner about what this skill enables.

## When to Use
- Trigger phrase A
- Trigger phrase B

## Step-by-Step Workflow
1. First, do X
2. Then, do Y
3. Finally, produce Z

## Output Format
ALWAYS use this exact structure:
[define the template here]

## Examples
**Example 1:**
Input: ...
Output: ...

## Edge Cases
- If X happens, do Y instead
- For Z inputs, warn the user that...
```

---

## The Three-Level Loading System

Skills use **progressive disclosure** — only what's needed is loaded:

| Level | What's loaded | Size limit |
|---|---|---|
| 1. Metadata | `name` + `description` only | ~100 words, always in context |
| 2. SKILL.md body | Full instructions | <500 lines ideal |
| 3. Bundled resources | Scripts/references | Unlimited, loaded on demand |

---

## Key Principles from Industry Practice

**Descriptions make or break triggering.** Claude decides whether to consult a skill based solely on the description. Too vague = undertriggering. The fix: be specific about trigger phrases and make it slightly assertive ("use this skill whenever...").

**Write instructions in imperative form.** "Extract the table" not "The model should extract the table."

**Explain *why*, not just *what*.** Claude performs better when it understands the reasoning behind a rule, not just the rule itself.

**For large reference material**, split into a `references/` folder with a table of contents in `SKILL.md`, and tell Claude *when* to read each file.

**Domain variants** (e.g., AWS vs GCP vs Azure) should each have their own reference file — Claude reads only the one relevant to the current task.

---

## The Iteration Loop (how pros build skills)

1. **Draft** — write the `SKILL.md` based on the workflow
2. **Write 2–3 test prompts** — realistic things a real user would say
3. **Run Claude with the skill** on those prompts
4. **Evaluate** — qualitatively review outputs, add quantitative assertions for objectively verifiable things
5. **Revise** — improve the skill based on failures
6. **Repeat** until satisfied, then expand the test set

---

Want me to help you build a skill for a specific workflow you have in mind? I can draft the full `SKILL.md` and test cases for you.