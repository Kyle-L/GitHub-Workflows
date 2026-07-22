# Template: `minimal`

No image. Status emoji prefixes the title. All three fields — Commit, Author, Workflow — are packed into a single inline row.

```text
┌─────────────────────────────────────────────────────────┐
│  ✅ Deploy to Production                                │
│  Deployment triggered on main.                          │
│                                                         │
│  Commit         │  Author         │  Workflow           │
│  `a1b2c3d4`     │  kyle-lierer    │  View Run           │
└─────────────────────────────────────────────────────────┘
```

**Status emoji map:**

| Status    | Emoji |
| --------- | ----- |
| starting  | 🔵    |
| success   | ✅    |
| failure   | ❌    |
| cancelled | ⏹️    |
| live      | 🟣    |

**When to use:** High-volume pipelines, noisy channels, or anywhere you want compact notifications that don't dominate the feed. No banner image is generated so there are no external requests.

**Embed fields:**

- No image
- `Commit` (inline) — short SHA linked to commit
- `Author` (inline) — committer username
- `Workflow` (inline) — linked "View Run"

**Example usage:**

```yaml
- uses: ./.github/actions/discord-notify
  with:
    webhook: ${{ secrets.DISCORD_WEBHOOK }}
    status: success
    title: Deploy to Production
    description: Deployment triggered on main.
    commit_sha: ${{ github.sha }}
    author_name: ${{ github.actor }}
    repository: ${{ github.repository }}
    server_url: ${{ github.server_url }}
    run_id: ${{ github.run_id }}
    template: minimal
```
