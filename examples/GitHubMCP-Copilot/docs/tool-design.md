# Designing Prompts That Work Well With GitHub MCP Tools

## How Copilot Selects Tools

When you type a prompt in Agent mode, Copilot decides which GitHub MCP tools to call based on what you're asking. Understanding how it makes that decision helps you write prompts that get the right tool called the first time.

---

## Be Specific About What You Want

Vague prompts lead to tool calls that return more than you need or not quite what you intended.

| Less effective | More effective |
|----------------|---------------|
| "Show me the repo" | "List all open issues labeled 'bug' in this repo" |
| "Check the PRs" | "Show me PRs opened in the last 7 days that haven't had a review yet" |
| "What's in the codebase" | "Search for all files that import the auth module" |

---

## Scope Your Requests

GitHub MCP tools operate on specific resources. Telling Copilot the exact resource reduces unnecessary tool calls.

```
# Scoped
"Get the last 5 commits on the main branch of this repo"

# Unscoped (may call multiple tools to figure out what you mean)
"Show me recent commits"
```

---

## Confirm Before Write Operations

Tools that create or modify GitHub resources (create issue, merge PR, post comment) will prompt for confirmation before executing. This is by design — don't work around it.

If you're building automated workflows that need to skip confirmation, that's a use case for the Webhook & Event Response pattern with pre-defined actions, not open-ended Copilot prompts.

---

## Chaining Operations

You can chain multiple tool calls in a single prompt. Copilot will execute them in sequence.

```
"List all issues labeled 'needs-triage', then add the label 'triaged' to the ones that have a clear reproduction step in the description"
```

For complex chains, break them into steps if you want to review the output at each stage.

---

## Tool Limitations

- MCP tools operate on what your PAT or OAuth token has access to — if a tool call fails, check your token scopes first
- Some tools require a paid Copilot license — they surface the same access requirements as their underlying GitHub features
- Agent mode is required — tools are invisible in Ask or Edit mode
