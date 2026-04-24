# Glossary

New to MCP and GitHub Copilot's agent capabilities? This page explains every key term in plain language — no prior background required.

---

## Core Concepts

### MCP (Model Context Protocol)
An open standard that defines how AI models connect to external tools and data sources. Instead of building a custom integration for every service, MCP gives AI systems a shared protocol — like a universal adapter — so they can interact with repositories, databases, APIs, files, and other resources through a consistent interface.

### GitHub MCP Server
GitHub's official implementation of the MCP protocol. It gives Copilot direct access to your GitHub environment: repositories, issues, pull requests, code search, commit history, and more. Maintained by GitHub at [github/github-mcp-server](https://github.com/github/github-mcp-server).

### MCP Server
Any program that exposes tools, data, or services to an AI model using the MCP protocol. The GitHub MCP server is one example. Others might expose a database, a Slack workspace, or a Jira project.

### MCP Tool
A specific action the GitHub MCP server can perform on your behalf — like creating an issue, listing pull requests, or searching code. Each tool has defined inputs and outputs. Copilot selects and calls tools based on what you ask.

### Agent Mode
The Copilot Chat mode where MCP tools are available. You must be in Agent mode to use the GitHub MCP server. Ask and Edit modes do not surface MCP tools.

---

## Setup & Configuration

### mcp.json
The configuration file where you define which MCP servers your IDE should connect to. In VS Code, the root key is `"servers"` — not `"mcpServers"`. Getting this wrong is the most common setup mistake.

### Remote MCP Server
A GitHub-hosted version of the GitHub MCP server. The recommended option for most users — no local installation, uses OAuth auth by default, always up to date.

### Local MCP Server
A version of the GitHub MCP server that runs on your own machine. More control, more configuration. Useful for specific security requirements or custom setups.

### MCP Registry
A curated list of approved MCP servers that an org or enterprise makes available to its members. Org admins configure a registry URL; members see those servers in their IDE's extensions panel.

---

## Authentication

### OAuth
A standard authorization flow that lets you grant Copilot limited access to your GitHub account without sharing your password. Default auth for the remote GitHub MCP server — one click to authorize, GitHub manages the token.

### Personal Access Token (PAT)
A credential you create in GitHub settings that grants specific permissions. Used instead of OAuth when you need explicit scope control, or when using GitHub Enterprise Cloud with data residency. Give PATs only the scopes needed for the tools you're actually using.

### Token Scope
The specific permissions attached to a PAT. A PAT with `repo:read` scope can read repository content but can't create issues or approve PRs. Keeping scopes narrow reduces the impact if a token is ever compromised.

---

## Safety & Guardrails

### Guardrail
A safety mechanism that watches MCP tool activity and enforces rules. In alert mode, it logs and notifies when something looks off. In intervene mode, it can actively block a tool call. Think of it as a security layer for your MCP usage.

### Risk Scanner
A tool that analyzes your MCP configuration and PAT scopes before you go live. It looks for things like overly broad permissions, hardcoded tokens, and missing auth gates. Its findings become the rules your guardrail enforces.

### Risk Profile
The output of the risk scanner — a structured report listing potential issues by category and severity. This file drives your guardrail's rule set.

---

## Org & Enterprise

### MCP Policy
An org or enterprise setting that controls whether members can use MCP with Copilot. Disabled by default for Copilot Business and Enterprise. Must be enabled by an admin before members can connect to any MCP server.

### Allowlist
A policy setting that restricts which MCP servers members can use — either all servers (allow all) or only servers listed in the org's registry (registry only).

### Data Residency
A GitHub Enterprise Cloud option that keeps your data in a specific geographic region. When data residency is enabled, the MCP server URL and auth method are different from the standard setup.

---

## Communication Layer

### Correlation ID
A unique identifier that follows a request as it moves through a chain of tool calls. If one action triggers another, they share the same correlation ID. This makes it possible to trace a full request chain when debugging.

### Agent Registry
A directory that keeps track of all active tool patterns — where they run, what they do, whether they're currently active. Patterns look each other up in the registry rather than using hardcoded addresses.

### Message Envelope
The standard wrapper around every message tool patterns send to each other. Includes metadata: sender, target, timestamp, and correlation ID.

### Fire-and-Forget
A pattern where a message is sent without waiting for a response. Tool patterns report to the guardrail this way so security logging never slows down the actual work.

---

## Infrastructure

### Serverless
A way of running code in the cloud without managing servers. You upload your code and the provider (Vercel, in this setup) runs it on demand.

### Webhook
A way for one service to automatically notify another when something happens. Instead of constantly asking "did anything change?", a webhook pushes a notification the moment something does.

### API (Application Programming Interface)
A way for two programs to communicate. When Copilot calls a GitHub MCP tool, it's making an API call — a structured request that follows agreed-upon rules — and getting a structured response back.
