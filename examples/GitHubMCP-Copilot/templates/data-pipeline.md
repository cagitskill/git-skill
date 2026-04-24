# Template: Enterprise Config (Copilot Enterprise)

For large orgs on Copilot Enterprise. Policy enforced at enterprise level, registry configured, allowlist active.

---

## Prerequisites

- Enterprise admin has enabled "MCP servers in Copilot" at enterprise level
- Registry configured and allowlist set to "Registry only"
- Data residency requirements confirmed

---

## mcp.json (standard GitHub.com)

```json
{
  "servers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/"
    }
  }
}
```

## mcp.json (GitHub Enterprise Cloud with data residency)

```json
{
  "servers": {
    "github": {
      "url": "https://copilot-api.YOURSUBDOMAIN.ghe.com/mcp/",
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

---

## Active Tool Patterns

All 9 patterns active. Guardrail in intervene mode after 30-day alert validation period.

---

## Auth

PAT required for GitHub Enterprise Cloud with data residency. PATs issued per-user, scoped to minimum required permissions, reviewed quarterly.

---

## Guardrail Config

```json
{
  "mode": "intervene",
  "escalation_threshold": 2,
  "alert_destination": "https://your-pagerduty-endpoint",
  "intervention_actions": ["block", "log", "notify"]
}
```

---

## Admin Checklist

- [ ] Enterprise policy enabled
- [ ] Registry URL configured and validated
- [ ] Allowlist set to registry-only
- [ ] PAT scope requirements documented for each team
- [ ] Risk scanner run across representative configs
- [ ] Guardrail validated in alert mode before switching to intervene
- [ ] Audit log retention configured
