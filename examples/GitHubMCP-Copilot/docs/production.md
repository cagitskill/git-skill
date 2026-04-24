# Production Readiness for GitHub MCP Setups

## Pre-Production Checklist

### Configuration
- [ ] `mcp.json` uses environment variable references for all credentials — no hardcoded tokens
- [ ] PAT scopes limited to what active tool patterns actually require
- [ ] Onboarding flow completed and `config/platform.json` generated
- [ ] Risk scanner run and `risk-profile.json` reviewed
- [ ] Critical and high risk findings addressed

### Guardrail
- [ ] Guardrail active before any other pattern
- [ ] Guardrail in alert mode for at least 2 weeks before considering intervene mode
- [ ] Alert destination configured (webhook, log file, or console)
- [ ] Guardrail audit log retention defined

### Authentication
- [ ] Auth confirmed working in your IDE (OAuth flow completed or PAT validated)
- [ ] PAT expiry handling confirmed — expired tokens fail clearly, not silently
- [ ] No PATs shared across team members (each developer has their own)

### Org Policy (Business/Enterprise only)
- [ ] "MCP servers in Copilot" policy enabled by org admin
- [ ] Registry configured if using registry-only access
- [ ] Members confirmed signed into GitHub in their IDEs

---

## Health Checks

Each tool pattern's `agent.js` includes a health check endpoint. Verify patterns are running:

```bash
# Check all active patterns
GET /admin/status

# Check a specific pattern
GET /agents/guardrail/health
```

---

## Logging

Every tool call produces a log entry. Minimum fields:
- Timestamp
- Pattern name
- Tool called
- Correlation ID
- Auth result
- Outcome (success / failure with reason)

Guardrail decisions log independently from tool activity — two separate audit trails.

---

## Rotating PATs

When a PAT is rotated:
1. Generate the new PAT with the same scopes as the old one
2. Update the environment variable in your IDE or CI system
3. Re-run the risk scanner against the new PAT scopes
4. Confirm the guardrail picks up any scope changes on next restart
5. Revoke the old PAT

---

## Incident Response

If a guardrail rule triggers unexpectedly:
1. Check the guardrail audit log for the full activity chain (use the correlation ID)
2. Determine if it's a true positive or a rule that needs tuning
3. If true positive: address the root cause before resuming that tool pattern
4. If false positive: adjust the rule threshold or specificity, document the change

Do not disable the guardrail to resolve an incident. Switch to alert mode and investigate.
