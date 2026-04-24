# Rule: Auth Patterns (GitHub MCP)

## What This Rule Checks

Whether authentication for the GitHub MCP server is implemented correctly and securely.

## What It Looks For

- PAT stored in `mcp.json` as a plaintext value rather than an environment variable reference
- No auth header configured when connecting to GitHub Enterprise Cloud (PAT required)
- OAuth token with broader scopes than the connected tools require
- Token validation skipped at the tool call level (relies solely on connection-time auth)
- PAT shared across multiple configs or team members rather than issued per-user

## Severity Levels

| Finding | Severity |
|---------|----------|
| PAT in plaintext in mcp.json | critical |
| No auth configured for GHE Cloud connection | high |
| OAuth scope broader than tool requirements | medium |
| PAT shared across multiple users | high |
| Token validation only at connection time | medium |

## Guardrail Rules Generated

- `block_plaintext_token_in_config`
- `alert_missing_auth_for_ghe`
- `alert_shared_pat_detected`
- `alert_oauth_scope_excess`
