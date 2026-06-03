# Simulation Studio

[← Home](Home) · **Simulation Studio**

Simulation Studio is the canvas — a visual graph where you build calculations, connect models to your data, and run them. It is the same feature as the canvas, just accessed from the left sidebar.

---

## On This Page

- [What It Is](#what-it-is)
- [Building a Canvas](#building-a-canvas)
- [Running It](#running-it)
- [Design Space Exploration (Sweep)](#design-space-exploration-sweep)
- [Tips](#tips)

---

## What It Is

When you open Simulation Studio you are opening a canvas. A canvas is a graph of connected blocks:

| Block | What it does |
|-------|-------------|
| **Input** | Pulls in data documents from the Data Studio |
| **Parameter** | A value you can dial up or down (e.g. temperature, concentration) |
| **Calculation** | Python code that transforms upstream data |
| **Model** | Calls an external model registered in the Model Library |
| **Visualization** | Plots the output as a chart |

You connect them left to right. Data flows through the chain automatically — when an input changes, everything downstream recalculates.

---

## Building a Canvas

1. Open **Simulation Studio** from the sidebar.
2. Create a new canvas or open an existing one.
3. Add blocks and connect them — the Copilot can help build the canvas if you describe what you are modelling.
4. For any **Calculation** block, mark it as verified before it will execute (this is a trust gate — you confirm the code is safe to run).

---

## Running It

**Automatic:** When the data in the Data Studio changes (e.g. you activate a different document), the canvas detects the new inputs and recalculates automatically.

**Manual:** Click **Start sequence** to force a full run from scratch. This finds all blocks with no upstream dependencies, runs those first, then cascades through everything downstream. Start sequence runs even unverified calculations, so it is the "run everything" button.

---

## Design Space Exploration (Sweep)

Instead of running one set of inputs at a time, a **sweep** varies a parameter across a range in a single batch run — useful for mapping how an output responds across a design space.

1. Add a **Parameter** block and configure it with a min, max, and step size instead of a fixed value.
2. Start sequence — Protos runs all combinations and returns the full output surface.
3. View results as a chart to find the optimal region.

---

## Tips

- **Connect inputs from the Data Studio** rather than typing values manually — this links your results back to the exact document that produced them.
- **Name your canvases clearly** — you can have multiple canvases per project for different calculations.
- **Use the Copilot** to build calculations — describe what you want to compute and it will write the Python code and wire it up.

---

## See Also

- [Data Studio](Data-Studio) — manage which data documents feed into the canvas
- [Model Library](Model-Library) — register models you want to call from a canvas
- [Glossary → Sweep](Glossary), [Glossary → Canvas](Glossary)

---

*[← Back to Home](Home)*
