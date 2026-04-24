# Template: Sensitive Repo Setup

For repos with elevated data sensitivity — internal tooling, regulated environments, private org infrastructure. Restricted PAT scopes, explicit allowlists, tighter guardrail.

---

## mcp.json

```json
{
  "servers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/",
      "env": {
        "GITHUB_TOKEN": "${GITHUB_SENSITIVE_REPO_TOKEN}"
      }
    }
  }
}
```

PAT stored in environment variable, scoped to read-only operations only.

---

## PAT Scopes

Read-only — no write access:
- `repo:read`
- `issues:read`
- `pull_requests:read`

Do not grant write scopes on sensitive repos unless explicitly required and reviewed.

---

## Active Tool Patterns

- Repository Access (read-only)
- Code Analytics Access
- Guardrail (intervene mode from day one)

Issue and PR management, event response, and multi-repo coordination are disabled until scopes are formally reviewed.

---

## Guardrail Config

```json
{
  "mode": "intervene",
  "escalation_threshold": 1,
  "alert_destination": "https://your-security-alert-endpoint",
  "intervention_actions": ["block", "log", "notify", "require_confirmation"]
}
```

Threshold of 1 — any rule trigger escalates immediately.

---

## Additional Controls

- Local Resource Access pattern disabled — no local file reads on sensitive repo workflows
- All PATs reviewed and rotated every 90 days
- Risk scanner re-run after every PAT rotation or config change
- Guardrail audit log retained for 12 months minimum
