# Template: Individual Developer

The fastest path to a working GitHub MCP server connection. Designed for Copilot Free or Pro users on VS Code with the remote server and OAuth auth.

---

## mcp.json

```json
{
  "servers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/"
    }
  }
}
```

Place in VS Code user config (Command Palette → `MCP: Open User Configuration`).

---

## Active Tool Patterns

- Repository Access
- Issue & PR Management
- Guardrail (alert mode)

---

## Auth

OAuth — one-click on first connection. No PAT required.

---

## Guardrail Config

Start in alert mode. Review logs for the first week before considering intervene mode.

```json
{
  "mode": "alert",
  "escalation_threshold": 5,
  "alert_destination": "console"
}
```

---

## Next Steps

1. Run the risk scanner against this config
2. Review risk profile
3. Activate the guardrail
4. Test with a simple prompt: `List open issues in this repo`
