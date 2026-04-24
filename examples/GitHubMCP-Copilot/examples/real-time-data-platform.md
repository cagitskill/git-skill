# Example: Open Source Maintainer

## Scenario

A solo maintainer managing a public GitHub repository with active contributors. They handle issue triage, PR reviews, and release workflows manually today and want to use the GitHub MCP server to bring Copilot into those workflows directly from VS Code.

---

## Setup

- **IDE:** VS Code 1.102
- **Copilot Plan:** Pro
- **Auth:** OAuth (remote server)
- **Config location:** User-level `mcp.json` (applies across all repos)

```json
{
  "servers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/"
    }
  }
}
```

---

## Active Tool Patterns

| Pattern | Why |
|---------|-----|
| Repository Access | Read file content and commit history for context during reviews |
| Issue & PR Management | Triage issues, add labels, post review comments |
| Code Analytics Access | Check contributor activity and commit frequency |
| Guardrail | Alert mode — log all tool calls, flag anything unexpected |

---

## Workflows in Use

### Issue Triage
```
"Look at the last 10 open issues and suggest labels based on their content"
```
Copilot calls `list_issues`, reads each body, and suggests `bug`, `enhancement`, or `question` based on content. Maintainer reviews suggestions and confirms.

### PR Review Assistance
```
"Summarize the changes in PR #47 and flag anything that looks like a breaking change"
```
Copilot calls `get_pull_request` and `list_pull_request_files`, reads the diff, and surfaces a summary with flagged sections.

### Release Notes Draft
```
"List all commits merged to main since the last release tag and draft release notes"
```
Copilot calls `list_commits` with a date range, groups by type, and produces a draft.

---

## Risk Profile Findings

The risk scanner found no critical issues on this setup. One medium finding:

- PAT scope check: OAuth token includes `repo:write` — slightly broader than read-only workflows need. Acceptable given the maintainer also creates issues and comments via Copilot.

Guardrail rule generated: `alert_write_scope_used_in_read_workflow` — alerts if write tools are called unexpectedly.

---

## Lessons from This Setup

- OAuth is sufficient and lowest friction for personal use
- Agent mode selection is the most common friction point for new users — it's not obvious that tools disappear in Ask/Edit mode
- The guardrail's audit log surfaced one unexpected tool call in the first week (a stale MCP config from an old project) — caught before any action was taken
