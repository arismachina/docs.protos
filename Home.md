# Protos Platform Wiki

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

> **Before you begin:** Search the [Knowledge Library](Knowledge-Library) for prior experiments and decisions relevant to your domain. [Copilot](Copilot) can surface these automatically as you work.

1. Sign in at [protos.arismachina.com](https://protos.arismachina.com) with your Aris Machina account.
2. Create or open a **Project**.
3. Define your data structure using **[Schemas](Schemas)**.
4. Run simulations in **[Simulation Studio](Simulation-Studio)**.
5. Access prior knowledge and sources via the **[Knowledge Library](Knowledge-Library)**.
6. Register reusable models in the **[Model Library](Model-Library)**.
7. Use **[Copilot](Copilot)** at any step to accelerate your work.

---

## Project Overview

A **Project** is your central workspace in Protos. It brings together all artifacts related to a design or R&D campaign — designs, test data, simulations, models, and decisions — in a single, connected canvas.

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Canvas** | Visual graph of all connected nodes: designs, data, parameters, models, results. Every node traces back to its origin. |
| **Design iteration** | Every change is versioned. Compare iterations, roll back, and understand exactly what changed and why. |
| **Traceability** | Every node is linked to its source — an academic paper, internal test, or model output. |
| **PSPP** | The reasoning framework underlying Protos: Process → Structure → Property → Performance. |

### Creating a Project

1. Click **New Project** from the dashboard.
2. Name it: `Domain / Product / Campaign`
   - Example: `Battery / Anode / Q2 Optimization`
   - Example: `Thermal / Cell Pack / Thermal Runaway Study`
3. Add a description: goal, scope, key constraints.
4. Invite collaborators and set their [access level](Collaboration-and-Sharing#canvas-access-levels).

### Navigating the Canvas

| Action | How |
|--------|-----|
| Zoom in/out | Scroll or pinch — inspect nodes or see the full picture |
| Open a node | Click it to view its properties, linked data, and history |
| Filter nodes | Filter bar: design, test, model, or parameter |
| Search | Search across all nodes by name or value |

---

## Features at a Glance

### [Schemas](Schemas)

Define the structure of your engineering data. Instead of disconnected spreadsheets, schemas give every artifact a consistent, queryable shape — reusable across all projects and connectable to external tools.

**What you can schema:** designs, test data, model parameterizations, operating conditions, and data extracted from external files via the Copilot.

[→ Schemas guide](Schemas)

---

### [Simulation Studio](Simulation-Studio)

Run physics-based models and first-principles calculations directly inside your project. Every input and output links back to the canvas — results are always traceable to the design parameters that produced them.

**Core capabilities:** deterministic physics-based models, design space exploration via sweeps, in-platform visualization, full output → input → source trace.

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

### [Copilot](Copilot)

An AI assistant available across all features. It accelerates work by structuring data, configuring simulations, and surfacing connections that would otherwise take hours to find manually — always with traceable sources.

[→ Copilot guide](Copilot)

---

### [Collaboration & Sharing](Collaboration-and-Sharing)

Share canvases with teammates (view-only for non-owners) or publish them as interactive snapshots for external stakeholders — no Protos account required.

[→ Collaboration & Sharing guide](Collaboration-and-Sharing)

---

## Integrations

| System | What Protos does |
|--------|-----------------|
| SharePoint / OneDrive | Browse and import files into the Copilot for data extraction |
| GitHub | Register computational models (Python, scripts) from a repo into the Model Library |

> **Note:** GitHub is for model registration only — not for importing data into schemas. See [Model Library](Model-Library) for details.

---

## Glossary Quick Reference

| Term | Definition |
|------|-----------|
| **Canvas** | The visual graph workspace connecting all project nodes |
| **Design freeze** | The point at which a validated design is locked for handoff — achieved in Protos by publishing a canvas snapshot |
| **Node** | Any artifact on the canvas (design, data, model, result, parameter) |
| **PSPP** | Process → Structure → Property → Performance |
| **Schema** | A defined structure for a type of engineering data |
| **Sweep** | A batch simulation run across a parameter space |
| **TRL** | Technology Readiness Level — Protos is optimized for TRL 0–3 |
| **Trace** | The ability to follow any value back to its original source |

[→ Full Glossary](Glossary)

---

*Maintained by Aris Machina · Questions? Contact [ella@arismachina.com](mailto:ella@arismachina.com)*
