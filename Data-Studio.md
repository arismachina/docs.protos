# Data Studio <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

[← Home](Home) · **Data Studio**

The Data Studio is where you manage your design data and decide what goes into your simulations.

> **The Co-Engineer can manage this for you.** Ask it to activate specific documents, create new documents from a file, or set up the Data Studio for a particular schema. You come here directly when you want to compare values visually or edit inline.

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

2. **Select documents to compare** — use the document selector to choose which documents to show. Each selected document becomes a column in the table. You can show multiple variants side by side.

3. **Edit inline** — double-click any cell to edit a value directly in the table. No need to open the document separately. Inline editing is available on documents you created.

4. **Add a requirement** — when creating a new document, use the **Create type** toggle to switch from Design to Requirement. Requirement documents use the same schema fields, but numeric inputs become bounds instead of single values — a min and a max, or a value with a tolerance in absolute or percentage terms. The requirement appears as a column in the table alongside your designs, and the Gap Analysis panel uses its bounds to show which designs fall within spec.

5. **Start a DOE** — with a design and a requirement both on the table, **Start a DOE** turns the sheet into the starting point for a [Design of Experiments](Design-of-Experiments) loop. Mark which fields to chase and which to vary, and the loop recommends the experiments worth running. Designs it produces come back here, beside the one it started from.

Once you have the right documents selected, they are available as inputs to any canvas in Simulation Studio.

You can also get here straight from a canvas: any Data input block in [Simulation Studio](Simulation-Studio) has an **Open in Data Studio** button that lands you on that block's schema.

### The rest of the toolbar

Alongside the schema and document pickers there is **New Document** — pick a schema and it opens the create form — a search box matching name or tag, and a tag filter that narrows both the document list and the schema picker. Each document column's **⋯** menu offers Tags, Copy, Edit, Publish/Unpublish, Share, Save version, and Remove.

Values render with their units, and requirement values render as their bounds — `≥ x`, `≤ x`, `min–max`, or `value ± tol`. Requirement bounds are also drawn onto charts as hatched out-of-spec zones, which you can toggle from the chart legend.

---

## The Analysis Panel

Below the table there is an analysis panel where you can plot the data across your selected documents. It opens on a **Gap Analysis** tab, which is always present and describes your current selection. Add your own charts with **New Graph**. Each graph can be one of:

| Chart type | Use for |
|-----------|---------|
| **Bar chart** | Compare a single field across all your variants — e.g. porosity across 5 formulations |
| **Scatter plot** | Plot one field against another — e.g. particle size vs. dissolution rate |

Each graph tab has a **Graph type** setting for switching between them.

You configure each chart by picking which schema fields go on which axis. The chart updates as you change your document selection.

Click **New Graph** to add as many chart tabs as you need — each is independently configured, and they're named Graph 1, Graph 2, and so on. Double-click a tab to rename it, or hover it and click the **×** to remove it. Gap Analysis can't be renamed or removed. You can also **pin** the current set of tabs as your default layout for new projects; the pin belongs to one project at a time, so pinning here moves it from wherever it was.

Bar chart and scatter tabs also carry a **download** button, which opens a menu offering **Download as PNG** and **Download as PDF** — the PDF is a single page sized to the chart. This works the same way as [Simulation Studio](Simulation-Studio#exporting-and-sharing) visualization exports. Gap Analysis has no download button.

---

## How It Connects to the Canvas

A canvas's **Data input** blocks read specific data documents. You pick those documents when you add the block, choosing from whatever is active in the Data Studio — so the Data Studio decides what is *available* to a canvas, and the block decides what it *reads*.

- Activating a document here makes it selectable in a Data input block. It doesn't add it to a block that already exists — to run on a new variant, open the block and add it there.
- Editing a value here does flow straight through: any approved calculation or model downstream of a block that reads that document re-runs automatically.

To run a canvas against a changed selection, go to Simulation Studio and click **Start sequence**.

**The relationship in one line:**

```
Data Studio (what's available) → Data input block (what this canvas reads) → Results
```

---

## See Also

- [Schemas](Schemas) — define the structure your data documents follow
- [Simulation Studio](Simulation-Studio) — build and run the calculations that use your activated documents
- [Design of Experiments](Design-of-Experiments) — starts from a design and a requirement on this table
- [Co-Engineer](Co-engineer) — the Co-Engineer can create data documents and activate them in the Data Studio on your behalf

---

*[← Back to Home](Home)*
