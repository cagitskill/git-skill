# Tool Pattern: Copilot-to-Agent Routing

## What It Does

Classifies Copilot output or GitHub MCP tool results and routes them to the right specialized workflow or downstream agent based on content type, label, or trigger condition.

## When to Use It

- You have multiple downstream workflows and need Copilot to route to the right one automatically
- You want to triage incoming GitHub events (issues, PRs, comments) and direct them based on type or content
- You're building multi-step automation where each step is handled by a different tool pattern

## How It Works

```
GitHub MCP tool returns result (e.g., new issue content)
        ↓
Router evaluates result against defined conditions
        ↓
Matched condition triggers target tool pattern
        ↓
Target executes with routing context
        ↓
Activity reported to guardrail
```

## Example Routing Conditions

- Issue labeled `bug` → route to bug triage workflow
- PR marked `ready for review` → route to review checklist pattern
- Commit message contains `[release]` → route to release notes workflow

## Required PAT Scopes

Inherits from whichever tools are used in the routing chain.

## Test Checklist

See [test.md](test.md).
