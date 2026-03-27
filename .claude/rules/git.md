**Never:**
- Force-push to `main`
- Use `--no-verify` to skip hooks
- Amend commits that have already been shared

**Only with user confirmation:**
- Commit directly to `main` (unless trunk-based mode is configured)
- Delete branches before squash-merge is confirmed
- Run destructive operations (`git restore .`, `git reset --hard`, `git clean -f`)
