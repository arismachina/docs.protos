# Protos Docs <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

> **Protos** is Aris Machina's AI-native R&D workspace for designing and industrializing complex physical systems. It connects first-principles models, experiments, and institutional knowledge into a single, traceable environment — from concept through manufacturing handoff.

---

## On This Page

- [Getting Started](#getting-started)
- [Projects & the Canvas](#project-overview)
- [Features at a Glance](#features-at-a-glance)
- [Integrations](#integrations)
- [Your Account](#your-account)
- [Glossary Quick Reference](#glossary-quick-reference)

---

## Getting Started

> **The [Co-Engineer](Co-engineer) can help throughout your workflow.** It can create schemas, populate data documents, build canvases, and search the Knowledge Library — all from a chat. Use it as much or as little as you like alongside the features directly.

1. Go to [protos.arismachina.com](https://protos.arismachina.com){target="_blank"}. With no project yet you land on the Co-Engineer, in the same chat window you'll use from then on: tell it what you want to create, and sending that message creates your first project and kicks off guided onboarding. Example projects sit below the composer — opening one shows it read-only, so you can look without starting anything.
2. From that description, the Co-Engineer works through five steps, one per turn — a starter **schema**, a **data document** from it, an entry in the **Knowledge Library**, a guided stop at the **Models Library**, and a starter **canvas** in Simulation Studio.
3. After the five steps, onboarding hands off to the general-purpose Co-Engineer so you can keep building or adjust anything it set up.

### Before you have a project

Nearly everything in Protos hangs off a project, so until you have one the feature links in the sidebar are greyed out. A card at the top of the sidebar always says why, and which one you get depends on where you are:

| Card | When you see it | Where it takes you |
|------|-----------------|--------------------|
| **Create a project** | You have no projects at all | The project library |
| **Start with Co-Engineer** | You've signed up but haven't sent a first message, and you've navigated away from the home screen | Back to the Co-Engineer home |
| **Choose a project** | You have projects but none is selected — typically a bookmark or an emailed link opened on a new browser | The project library |
| **Continue Onboarding** | Guided onboarding is still in progress | Back to the Co-Engineer to finish the remaining steps |

Delete your last project and you land back on the project library with the **Create a project** card waiting.

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

Setup runs as a short wizard — **Name → Details → Data → Configure** — and once the project exists you can click any step in the progress bar to jump back to it. Your progress is saved, so a setup you leave part-way through picks up where you stopped.

1. Click **Create new project**. Add a **name** and **description**.
2. **Details** — answer a few questions about the project, one at a time. Each is generated from your name and description, with clickable suggested answers you can take or type over. The first — *"What are you trying to achieve in this project?"* — sets the goal. Later questions are marked **(optional)** and have a **Skip** button, and **Skip** in the footer moves past the whole step.
3. **Data** — optionally bring in what you already have, grouped into three panels: **BOM**, **Materials & supplier data**, and **Simulation & design candidates**. Each panel's **Ingest data** menu takes files **From device**, **From SharePoint**, or **From GitHub**, or pulls in existing **Knowledge library** documents and **Data documents**. Leave any panel empty to skip it. A drop zone at the bottom takes any other supporting documents.
4. **Configure** — the [Co-Engineer](Co-engineer) sets up your workspace and drafts a kickoff plan from everything you gave it. You land on the new project with the plan waiting as a pill in the chat: **Run** it as drafted, **Edit** it first, or dismiss it and start on your own.

### Finding an Existing Project

The project switcher in the header — click your current project's name — is available from every page, and lists projects you own alongside those shared with you. Which actions a row offers depends on your role on that project; see [Projects shared with you](Collaboration-and-Sharing#projects-shared-with-you).

At the bottom of the switcher, click **Browse all projects** (only shown while its search box is empty) to open the full project library, where you can:

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

### [Design of Experiments](Design-of-Experiments)

Spend a limited budget of experiments where they buy the most information. Mark what you're chasing and what you're free to vary, and each round recommends the next batch from everything measured so far — whether the results come from a simulation canvas, a formula, or a lab bench.

[→ Design of Experiments guide](Design-of-Experiments)

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

Connect external tools — e.g. Notion, Linear, or Atlassian — to the Co-Engineer. Once connected, the Co-Engineer can use those tools directly in chat without you switching tabs. Found under **Integrations** in the sidebar, not as a top-level page.

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
| SharePoint | Browse and import files and folders into the Knowledge Library |
| GitHub | Register computational models (Python, scripts) from a repo into the Models Library |
| MCP servers | Connect external tools to the Co-Engineer so it can use them directly in chat |
| Register a Model | Register external computational models into the Model Library |

> **Note:** GitHub is for model registration only — not for importing data into schemas. See [Model Library](Model-Library) for details.

---

## Your Account

Click your name or avatar at the bottom of the left sidebar and choose **Billing & Usage**. That page holds your subscription and your account settings:

| Card | What you can do |
|------|-----------------|
| **Subscription** | See your current plan and trial status. **Subscribe to Pro** opens Stripe Checkout, which collects your full billing address for invoicing and tax. **Change Plan** and **Manage Subscription** open the Stripe customer portal. |
| **Refer friends, earn free Pro** | Share a referral link — a referred signup gets a longer trial, and so do you. |
| **Email preferences** | Turn **Marketing emails** on or off. Account emails — sign-in, security, and billing notices — are always sent. |
| **Display name** | Set or change the name shown across the app — in the user menu and on your avatar. The card tells you the current one, and the button reads **Change name** (or **Set name** if you haven't set one). |
| **Password** | **Change password**. Accounts that sign in with Google don't have one to change, and the card says so instead. |

The same page has a **Danger Zone** for deleting your account.

New accounts start on a **15-day Pro trial** with no card required — 30 days if you were referred. There is no free tier behind it, so subscribe before it ends to keep working.

### What's new

The same user menu has **What's new** — a searchable history of everything shipped, with the full changelog alongside it. Click any entry to read the detail, and **Check it out** takes you straight to the feature. Browsing never marks an update as read, so anything you haven't seen yet still appears in the pop-up.

---

## Glossary Quick Reference

| Term | Definition |
|------|-----------|
| **Canvas** | The visual graph workspace connecting all project nodes |
| **Design freeze** | The point at which a validated design is locked for handoff — achieved in Protos by publishing a canvas snapshot |
| **Node** | A block on the canvas — parameter, input, calculation, model, or visualization |
| **PSPP** | Process → Structure → Property → Performance |
| **Schema** | A defined structure for a type of engineering data |
| **Sweep** | A single run that varies one or more parameters across a range, returning an output surface |
| **TRL** | Technology Readiness Level — Protos is optimized for TRL 0–3 |
| **Trace** | The ability to follow any value back to its original source |

[→ Full Glossary](Glossary)

---

*Maintained by Aris Machina*
