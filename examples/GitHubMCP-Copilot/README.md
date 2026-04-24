# GitHub Copilot MCP Field Guide

> The GitHub MCP server is the main character. Your repositories, issues, and pull requests are the backstory. The guardrail watches every action to make sure nothing goes off script.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Status: Active](https://img.shields.io/badge/Status-Active-green.svg)]()

---

## What This Is

A practical, opinionated guide to setting up, configuring, and extending the **GitHub MCP server** with GitHub Copilot — across every supported IDE, for individuals and organizations alike.

Most people setting up the GitHub MCP server run into the same gaps: unclear auth options, missing context on what each tool actually does, no guidance on org-level policy, and no structured way to add guardrails before rolling it out to a team. This repo closes those gaps.

Whether you're a solo developer getting started or an org admin rolling this out to an enterprise team, this guide walks you through the full picture.

---

## What Is MCP?

MCP (Model Context Protocol) is an open standard that defines how AI models connect to external tools and data sources. Instead of building custom integrations for every service, MCP provides a shared protocol — like a universal adapter — so AI systems can interact with repositories, APIs, files, and other resources through a consistent interface.

GitHub's MCP server uses this protocol to give Copilot direct access to your GitHub environment. You can ask Copilot to create issues, search code, review PRs, and more — all without leaving your editor.

New to MCP? Start with the [Glossary](docs/glossary.md) for plain-language definitions of every term used in this guide.

---

## How It Works

```
Choose your setup path (remote or local)
        ↓
Configure mcp.json in your IDE
        ↓
Authenticate via OAuth or Personal Access Token
        ↓
Open Copilot Chat in Agent mode
        ↓
Run the risk scanner against your token scopes and config
        ↓
Guardrail rules generated from scan findings
        ↓
Use GitHub MCP tools directly from Copilot Chat
        ↓
Org admins: enable policy and manage registry access
```

---

## What's Included

For the complete feature breakdown see [docs/features.md](docs/features.md).

### IDE Setup Guides

Step-by-step configuration for every supported editor:

| IDE | Guide |
|-----|-------|
| VS Code | [docs/setup/vscode.md](docs/setup/vscode.md) |
| Visual Studio | [docs/setup/visual-studio.md](docs/setup/visual-studio.md) |
| JetBrains IDEs | [docs/setup/jetbrains.md](docs/setup/jetbrains.md) |
| GitHub Copilot CLI | [docs/setup/copilot-cli.md](docs/setup/copilot-cli.md) |

### 9 GitHub MCP Tool Patterns

Every tool pattern ships with four files: a plain-markdown definition, an example `mcp.json` config, a working usage script, and a test checklist.

| # | Pattern | What It Does |
|---|---------|-------------|
| 1 | [Repository Access](agents/direct-api-wrapper/definition.md) | Search repos, read files, fetch content directly via Copilot |
| 2 | [Issue & PR Management](agents/composite-service/definition.md) | Create, update, list, and comment on issues and pull requests |
| 3 | [Copilot-to-Agent Routing](agents/mcp-to-agent/definition.md) | Route Copilot output to specialized workflows based on context |
| 4 | [Webhook & Event Response](agents/event-driven/definition.md) | React to GitHub events like pushes, reviews, and status checks |
| 5 | [Context-Adaptive Config](agents/configuration-use/definition.md) | Adjust MCP tool behavior based on repo or org context |
| 6 | [Code Analytics Access](agents/analytics-data-access/definition.md) | Pull commit history, contributor metrics, and activity data |
| 7 | [Multi-Repo Coordination](agents/hierarchical-mcp/definition.md) | Coordinate actions across multiple repositories or teams |
| 8 | [Local Resource Access](agents/local-resource-access/definition.md) | Scoped access to local files alongside GitHub context |
| 9 | [Guardrail](agents/guardrail/definition.md) | Monitor and protect all MCP tool activity in real time |

### Authentication Guide

Two auth paths — OAuth (recommended) and Personal Access Token (PAT). Know which to use and when. See [docs/auth-patterns.md](docs/auth-patterns.md).

### Risk Scanner

Scans your MCP config, PAT scopes, and connected tool permissions before you go live. Produces a structured risk profile that becomes the source for your guardrail's rule set. See [risk-scanner/README.md](risk-scanner/README.md).

### Guardrail Agent

Monitors all active MCP tool activity. Flags scope violations, token exposure, unexpected activations, and loop conditions. Runs in alert or intervene mode. See [agents/guardrail/definition.md](agents/guardrail/definition.md).

### Org & Enterprise Policy Guide

Enable the MCP policy for your organization, configure a registry, set allowlists, and manage access across teams. See [docs/org-policy.md](docs/org-policy.md).

### Templates

Starter `mcp.json` configurations for four common setups:
- [Individual Developer](templates/minimal-setup.md) — get running fast on Free or Pro
- [Team Setup](templates/api-platform.md) — Copilot Business with shared registry
- [Enterprise Config](templates/data-pipeline.md) — Enterprise policy, allowlists, data residency
- [Sensitive Repo Setup](templates/hr-sensitive.md) — restricted scopes, tighter guardrail, PAT management

### Examples

Three reference scenarios showing real GitHub MCP workflows:
- [Open Source Maintainer](examples/real-time-data-platform.md) — issue triage, PR review, release workflows
- [Enterprise Dev Team](examples/sensitive-data-platform.md) — policy-governed access, org registry, audit trails
- [Solo Developer](examples/api-monitoring-platform.md) — fast personal setup, VS Code remote config, OAuth auth

---

## Quick Start

**1. Add the GitHub MCP server to your IDE**

For VS Code, open `mcp.json` (Command Palette → `MCP: Open User Configuration`) and add:

```json
{
  "servers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/"
    }
  }
}
```

For Visual Studio, JetBrains, or GitHub Enterprise Cloud, see [docs/setup/](docs/setup/).

**2. Open Copilot Chat in Agent mode**

MCP tools only work in Agent mode. Select **Agent** from the mode dropdown in Copilot Chat, then click the tools icon to see available GitHub MCP tools.

**3. Authenticate**

The remote server uses one-click OAuth by default. For PAT-based auth or GitHub Enterprise Cloud, see [docs/auth-patterns.md](docs/auth-patterns.md).

**4. Run the risk scanner**

Before rolling out to a team:

```bash
git clone https://github.com/your-username/GitHubMCP-Copilot.git
cd GitHubMCP-Copilot
npm install
node risk-scanner/scanner.js --target /path/to/your/mcp-config
```

Output saved to `risk-scanner/output/risk-profile.json`. Review findings, then activate the guardrail.

**5. Activate the guardrail**

The guardrail should be the first thing active and the last thing turned off. See [agents/guardrail/definition.md](agents/guardrail/definition.md).

---

## Repo Structure

```
GitHubMCP-Copilot/
│
├── README.md
├── CONTRIBUTING.md
├── LICENSE
├── package.json
├── vercel.json
├── .env.example
│
├── docs/
│   ├── overview.md
│   ├── features.md
│   ├── getting-started.md
│   ├── glossary.md                    # Start here if new to MCP
│   ├── auth-patterns.md               # OAuth vs PAT, scoped access
│   ├── org-policy.md                  # Org/Enterprise policy, registry, allowlists
│   ├── agent-activation.md
│   ├── agent-communication.md
│   ├── risk-scanner.md
│   ├── tool-design.md
│   ├── production.md
│   └── setup/
│       ├── vscode.md
│       ├── visual-studio.md
│       ├── jetbrains.md
│       └── copilot-cli.md
│
├── lib/
│   ├── correlation.js
│   ├── agent-registry.js
│   └── agent-bus.js
│
├── agents/
│   ├── direct-api-wrapper/            # Repository access
│   ├── composite-service/             # Issue & PR management
│   ├── mcp-to-agent/                  # Copilot-to-agent routing
│   ├── event-driven/                  # Webhook & event response
│   ├── configuration-use/             # Context-adaptive config
│   ├── analytics-data-access/         # Code analytics
│   ├── hierarchical-mcp/              # Multi-repo coordination
│   ├── local-resource-access/         # Local resource access
│   └── guardrail/                     # Always activate first
│
├── risk-scanner/
│   ├── README.md
│   ├── scanner.js
│   ├── output-schema.md
│   ├── output/
│   └── rules/
│       ├── api-scope.md
│       ├── auth-patterns.md
│       ├── data-exposure.md
│       └── loop-detection.md
│
├── admin-panel/
│   ├── README.md
│   ├── index.js
│   ├── onboarding.js
│   ├── onboarding-prompts.md
│   ├── features.md
│   └── upload-spec.md
│
├── config/
├── templates/
└── examples/
```

---

## Core Principles

**Guardrails from real findings, not guesswork.**
Rules come from what the scanner actually found in your MCP config and token scopes — not a generic checklist.

**Markdown as the source of truth.**
Every tool pattern definition is plain markdown. No proprietary formats, no tooling lock-in.

**Modular and opt-in.**
Nothing is forced on. Activate the tool patterns your workflow needs.

**Start with alert, grow into intervene.**
The guardrail runs in two modes. Start in alert mode, validate, then graduate to intervene.

---

## Guardrail Agent

Activate it first, deactivate it last.

| Mode | Behavior |
|------|----------|
| `alert` | Logs suspicious activity and sends notifications. Nothing automatically stopped. Recommended starting point. |
| `intervene` | Blocks or reroutes a tool call when a rule is triggered. Use after validating alert mode. |

**Baseline rules always active:**
- Log all MCP tool calls with timestamps
- Alert on silent failures
- Flag repeated identical failed calls
- Alert on scope violations
- Block token or credential exposure in output
- Alert on unexpected tool activations

---

## Risk Scanner

| Category | What It Looks For |
|----------|-------------------|
| Data Exposure | Repo data in payloads, credentials in output, unvalidated write paths |
| API Scope | Overly broad PAT permissions, calls to unlisted endpoints, hardcoded tokens |
| Auth Patterns | Missing auth gates, admin access without role checks |
| Loop Detection | Unbounded retries, circular tool dependencies, cascading event triggers |

---

## Glossary

New to MCP? See the [Glossary](docs/glossary.md) — no prior background required.

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for standards and submission process.

---

## License

MIT — use it, fork it, build on it. See [LICENSE](LICENSE).
