# MCP Connections

[← Home](Home) · **MCP Connections**

MCP (Model Context Protocol) connects Protos and external tools. It works in **two directions**:

- **Protos → external tools (outbound):** connect tools like Notion, Linear, or Sentry *into* the Co-Engineer, so it can use them during a conversation. This is the bulk of this page.
- **An MCP client → Protos (inbound):** Protos is itself a standard MCP server, so an MCP-compatible client can connect *to* Protos and drive your Co-Engineer from outside the app. Claude (desktop or web) is the worked example, with in-app setup steps. See [Use Protos from an MCP client](#use-protos-from-an-mcp-client).

---

## On This Page

- [Use Protos from an MCP client](#use-protos-from-an-mcp-client)
- [Connecting external tools to the Co-Engineer](#connecting-external-tools-to-the-co-engineer)
- [How It Works](#how-it-works)
- [Discovering servers](#discovering-servers)
- [Setting Up a Connection](#setting-up-a-connection)
- [Discovering and Enabling Tools](#discovering-and-enabling-tools)
- [Using MCP servers in a conversation](#using-mcp-servers-in-a-conversation)
- [Connection Statuses](#connection-statuses)

---

## Use Protos from an MCP client

*This is the **inbound** direction: an MCP client connecting to Protos.*

Protos exposes a **standard remote MCP server** so an MCP-compatible client can talk to your Co-Engineer directly — list and create projects, browse conversations, and send messages — without opening Protos. The server URL is `https://<your-protos-domain>/mcp` (for example `https://protos.arismachina.com/mcp`); any client that supports remote MCP servers with OAuth can point at it. The steps below use **Claude**, which has a built-in setup path in Protos.

At the top of the **MCP servers** page (Integrations → MCP servers) you'll see a **"Use your Co-Engineer in Claude"** card. It shows the MCP server URL with a copy button and the steps to add it as a custom connector in Claude:

1. In Claude, open **Settings → Connectors**.
2. Click **Add custom connector**.
3. Paste the URL above and continue.
4. Approve the connection when Protos asks you to.

### Signing in (OAuth)

Connecting starts a standard **OAuth 2.1** consent flow. Claude redirects you to a Protos consent screen where you **Allow** or **Deny** access; on approval, Claude receives scoped, revocable access tokens. No API keys are exchanged, and you can revoke access at any time.

> **A Pro plan is required** to use Protos as an MCP server — the same gate as the in-app Co-Engineer.

---

## Connecting external tools to the Co-Engineer

*This is the **outbound** direction: external servers plugged into your Co-Engineer. Everything below covers this direction.*

Any MCP-compatible server works — e.g. Notion, Linear, or Sentry — with no fixed list of supported integrations. Once connected, the Co-Engineer can use those tools directly in chat, pulling in data and taking actions across your other systems without you switching tabs.

## How It Works

Each MCP connection points to an MCP-compatible server. Once you connect and enable tools, the Co-Engineer can call those tools during a conversation — for example, searching your Notion workspace, creating a Linear issue, or fetching an error from Sentry.

---

## Discovering servers

The MCP servers page doesn't open on a blank slate. Your first stop is a grid of **preset cards** for well-known servers, so you can connect the popular ones without hunting down their endpoint URLs. **Onshape** leads the grid, followed by **Linear**, **Notion**, **Wolfram Cloud**, **Atlassian**, **Mendeley**, **arXiv**, **Semantic Scholar**, **PubMed Central**, and **Perplexity**.

**Onshape** is the built-in CAD connector — *"Read and build CAD in your Onshape documents"*. Connecting it authorises Protos against **your own** Onshape account, and the Co-Engineer can then search your Onshape documents, inspect and build geometry, and work with the Onshape API on your behalf. It answers CAD and FeatureScript questions from a built-in Onshape reference as well.

Once Onshape is connected, the Co-Engineer's [CAD specialist](Co-engineer#cad-onshape) can author and edit parametric geometry in those documents and save fabrication files back into the project. Until it is connected, the specialist explains what it needs and sends you here.

Each preset card already carries that server's name, endpoint URL, and authentication type. Click **Connect** on a card and Protos creates the connection for you — no form to fill in — and takes you straight into authentication:

- **OAuth servers** (most presets) — you're redirected to the provider to authorise access, then returned to Protos.
- **No-auth servers** (e.g. Wolfram Cloud) — tool discovery runs and the tool picker opens so you can choose which tools to enable.

Servers you've already connected sort to the top of the grid, and a preset that matches one of your existing connections is shown once — as the live connection. To connect a server that isn't in the grid, use **Add connection** in the top-right, described below.

---

## Setting Up a Connection

*Use this for any server that isn't already a preset card — the **Add connection** button handles arbitrary MCP servers.*

1. Open the **Integrations** section in the left sidebar (near the bottom) and click **MCP servers**.
2. Click **Add connection**, give it a name, enter the server URL, and choose your authentication method (see below).
3. Select which tools you want the Co-Engineer to be able to use.
4. Tool discovery runs automatically after you save.

After saving, use the **⋯** menu on the connection card and choose **Test connection** to verify it.

The Co-Engineer can now use those tools in chat.

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

After connecting, tool discovery runs automatically. You then choose which tools to enable — only enabled tools are available to the Co-Engineer.

If the tools on a server change, open the **⋯** menu on the connection card and choose **Run tools discovery** to sync the list.

---

## Using MCP servers in a conversation

Once servers are configured, you can control which ones are active per conversation directly from the chat. Click the **MCP** button in the lower-left of the Co-Engineer composer to open a popover listing your connected servers. Toggle individual servers on or off for the current conversation. The popover also includes **Enable all** and **Disable all** buttons to apply to every server at once. Check **Default in new conversations** per server to carry your preferred setup forward automatically.

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

- [Co-Engineer](Co-engineer) — the assistant that uses your connected tools

---

*[← Back to Home](Home)*
