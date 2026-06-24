# Tutorial: Using the Data Studio

[← Home](Home) · [← Data Studio](Data-Studio)

> For a full explanation of how the Data Studio connects to the canvas, see [Data Studio](Data-Studio).

This tutorial shows you how to get your data documents loaded and ready for a simulation. Takes about 5 minutes.

> **The Co-engineer can do this for you.** Ask it: *"Activate the three most recent electrode coating documents in the Data Studio."* It will select and activate the right documents without you needing to navigate here manually. Come to the Data Studio directly when you want to compare values visually or edit inline.

---

## Step 1 — Open the Data Studio

Click **Data Studio** in the sidebar. When you first open it, it asks you to pick a schema.

![Data Studio empty state — "Select a schema to view documents"](images/ds-01-select-schema-prompt.png)

---

## Step 2 — Pick a schema

Click the **Select a schema…** dropdown. All schemas in your project appear.

![Schema picker open showing Electrode Coating at the top of the list](images/ds-02-schema-picker-open.png){ width="500" }

Click the schema you want to work with — for example **Electrode Coating**.

---

## Step 3 — Activate documents

Click **Select documents…** in the toolbar above the table and choose the documents you want to compare — each selected document becomes a column.

With 3 documents activated you can compare their values side by side at a glance:

![Data Studio with 3 coating variants as columns — Coating A Baseline, Coating B Thinner, and Coating C Silicon Blend](images/ds-03-docs-as-columns.png)

---

## Step 4 — Pin a requirement (optional)

If your project has a requirements document (your target spec), you can pin it as a column next to your designs. This makes the gap between your current designs and your target immediately visible.

To add a requirements document, click **Select documents…** and choose it from the Requirements section of the popover — it appears as a fixed column alongside your design documents.

---

## Step 5 — Edit values directly in the table

Double-click any cell to change its value without leaving the Data Studio. Press **Enter** or click away to save — the change updates the original document immediately.

---

## Step 6 — Analysis Panel

**Analysis Panel** — below the table is a resizable Analysis Panel (toggle with the **g** key) that contains chart tabs (bar chart, scatter plot) and a Gap Analysis tab (coming soon) for comparing values against requirement bounds.

---

## Step 7 — The canvas uses whatever is activated here

Whatever documents are activated here are what the canvas runs on. Go to **Simulation Studio** and click **Start sequence** to run with the new inputs.

---

## Next step

→ [Tutorial: Building Your First Canvas](Tutorial-Simulation-Studio) — now that your data is set up, build the calculation that runs on it.

---

*[← Back to Home](Home)*
