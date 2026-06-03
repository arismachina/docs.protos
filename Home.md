# Protos Platform Wiki

> **Protos** is Aris Machina's AI-native R&D workspace for designing and industrializing complex physical systems. It connects first-principles models, experiments, and institutional knowledge into a single, traceable environment.

---

## Table of Contents

- [Getting Started](#getting-started)
- [Project Overview](#project-overview)
- [Schemas](Schemas)
- [Simulation Studio](Simulation-Studio)
- [Model Library](Model-Library)
- [Knowledge Library](Knowledge-Library)
- [Copilot](Copilot)
- [Collaboration & Sharing](Collaboration-and-Sharing)
- [Integrations](Integrations)
- [Glossary](Glossary)

---

## Getting Started

1. Sign in at [protos.arismachina.com](https://protos.arismachina.com) with your Aris Machina account.
2. Create or open a **Project**.
3. Define your data structure using **Schemas**.
4. Run simulations in **Simulation Studio**.
5. Access prior knowledge and sources via the **Knowledge Library**.
6. Register reusable models in the **Model Library**.
7. Use the **Copilot** at any step to accelerate your work.

---

## Project Overview

A **Project** is your central workspace in Protos. It brings together all artifacts related to a design or R&D campaign — designs, test data, simulations, models, and decisions — in a single, connected canvas.

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Canvas** | Visual graph of all connected nodes (designs, data, parameters, models, results). Each node traces back to its origin. |
| **Design iteration** | Every change is versioned. Compare iterations, roll back, and understand exactly what changed and why. |
| **Traceability** | Every node is linked to its source — an academic paper, internal test, or model output. |

### Creating a Project

1. Click **New Project** from the dashboard.
2. Name it: `Domain / Product / Campaign` (e.g. `Battery / Anode / Q2 Optimization`).
3. Add a description: goal, scope, key constraints.
4. Invite collaborators and set their access level.

### Navigating the Canvas

- **Zoom** in/out to inspect nodes or see the full picture.
- **Click a node** to open its detail panel — properties, linked data, and history.
- **Filter** by node type (design, test, model, parameter).
- **Search** across all nodes by name or value.

---

## Schemas

See the full [Schemas](Schemas) page.

Schemas define the **structure of your engineering data**. Instead of disconnected spreadsheets or files, schemas give every artifact a consistent, queryable shape across all your projects.

**What you can schema:**
- Designs and design parameters
- Test data and experimental conditions
- Model parameterizations
- Operating conditions and environmental variables
- Data ingested from external sources (SharePoint, GitHub, Google Drive)

---

## Simulation Studio

See the full [Simulation Studio](Simulation-Studio) page.

Run **physics-based models and first-principles calculations** directly inside your project. Every input and output links back to the canvas — results are always traceable to the design parameters that produced them.

**Core capabilities:**
- Deterministic physics-based models (electrochemistry, thermodynamics, materials science)
- Design space exploration: sweep parameters and visualize performance across a range
- In-platform visualization of results
- Full trace from output → input → source

---

## Model Library

See the full [Model Library](Model-Library) page.

A registry of all computational models in your workspace — physics models, ML models, kinetic models, and custom scripts. Models are registered once and reusable across any project, with full version history.

---

## Knowledge Library

See the full [Knowledge Library](Knowledge-Library) page.

Protos's institutional memory. Every decision, data point, and reference is captured and linked to the design artifacts that used it — so nothing is lost between projects or team members.

**What lives here:**
- Academic papers and literature references
- Internal reports and experimental results
- Decisions and their rationale
- AI-surfaced connections between prior work and current designs

---

## Copilot

See the full [Copilot](Copilot) page.

An AI assistant available across all features. It accelerates work by structuring data, configuring simulations, and surfacing connections that would otherwise take hours to find manually.

---

## Collaboration & Sharing

See the full [Collaboration and Sharing](Collaboration-and-Sharing) page.

### Access Levels

| Role | View | Edit | Share | Publish |
|------|------|------|-------|---------|
| Viewer | ✓ | | | |
| Contributor | ✓ | ✓ | | |
| Editor | ✓ | ✓ | ✓ | |
| Admin | ✓ | ✓ | ✓ | ✓ |

---

## Integrations

| System | What Protos does |
|--------|-----------------|
| SharePoint / OneDrive | Import structured data; sync documents |
| GitHub | Pull code, configs, and datasets into schemas |
| Google Drive | Import spreadsheets and CSVs |
| ERP / PLM | Push design freeze data for production handoff |
| MES | Receive manufacturing feedback to close the PSPP loop |

---

## Glossary

| Term | Definition |
|------|-----------|
| **Canvas** | The visual graph workspace connecting all project nodes |
| **Design freeze** | An immutable snapshot of a validated design |
| **Node** | Any artifact on the canvas (design, data, model, result, parameter) |
| **PSPP** | Process → Structure → Property → Performance — the reasoning framework underlying Protos |
| **Schema** | A defined structure for a type of engineering data |
| **Sweep** | A batch simulation run across a parameter space |
| **TRL** | Technology Readiness Level (Protos is optimized for TRL 0–3) |
| **Trace** | The ability to follow any value back to its original source |

---

*Maintained by Aris Machina. For questions, contact [ella@arismachina.com](mailto:ella@arismachina.com).*
