# Data Studio <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

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

1. **Pick a schema** — use the schema picker at the top to choose which type of data you want to work with (e.g. "Electrode Coating"). Documents of that type become available.

2. **Select documents to compare** — use the document selector dropdown to choose which documents to show. Each selected document becomes a column in the table. You can show multiple variants side by side.

3. **Edit inline** — double-click any cell to edit a value directly in the table. No need to open the document separately.

4. **Add a requirement** — when creating a new document, use the **Create type** toggle to switch from Design to Requirement. Requirement documents use the same schema fields but numeric inputs become min/max ranges instead of single values. The requirement appears as a column in the table alongside your designs, and the Gap Analysis panel uses its bounds to show which designs fall within spec.

Once you have the right documents selected, they are available as inputs to any canvas in Simulation Studio.

---

## The Analysis Panel

Alongside the table there is an analysis panel where you can plot the data across your selected documents:

| Chart type | Use for |
|-----------|---------|
| **Bar chart** | Compare a single field across all your variants — e.g. porosity across 5 formulations |
| **Scatter plot** | Plot one field against another — e.g. particle size vs. dissolution rate |

You configure each chart by picking which schema fields go on which axis. The chart updates as you change your document selection.

When a requirement document is selected, the Gap Analysis tab shows which of your design values fall inside or outside the requirement bounds.

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
