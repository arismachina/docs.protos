# Data Studio

[← Home](Home) · **Data Studio**

The Data Studio is where you manage your design data and decide what goes into your simulations.

> **The Co-engineer can manage this for you.** Ask it to activate specific documents, create new documents from a file, or set up the Data Studio for a particular schema. You come here directly when you want to compare values visually or edit inline.

---

## On This Page

- [What It Is](#what-it-is)
- [How to Use It](#how-to-use-it)
- [The Analysis Panel](#the-analysis-panel)
- [How It Connects to the Canvas](#how-it-connects-to-the-canvas)

---

## What It Is

The Data Studio is a side-by-side comparison table for your data documents. You pick a schema, select which documents to put on the table, and they all line up as columns so you can compare them at a glance.

It is the step between *"I have data"* and *"I want to run a calculation"*. You use it to decide what goes in before the canvas runs.

---

## How to Use It

1. **Pick a schema** — choose which type of data you want to work with (e.g. "Tablet Formulation", "Electrode Coating"). All documents of that type appear in a list on the left.

2. **Select documents to compare** — each document you activate becomes a column in the table. You can activate multiple variants side by side.

3. **Pin a requirement** — if your project has a requirements document (your target spec), you can pin it as a column next to your designs. This makes the gap between your current designs and your target immediately visible.

4. **Edit inline** — double-click any cell to edit a value directly in the table. No need to open the document separately.

Once you have the right documents active, the canvas picks them up automatically and runs.

---

## The Analysis Panel

Alongside the table there is an analysis panel where you can plot the data across your selected documents:

| Chart type | Use for |
|-----------|---------|
| **Bar chart** | Compare a single field across all your variants — e.g. porosity across 5 formulations |
| **Scatter plot** | Plot one field against another — e.g. particle size vs. dissolution rate |

You configure each chart by picking which schema fields go on which axis. The chart updates as you change your document selection.

> **Note:** Gap Analysis is coming soon.

---

## How It Connects to the Canvas

A canvas contains **Input components** that read from whichever documents are active in the Data Studio. This means:

- You do not hardwire specific documents into a canvas — you choose them in the Data Studio
- Swapping which documents are active is enough to re-run the canvas on different data
- You can test a new variant by activating it in the Data Studio without touching the canvas at all

The canvas detects the change and recalculates automatically.

**The relationship in one line:**

```
Data Studio (which data) → Canvas (what to do with it) → Results
```

---

## See Also

- [Schemas](Schemas) — define the structure your data documents follow
- [Simulation Studio](Simulation-Studio) — build and run the calculations that use your activated documents
- [Co-engineer](Co-engineer) — the Co-engineer can create data documents and activate them in the Data Studio on your behalf

---

*[← Back to Home](Home)*
