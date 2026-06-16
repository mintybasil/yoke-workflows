# Yoke Workflows

A collection of workflow definitions for [Yoke](https://github.com/mintybasil/yoke) — a Rust daemon that receives webhook events from GitHub/GitLab and runs multi-step agent workflows through the Hermes Agent REST API.

## Workflows

The following workflows are defined in the `workflows/` directory:

- `implement_issue.toml`: Triggered when an issue is assigned. A two-stage workflow where a PM agent creates an implementation plan and an SWE agent implements it.
- `issue_mention.toml`: Triggered when the designated user is mentioned in an issue comment. Used for reviewing and responding to issue-related queries.
- `pr_comment_mention.toml`: Triggered when the designated user is mentioned in a PR comment. Used for responding to feedback or requests for changes on a pull request.
- `pr_review_comment.toml`: Triggered by a PR review. Used to analyze and address review comments.

## Important Notes

- **Skill Dependencies:** Some workflows utilize skills from `obra/superpowers`. These must be loaded into Hermes Agent before running Yoke.
- **Agent Configuration:** Workflows are configured to use two distinct agents (typically a `pm` for planning and an `swe` for implementation). See `config.example.toml` for an example of how to define these agents in your configuration.

For detailed instructions on how Yoke works or how to run it, please refer to the [main Yoke repository](https://github.com/mintybasil/yoke).