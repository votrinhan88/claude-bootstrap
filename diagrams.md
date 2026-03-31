# Bootstrap Architecture Diagrams

---

## Diagram 1: Bootstrap Workflow

```mermaid
flowchart TD
    subgraph s0["<b style='font-size:18px'>📦 BOOTSTRAP SESSION</b>"]
        direction LR
        skill-bootstrap("🔧 /bootstrap")
        user-interview("👤 User Interview")
        system-scaffold("⚙️ System<br/>Scaffold infrastructure")
        artifacts-scaffold("<div style='text-align:left'>📂 .claude/<br/>&nbsp;&nbsp;├─ 📋 docs/<br/>&nbsp;&nbsp;├─ 🧠 state/<br/>&nbsp;&nbsp;└─ 📄 CLAUDE.md</div>")
        complete-0["✅ Complete"]
    end

    subgraph s1["<b style='font-size:18px'>📋 PLANNING SESSION</b>"]
        direction LR
        skill-plan("🔧 /plan")
        system-specs("⚙️ System<br/>Draft specs")
        artifacts-specs("📋 .claude/docs/SPEC.md<br/>🧠 .claude/state/PLAN.md")
        user-review{"👤 User Review"}
        complete-1["✅ Complete"]
    end

    skill-bootstrap --> user-interview --> system-scaffold --> artifacts-scaffold --> complete-0
    skill-plan --> system-specs --> artifacts-specs --> user-review -->|approve| complete-1
    user-review -->|reject| system-specs
    s0 --> s1

    style s0 fill:transparent,stroke:darkdodgerblue,stroke-width:5px
    style s1 fill:transparent,stroke:darkdodgerblue,stroke-width:5px

    style skill-bootstrap fill:orange,color:black,stroke:none
    style skill-plan fill:orange,color:black,stroke:none

    style user-interview fill:dodgerblue,color:white,stroke:none
    style user-review fill:dodgerblue,color:white,stroke:none

    style system-scaffold fill:purple,color:white,stroke:none
    style system-specs fill:purple,color:white,stroke:none

    style artifacts-scaffold fill:gold,color:black,stroke:none
    style artifacts-specs fill:gold,color:black,stroke:none

    style complete-0 fill:forestgreen,color:white,stroke:none
    style complete-1 fill:forestgreen,color:white,stroke:none
```

---

## Diagram 2: Runtime Workflow

```mermaid
graph LR
    skill-orchestrate("🔧 /orchestrate")
    skill-orchestrate --> orchestrator
    style skill-orchestrate fill:orange,color:black,stroke:none

    skill-health("🔧 /health")
    skill-health --> state
    style skill-health fill:orange,color:black,stroke:none

    subgraph system["⚙️ RUNTIME"]
        direction TB
        orchestrator("🤖 Orchestrator")
        agent-role("🤖 Role agents")
        state("🧠 State")

        orchestrator -->|assign| agent-role -->|report| orchestrator
        state <--> orchestrator

        style orchestrator fill:hotpink,color:white,stroke:none
        style agent-role fill:hotpink,color:white,stroke:none
        style state fill:gold,color:black,stroke:none
    end
    style system fill:transparent,stroke:purple,stroke-width:5px
    orchestrator --> if-blocked

    subgraph decision["🚨 DECISION FLOW"]
        direction TB
        if-blocked{"🚨 Blocker?"}
        if-gated{"🚪 Gated?"}
        if-auto{"🔄 <code style='color:black'>--auto</code>?"}
        outcome-blocked["❌ Blocked"]
        outcome-gated["⚠️ Gated"]
        outcome-yield["✅ Yielded"]

        if-blocked -->|Yes| outcome-blocked
        if-blocked -->|No| if-gated
        if-gated -->|Yes| outcome-gated
        if-gated -->|No| outcome-yield
        outcome-yield --> if-auto
        if-auto -->|Yes| orchestrator

        style if-blocked fill:lightyellow,color:black,stroke:none
        style if-gated fill:lightyellow,color:black,stroke:none
        style if-auto fill:lightyellow,color:black,stroke:none
        style outcome-blocked fill:red,color:white,stroke:none
        style outcome-gated fill:orange,color:black,stroke:none
        style outcome-yield fill:forestgreen,color:white,stroke:none
    end
    style decision fill:transparent,stroke:orange,stroke-width:3px
    outcome-blocked --> user-escalate
    outcome-gated --> user-review
    if-auto -->|No| user-review

    subgraph user["👤 USER"]
        direction TB
        user-review("👀 User Review")
        user-escalate("📢 User Escalate")

        style user-review fill:dodgerblue,color:white,stroke:none
        style user-escalate fill:dodgerblue,color:white,stroke:none
    end
    style user fill:transparent,stroke:dodgerblue,stroke-width:5px
```

**Runtime note:** State-control skill handles CONTEXT.md curation; callers (orchestrate, health) determine which log type to write (orchestrate writes checkpoint at phase gates; health writes handoff when user escalates to Level 3).

---

## Diagram 3b: Eval Workflow

```mermaid
flowchart LR
    persona["👤 Create persona"]
    simulate["⚙️ Simulate"]
    results["📊 Results"]
    score["🎯 Score<br/>4 dimensions"]
    Pass["✅ PASS"]
    Partial["⚠️ PARTIAL"]
    Fail["❌ FAIL"]

    style persona fill:dodgerblue,color:white,stroke:none
    style simulate fill:purple,color:white,stroke:none
    style results fill:purple,color:white,stroke:none
    style score fill:purple,color:white,stroke:none
    style Pass fill:forestgreen,color:white,stroke:none
    style Partial fill:gold,color:black,stroke:none
    style Fail fill:red,color:white,stroke:none

    persona --> simulate
    simulate --> results
    results --> score
    score --> Pass
    score --> Partial
    score --> Fail
```

---

## Scope Boundary

**Bootstrap framework** (diagrams 1–3) — provides methodology templates and patterns. Bootstrap agents operate read-only on project files; write only to `.claude/` and CLAUDE.md.

**Project scope** — downstream agents own runtime tasks, customized skills, and project deliverables.

---

## Quick Reference

**Diagram 1:** User → CLI → System flow across three sessions (Bootstrap, Planning, Runtime), producing key artifacts.

**Diagram 2:** Runtime loop showing how `/orchestrate` and `/health` read/update state, with decision points for hard stops, gates, and yields.

**Diagram 3:** Eval framework execution pipeline: pick scenario → run → capture artifacts → score → pass/partial/fail.

---

## Legend

| Symbol | Color | Meaning |
| --- | --- | --- |
| 🔵 | dodgerblue | User input/decisions |
| 🟠 | orange | CLI interface / Commands |
| 🟣 | purple | System processing |
| 🤖 | hotpink | Agents |
| 🟡 | gold | Data/State |
| 🚪 | lightyellow | Decision gates/conditions |
| ✅ | forestgreen | Pass / Yielded (success) |
| ⚠️ | orange | Partial / Gated (pending decision) |
| ❌ | red | Fail / Blocked (hard stop) |
| 🔧 | — | CLI command / Skill |
| 👤 | — | User / Persona |
| ⚙️ | — | System / Processing |
| 📂 | — | Directory |
| 📋 | — | Documentation / Specs |
| 📄 | — | File |
| 🧠 | — | State / Brain (memory) |
| 📢 | — | Escalation / Alert |
| 👀 | — | Review / Observation |
| 🚨 | — | Alert / Blocker |
| 🔄 | — | Auto / Refresh / Cycle |
| 📊 | — | Results / Analytics |
| 🎯 | — | Score / Target |
| 🚪 | — | Checkpoint / Phase boundary |
