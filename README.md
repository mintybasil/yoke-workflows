# Yoke Workflows

A collection of workflow definitions for [Yoke](https://github.com/mintybasil/yoke) — a Rust daemon that receives webhook events from GitHub/GitLab and runs multi-step agent workflows through the Hermes Agent REST API.

## Quick Start

1. **Copy the example config**:
   ```bash
   cp config.example.toml config.toml
   ```

2. **Edit `config.toml`**:
   - Add your repositories to the `repos` array
   - Configure your Hermes API agent instances
   - Set your webhook secret (or use `WEBHOOK_SECRET` env var)

3. **Set environment variables**:
   ```bash
   export HERMES_API_KEY="your-hermes-api-key"
   export WEBHOOK_SECRET="your-webhook-secret"
   export GITHUB_TOKEN="your-github-token"
   ```

4. **Register webhooks**:
   ```bash
   yoke --config config.toml webhooks add --workflows .
   ```

5. **Run Yoke**:
   ```bash
   yoke --config config.toml --workflows .
   ```

## Available Workflows

### implement-issue.toml

**Trigger**: Issue assigned to `zeroklaw`

**Steps**:
1. **Plan** — Reads the GitHub issue and creates an implementation plan using the `writing-plans` skill
2. **Implement** — Executes the plan, runs tests, opens a draft PR, waits for CI, resolves conflicts, and requests review

### issue_mention.toml

**Trigger**: User `zeroklaw` is @mentioned in an issue comment

**Steps**:
1. **Review and Respond** — Reviews the issue thread and responds to the mention

### pr-review-comment.toml

**Trigger**: PR review submitted by `mintybasil`

**Steps**:
1. **Address Review** — Reacts to the review, analyzes feedback, implements changes, and responds

## Configuration

### config.toml

Global configuration defining:
- Platform (`github` or `gitlab`)
- Repositories to monitor
- Named Hermes API agent instances
- Runtime settings (concurrency, workdir)
- Server settings (host, port, webhook secret)

See `config.example.toml` for a complete example.

### Workflow Files

Each `.toml` file defines:
- `[trigger]` — Event type and filters
- `[git]` — Git clone/worktree settings
- `[[steps]]` — Sequential steps with agent, prompt template, and hooks

## Template Variables

Available in all prompt templates:
- `{{owner}}` — Repository owner
- `{{repo}}` — Repository name
- `{{output_dir}}` — Per-event workspace directory

Trigger-specific variables:
- `{{issue_number}}` — Issue number (for issue triggers)
- `{{pr_number}}` — PR number (for PR triggers)
- `{{review_id}}` — Review ID (for PR review triggers)
- `{{mentioned_user}}` — Username that was mentioned

## Hooks

Steps support pre/post hooks for validation:

```toml
[[steps.post_hooks]]
type = "file_not_empty"
path = "{{output_dir}}/plan.md"
```

Available hook types:
- `file_not_empty` — Checks file exists and has content
- `file_contains` — Checks file contains a specific string

## License

MIT
