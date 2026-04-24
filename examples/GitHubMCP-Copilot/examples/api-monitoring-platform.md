# Example: Solo Developer

## Scenario

A developer working on a mix of personal and client projects. Uses VS Code primarily. Wants a fast, low-overhead GitHub MCP setup that works across repos without per-project configuration.

---

## Setup

- **IDE:** VS Code
- **Copilot Plan:** Pro
- **Auth:** OAuth for personal repos, PAT for client repos (separate tokens per client)
- **Config location:** User-level `mcp.json` for personal setup; `.vscode/mcp.json` per client project

### User-level mcp.json (personal repos)

```json
{
  "servers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/"
    }
  }
}
```

### Per-project .vscode/mcp.json (client repos)

```json
{
  "servers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/",
      "env": {
        "GITHUB_TOKEN": "${GITHUB_CLIENT_A_TOKEN}"
      }
    }
  }
}
```

Each client project uses its own environment variable pointing to a scoped PAT for that client's GitHub org.

---

## Active Tool Patterns

| Pattern | Why |
|---------|-----|
| Repository Access | Read files and commit history across active projects |
| Issue & PR Management | Manage issues and PRs across personal and client repos |
| Code Analytics Access | Track commit activity on long-running client projects |
| Guardrail | Alert mode — lightweight for personal use |

---

## Workflows in Use

### Daily Standup Prep
```
"What did I commit yesterday across all my active repos?"
```
Copilot calls `list_commits` filtered by author and date, summarizes activity across repos.

### Client PR Review
```
"Summarize what changed in the last 5 PRs merged to main"
```
Copilot calls `list_pull_requests` with state=closed and limit=5, returns a readable summary.

### Issue Cleanup
```
"Find all issues in this repo that haven't been updated in more than 60 days"
```
Copilot calls `list_issues` and filters by `updated_at`, surfaces a list for review.

---

## Risk Profile Findings

**For personal repos (OAuth):**
- No critical findings
- One medium: OAuth token scope slightly broader than read-only tools require — acceptable for mixed-use workflow

**For client repos (PAT):**
- All PATs pass scope check — scoped to minimum required per client
- No hardcoded tokens found
- Guardrail rule added: `alert_cross_client_token_usage` — flags if a client PAT is used outside its configured project scope

---

## Lessons from This Setup

- Per-project `.vscode/mcp.json` with separate env variable names per client is clean and avoids token crossover
- User-level config for personal repos + project-level config for client work — the two layers coexist without conflict
- VS Code only loads the project-level config when a workspace is open, so the right token is active automatically
- The risk scanner is worth running even on solo setups — caught one PAT that had drifted to a broader scope than originally set
