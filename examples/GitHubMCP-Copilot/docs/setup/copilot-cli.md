# Setting Up MCP Servers in GitHub Copilot CLI

## The Built-In GitHub MCP Server

The GitHub MCP server is **built into Copilot CLI** — it's already available without any additional configuration. You do not need to add it manually.

The steps below are for adding *other* MCP servers to Copilot CLI.

---

## Adding Additional MCP Servers

### Interactive Method

In a Copilot CLI session, run:

```bash
/mcp add
```

Follow the prompts:
1. Select server type: **Local/STDIO** (local process) or **HTTP/SSE** (remote server)
2. Enter the command or URL
3. Set any required environment variables as JSON key-value pairs

### Manual Config Method

Edit `~/.copilot/mcp-config.json` directly:

```json
{
  "mcpServers": {
    "my-server": {
      "type": "local",
      "command": "npx",
      "args": ["-y", "@my-org/my-mcp-server"]
    }
  }
}
```

For a remote HTTP server:
```json
{
  "mcpServers": {
    "my-remote-server": {
      "type": "http",
      "url": "https://my-mcp-server.example.com/mcp"
    }
  }
}
```

---

## Server Type Reference

| Type | Use When |
|------|----------|
| `local` / `stdio` | Starting a local process. Compatible with VS Code, Copilot cloud agent, and other MCP clients. |
| `http` | Connecting to a remote server using Streamable HTTP transport |
| `sse` | Legacy HTTP with Server-Sent Events — still supported but deprecated in MCP spec |

---

## Managing MCP Servers in CLI

```bash
/mcp show                    # List all configured servers and status
/mcp show SERVER-NAME        # Details and tools for a specific server
/mcp edit SERVER-NAME        # Edit a server's configuration
/mcp delete SERVER-NAME      # Remove a server
/mcp disable SERVER-NAME     # Disable without removing (stays configured)
/mcp enable SERVER-NAME      # Re-enable a disabled server
```

---

## Workspace-Level Config

For project-specific MCP configs that travel with the repo, place `.copilot/mcp-config.json` in your project root. The CLI discovers configs at every directory level from current working directory up to the git root.

---

## Org Allowlists

Organizations can enforce allowlists for third-party MCP servers. If a server is blocked by policy, the CLI surfaces a warning. Contact your org admin if you need access to a specific server.
