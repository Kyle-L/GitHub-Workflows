# GitHub-Workflows

A collection of reusable GitHub Actions for common CI/CD tasks.

## Available Actions

- [Dependabot Auto-merge](#dependabot-auto-merge) - Automatically enable auto-merge for Dependabot PRs
- [Discord Notification](#discord-notification) - Send rich Discord notifications with workflow status
- [Free Disk Space](#free-disk-space) - Free up disk space on Ubuntu runners before resource-intensive builds

---

## Dependabot Auto-merge

Automatically enables auto-merge for Dependabot pull requests using the GitHub CLI.

### Usage

```yaml
name: Dependabot Auto Merge
on: pull_request

permissions:
  contents: write
  pull-requests: write

jobs:
  dependabot:
    runs-on: ubuntu-latest
    if: github.actor == 'dependabot[bot]'
    steps:
      - name: Enable auto-merge for Dependabot PRs
        uses: Kyle-L/GitHub-Workflows/.github/actions/dependabot-auto-merge@main
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

## Discord Notification

Send rich Discord notifications with commit info, dynamic gradient images, and workflow status.

### Inputs

| Input | Required | Description |
|-------|----------|-------------|
| `webhook` | Yes | Discord webhook URL |
| `status` | Yes | Status of the workflow (`starting`, `success`, `failure`, `cancelled`, `live`) |
| `title` | Yes | Title for the notification |
| `description` | No | Description for the notification |
| `commit_sha` | Yes | Full commit SHA |
| `author_name` | Yes | Author username |
| `repository` | Yes | Repository (owner/repo) |
| `server_url` | Yes | GitHub server URL |
| `run_id` | Yes | Workflow run ID |

### Usage

```yaml
name: Build and Deploy
on: push

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      # Send starting notification
      - name: Discord - Starting
        uses: Kyle-L/GitHub-Workflows/.github/actions/discord-notify@main
        with:
          webhook: ${{ secrets.DISCORD_WEBHOOK }}
          status: 'starting'
          title: 'Build Starting'
          description: 'Building the project...'
          commit_sha: ${{ github.sha }}
          author_name: ${{ github.actor }}
          repository: ${{ github.repository }}
          server_url: ${{ github.server_url }}
          run_id: ${{ github.run_id }}

      # Your build steps here...

      # Send success notification
      - name: Discord - Success
        if: success()
        uses: Kyle-L/GitHub-Workflows/.github/actions/discord-notify@main
        with:
          webhook: ${{ secrets.DISCORD_WEBHOOK }}
          status: 'success'
          title: 'Build Successful'
          description: 'The build completed successfully!'
          commit_sha: ${{ github.sha }}
          author_name: ${{ github.actor }}
          repository: ${{ github.repository }}
          server_url: ${{ github.server_url }}
          run_id: ${{ github.run_id }}

      # Send failure notification
      - name: Discord - Failure
        if: failure()
        uses: Kyle-L/GitHub-Workflows/.github/actions/discord-notify@main
        with:
          webhook: ${{ secrets.DISCORD_WEBHOOK }}
          status: 'failure'
          title: 'Build Failed'
          description: 'The build encountered an error.'
          commit_sha: ${{ github.sha }}
          author_name: ${{ github.actor }}
          repository: ${{ github.repository }}
          server_url: ${{ github.server_url }}
          run_id: ${{ github.run_id }}
```

### Status Colors

| Status | Color | Gradient |
|--------|-------|----------|
| `starting` | Blue | Deep blue → Light blue |
| `success` | Green | Dark green → Bright green |
| `failure` | Red | Dark red → Bright red |
| `cancelled` | Gray | Dark gray → Light gray |
| `live` | Purple | Dark purple → Bright purple |

---

## Free Disk Space

Frees up disk space on Ubuntu GitHub Actions runners by removing optional runtimes, packages, Docker images, and swap storage. Useful before resource-intensive build or test steps.

### Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `android` | No | `true` | Remove Android runtime |
| `dotnet` | No | `true` | Remove .NET runtime |
| `haskell` | No | `true` | Remove Haskell runtime |
| `large-packages` | No | `true` | Remove large packages (LLVM, PHP, MongoDB, MySQL, Azure CLI, etc.) |
| `docker-images` | No | `true` | Remove Docker images |
| `tool-cache` | No | `false` | Remove image tool cache |
| `swap-storage` | No | `true` | Remove swap storage |

### Usage

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Free Disk Space
        uses: Kyle-L/GitHub-Workflows/.github/actions/free-space@main

      - uses: actions/checkout@v4

      # Your build steps here...
```

You can disable individual cleanups if your workflow depends on those tools:

```yaml
      - name: Free Disk Space
        uses: Kyle-L/GitHub-Workflows/.github/actions/free-space@main
        with:
          dotnet: 'false'
          tool-cache: 'true'
```

---

## License

MIT
