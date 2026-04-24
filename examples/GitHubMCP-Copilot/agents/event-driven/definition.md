# Tool Pattern: Webhook & Event Response

## What It Does

Listens for GitHub events — pushes, pull request updates, issue comments, status checks, reviews — and triggers defined actions through the GitHub MCP server in response.

## When to Use It

- You want automated responses to GitHub repository events
- You're building CI/CD-adjacent workflows that react to PR status changes
- You need to auto-label, auto-assign, or auto-comment based on event content

## How It Works

```
GitHub webhook fires (push, PR update, issue event, etc.)
        ↓
Event payload received and filtered
        ↓
Rate limit check applied
        ↓
Matching action executed via GitHub MCP tool call
        ↓
Activity logged and reported to guardrail
```

## Common Event + Action Pairs

| Event | Action |
|-------|--------|
| Issue opened | Auto-label based on title keywords |
| PR opened | Assign reviewer based on changed files |
| PR review approved | Post merge-ready comment |
| Status check failed | Create issue with failure context |
| Push to main | Trigger release notes draft |

## Required PAT Scopes

Depends on the actions triggered. At minimum: `issues:write`, `pull_requests:write`.

## Test Checklist

See [test.md](test.md).
