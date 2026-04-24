# Tool Pattern: Code Analytics Access

## What It Does

Pulls GitHub activity data — commit history, contributor metrics, code frequency, issue and PR trends — and surfaces it as structured context for Copilot analysis.

## When to Use It

- You want Copilot to reason about repository activity, velocity, or contributor patterns
- You need historical data to inform decisions about code health or team workflows
- You're building dashboards or reports driven by GitHub data

## How It Works

```
Copilot receives analysis prompt
        ↓
GitHub MCP tool calls fetch relevant metrics
        ↓
Data cached to avoid repeated identical calls
        ↓
Structured data returned as Copilot context
        ↓
Activity reported to guardrail
```

## GitHub MCP Tools Used

- `list_commits` — commit history with author and timestamp data
- `get_commit` — per-commit detail
- `list_issues` — issue trends with date filters
- `list_pull_requests` — PR activity and merge patterns
- `search_code` — code pattern frequency

## Required PAT Scopes

- `repo:read` — for all analytics tool calls

## Test Checklist

See [test.md](test.md).
