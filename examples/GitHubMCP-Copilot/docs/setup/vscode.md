# Setting Up the GitHub MCP Server in VS Code

## Requirements

- VS Code 1.99 or later
- GitHub Copilot (any plan)
- Signed into GitHub in VS Code
- If on Copilot Business or Enterprise: "MCP servers in Copilot" policy enabled by your org admin

---

## Remote Setup (Recommended)

The remote GitHub MCP server is hosted by GitHub. No local installation needed.

**Option A — Via MCP Registry (easiest)**

1. Open the Extensions panel (`Cmd+Shift+X` / `Ctrl+Shift+X`)
2. Search `@mcp` to browse the VS Code MCP marketplace
3. Find the GitHub MCP server and click install
4. VS Code writes the config entry automatically

**Option B — Manual config**

1. Open the Command Palette → **MCP: Open User Configuration**
2. Add to your `mcp.json`:

```json
{
  "servers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/"
    }
  }
}
```

3. Save. A **Start** button appears at the top of the servers list — click it.
4. VS Code triggers OAuth authentication. Authorize in the browser prompt.

> The root key is `"servers"` — not `"mcpServers"`. This is the number one copy-paste mistake from Cursor or Claude Desktop configs.

**For team/project-level config:** Create `.vscode/mcp.json` in your workspace root and commit it. Team members get the same server config automatically.

---

## Using GitHub MCP Tools

1. Open Copilot Chat (icon in the title bar)
2. Select **Agent** from the mode dropdown — tools only work in Agent mode
3. Click the tools icon (top left of chat box) to see available GitHub MCP tools
4. Type a prompt. Examples:
   - `List open issues assigned to me`
   - `Create a PR from this branch to main`
   - `Search for all usages of this function across the repo`
5. Copilot shows a confirmation before calling any MCP tool — click **Allow**

---

## PAT Authentication (Alternative)

If you need PAT-based auth instead of OAuth:

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

Set `GITHUB_TOKEN` as an environment variable. Never hardcode the value in `mcp.json`.

See [docs/auth-patterns.md](../auth-patterns.md) for PAT scope recommendations.

---

## Troubleshooting

- **Tools not visible** → Confirm Agent mode is selected, not Ask or Edit
- **Server not starting** → Check VS Code is 1.99+, click the Start button in `mcp.json`
- **Auth failing** → Confirm you're signed into GitHub, or check PAT scopes
- **Org policy blocking access** → Ask your admin to enable "MCP servers in Copilot"
