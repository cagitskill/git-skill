# Tool Pattern: Context-Adaptive Config

## What It Does

Adjusts GitHub MCP tool behavior at runtime based on context signals — repository type, org membership, branch name, user role, or other metadata — without requiring manual config changes.

## When to Use It

- Different repos in your org need different MCP tool behavior
- You want default behavior to shift automatically based on branch (e.g., stricter rules on main)
- You're managing MCP across teams with different workflow requirements

## How It Works

```
MCP tool call initiated
        ↓
Context signals evaluated (repo, branch, org, user role)
        ↓
Config adjusted to match context
        ↓
Tool call executes with context-appropriate settings
        ↓
Activity reported to guardrail
```

## Example Context Signals

- Branch is `main` or `release/*` → require additional confirmation before write operations
- Repo is marked `sensitive` → restrict to read-only tool calls
- User is not a repo collaborator → limit to public data tools

## Required PAT Scopes

- `repo:read` — to evaluate repo metadata
- Additional scopes based on tools in the chain

## Test Checklist

See [test.md](test.md).
