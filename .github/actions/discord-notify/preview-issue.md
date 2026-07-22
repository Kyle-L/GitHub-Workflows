# Template: `issue`

Orange gradient banner with a ⚠️ prefix on the title. Covers both bugs and feature requests. Author field is labelled "Opened by". Color is hardcoded orange regardless of `status`.

```text
┌─────────────────────────────────────────────────────────┐
│ ░░░░░░░░░░ dark-orange → orange gradient banner ░░░░░░░ │
├─────────────────────────────────────────────────────────┤
│  ⚠️ Login crashes when session token is missing         │
│  Reproducible on all environments after commit a1b2.    │
│                                                         │
│  Commit         │  Opened by                            │
│  `a1b2c3d4`     │  kyle-lierer                          │
│                                                         │
│  Workflow                                               │
│  View Run                                               │
└─────────────────────────────────────────────────────────┘
```

**Banner URL format:**
`https://via.assets.so/img.jpg?w=1200&h=200&gradientFrom=9a4400&gradientTo=e67e22&gradientAngle=135&text=<title>&textColor=FFFFFF&f=png`

[Example banner (orange)](https://via.assets.so/img.jpg?w=1200&h=200&gradientFrom=9a4400&gradientTo=e67e22&gradientAngle=135&text=Login+crashes+when+session+token+is+missing&textColor=FFFFFF&f=png)

**When to use:** Bug reports, regression catches, or feature requests — anything that opens a tracking issue. The orange color is always applied, so `status` only affects the embed side-bar accent in Discord's dark theme.

**Embed fields:**

- Banner image — hardcoded orange gradient (`#9a4400` → `#e67e22`)
- `Commit` (inline) — short SHA linked to commit
- `Opened by` (inline) — committer username
- `Workflow` — linked "View Run" on its own row

**Example usage:**

```yaml
- uses: ./.github/actions/discord-notify
  with:
    webhook: ${{ secrets.DISCORD_WEBHOOK }}
    status: failure
    title: Login crashes when session token is missing
    description: Reproducible on all environments after commit a1b2.
    commit_sha: ${{ github.sha }}
    author_name: ${{ github.actor }}
    repository: ${{ github.repository }}
    server_url: ${{ github.server_url }}
    run_id: ${{ github.run_id }}
    template: issue
```
