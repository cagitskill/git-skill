# Template: Team Setup (Copilot Business)

For teams on Copilot Business. Shared `mcp.json` committed to the repo, org policy enabled, registry configured.

---

## Prerequisites

- Org admin has enabled "MCP servers in Copilot" policy
- Registry URL configured (optional but recommended)

---

## .vscode/mcp.json (commit to repo)

```json
{
  "servers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/"
    }
  }
}
```

Committed to `.vscode/mcp.json` in the repo root so all team members get the same config.

---

## Active Tool Patterns

- Repository Access
- Issue & PR Management
- Webhook & Event Response
- Multi-Repo Coordination
- Guardrail (alert mode, escalating to intervene after 2 weeks)

---

## Auth

OAuth for individual members. Each developer authenticates with their own GitHub account — no shared tokens.

---

## Guardrail Config

```json
{
  "mode": "alert",
  "escalation_threshold": 3,
  "alert_destination": "https://hooks.slack.com/YOUR_WEBHOOK"
}
```

---

## Checklist Before Rollout

- [ ] Policy enabled by org admin
- [ ] Risk scanner run on team mcp.json
- [ ] Guardrail active in alert mode
- [ ] All team members signed into GitHub in their IDE
- [ ] Registry configured if using registry-only policy
