# Tutorial: Using the Data Studio

[← Home](Home) · [← Data Studio](Data-Studio)

> For a full explanation of how the Data Studio connects to the canvas, see [Data Studio](Data-Studio).

This tutorial shows you how to get your data documents loaded and ready for a simulation. Takes about 5 minutes.

---

## Step 1 — Open the Data Studio

Click **Data Studio** in the sidebar. When you first open it, it asks you to pick a schema.

![Data Studio empty state — "Select a schema to view documents"](images/ds-01-select-schema-prompt.png)

---

## Step 2 — Pick a schema

Click the **Select a schema…** dropdown. All schemas in your project appear.

![Schema picker open showing Electrode Coating at the top of the list](images/ds-02-schema-picker-open.png)

Click the schema you want to work with — for example **Electrode Coating**.

---

## Step 3 — Activate documents

All data documents following that schema appear in a list on the left. Click the ones you want to compare — each one becomes a **column** in the table.

With 3 documents activated, the table shows all fields as rows and each document as a column, so you can compare their values side by side.

![Data Studio with 3 coating variants as columns — Coating A, B, and C side by side](images/ds-03-docs-as-columns.png)

---

## Step 4 — Edit inline if needed

Double-click any cell to edit the value directly. Press Enter to save. The change saves back to the original document immediately.

---

## Step 5 — The canvas picks this up automatically

Whatever documents are activated here are what the canvas runs on. Go to **Simulation Studio** — the canvas will re-run automatically using the documents you just selected. Swap a document out, it recalculates.

---

*[← Back to Home](Home)*
