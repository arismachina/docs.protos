# Protos Docs <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

> **Protos** is Aris Machina's AI-native R&D workspace for designing and industrializing complex physical systems. It connects first-principles models, experiments, and institutional knowledge into a single, traceable environment — from concept through manufacturing handoff.

---

## On This Page

- [Getting Started](#getting-started)
- [Projects & the Canvas](#project-overview)
- [Features at a Glance](#features-at-a-glance)
- [Integrations](#integrations)
- [Glossary Quick Reference](#glossary-quick-reference)

---

## Getting Started

> **The [Co-Engineer](Co-engineer) can help throughout your workflow.** It can create schemas, populate data documents, build canvases, and search the Knowledge Library — all from a chat. Use it as much or as little as you like alongside the features directly.

1. Go to [protos.arismachina.com](https://protos.arismachina.com){target="_blank"} and tell Protos what you want to create — this creates your first project and kicks off guided onboarding.
2. From that description, the Co-Engineer creates one artifact at a time — a starter **schema**, a **data document** from it, an entry in the **Knowledge Library**, and a starter **canvas** in Simulation Studio.
3. After each one it tells you what it made and pauses, so send another message to move on to the next step. Once all four exist, onboarding hands off to the general-purpose Co-Engineer so you can keep building or adjust anything it set up.

---

## Project Overview

A **Project** is your central workspace in Protos. It brings together all artifacts related to a design or R&D campaign — designs, test data, simulations, models, and decisions — in a single, connected canvas.

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Canvas** | A graph of connected blocks (parameters, inputs, calculations, models, visualizations). Data flows through the chain automatically. |
| **Traceability** | Field values created by the Co-Engineer link back to the Knowledge Library source they came from. |
| **PSPP** | The reasoning framework underlying Protos: Process → Structure → Property → Performance. |

### Creating a Project

1. Click **Create new project**.
2. Add a name and description for your project.
3. Answer questions about your project — goals, constraints, and any other relevant context.
4. The [Co-Engineer](Co-engineer) automatically generates and runs a kickoff plan to set up your workspace.

### Finding an Existing Project

The project switcher in the header — click your current project's name — is available from every page. At the bottom of it, click **Browse all projects** (only shown while its search box is empty) to open the full project library, where you can:

- Filter by **Mine**, **Shared with me**, **Public**, or **All**, with live counts on each tab
- Search by name, description, tags, or domain
- See which project is currently active (marked **Active** with a highlighted border) and **set another one active**
- **Open** a project, or as its owner, upload a cover image or **Delete** it
- On the **Public** tab, clone an Aris-published example project as a starting point

### Navigating the Canvas

| Action | How |
|--------|-----|
| Zoom in/out | Scroll or pinch — inspect blocks or see the full picture |
| Open a block | Click it to view its properties, inputs, and results |
| Filter | Filter bar: parameter, Data input, calculation, model, or visualization |
| Search | Search across all blocks by name |

---

## Features at a Glance

### [Schemas](Schemas)

Define the structure of your engineering data. Instead of disconnected spreadsheets, schemas give every artifact a consistent, queryable shape — reusable across all projects and connectable to external tools.

Use schemas for: designs, test data, model parameterizations, operating conditions, and data extracted from external files via the Co-Engineer.

[→ Schemas guide](Schemas)

---

### [Data Studio](Data-Studio)

Your workbench for managing and comparing design data before running a simulation. Pick a schema, select which data documents to activate, and the canvas picks them up automatically. Includes side-by-side comparison, inline editing, and charting across document variants.

[→ Data Studio guide](Data-Studio)

---

### [Simulation Studio](Simulation-Studio)

Build and run calculation canvases — connect your data to models and calculations, and see results update automatically. Supports Python calculations, external models, parameter sweeps, and in-canvas visualization.

[→ Simulation Studio guide](Simulation-Studio)

---

### [Model Library](Model-Library)

A registry of all computational models in your workspace — physics models, ML models, kinetic models, and custom scripts. Register once, reuse across any project, with full version history.

[→ Models Library guide](Model-Library)

---

### [Knowledge Library](Knowledge-Library)

Protos's institutional memory. Every decision, data point, and reference is captured and linked to the design artifacts that used it — so nothing is lost between projects or team members.

What lives here: academic papers, internal reports, decisions and rationale, experimental results, AI-surfaced connections.

[→ Knowledge Library guide](Knowledge-Library)

---

### [Co-Engineer](Co-engineer)

An AI assistant available across all features. It accelerates work by structuring data, configuring simulations, and surfacing connections that would otherwise take hours to find manually — always with traceable sources.

[→ Co-Engineer guide](Co-engineer)

---

### [MCP Connections](MCP-Connections)

Connect external tools — e.g. Notion, Linear, or Sentry — to the Co-Engineer. Once connected, the Co-Engineer can use those tools directly in chat without you switching tabs. Found under **Integrations** in the sidebar, not as a top-level page.

[→ MCP Connections guide](MCP-Connections)

---

### [Collaboration & Sharing](Collaboration-and-Sharing)

A cross-cutting capability, not a page of its own — share canvases, schemas, data documents, models, and Co-Engineer chats from each resource's own Share dialog. Two sharing roles: **Editor** (can co-edit in place, on some resource types) and **Viewer** (read-only). The resource creator is always the owner. Organise teammates into teams or share with your whole community. Publications is a separate feature that lets you publish canvases as interactive snapshots for external stakeholders.

[→ Collaboration & Sharing guide](Collaboration-and-Sharing)

---

### [Versioning](Versioning)

Schemas, canvases, models, data documents, and knowledge documents all keep a version history — view past versions and restore one if a change turns out to be wrong.

[→ Versioning guide](Versioning)

---

## Integrations

Integrations are accessible from the **Integrations** section in the left sidebar.

| System | What Protos does |
|--------|-----------------|
| SharePoint / OneDrive | Browse and import files into the Co-Engineer for data extraction |
| GitHub | Register computational models (Python, scripts) from a repo into the Models Library |
| MCP servers | Connect external tools to the Co-Engineer so it can use them directly in chat |
| Register a Model | Register external computational models into the Model Library |

> **Note:** GitHub is for model registration only — not for importing data into schemas. See [Model Library](Model-Library) for details.

---

## Glossary Quick Reference

| Term | Definition |
|------|-----------|
| **Canvas** | The visual graph workspace connecting all project nodes |
| **Design freeze** | The point at which a validated design is locked for handoff — achieved in Protos by publishing a canvas snapshot |
| **Node** | A block on the canvas — parameter, input, calculation, model, or visualization |
| **PSPP** | Process → Structure → Property → Performance |
| **Schema** | A defined structure for a type of engineering data |
| **Sweep** | A batch simulation run across a parameter space |
| **TRL** | Technology Readiness Level — Protos is optimized for TRL 0–3 |
| **Trace** | The ability to follow any value back to its original source |

[→ Full Glossary](Glossary)

---

*Maintained by Aris Machina*
