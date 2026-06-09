# Protos Docs

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

> **The [Co-engineer](Co-engineer) can help throughout your workflow.** It can create schemas, populate data documents, build canvases, and search the Knowledge Library — all from a chat. Use it as much or as little as you like alongside the features directly.

1. Sign in at [protos.arismachina.com](https://protos.arismachina.com){target="_blank"} with your Aris Machina account.
2. Upon first login, you will be prompted with an **onboarding tutorial**. Follow it to unlock all the protos features.
3. Once the onboarding is finished, you can create or open a new **Project**. This will take you through a **Project Configuration** step that we recommend you follow for best performance of the **[Co-engineer](Co-engineer)**.
4. You do not need to be overly detailed during the configuration step as you can always refine your project later.
5. During configuration, upload your reference material to the **[Knowledge Library](Knowledge-Library)** — the Co-engineer draws on this for every task. You can always upload later.
6. After the configuration, you can open the **[Co-engineer](Co-engineer)** on the right-hand side and and keep describing your project. It will set up schemas, create data documents, and guide you through the workflow.
7. Review and refine what it creates in the **[Data Studio](Data-Studio)** and **[Schemas](Schemas)** pages.
8. Build or run simulations in **[Simulation Studio](Simulation-Studio)**.
9. Register external models in the **[Model Library](Model-Library)** if you have your own simulation code.

---

## Project Overview

A **Project** is your central workspace in Protos. It brings together all artifacts related to a design or R&D campaign — designs, test data, simulations, models, and decisions — in a single, connected canvas.

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Canvas** | A graph of connected blocks (parameters, inputs, calculations, models, visualizations). Data flows through the chain automatically. |
| **Traceability** | Field values created by the Co-engineer link back to the Knowledge Library source they came from. |
| **PSPP** | The reasoning framework underlying Protos: Process → Structure → Property → Performance. |

### Creating a Project

1. Click **New Project** from the dashboard.
2. Add a name and description for your project.
3. Answer questions about your project — goals, constraints, and any other relevant context.
4. The [Co-engineer](Co-engineer) generates a kickoff plan. Review it, then run it to set up your workspace.

### Navigating the Canvas

| Action | How |
|--------|-----|
| Zoom in/out | Scroll or pinch — inspect blocks or see the full picture |
| Open a block | Click it to view its properties, inputs, and results |
| Filter | Filter bar: parameter, input, calculation, model, or visualization |
| Search | Search across all blocks by name |

---

## Features at a Glance

### [Schemas](Schemas)

Define the structure of your engineering data. Instead of disconnected spreadsheets, schemas give every artifact a consistent, queryable shape — reusable across all projects and connectable to external tools.

**What you can schema:** designs, test data, model parameterizations, operating conditions, and data extracted from external files via the Co-engineer.

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

[→ Model Library guide](Model-Library)

---

### [Knowledge Library](Knowledge-Library)

Protos's institutional memory. Every decision, data point, and reference is captured and linked to the design artifacts that used it — so nothing is lost between projects or team members.

**What lives here:** academic papers, internal reports, decisions and rationale, experimental results, AI-surfaced connections.

[→ Knowledge Library guide](Knowledge-Library)

---

### [Co-engineer](Co-engineer)

An AI assistant available across all features. It accelerates work by structuring data, configuring simulations, and surfacing connections that would otherwise take hours to find manually — always with traceable sources.

[→ Co-engineer guide](Co-engineer)

---

### [Collaboration & Sharing](Collaboration-and-Sharing)

Share canvases with teammates (view-only for non-owners) or publish them as interactive snapshots for external stakeholders — no Protos account required.

[→ Collaboration & Sharing guide](Collaboration-and-Sharing)

---

## Integrations

| System | What Protos does |
|--------|-----------------|
| SharePoint / OneDrive | Browse and import files into the Co-engineer for data extraction |
| GitHub | Register computational models (Python, scripts) from a repo into the Model Library |

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
