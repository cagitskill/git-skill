# GitHub Copilot MCP Field Guide — Features

A structured, safety-first guide for setting up, configuring, and extending the GitHub MCP server with GitHub Copilot. Covers every supported IDE, both auth paths, org/enterprise policy, and a guardrail layer built from real findings in your own configuration.

**Philosophy:** The GitHub MCP server is the main character. Your repositories, issues, and pull requests are the backstory. The guardrail watches every action to make sure nothing goes off script.

---

## IDE Setup Guides (4)

| IDE | What's Covered |
|-----|---------------|
| VS Code | Remote setup, MCP Registry path, manual config, PAT auth, per-project config |
| Visual Studio | Remote setup, Enterprise Cloud config, agent mode walkthrough |
| JetBrains | Plugin installation, remote setup, agent mode |
| GitHub Copilot CLI | Built-in server, adding external servers, workspace config, org allowlists |

---

## GitHub MCP Tool Patterns (9)

Every tool pattern ships with four files: a plain-markdown definition, an example `mcp.json` config, a working usage script, and a test checklist.

| Pattern | GitHub MCP Capability |
|---------|-----------------------|
| Repository Access | Search repos, read files, fetch content |
| Issue & PR Management | Create, update, list, comment on issues and pull requests |
| Copilot-to-Agent Routing | Route output to specialized workflows based on context |
| Webhook & Event Response | React to GitHub events: pushes, reviews, status checks |
| Context-Adaptive Config | Adjust tool behavior based on repo or org context |
| Code Analytics Access | Commit history, contributor metrics, activity data |
| Multi-Repo Coordination | Actions across multiple repositories or teams |
| Local Resource Access | Scoped access to local files alongside GitHub context |
| Guardrail | Monitor and protect all MCP tool activity |

---

## Authentication

- OAuth path: one-click, GitHub-managed, recommended for most users
- PAT path: explicit scope control, required for GitHub Enterprise Cloud with data residency
- Scope recommendations by use case
- Environment variable storage patterns (no hardcoded tokens)
- Risk scanner auth findings and guardrail auth rules

See [docs/auth-patterns.md](auth-patterns.md).

---

## Risk Scanner

- Scans your `mcp.json`, PAT scopes, and tool permissions
- Four scan categories: data exposure, API scope, auth patterns, loop detection
- Produces `risk-profile.json` — read automatically by the guardrail on startup
- GitHub-specific rules: PAT scope violations, hardcoded tokens, overly broad permissions
- Verbose mode for detailed terminal output
- Output schema documented in `risk-scanner/output-schema.md`

---

## Guardrail Agent

- Two operating modes: `alert` (log and notify) and `intervene` (block or reroute)
- Six baseline rules active on every setup regardless of scanner findings
- Risk scanner output auto-loaded as additional rules
- Escalation threshold: configurable alert count before severity escalates
- Alert destination: any webhook (Slack, PagerDuty, email, custom)
- Credential exposure rule always intervenes regardless of mode
- Independent audit log separate from tool activity logs

---

## Org & Enterprise Policy

- Step-by-step policy enablement for orgs and enterprises
- Registry configuration — curated server lists for teams
- Allow-all vs. registry-only access policy
- GitHub Enterprise Cloud with data residency — different URL, PAT required
- Tool-level access requirements by Copilot plan type
- Admin checklist before org rollout

See [docs/org-policy.md](org-policy.md).

---

## Agent Communication Layer (`lib/`)

### Correlation (`lib/correlation.js`)
- Correlation ID threaded across every inter-tool call in a chain
- Correlated logger: every log line prefixed with ID and pattern name

### Agent Registry (`lib/agent-registry.js`)
- Central registry of all tool patterns
- `getAgent(id)` — resolve URL by pattern ID
- `listActiveAgents()` — all currently active patterns
- Soft-disable patterns without removing registry entries

### Agent Bus (`lib/agent-bus.js`)
- Standard message envelope with sender, target, correlation ID, timestamp
- `send()`, `broadcast()`, `reportToGuardrail()` — fire-and-forget, never blocks

---

## Onboarding Prompt Flow

- Guided setup questions covering plan type, IDE, auth preference, org vs. personal
- Answers generate a ready-to-use `mcp.json` config
- One-time pre-deploy step

See [admin-panel/onboarding-prompts.md](../admin-panel/onboarding-prompts.md).

---

## Templates (4)

| Template | For |
|----------|-----|
| Individual Developer | Fast setup on Free or Pro — remote server, OAuth, VS Code |
| Team Setup | Copilot Business — shared registry, policy enabled, team mcp.json |
| Enterprise Config | Copilot Enterprise — policy, allowlists, data residency options |
| Sensitive Repo Setup | Restricted PAT scopes, tighter guardrail, explicit token management |

---

## Examples (3)

- Open Source Maintainer — issue triage, PR review, release workflow automation
- Enterprise Dev Team — policy-governed access, org registry, audit trails
- Solo Developer — fast personal setup, VS Code remote config, OAuth

---

## Developer Experience

- Every tool pattern folder self-contained: `definition.md`, `config.example.json`, usage script, `test.md`
- Pattern definitions in plain markdown — no proprietary format
- `// Customize:` comments mark every decision point in every script
- `.env.example` documents every environment variable the system uses
- MIT license — fork, adapt, ship

---

## Security

- Guardrail active before any tool pattern goes live
- Internal calls authenticated with `INTERNAL_AGENT_TOKEN`
- PAT scope recommendations aligned to actual tool use
- Risk scanner findings drive guardrail rules — not hand-written assumptions
- No secrets in code — all sensitive values in environment variables
