# Activating and Managing GitHub MCP Tool Patterns

## Activation Order

Always activate the guardrail first. It needs to be running before any other tool pattern goes live.

Recommended order:
1. **Guardrail** — always first
2. **Repository Access** — foundational for most workflows
3. **Issue & PR Management** — if your workflow involves creating or updating GitHub resources
4. Additional patterns based on your use case

## How to Activate

Toggle patterns on or off through the admin panel after deployment, or directly in `config/platform.json`:

```json
{
  "agents": {
    "guardrail": { "active": true },
    "direct-api-wrapper": { "active": true },
    "composite-service": { "active": true },
    "event-driven": { "active": false }
  }
}
```

Setting `active: false` soft-disables the pattern — it stays in the config but won't run. Reactivating is just flipping the flag.

## Deactivation Order

Deactivate in reverse order. The guardrail should be the last thing turned off.

## Tool Pattern Dependencies

| Pattern | Requires |
|---------|----------|
| All patterns | Guardrail active first |
| Multi-Repo Coordination | Repository Access active |
| Copilot-to-Agent Routing | At least one target pattern active |
| Webhook & Event Response | GitHub webhooks configured in your repo settings |

## Testing After Activation

Every tool pattern folder includes a `test.md` checklist. Work through it after activating each pattern before going to production.

See [docs/getting-started.md](getting-started.md) for the full setup sequence.
