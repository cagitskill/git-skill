# Setting Up the GitHub MCP Server in Visual Studio

## Requirements

- Visual Studio 17.14 or later
- GitHub Copilot access
- Signed into GitHub in Visual Studio
- If on Copilot Business or Enterprise: "MCP servers in Copilot" policy enabled by your org admin

---

## Remote Setup

1. In the Visual Studio menu bar, click **View → GitHub Copilot Chat**
2. At the bottom of the chat panel, select **Agent** from the mode dropdown
3. Click the tools icon (labeled "Configure your MCP server")
4. Click **Add MCP Tools**
5. In the "Configure MCP server" popup:
   - **Server ID:** `github`
   - **Type:** `HTTP/SSE`
   - **URL:** `https://api.githubcopilot.com/mcp/`
6. Click **Save**

OAuth authentication triggers on first use.

---

## GitHub Enterprise Cloud

For GitHub Enterprise Cloud with data residency:

- **URL:** `https://copilot-api.YOURSUBDOMAIN.ghe.com/mcp`
- Add an **Authorization** header with value `Bearer YOUR_GITHUB_PAT`

Replace `YOURSUBDOMAIN` with your instance subdomain.

---

## Using GitHub MCP Tools

1. Open GitHub Copilot Chat via **View → GitHub Copilot Chat**
2. Confirm **Agent** is selected in the mode dropdown
3. Type a prompt. Examples:
   - `Show me all open PRs for this repo`
   - `Create an issue for this TODO comment`
4. Confirm the tool call when prompted

---

## Troubleshooting

- **Agent mode not visible** → Confirm Visual Studio is 17.14 or later
- **Auth failing** → Verify you're signed into GitHub in Visual Studio
- **Org policy blocking** → Ask your admin to enable "MCP servers in Copilot"
