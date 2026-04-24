# Rule: Data Exposure (GitHub MCP)

## What This Rule Checks

Whether GitHub MCP tool calls could expose sensitive repository data, credentials, or PII in tool outputs or logs.

## What It Looks For

- GitHub tokens or credentials appearing in tool output payloads
- Sensitive file content (`.env`, private keys, secrets) returned by file-read tools
- Repository content from private repos surfaced without access verification
- PII in issue or PR content passed through to unprotected output channels
- Secrets stored in GitHub Actions variables being accessible through MCP tools

## Severity Levels

| Finding | Severity |
|---------|----------|
| Token or credential in tool output | critical |
| Private key or secret file read by file tool | critical |
| Private repo content returned without verified access | high |
| PII passed to unprotected output channel | high |
| Actions secret accessible via MCP tool | high |

## Guardrail Rules Generated

- `block_credential_in_output`
- `block_private_key_file_read`
- `alert_private_repo_unverified_access`
- `alert_pii_in_output_channel`
- `alert_actions_secret_exposure`
