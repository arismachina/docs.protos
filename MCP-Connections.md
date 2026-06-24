# MCP Connections

[← Home](Home) · **MCP Connections**

MCP (Model Context Protocol) lets you connect external tools — like Notion, Linear, or Sentry — to the Co-engineer. Once connected, the Co-engineer can use those tools directly in chat, pulling in data and taking actions across your other systems without you switching tabs.

---

## On This Page

- [How It Works](#how-it-works)
- [Setting Up a Connection](#setting-up-a-connection)
- [Discovering and Enabling Tools](#discovering-and-enabling-tools)
- [Using MCP servers in a conversation](#using-mcp-servers-in-a-conversation)
- [Connection Statuses](#connection-statuses)

---

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
