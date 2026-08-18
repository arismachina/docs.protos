# Co-Engineer

[← Home](Home) · **Co-Engineer**

The Protos **Co-Engineer** is a multi-agent AI system available across all features. It structures data, configures simulations, and surfaces connections — always with traceable sources. Behind the scenes it calls specialist agents to do focused work, but it stays the one you're talking to.

> **Important:** Co-Engineer is built to surface information it can trace rather than speculate. When a response draws on the Knowledge Library, it carries source traces you can click to see the exact passage behind it.

---

## On This Page

- [What Co-Engineer Can Do](#what-co-engineer-can-do)
- [How to Use It](#how-to-use-it)
- [Multi-agent system](#multi-agent-system)
- [Sharing a chat session](#sharing-a-chat-session)
- [Co-Engineer and Your Data](#co-engineer-and-your-data)
- [How Co-Engineer Gets Smarter](#how-co-engineer-gets-smarter)

---

## What Co-Engineer Can Do

| Capability | Example |
|-----------|---------|
| **Schema authoring** | Create or update a schema based on your description — e.g. *"Create a schema for tablet formulation experiments"* |
| **Data document creation** | Extract structured data from an uploaded file and create a data document following your schema |
| **Canvas building** | Builds and edits a simulation canvas — adds blocks, writes the code, and wires everything together |
| **Running models** | Run a registered model, or a component on a canvas, and report the result |
| **Model registration** | Register a model from a script, a repo, or an API endpoint, inferring its input and output schemas |
| **Knowledge surfacing** | Find relevant entries from the [Knowledge Library](Knowledge-Library) as you work |
| **Web research** | Search the web and import a page straight into the Knowledge Library |
| **SharePoint import** | Pull files and folders from SharePoint into the Knowledge Library |
| **Requirements parsing** | Parse an uploaded spec document into structured targets and constraints |
| **Connecting external tools** | Register an MCP server you name, then take you to the page to finish setting it up |
| **Taking you to an asset** | Navigate you straight to the schema, canvas, or document it just worked on |
| **Version history** | *"What changed between v2 and v3 of this schema?"* — browse and diff version history, check whether a canvas's dependencies are outdated, and restore a schema, canvas, or data document to a past version |

---

## How to Use It

The Co-Engineer panel opens from the **Co-Engineer icon** in the top-right corner of the header bar, on any project screen.

Type in plain language:

- *"Create a schema for tablet formulation experiments with fields for particle size, binder type, and dissolution rate."*
- *"Extract the key parameters from this test report and create a data document."*
- *"Are there any prior experiments on this material in the Knowledge Library?"*
- *"Build a canvas that takes my formulation data and calculates the adjusted capacity."*

When a response draws on the Knowledge Library it includes **source traces** — click one to see where it came from.

You can attach files to any message — PDFs, documents, images, and more. Up to **10 files** per message, **8 MB** each.

> **Co-Engineer is a Pro feature.** On a free plan the chat panel shows an *Upgrade to Pro* banner and messages can't be sent.

---

## Multi-agent system

The Co-Engineer is built on a multi-agent architecture, but you only ever talk to one agent. Behind the scenes it calls specialists to do focused work and folds their results into its own reply.

### Specialists

- **Knowledge** — searches the Knowledge Library and the web, and imports sources into the library
- **Data** — creates and edits schemas, data documents, and requirements, and sets up the Data Studio
- **Simulation** — builds and edits canvases, and runs models

You never talk to a specialist directly. One other agent does take over the conversation when you ask for it: **Help**, which answers questions about Protos itself (type `/help`).

### Sub-agent progress card

While a specialist is working, a card appears in the chat with its name and a spinner. Expand **Task** to see the brief the Co-Engineer sent it, and watch the steps as it works. The card collapses to a green check when the specialist finishes, or a red cross if it failed — click it open again at any time to see what it did.

### Agent badge

The chat panel shows an **agent badge** with the current agent's identity icon and accent colour. Hover the badge to see a "Talking to" label. It reads Co-engineer for normal work, and changes only when you're with Help.

### Slash commands

Typing `/` in the chat composer opens a command picker. Available commands:

- `/help` — get context-aware help from the docs
- `/feedback` (or `/idea`) — submit feedback about Co-Engineer

### Versioning skill

Ask about version history, whether a canvas's dependencies are current, or to restore something to an earlier state, and the Co-Engineer loads its versioning tools automatically — there's nothing to turn on. See [Versioning](Versioning) for what it can and can't do (model and knowledge-document versions can only be browsed, not restored, this way).

### MCP servers

An **MCP** button in the lower-left of the chat composer lets you toggle individual MCP servers on or off for the current conversation. You can also check **Default in new conversations** per server so your preferred setup carries over automatically. The popover also has **Enable all** and **Disable all**. If no servers are configured yet, it links you to [MCP Connections](MCP-Connections) to add one.

You can also just ask the Co-Engineer to connect a server — give it the URL and it will register it and take you to the MCP servers page to run tool discovery.

---

## Sharing a chat session

Co-Engineer sessions can be shared with community members. Open a chat session, click the **Share** icon, and assign **Editor** or **Viewer** access to individuals, teams, or the whole community — chats can't be made public. Sessions shared with you appear under **Shared with me** in the project's Co-Engineer sessions panel, and open as a read-only transcript. See [Collaboration & Sharing](Collaboration-and-Sharing) for details.

---

## Co-Engineer and Your Data

Co-Engineer can take direct actions in your project — creating schemas, data documents, canvases, and requirements — based on what you ask it. These actions happen through the chat; the Co-Engineer will tell you what it created or changed.

You can review all Co-Engineer sessions for a project by opening the Co-Engineer panel and browsing past sessions.

---

## How Co-Engineer Gets Smarter

Co-Engineer's quality depends on the richness of your workspace:

- **[Models](Model-Library):** The more models registered and documented, the better Co-Engineer's simulation configuration suggestions.
- **[Knowledge Library](Knowledge-Library):** The richer the library, the more relevant connections Co-Engineer can surface as you design.
- **Schema quality:** Well-defined schemas with units and descriptions give Co-Engineer more signal when extracting data from files and creating documents.

Investing in these foundations makes Co-Engineer progressively more useful over time.

---

## See Also

- [Knowledge Library](Knowledge-Library) — Co-Engineer draws on this to surface relevant prior work
- [Models](Model-Library) — Co-Engineer uses this for simulation configuration recommendations
- [Schemas](Schemas) — Co-Engineer can create and update schemas based on your description
- [Simulation Studio](Simulation-Studio) — Co-Engineer can recommend model and input configurations
- [MCP Connections](MCP-Connections) — connect external tools the Co-Engineer can use in chat
- [Versioning](Versioning) — what the Co-Engineer's version tools can browse, diff, and restore

---

*[← Back to Home](Home)*
