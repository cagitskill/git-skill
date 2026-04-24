# Setting Up the GitHub MCP Server in JetBrains IDEs

## Requirements

- A compatible JetBrains IDE (IntelliJ IDEA, PyCharm, WebStorm, etc.)
- Latest version of the GitHub Copilot plugin
- Signed into GitHub in your JetBrains IDE
- If on Copilot Business or Enterprise: "MCP servers in Copilot" policy enabled by your org admin

## Compatible IDEs

IntelliJ IDEA, PyCharm, WebStorm, GoLand, Rider, CLion, RubyMine, PhpStorm, DataGrip, DataSpell, Aqua, Android Studio. See the JetBrains IDE tool finder for downloads.

---

## Remote Setup

1. Open GitHub Copilot Chat in your IDE
2. Make sure you are in **Agent** mode
3. Click the tools icon (labeled "Configure your MCP server") at the bottom of the chat window
4. Click **Add MCP Tools**
5. In the configuration dialog:
   - **Server ID:** `github`
   - **Type:** `HTTP/SSE`
   - **URL:** `https://api.githubcopilot.com/mcp/`
6. Save

OAuth authentication triggers on first use.

---

## GitHub Enterprise Cloud

For GitHub Enterprise Cloud with data residency:

- **URL:** `https://copilot-api.YOURSUBDOMAIN.ghe.com/mcp`
- Add **Authorization** header: `Bearer YOUR_GITHUB_PAT`

---

## Using GitHub MCP Tools

1. Open Copilot Chat
2. Confirm **Agent** mode is active
3. Type a GitHub-related prompt:
   - `List all issues labeled 'bug' in this repo`
   - `Summarize the changes in the last 5 commits`
4. Approve the tool call when prompted

---

## Troubleshooting

- **Plugin not installed** → Install GitHub Copilot from the JetBrains Marketplace
- **Agent mode not visible** → Update the Copilot plugin to the latest version
- **Auth failing** → Re-authenticate GitHub in IDE settings
