# MCP Connections

[← Home](Home) · **MCP Connections**

MCP (Model Context Protocol) lets you connect external tools — like Notion, Linear, or Sentry — to the Co-engineer. Once connected, the Co-engineer can use those tools directly in chat, pulling in data and taking actions across your other systems without you switching tabs.

---

## On This Page

- [How It Works](#how-it-works)
- [Setting Up a Connection](#setting-up-a-connection)
- [Authentication Types](#authentication-types)
- [Discovering and Enabling Tools](#discovering-and-enabling-tools)
- [Connection Statuses](#connection-statuses)

---

## How It Works

Each MCP connection points to an MCP-compatible server. Once you connect and enable tools, the Co-engineer can call those tools during a conversation — for example, searching your Notion workspace, creating a Linear issue, or fetching an error from Sentry.

---

## Setting Up a Connection

1. Go to **Settings → MCP Connections**.
2. Click **+ Add Connection**.
3. Give it a name, enter the server URL, and choose your authentication type.
4. Optionally click **Test** to verify the connection.
5. Click **Run Discovery** to fetch the tools available on that server.
6. Select which tools you want the Co-engineer to be able to use.

The Co-engineer can now use those tools in chat.

---

## Authentication Types

| Type | When to use |
|------|-------------|
| **OAuth** | Services like Notion, Linear, Sentry — you'll be redirected to authorise |
| **API key** | Services that issue a token — paste the key and set the header name |
| **Custom headers** | When you need to pass specific HTTP headers for auth |
| **None** | Public endpoints that require no authentication |

---

## Discovering and Enabling Tools

After connecting, click **Run Discovery** to fetch the list of tools available on that server. You then choose which tools to enable — only enabled tools are available to the Co-engineer.

If the tools on a server change, run discovery again to sync the list.

---

## Connection Statuses

| Status | Meaning |
|--------|---------|
| **Pending** | Connection added but discovery hasn't run yet |
| **Active** | Discovery succeeded and at least one tool is enabled |
| **Error** | Last discovery attempt failed — check the URL and credentials |
| **Needs reconnect** | OAuth token expired — click reconnect to re-authorise |

---

## See Also

- [Co-engineer](Co-engineer) — the assistant that uses your connected tools

---

*[← Back to Home](Home)*
