# Risk Scanner

The risk scanner analyzes your GitHub MCP configuration and PAT scopes before you go live. It finds real exposure areas and produces a structured risk profile. That profile becomes the source for your guardrail's rule set.

Run it before your first team rollout. Re-run it whenever you change your `mcp.json`, rotate PATs, or add new tool patterns.

---

## What It Scans

| Category | What It Checks |
|----------|---------------|
| **Data Exposure** | Credentials in output, private key file access, unverified private repo access |
| **API Scope** | PAT permissions vs. actual tool usage, hardcoded tokens, admin scope on standard tools |
| **Auth Patterns** | Plaintext PAT in config, missing auth for GHE, shared PATs across users |
| **Loop Detection** | Unbounded retries, circular tool dependencies, cascading event triggers |

Full rule definitions in [rules/](rules/).

---

## How to Run

```bash
# Scan your mcp.json and config
node risk-scanner/scanner.js --target /path/to/your/mcp-config

# Verbose output (shows all findings with detail)
node risk-scanner/scanner.js --target /path/to/your/mcp-config --verbose

# Skip a specific rule category
node risk-scanner/scanner.js --target /path/to/your/mcp-config --skip data-exposure
```

Output saved to `risk-scanner/output/risk-profile.json`.

---

## Output

The scanner produces a `risk-profile.json` with:

- One entry per finding
- Severity level: `info`, `medium`, `high`, `critical`
- Category: `data-exposure`, `api-scope`, `auth-patterns`, `loop-detection`
- Description of what was found
- Suggested guardrail rule to address it

The guardrail agent reads this file automatically on startup and adds the findings to its active rule set.

See [output-schema.md](output-schema.md) for the full format.

---

## After the Scan

1. Review the findings in `risk-scanner/output/risk-profile.json`
2. Remove any false positives (add a comment explaining why)
3. Address critical and high findings before going live
4. Activate the guardrail — it reads the profile automatically
5. Commit `risk-profile.json` to the repo so the guardrail loads it on every startup

---

## When to Re-Run

- After changing `mcp.json`
- After rotating or creating a new PAT
- After adding a new tool pattern
- After a significant change to your repo structure or org policy
- On a regular schedule (quarterly minimum for production setups)
