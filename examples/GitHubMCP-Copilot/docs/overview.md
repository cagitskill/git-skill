# GitHub Copilot MCP Field Guide — Overview

## What This Is

This repo is a structured, practical guide to setting up, configuring, and extending the **GitHub MCP server** with GitHub Copilot. It covers every supported IDE, both auth paths, org and enterprise policy, and a safety layer built on real findings from your own configuration.

It's not a hosted service. It's a field guide you drop into your workflow, adapt for your environment, and push back improvements to.

---

## What Is MCP?

MCP (Model Context Protocol) is an open standard that defines how AI models connect to external tools and data sources. Instead of building custom integrations for every service, MCP provides a shared protocol — like a universal adapter — so AI systems can talk to databases, APIs, files, and other resources through a consistent interface.

GitHub's MCP server implements this protocol to give Copilot direct access to your GitHub environment: repositories, issues, pull requests, code search, and more — all accessible from Copilot Chat in your IDE.

---

## How It Works

### Step 1 — Choose Your Setup Path
The remote GitHub MCP server is hosted by GitHub and is the recommended option for most users. No local installation required. The local server is for users who want more control or have specific security requirements.

### Step 2 — Configure mcp.json
Add the GitHub MCP server to your IDE's `mcp.json` config file. The root key is `"servers"` (not `"mcpServers"` — that's a common copy-paste mistake from other MCP clients).

### Step 3 — Authenticate
Two options: OAuth (one-click, recommended) or Personal Access Token (PAT). OAuth is the default for the remote server. PAT gives you more explicit scope control and is required for GitHub Enterprise Cloud with data residency.

### Step 4 — Open Copilot Chat in Agent Mode
MCP tools are only available in Agent mode. Switch to it from the mode dropdown in Copilot Chat. You'll see the GitHub MCP tools listed under the tools icon.

### Step 5 — Run the Risk Scanner
Before rolling out to a team, run the scanner against your `mcp.json` and PAT scopes. It identifies real exposure areas and produces a risk profile that becomes the basis for your guardrail rules.

### Step 6 — Activate the Guardrail
The guardrail monitors all active MCP tool activity. Start it in alert mode, validate for a couple of weeks, then graduate to intervene mode if you want active blocking.

### Step 7 — Org Admins: Configure Policy
If you're on Copilot Business or Enterprise, the "MCP servers in Copilot" policy must be enabled at the org or enterprise level before members can use MCP. You can also configure a registry URL and set an allowlist to control which MCP servers are available.

---

## What the GitHub MCP Server Gives You

Once configured, Copilot can interact with your GitHub environment directly from the chat interface. Examples of what you can ask:

- "Create an issue in this repo for the bug we just found"
- "List all open PRs assigned to me"
- "Search for all files that import this module across the org"
- "Get the last 10 commits on the main branch"
- "Add a comment to PR #142 asking for clarification on the auth change"

The full list of available tools depends on your Copilot plan. Tools that require a paid Copilot license surface the same access requirements as their underlying GitHub features.

---

## What This Repo Is Not

- Not a runtime engine — it's a guide, patterns, and safety layer
- Not a replacement for GitHub's official MCP server documentation
- Not opinionated about your tech stack beyond the patterns described here

---

## Repo Navigation

| You want to... | Go to... |
|----------------|----------|
| Set up your IDE | [docs/setup/](setup/) |
| Understand auth options | [docs/auth-patterns.md](auth-patterns.md) |
| Configure for an org or enterprise | [docs/org-policy.md](org-policy.md) |
| Understand the 9 tool patterns | [agents/](../agents/) |
| Run the risk scanner | [risk-scanner/README.md](../risk-scanner/README.md) |
| Set up the guardrail | [agents/guardrail/definition.md](../agents/guardrail/definition.md) |
| Start from a template | [templates/](../templates/) |
| Learn MCP terminology | [docs/glossary.md](glossary.md) |
