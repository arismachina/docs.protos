# Simulation Studio

[← Home](Home) · **Simulation Studio**

Simulation Studio is the canvas — a visual graph where you build calculations, connect models to your data, and run them. It is the same feature as the canvas, just accessed from the left sidebar.

> **The Co-engineer can build canvases for you.** Describe what you want to calculate and it will create the blocks, write the code, and wire everything together. You then review the code and approve it before it runs.

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
3. Add blocks and connect them — the Co-engineer can help build the canvas if you describe what you are modelling.
4. For any **Calculation** block, click **Approve** before it will execute (this is a trust gate — you confirm the code is safe to run).

---

## Running It

**Automatic:** When the data in the Data Studio changes (e.g. you activate a different document), the canvas detects the new inputs and recalculates automatically.

**Manual:** Click **Start sequence** to force a full run from scratch. This finds all blocks with no upstream dependencies, runs those first, then cascades through everything downstream. Start sequence runs even unverified calculations, so it is the "run everything" button.

---

## Design Space Exploration (Sweep)

A sweep means running your canvas across a range of values for a parameter instead of a single value — so instead of one result, you get a full curve.

You do this with an **array parameter**. Instead of setting temperature = 25, you set it as an array from 10 to 60 across 20 points. Protos generates the list automatically (linear or logarithmic spacing). When the canvas runs, the downstream calculation executes once for each value and returns all results together.

**Example:** you want to see how dissolution rate changes with temperature. Set temperature as an array from 10°C to 60°C, run the canvas, and you get a chart of dissolution rate across the full range — one run instead of dozens.

To set up a sweep:
1. Add an **array parameter** block instead of a regular parameter.
2. Set min, max, number of points, and spacing (linear or log).
3. Start sequence — results come back as a full output surface you can plot.

---

## Tips

- **Connect inputs from the Data Studio** rather than typing values manually — this links your results back to the exact document that produced them.
- **Name your canvases clearly** — you can have multiple canvases per project for different calculations.
- **Use the Co-engineer** to build calculations — describe what you want to compute and it will write the Python code and wire it up.

---

## See Also

- [Data Studio](Data-Studio) — manage which data documents feed into the canvas
- [Model Library](Model-Library) — register models you want to call from a canvas
- [Glossary → Sweep](Glossary), [Glossary → Canvas](Glossary)

---

*[← Back to Home](Home)*
