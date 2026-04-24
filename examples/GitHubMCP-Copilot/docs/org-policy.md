# Org & Enterprise MCP Policy

## The Policy Requirement

If your organization is on Copilot Business or Copilot Enterprise, the "MCP servers in Copilot" policy is **disabled by default**. Members cannot use MCP with Copilot until an admin enables it. This applies to both the GitHub MCP server and any third-party MCP servers.

Copilot Free, Pro, and Pro+ users are not governed by this policy — they can use MCP regardless of org settings.

---

## Enabling the Policy

### For Organizations

1. Go to your organization on GitHub
2. Click **Settings** under your org name
3. In the sidebar, under *Code, planning, and automation*, click **Copilot → Policies**
4. In the **Features** section, set **MCP servers in Copilot** to **Enabled**
5. Optionally add a registry URL (see below)
6. Click **Save**

### For Enterprises

1. Navigate to your enterprise on GitHub
2. At the top of the page, click **AI controls**
3. In the sidebar, click **MCP**
4. Set **MCP servers in Copilot** to **Enabled everywhere**
5. Choose your access policy:
   - **Allow all** — no restrictions, any MCP server can be used
   - **Registry only** — only servers from your configured registry may run

Changes apply immediately to all members.

---

## Configuring a Registry

A registry is a curated list of approved MCP servers your team can discover and use. Setting one up gives you control over which servers are available without blocking MCP entirely.

**Why use a registry:**
- Prevent members from connecting to unapproved or unknown MCP servers
- Standardize the toolset across your team
- Give members a discoverable list of approved servers from within their IDE

**Setting a registry URL:**

In org or enterprise settings under the MCP section, enter your registry URL in the **MCP Registry URL** field and click **Save**.

If you're using Azure API Center as your registry backend, enter the base URL only — do not include route suffixes like `/v0.1/servers`.

---

## Access Control Policies

| Policy | Behavior |
|--------|----------|
| Allow all | No restrictions — members can connect to any MCP server |
| Registry only | Only servers listed in your registry are allowed |

Set the policy at the enterprise level for uniform access across all orgs. Set it at the org level if different teams have different requirements.

---

## GitHub Enterprise Cloud with Data Residency

For GitHub Enterprise Cloud instances with data residency, the MCP server URL is different:

```
https://copilot-api.YOURSUBDOMAIN.ghe.com/mcp
```

Replace `YOURSUBDOMAIN` with your GitHub Enterprise Cloud subdomain. PAT authentication is required (OAuth is not supported for data residency instances).

mcp.json configuration:
```json
{
  "servers": {
    "github": {
      "url": "https://copilot-api.YOURSUBDOMAIN.ghe.com/mcp",
      "requestInit": {
        "headers": {
          "Authorization": "Bearer YOUR_GITHUB_PAT"
        }
      }
    }
  }
}
```

---

## What Tools Require a Paid License

The GitHub MCP server is available to all GitHub users regardless of plan. However, specific tools within the server inherit the access requirements of their corresponding GitHub features.

- If a feature requires a paid GitHub or Copilot license, the equivalent MCP tool requires the same subscription
- Tools that interact with Copilot cloud agent require a paid Copilot license

Check the [GitHub MCP server repository](https://github.com/github/github-mcp-server) for the latest tool-level access requirements.

---

## Admin Checklist

Before enabling MCP for your org:

- [ ] Decide on registry vs. allow-all policy
- [ ] Set up registry if using registry-only access
- [ ] Communicate to members which MCP servers are approved
- [ ] Confirm members are on Copilot Business or Enterprise (Free/Pro users manage their own access)
- [ ] Run the risk scanner on representative team configs
- [ ] Enable guardrail monitoring before rollout
- [ ] Document approved PAT scopes for your team's use case
