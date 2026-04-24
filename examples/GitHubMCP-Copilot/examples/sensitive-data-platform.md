# Example: Enterprise Dev Team

## Scenario

A 40-person engineering team at a company on GitHub Copilot Enterprise. The org has multiple private repos. The security team requires all MCP usage to go through an approved registry, PATs to be scoped explicitly, and audit trails maintained for all tool calls.

---

## Setup

- **IDE:** VS Code (all developers) and JetBrains (subset of backend team)
- **Copilot Plan:** Enterprise
- **Auth:** PAT per developer, scoped to minimum required permissions
- **Config location:** `.vscode/mcp.json` committed to each repo, plus enterprise registry

### .vscode/mcp.json (committed to repo)

```json
{
  "servers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/",
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

Each developer sets `GITHUB_TOKEN` in their local environment. The config is shared; the credential is not.

---

## Org Policy Configuration

- "MCP servers in Copilot" enabled at enterprise level
- Registry URL configured — points to internal Azure API Center instance
- Access policy: **Registry only** — no unapproved servers can connect

---

## Active Tool Patterns

| Pattern | Why |
|---------|-----|
| Repository Access | Code search and file reads for context during reviews |
| Issue & PR Management | Cross-team issue tracking and PR coordination |
| Multi-Repo Coordination | Actions across the org's 15 active repos |
| Webhook & Event Response | Auto-label and auto-assign on new issues and PRs |
| Guardrail | Alert mode for 30 days → intervene mode after validation |

---

## PAT Scope Standard

The security team defined a minimum scope set for each use case:

| Team Role | Scopes |
|-----------|--------|
| Developer (standard) | `repo:read`, `issues:write`, `pull_requests:write` |
| Repo admin | `repo:read`, `repo:write`, `issues:write`, `pull_requests:write` |
| Read-only reviewer | `repo:read`, `issues:read`, `pull_requests:read` |

PATs rotate every 90 days. The risk scanner re-runs after each rotation cycle.

---

## Risk Profile Findings

Scanner run on representative config from three repo types:

**Critical (resolved before rollout):**
- One developer had hardcoded PAT in their local `mcp.json` — caught by scanner, rotated immediately

**High:**
- Two repos had PATs with `admin:org` scope not needed for their tool patterns — scopes narrowed

**Medium:**
- Several configs missing explicit `allowed_endpoints` list — added during review

Guardrail rules generated from findings:
- `block_hardcoded_pat`
- `alert_admin_scope_on_standard_tool`
- `alert_unlisted_endpoint_call`

---

## Audit Trail Setup

Every tool call logged to the guardrail's independent audit log. Log retention: 12 months. Reviewed by security team monthly during the first quarter, quarterly after that.

---

## Lessons from This Setup

- Committed `.vscode/mcp.json` with env variable references is the cleanest team pattern — config is shared, credentials are not
- The registry-only policy prevented two developers from connecting unapproved MCP servers they'd used in personal projects
- Running the risk scanner on representative configs before rollout caught the hardcoded PAT — worth the extra step
- 30-day alert validation period before switching to intervene mode was the right call — 3 guardrail rules needed tuning before they were reliable enough to block
