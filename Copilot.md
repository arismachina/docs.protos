# Copilot

[← Home](Home) · **Copilot**

The Protos **Copilot** is an AI assistant available across all features. It accelerates work by structuring data, configuring simulations, and surfacing connections that would take hours to find manually — always with traceable sources.

> **Important:** Copilot only surfaces information it can trace. It will not speculate or fill in gaps with assumptions. Every claim links back to a source you can inspect.

---

## On This Page

- [What Copilot Can Do](#what-copilot-can-do)
- [How to Use It](#how-to-use-it)
- [Copilot and the Canvas](#copilot-and-the-canvas)
- [How Copilot Gets Smarter](#how-copilot-gets-smarter)

---

## What Copilot Can Do

| Capability | Example |
|-----------|---------|
| **Schema suggestions** | Suggest fields for a new schema based on your domain and project description |
| **Data mapping** | Auto-map imported CSV columns to an existing schema |
| **Simulation config** | Recommend a model and input configuration based on your design parameters |
| **Knowledge surfacing** | Find relevant entries from the [Knowledge Library](Knowledge-Library) as you work |
| **Decision drafts** | Draft a rationale note for a design decision |
| **Anomaly detection** | Flag unexpected values in test data or simulation outputs |
| **Summaries** | Summarize the history of a design iteration across versions |

---

## How to Use It

The Copilot panel is available from any screen via the **chat icon** in the bottom-right corner.

Type in plain language:

- *"What model should I use for ionic conductivity at 60°C?"*
- *"Structure this CSV into a schema for cathode coating test data."*
- *"Summarize what changed between design v0.3 and v0.5."*
- *"Are there any prior experiments on this material in the Knowledge Library?"*
- *"Flag anything unusual in this simulation output."*

Copilot responses always include **source traces** — click any claim to see where it came from.

---

## Copilot and the Canvas

When Copilot surfaces a connection or makes a suggestion, it can take action on the canvas — always with your approval first:

| Action | What happens |
|--------|-------------|
| **Add a node** | Copilot proposes a new node; you confirm before it's added |
| **Link existing nodes** | Copilot detects related nodes not yet connected and proposes a link |
| **Annotate a node** | Copilot adds a rationale or knowledge reference to a node |

You always approve before any change is made to the canvas.

**Reviewing Copilot history:** Go to **Project Settings → Copilot History** to see a log of all Copilot interactions on a project, including what was suggested and what was approved or dismissed.

---

## How Copilot Gets Smarter

Copilot's quality depends on the richness of your workspace:

- **[Model Library](Model-Library):** The more models registered and documented, the better Copilot's simulation configuration suggestions.
- **[Knowledge Library](Knowledge-Library):** The richer the library, the more relevant connections Copilot can surface as you design.
- **Schema quality:** Well-defined schemas with units and descriptions give Copilot more signal for data mapping and anomaly detection.

Investing in these foundations makes Copilot progressively more useful over time.

---

## See Also

- [Knowledge Library](Knowledge-Library) — Copilot draws on this to surface relevant prior work
- [Model Library](Model-Library) — Copilot uses this for simulation configuration recommendations
- [Schemas](Schemas) — Copilot can suggest and auto-map schema fields
- [Simulation Studio](Simulation-Studio) — Copilot can recommend model and input configurations

---

*[← Back to Home](Home)*
