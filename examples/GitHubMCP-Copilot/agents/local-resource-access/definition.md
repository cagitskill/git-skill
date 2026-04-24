# Tool Pattern: Local Resource Access

## What It Does

Provides scoped, access-controlled reads and writes to local files — config files, local notes, project assets — alongside GitHub MCP context from the remote server.

## When to Use It

- You want Copilot to reason about local files and GitHub context together
- You have config files or local documentation that should inform GitHub actions
- You need to write outputs locally (e.g., generated release notes saved to a file)

## When Not to Use It

- For GitHub-hosted files, use [Repository Access](../direct-api-wrapper/definition.md) instead

## How It Works

```
Request received for local resource
        ↓
Path validated against allowlist
        ↓
Credential pattern check (no tokens/secrets in path or content)
        ↓
Scoped read or write executed
        ↓
Audit log entry written (every access, every time)
        ↓
Activity reported to guardrail
```

## Safety Requirements

- Path allowlist enforced — no access outside defined directories
- Exclusion list blocks sensitive paths (`.env`, credential files, key material)
- Every access produces an audit log entry — success and failure both logged
- Guardrail monitors for credential patterns in file content

## Test Checklist

See [test.md](test.md).
