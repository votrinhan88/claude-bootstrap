# Evaluation Framework — Runtime Directory

**📖 Full documentation:** See [`docs/eval/README.md`](../docs/eval/README.md)

This directory (`eval/`) contains runtime outputs from bootstrap pipeline simulations.

---

## Quick Reference

**To run an evaluation:**
1. Pick a test scenario from [`docs/eval/scenarios/`](../docs/eval/scenarios/)
2. Follow setup instructions
3. Run a simulation through STARTUP.md
4. Record results here using [`docs/eval/templates/result-template.md`](../docs/eval/templates/result-template.md)
5. Analyze against [`docs/eval/METRICS.md`](../docs/eval/METRICS.md)

**To understand the framework:**
- Read [`docs/eval/README.md`](../docs/eval/README.md) — overview and workflow
- Read [`docs/eval/METRICS.md`](../docs/eval/METRICS.md) — scoring rubric
- Review [`docs/eval/scenarios/`](../docs/eval/scenarios/) — test cases

---

## This Directory

```
eval/
├── README.md (this file — quick reference)
├── results/ (simulation outputs, one file per run)
│   └── {date}_{testcase}_{run-number}.md
└── summary.md (aggregate results dashboard)
```

**Note:** Static documentation is in `docs/eval/`. This directory is for runtime accumulation of results.
