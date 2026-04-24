# GitHub Copilot MCP Onboarding Prompt Flow

This guided flow generates a ready-to-use `mcp.json` config and guardrail baseline based on your environment. Answer each question — your answers drive the output.

---

## Questions

### 1. What is your Copilot plan?

- Free
- Pro / Pro+
- Business (org-managed)
- Enterprise (enterprise-managed)

*Drives: whether org policy is required, which tools are available, auth recommendation*

---

### 2. Which IDE are you setting up?

- VS Code
- Visual Studio
- JetBrains IDE
- GitHub Copilot CLI
- Multiple IDEs

*Drives: config file path, setup instructions, IDE-specific notes*

---

### 3. Is this for personal use or a team?

- Just me
- A small team (under 10)
- A larger team or org (10+)
- Enterprise rollout

*Drives: whether to use user-level or project-level config, registry recommendation*

---

### 4. Which setup path do you prefer?

- Remote server (GitHub-hosted, recommended)
- Local server (runs on my machine)

*Drives: server URL, installation steps*

---

### 5. Which authentication method do you want to use?

- OAuth (one-click, recommended)
- Personal Access Token (explicit scope control)

*Drives: auth config block in mcp.json, PAT scope recommendations*

---

### 6. Are you connecting to GitHub.com or GitHub Enterprise Cloud?

- GitHub.com
- GitHub Enterprise Cloud (with data residency)

*Drives: server URL, auth requirements (data residency requires PAT)*

---

### 7. Which tool patterns do you want to activate?

Select all that apply:
- Repository Access (read files, search code, commit history)
- Issue & PR Management (create, update, comment)
- Webhook & Event Response (react to GitHub events)
- Copilot-to-Agent Routing (route output to workflows)
- Context-Adaptive Config (adjust behavior by repo/branch)
- Code Analytics Access (metrics, contributor data)
- Multi-Repo Coordination (actions across multiple repos)
- Local Resource Access (local files alongside GitHub context)
- Guardrail (always recommended)

*Drives: which pattern folders to activate, PAT scope requirements*

---

### 8. What is the sensitivity level of the repositories you'll be working with?

- Public repos only
- Mix of public and private
- Private repos only
- Sensitive / regulated content (internal tooling, compliance-adjacent)

*Drives: guardrail mode recommendation, PAT scope strictness, template selection*

---

### 9. Do you need audit logging?

- No, console output is fine
- Yes, log to a file
- Yes, send alerts to a webhook (Slack, PagerDuty, etc.)

*Drives: guardrail alert_destination config*

---

### 10. Where do you want to store PAT credentials?

- Environment variables (recommended)
- IDE secret storage
- I'm using OAuth — no PAT needed

*Drives: mcp.json auth block format*

---

### 11. What is your preferred guardrail starting mode?

- Alert mode (log and notify, nothing blocked — recommended for first 2 weeks)
- Intervene mode (block rule-violating tool calls — use after validating alert mode)

*Drives: guardrail mode config*

---

## Output

Your answers generate:

1. **`config/platform.json`** — your environment profile
2. **`mcp.json`** — ready to paste into your IDE config
3. **Agent activation list** — which tool patterns to turn on first
4. **Template recommendation** — which starter template fits your setup
5. **Guardrail baseline config** — starting rule set before the risk scanner runs

Run the onboarding script:

```bash
node admin-panel/onboarding.js
```

Then run the risk scanner to refine your guardrail rules before going live:

```bash
node risk-scanner/scanner.js --target /path/to/your/mcp-config
```
