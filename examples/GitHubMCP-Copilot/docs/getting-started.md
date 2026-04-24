# Getting Started with the GitHub MCP Server

This guide walks you through the full setup from zero to a working GitHub MCP connection with Copilot.

---

## Prerequisites

- A GitHub account
- Access to GitHub Copilot (Free, Pro, Business, or Enterprise)
- A supported IDE: VS Code 1.99+, Visual Studio 17.14+, a compatible JetBrains IDE, or GitHub Copilot CLI
- If on Copilot Business or Enterprise: the "MCP servers in Copilot" policy must be enabled by your org admin

---

## Step 1 — Choose Remote or Local

**Remote (recommended for most users)**
Hosted by GitHub. No local installation. Uses OAuth authentication by default. This is the fastest path.

**Local**
Runs on your machine. More control over configuration. Useful if you have specific security requirements or want to customize the server setup.

This guide covers the remote path. For local setup, see the IDE-specific guides in [docs/setup/](setup/).

---

## Step 2 — Configure Your IDE

### VS Code

Open the Command Palette (`Cmd+Shift+P` / `Ctrl+Shift+P`) and run **MCP: Open User Configuration**.

Add the following to your `mcp.json`:

```json
{
  "servers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/"
    }
  }
}
```

Save the file. VS Code will prompt you to start the server. Click **Start**.

> **Common mistake:** VS Code uses `"servers"` as the root key — not `"mcpServers"`. If you copy config from Cursor or Claude Desktop, change this key or the server won't load.

For per-project config (shared with your team), create `.vscode/mcp.json` in your workspace root instead.

### Visual Studio

See [docs/setup/visual-studio.md](setup/visual-studio.md).

### JetBrains

See [docs/setup/jetbrains.md](setup/jetbrains.md).

### GitHub Copilot CLI

The GitHub MCP server is built into Copilot CLI — no additional configuration needed. See [docs/setup/copilot-cli.md](setup/copilot-cli.md) for adding other MCP servers.

---

## Step 3 — Authenticate

The remote server triggers OAuth authentication when you first use it. Follow the prompt in your IDE to authorize.

For PAT-based auth or GitHub Enterprise Cloud, see [docs/auth-patterns.md](auth-patterns.md).

---

## Step 4 — Open Copilot Chat in Agent Mode

MCP tools are only available in **Agent** mode.

In VS Code: Open Copilot Chat → select **Agent** from the mode dropdown.

Click the tools icon to see the list of available GitHub MCP tools. If the GitHub MCP server entry is there, you're connected.

---

## Step 5 — Try a Tool Call

Type a prompt in the Copilot Chat box that triggers a GitHub action. For example:

```
List the open issues in this repository
```

Copilot will show a confirmation dialog before calling any MCP tool. Click **Allow**.

If it works, the GitHub MCP server is set up correctly.

---

## Step 6 — Run the Risk Scanner (Recommended Before Team Rollout)

```bash
git clone https://github.com/your-username/GitHubMCP-Copilot.git
cd GitHubMCP-Copilot
npm install
node risk-scanner/scanner.js --target /path/to/your/mcp-config
```

Output saved to `risk-scanner/output/risk-profile.json`. Review the findings before activating the guardrail.

---

## Step 7 — Activate the Guardrail

Activate the guardrail before using the GitHub MCP server in any team or production context.

See [agents/guardrail/definition.md](../agents/guardrail/definition.md) for setup.

---

## Org Admins: Enable the Policy First

If your org is on Copilot Business or Enterprise, the "MCP servers in Copilot" policy is disabled by default. Members can't use MCP until it's enabled.

See [docs/org-policy.md](org-policy.md) for the full configuration walkthrough.

---

## Troubleshooting

**MCP tools not visible in Copilot Chat**
- Confirm you're in Agent mode, not Ask or Edit mode
- Confirm VS Code is 1.99 or later
- Check that the MCP server is running (look for the Start button in `mcp.json`)

**Auth issues**
- If using OAuth, make sure you're signed into GitHub in your IDE
- If using a PAT, confirm the token is valid and has the required scopes — see [docs/auth-patterns.md](auth-patterns.md)

**Org policy blocking access**
- Contact your org admin to enable "MCP servers in Copilot" — see [docs/org-policy.md](org-policy.md)

For more, see [GitHub's official MCP troubleshooting docs](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp/use-the-github-mcp-server).
