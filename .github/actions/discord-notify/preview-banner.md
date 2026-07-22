# Template: `banner` (default)

A full-width gradient image sits above the message body. Commit and Author are inline; Workflow Run is its own full-width field beneath them.

```text
┌─────────────────────────────────────────────────────────┐
│ ░░░░░░░░░░ gradient image · title text ░░░░░░░░░░░░░░░░ │
├─────────────────────────────────────────────────────────┤
│  Deploy to Production                                   │
│  Deployment triggered on main.                          │
│                                                         │
│  Commit         │  Author                               │
│  `a1b2c3d4`     │  kyle-lierer                          │
│                                                         │
│  Workflow                                               │
│  View Run                                               │
└─────────────────────────────────────────────────────────┘
```

**Banner URL format:**
`https://via.assets.so/img.jpg?w=1200&h=200&gradientFrom=<from>&gradientTo=<to>&gradientAngle=135&text=<title>&textColor=FFFFFF&f=png`

[Example banner (success)](https://via.assets.so/img.jpg?w=1200&h=200&gradientFrom=196f3d&gradientTo=28a745&gradientAngle=135&text=Deploy+to+Production&textColor=FFFFFF&f=png)

**When to use:** Any standard CI/CD notification where visual brand presence matters.

**Embed fields:**

- Banner image — gradient generated from status color pair
- `Commit` (inline) — short SHA linked to commit
- `Author` (inline) — committer username
- `Workflow` — linked "View Run" on its own row

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
    template: banner
```
