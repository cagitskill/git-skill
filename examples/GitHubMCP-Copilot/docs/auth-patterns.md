# Authentication Patterns for the GitHub MCP Server

## Two Auth Paths

The GitHub MCP server supports two authentication methods. Which one you use depends on your plan, IDE, and security requirements.

---

## OAuth (Recommended)

The remote GitHub MCP server uses OAuth by default. When you first connect, your IDE triggers a one-click authorization flow. You grant access, GitHub issues a token scoped to what Copilot needs, and the server handles the rest.

**When to use OAuth:**
- You're on Copilot Free, Pro, or Pro+
- You're using VS Code, Visual Studio, or JetBrains with the remote server
- You want the simplest, lowest-friction setup

**How it works:**
```
IDE connects to remote server
        ↓
OAuth authorization prompt appears
        ↓
You authorize in browser
        ↓
Token issued and scoped automatically
        ↓
Connection active — no token management on your end
```

No token storage, no manual scope selection, no renewal management. GitHub handles it.

---

## Personal Access Token (PAT)

PAT-based auth gives you explicit control over what scopes the MCP server has access to. Required for GitHub Enterprise Cloud with data residency.

**When to use a PAT:**
- GitHub Enterprise Cloud with data residency
- You need explicit scope control for security or compliance
- Your org requires documented token scopes for audit purposes
- You're configuring the local MCP server

**PAT configuration in mcp.json:**

```json
{
  "servers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/",
      "requestInit": {
        "headers": {
          "Authorization": "Bearer YOUR_GITHUB_PAT"
        }
      }
    }
  }
}
```

For GitHub Enterprise Cloud:
```json
{
  "servers": {
    "github": {
      "url": "https://copilot-api.YOURSUBDOMAIN.ghe.com/mcp/",
      "requestInit": {
        "headers": {
          "Authorization": "Bearer YOUR_GITHUB_PAT"
        }
      }
    }
  }
}
```

Replace `YOURSUBDOMAIN` with your GitHub Enterprise Cloud subdomain and `YOUR_GITHUB_PAT` with your token. Never commit a real PAT to a repo — use environment variables.

**PAT in environment variables (preferred):**
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

---

## PAT Scope Recommendations

Give your PAT only the scopes needed for the GitHub MCP tools you're using. Broader scopes mean broader exposure if a token is compromised.

| Use Case | Minimum Scopes |
|----------|---------------|
| Read repos and issues | `repo:read`, `issues:read` |
| Create and update issues | `issues:write` |
| Read and comment on PRs | `pull_requests:read`, `pull_requests:write` |
| Code search | `repo:read` |
| Org-level access | `org:read` |

The risk scanner's api-scope rule flags tokens with broader scopes than their declared use case requires. See [risk-scanner/rules/api-scope.md](../risk-scanner/rules/api-scope.md).

---

## What the Risk Scanner Checks

| Finding | Severity |
|---------|----------|
| PAT with write scope used for read-only workflows | high |
| Hardcoded PAT in mcp.json or source file | critical |
| Admin scope on a PAT used for non-admin tools | medium |
| Token stored in plaintext outside environment variables | critical |
| OAuth token with broader scopes than tools require | medium |

Each finding maps to a guardrail rule. See [agents/guardrail/definition.md](../agents/guardrail/definition.md).

---

## Guardrail Auth Rules (Always Active)

Regardless of what the risk scanner finds, these auth-related rules are always active:

- Alert on any MCP tool call that executes without a logged auth check
- Block any token or credential string appearing in a tool output payload
- Alert on admin-level tool actions outside a verified session
- Alert on unexpected tool activations with no recorded auth event

---

## Auth Checklist

Before using the GitHub MCP server in a team or org context:

- [ ] OAuth or PAT auth confirmed working in your IDE
- [ ] PAT scopes limited to what your tools actually need
- [ ] PAT stored in environment variables, not hardcoded
- [ ] Risk scanner run against your auth config
- [ ] Guardrail auth rules active
- [ ] Token expiry handled — expired tokens should fail clearly, not silently
