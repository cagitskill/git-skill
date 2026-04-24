# Rule: API Scope (GitHub MCP)

## What This Rule Checks

Whether your GitHub MCP configuration grants broader access than your active tool patterns actually require.

## What It Looks For

- PAT with write scopes (`issues:write`, `pull_requests:write`) when only read tools are active
- Admin-level scopes (`admin:org`, `admin:repo`) on PATs used for standard tool calls
- Hardcoded PAT values in `mcp.json` or source files rather than environment variables
- Tool calls to GitHub API endpoints not listed in the pattern's `allowed_endpoints`
- Org-level scopes when access is only needed at the repo level

## Severity Levels

| Finding | Severity |
|---------|----------|
| Write scope granted to read-only tool patterns | high |
| Admin scope on non-admin tool calls | critical |
| Hardcoded PAT in mcp.json or source file | critical |
| Unlisted GitHub endpoint accessible | medium |
| Org scope when repo scope is sufficient | medium |

## Guardrail Rules Generated

- `block_hardcoded_pat`
- `alert_write_scope_on_read_workflow`
- `alert_admin_scope_on_standard_tool`
- `alert_unlisted_endpoint_call`
- `alert_org_scope_not_needed`
