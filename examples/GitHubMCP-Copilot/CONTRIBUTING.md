# Contributing to GitHub Copilot MCP Field Guide

Contributions are welcome. This guide covers what's useful to add, how to submit it, and what won't be merged.

---

## What's Most Useful

**New tool pattern definitions**
If you've built a GitHub MCP workflow that isn't covered by the existing 9 patterns, add it. The pattern needs a `definition.md`, `config.example.json`, a usage script with `// Customize:` comments, and a `test.md` checklist.

**Improvements to existing definitions**
Clearer explanations, better examples, corrections to config snippets, updated PAT scope recommendations.

**New templates**
If you've set up the GitHub MCP server for a team type or environment not covered by the existing 4 templates, add one. Include a real `mcp.json` config, the rationale for each decision, and a checklist.

**Additional risk scanner rules**
GitHub-specific rules for PAT scope patterns, org policy edge cases, or new tool exposure scenarios.

**IDE-specific setup notes**
Edge cases, version-specific behavior, or setup paths not covered in the existing guides.

**New examples**
Real scenarios showing how the GitHub MCP server is used in practice. Keep examples realistic — they're the most useful content in the repo.

---

## Standards

**Accuracy first.**
Every config snippet, PAT scope recommendation, and setup step should reflect how the GitHub MCP server actually works. If you're not certain something is accurate, check against [GitHub's official MCP documentation](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp) before submitting.

**Plain language.**
Definitions and explanations should be readable by someone new to MCP, not just experienced developers. The glossary sets the tone — match it.

**No proprietary formats.**
All definitions stay in plain markdown. No JSON schemas, no custom DSLs, no tooling requirements beyond a text editor.

**No hardcoded credentials.**
Any config snippet that shows a token or credential must use an environment variable reference, not a placeholder that looks like a real value.

---

## Submission Process

1. Fork the repo
2. Create a branch named for what you're adding: `add-xcode-setup`, `fix-pat-scope-table`, `new-pattern-code-review`
3. Make your changes
4. Open a PR with a clear description of what changed and why
5. Tag it: `new-pattern`, `fix`, `docs`, `template`, or `example`

---

## What Won't Be Merged

- Config snippets that hardcode real credentials or tokens
- Patterns that require proprietary tooling or paid services beyond GitHub Copilot
- Setup guides for IDEs or tools not officially supported by the GitHub MCP server
- Content that contradicts GitHub's official documentation without a clear rationale
- Marketing or promotional content

---

## Questions

Open an issue tagged `question` before building anything large. It's worth a quick check that what you're planning fits the scope of the repo.
