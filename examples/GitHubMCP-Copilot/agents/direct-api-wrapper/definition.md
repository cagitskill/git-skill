# Tool Pattern: Repository Access

## What It Does

Connects directly to the GitHub MCP server to read repository content — files, directories, branches, commit history — and expose it as context for Copilot. No intermediary, no aggregation, single-repo focus.

## When to Use It

- You want Copilot to read and reason about specific files or directories in a repo
- You need commit history or branch metadata surfaced in Copilot Chat
- You're building a workflow around a single repository's content

## When Not to Use It

- You need to act on multiple repos simultaneously — use [Multi-Repo Coordination](../hierarchical-mcp/definition.md) instead
- You need to create or update GitHub resources — use [Issue & PR Management](../composite-service/definition.md)

## How It Works

```
Copilot receives user prompt
        ↓
GitHub MCP tool call: get_file_contents / search_code / list_commits
        ↓
Response returned as context to Copilot
        ↓
Copilot answers using that context
        ↓
Activity reported to guardrail
```

## GitHub MCP Tools Used

- `get_file_contents` — read a specific file from a repo
- `search_code` — search across code in a repository
- `list_commits` — fetch commit history for a branch
- `get_commit` — details for a specific commit
- `list_branches` — list available branches

## Example mcp.json Config

See [config.example.json](config.example.json).

## Required PAT Scopes

- `repo:read` — minimum required for all repository access tools

## Test Checklist

See [test.md](test.md).
