# Scaling Patterns

| Pattern | Cost | When to use |
|---------|------|-------------|
| Single session | 1x | Default — solo, tight budget |
| Subagent delegation | 1.5–2x | Peer-consult or specialized tasks |
| Parallel worktrees | 3x+ | Independent parallel features |
| Agent teams (experimental) | 2.5–3x | Complex back-and-forth coordination |

**Parallel worktrees:**
```bash
git worktree add ../project-api feature/api-layer
git worktree add ../project-tests feature/test-coverage
```

**Agent teams:**
```bash
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```
