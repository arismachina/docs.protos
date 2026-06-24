# Co-engineer

[← Home](Home) · **Co-engineer**

The Protos **Co-engineer** is a multi-agent AI system available across all features. It structures data, configures simulations, and surfaces connections — always with traceable sources. Depending on your task, the Co-engineer hands off to specialised sub-agents best suited to help.

> **Important:** Co-engineer only surfaces information it can trace. It will not speculate or fill in gaps with assumptions. Every claim links back to a source you can inspect.

---

## On This Page

- [What Co-engineer Can Do](#what-co-engineer-can-do)
- [How to Use It](#how-to-use-it)
- [Multi-agent system](#multi-agent-system)
- [Sharing a chat session](#sharing-a-chat-session)
- [Co-engineer and Your Data](#co-engineer-and-your-data)
- [How Co-engineer Gets Smarter](#how-co-engineer-gets-smarter)

---

## What Co-engineer Can Do

| Capability | Example |
|-----------|---------|
| **Schema authoring** | Create or update a schema based on your description — e.g. *"Create a schema for tablet formulation experiments"* |
| **Data document creation** | Extract structured data from an uploaded file and create a data document following your schema |
| **Canvas building** | Builds a simulation canvas — adds blocks, writes the code, and wires everything together |
| **Knowledge surfacing** | Find relevant entries from the [Knowledge Library](Knowledge-Library) as you work |
| **Requirements parsing** | Parse an uploaded spec document into structured targets and constraints |
| **Design comparison** | Compare design variants against requirements and surface gaps |

---

## How to Use It

The Co-engineer panel is available from any screen via the **chat icon** in the bottom-right corner.

Type in plain language:

- *"Create a schema for tablet formulation experiments with fields for particle size, binder type, and dissolution rate."*
- *"Extract the key parameters from this test report and create a data document."*
- *"Are there any prior experiments on this material in the Knowledge Library?"*
- *"Build a canvas that takes my formulation data and calculates the adjusted capacity."*

Co-engineer responses always include **source traces** — click any claim to see where it came from.

You can attach files to any message — PDFs, documents, images, and more. The maximum file size per attachment is **15 MB**.

> **Pro features:** Some Co-engineer capabilities require a Pro plan. If you're on a free plan you'll see a prompt to upgrade when you reach a Pro-only feature.

---

## Multi-agent system

The Co-engineer is built on a multi-agent architecture. Rather than a single assistant handling every task, it orchestrates specialised sub-agents and hands off to them mid-conversation depending on what you're doing.

### Sub-agents

- **Co-engineer** — the generalist; handles open-ended questions and coordinates with the other agents
- **Knowledge** — searches the Knowledge Library and the web to surface relevant information and prior work
- **Data** — creates, edits, and validates data documents against your schemas
- **Simulation** — builds and edits canvases in the Simulation Studio

### Talking to indicator

The chat panel shows a **"Talking to"** indicator with the current agent's identity icon and accent colour. This updates automatically when the Co-engineer hands off to a sub-agent.

### Agent handoff

When the Co-engineer delegates to a sub-agent, the chat shows a handoff message and the "Talking to" indicator updates. You don't need to do anything — the handoff happens automatically based on what you asked.

### Slash commands

Typing `/` in the chat composer opens a command picker. Available commands:

- `/agent` — switch to a specific sub-agent by name
- `/help` — get context-aware help from the docs
- `/feedback` — submit feedback about Co-engineer

### MCP servers

An **MCP** button in the lower-left of the chat composer lets you toggle individual MCP servers on or off for the current conversation. You can also check **Default in new conversations** per server so your preferred setup carries over automatically. If no servers are configured yet, the popover links you to [MCP Connections](MCP-Connections) to add one.

---

## Sharing a chat session

Co-engineer sessions can be shared with org members. Open a chat session, click **Share**, and assign **Editor** or **Viewer** access to individuals, teams, or the whole org. Shared sessions appear under **Shared with me** in the chat session list. See [Collaboration & Sharing](Collaboration-and-Sharing) for details.

---

## Co-engineer and Your Data

Co-engineer can take direct actions in your project — creating schemas, data documents, canvases, and requirements — based on what you ask it. These actions happen through the chat; the Co-engineer will tell you what it created or changed.

You can review all Co-engineer sessions for a project by opening the Co-engineer panel and browsing past sessions.

---

## How Co-engineer Gets Smarter

Co-engineer's quality depends on the richness of your workspace:

- **[Model Library](Model-Library):** The more models registered and documented, the better Co-engineer's simulation configuration suggestions.
- **[Knowledge Library](Knowledge-Library):** The richer the library, the more relevant connections Co-engineer can surface as you design.
- **Schema quality:** Well-defined schemas with units and descriptions give Co-engineer more signal when extracting data from files and creating documents.

Investing in these foundations makes Co-engineer progressively more useful over time.

---

## See Also

- [Knowledge Library](Knowledge-Library) — Co-engineer draws on this to surface relevant prior work
- [Model Library](Model-Library) — Co-engineer uses this for simulation configuration recommendations
- [Schemas](Schemas) — Co-engineer can create and update schemas based on your description
- [Simulation Studio](Simulation-Studio) — Co-engineer can recommend model and input configurations
- [MCP Connections](MCP-Connections) — connect external tools the Co-engineer can use in chat

---

*[← Back to Home](Home)*
