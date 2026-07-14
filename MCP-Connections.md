# MCP Connections

[← Home](Home) · **MCP Connections**

MCP (Model Context Protocol) connects Protos and external tools. It works in **two directions**:

- **Protos → external tools (outbound):** connect tools like Notion, Linear, or Sentry *into* the Co-engineer, so it can use them during a conversation. This is the bulk of this page.
- **Claude → Protos (inbound):** Protos is itself an MCP server, so Claude (desktop or web) can connect *to* Protos and drive your Co-engineer from outside the app. See [Use Protos in Claude](#use-protos-in-claude).

---

## On This Page

- [Use Protos in Claude](#use-protos-in-claude)
- [Connecting external tools to the Co-engineer](#connecting-external-tools-to-the-co-engineer)
- [How It Works](#how-it-works)
- [Setting Up a Connection](#setting-up-a-connection)
- [Discovering and Enabling Tools](#discovering-and-enabling-tools)
- [Using MCP servers in a conversation](#using-mcp-servers-in-a-conversation)
- [Connection Statuses](#connection-statuses)

---

## Use Protos in Claude

*This is the **inbound** direction: Claude connecting to Protos.*

Protos exposes a **remote MCP server** so Claude can talk to your Co-engineer directly — list and create projects, browse conversations, and send messages — without opening Protos.

At the top of the **MCP servers** page (Integrations → MCP servers) you'll see a **"Use your Co-Engineer in Claude"** card. It shows your Protos MCP server URL — `https://<your-protos-domain>/mcp` (for example `https://protos.arismachina.com/mcp`) — with a copy button, and the steps to add it as a custom connector in Claude:

1. In Claude, open **Settings → Connectors**.
2. Click **Add custom connector**.
3. Paste the URL above and continue.
4. Approve the connection when Protos asks you to.

### Signing in (OAuth)

Connecting starts a standard **OAuth 2.1** consent flow. Claude redirects you to a Protos consent screen where you **Allow** or **Deny** access; on approval, Claude receives scoped, revocable access tokens. No API keys are exchanged, and you can revoke access at any time.

> **A Pro plan is required** to use Protos as an MCP server — the same gate as the in-app Co-engineer.

---

## Connecting external tools to the Co-engineer

*This is the **outbound** direction: external servers plugged into your Co-engineer. Everything below covers this direction.*

Any MCP-compatible server works — e.g. Notion, Linear, or Sentry — with no fixed list of supported integrations. Once connected, the Co-engineer can use those tools directly in chat, pulling in data and taking actions across your other systems without you switching tabs.

## How It Works

Each MCP connection points to an MCP-compatible server. Once you connect and enable tools, the Co-engineer can call those tools during a conversation — for example, searching your Notion workspace, creating a Linear issue, or fetching an error from Sentry.

---

## Setting Up a Connection

1. Open the **Integrations** section in the left sidebar (near the bottom) and click **MCP servers**.
2. Click **Add connection**, give it a name, enter the server URL, and choose your authentication method (see below).
3. Select which tools you want the Co-engineer to be able to use.
4. Tool discovery runs automatically after you save.

After saving, use the **⋯** menu on the connection card and choose **Test connection** to verify it.

The Co-engineer can now use those tools in chat.

### Authentication options

**OAuth**
For services like Notion, Linear, and Sentry. After saving, you'll be redirected to the service to authorise access. Protos handles token refresh automatically — if your token expires you'll be prompted to reconnect.

**API key**
For services that issue an API token. Paste your key and set the header name it should be sent under (defaults to `Authorization: Bearer`). You can also add a custom prefix if the service requires one.

**Custom headers**
For services that use non-standard authentication. Add as many key-value header pairs as needed — all values are encrypted at rest.

**None**
For public endpoints that require no authentication.

---

## Discovering and Enabling Tools

After connecting, tool discovery runs automatically. You then choose which tools to enable — only enabled tools are available to the Co-engineer.

If the tools on a server change, open the **⋯** menu on the connection card and choose **Run tools discovery** to sync the list.

---

## Using MCP servers in a conversation

Once servers are configured, you can control which ones are active per conversation directly from the chat. Click the **MCP** button in the lower-left of the Co-engineer composer to open a popover listing your connected servers. Toggle individual servers on or off for the current conversation. The popover also includes **Enable all** and **Disable all** buttons to apply to every server at once. Check **Default in new conversations** per server to carry your preferred setup forward automatically.

---

## Connection Statuses

| Status | Meaning |
|--------|---------|
| **Pending** | Connection added but discovery hasn't run yet |
| **Active** | Discovery completed successfully (tools may still be empty) |
| **Error** | Last discovery attempt failed — check the URL and credentials |
| **Reconnect needed** | OAuth token expired — click reconnect to re-authorise |

---

## See Also

- [Co-engineer](Co-engineer) — the assistant that uses your connected tools

---

*[← Back to Home](Home)*
