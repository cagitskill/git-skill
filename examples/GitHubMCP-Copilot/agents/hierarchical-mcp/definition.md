# Tool Pattern: Multi-Repo Coordination

## What It Does

Coordinates GitHub MCP tool calls across multiple repositories or multiple teams. Executes named pipelines with sequential or parallel operations and collects results.

## When to Use It

- You manage multiple repos and need actions executed across all of them
- You're coordinating cross-team workflows (e.g., sync issue status across org repos)
- You need parallel GitHub operations with merged results

## How It Works

```
Coordination request received
        ↓
Pipeline defined: target repos, operation sequence, parallel vs. sequential
        ↓
GitHub MCP tool calls executed per repo (parallel or sequential)
        ↓
Results collected and merged
        ↓
Summary returned to Copilot
        ↓
Full activity reported to guardrail
```

## Example Pipelines

- List all open issues labeled `critical` across 5 repos
- Check PR status across every repo in an org
- Apply the same label update to issues in multiple repos simultaneously

## Required PAT Scopes

- `repo:read` for read operations across repos
- `org:read` for org-level queries
- Additional write scopes for any mutation operations

## Test Checklist

See [test.md](test.md).
