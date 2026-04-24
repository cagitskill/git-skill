# Tool Pattern: Issue & PR Management

## What It Does

Combines multiple GitHub MCP tool calls into coordinated issue and pull request workflows. Create, update, comment, list, and close issues and PRs — all from Copilot Chat.

## When to Use It

- You want Copilot to manage your GitHub issue backlog
- You need to create PRs, add reviewers, or comment on reviews from the chat interface
- You're building automated triage or review workflows

## When Not to Use It

- You only need to read repository content — use [Repository Access](../direct-api-wrapper/definition.md)
- You need actions across multiple repos — use [Multi-Repo Coordination](../hierarchical-mcp/definition.md)

## How It Works

```
Copilot receives prompt about an issue or PR
        ↓
One or more GitHub MCP tool calls executed in sequence
        ↓
Results combined and returned as context
        ↓
Copilot responds or takes further action
        ↓
Activity reported to guardrail
```

## GitHub MCP Tools Used

- `create_issue` — open a new issue
- `update_issue` — edit title, body, labels, assignees, state
- `list_issues` — fetch issues with filters
- `add_issue_comment` — post a comment
- `create_pull_request` — open a PR
- `list_pull_requests` — fetch PRs with filters
- `merge_pull_request` — merge a PR (requires write scope)

## Required PAT Scopes

- `issues:read` / `issues:write` — for issue operations
- `pull_requests:read` / `pull_requests:write` — for PR operations

## Example mcp.json Config

See [config.example.json](config.example.json).

## Test Checklist

See [test.md](test.md).
