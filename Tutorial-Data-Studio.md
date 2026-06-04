# Tutorial: Using the Data Studio

[← Home](Home) · [← Data Studio](Data-Studio)

This tutorial walks you through the Data Studio — how to select documents, compare variants, edit values, and set up what the canvas runs on. It takes about 5 minutes.

---

## What you'll do

Pick a schema, activate a set of data documents side by side, compare them against a requirement, edit a value inline, and confirm the canvas picks up the change.

---

## Before you start

You need:
- A schema with at least one data document. If you don't have these yet, go to [Schemas](Schemas) first.
- Ideally 2–3 data documents of the same schema so the comparison is useful.

---

## Step 1 — Open the Data Studio

Click **Data Studio** in the left sidebar.

You'll see a schema picker at the top. Select the schema you want to work with. All data documents that follow that schema appear in a list on the left.

---

## Step 2 — Activate documents

Click on one or more documents in the list to activate them. Each one you activate becomes a **column** in the table on the right.

The columns show all the fields from the schema as rows, with each document's values filling in the cells. If you activate three documents, you get three columns — all fields aligned so you can compare directly.

> **This is the core job of the Data Studio:** not just storing data, but laying it out so you can see differences at a glance.

---

## Step 3 — Add a requirement column (optional but useful)

If your project has a requirements document — a target spec defining what your design needs to hit — you can pin it alongside your design documents.

Look for the **Requirements** section in the document list on the left. Select your requirement document the same way you selected your design documents.

It appears as a column in the table, usually styled differently so it's visually distinct. Now every design value sits next to its target, making gaps immediately visible.

---

## Step 4 — Edit a value inline

If you spot a value that needs correcting, you can edit it directly in the table without opening the document separately.

**Double-click any cell** to enter edit mode. Type the new value and press Enter to save, or Escape to cancel.

The change is saved back to the original data document immediately — this is not a temporary view, it updates the real document.

---

## Step 5 — Check what the canvas will run on

The documents you have activated in the Data Studio are what the canvas uses as inputs.

Open **Simulation Studio** from the sidebar. If you have a canvas with an Input block pointing at this schema, it will now use the documents you just activated. If you've made any changes, the canvas will recalculate automatically.

> **The activation state persists.** When you come back to the Data Studio later, the same documents will still be selected. You don't need to re-activate them every session.

---

## Step 6 — Swap variants to test a different design

To test a different set of documents:

1. In the document list, deselect one document by clicking it again.
2. Select a different document.
3. The table updates immediately — the new document's values replace the old column.
4. Go back to Simulation Studio — the canvas will re-run automatically with the new inputs.

This is the fastest way to compare how different designs perform: activate one, see the canvas results, swap to another, compare.

---

## Step 7 — Use the Analysis Panel

At the right side of the Data Studio is an analysis panel. You can add charts here to visualise your data directly — without running a simulation.

Click **Add chart** and choose:
- **Bar chart** — compare a single field across all your active documents (e.g. porosity across 5 electrode formulations)
- **Scatter plot** — plot one field against another across all documents (e.g. particle size vs. mass loading)

Select which schema fields go on which axis and the chart renders from your active documents instantly.

> This is useful for spotting outliers or patterns in your data before you run a simulation.

---

## What you've learned

- The Data Studio is where you decide which documents the canvas runs on
- Activating documents puts them in columns for side-by-side comparison
- Adding a requirement column lets you see the gap between your designs and your target
- Double-clicking a cell edits the value directly in the underlying document
- Swapping active documents is how you test different variants without touching the canvas
- The analysis panel gives you instant charts without needing a simulation

---

## Next steps

- **Create new documents here:** If you need a new design variant, you can create a data document directly from the Data Studio rather than navigating to the schema page.
- **Ask Co-engineer:** Ask the Co-engineer to create or populate data documents based on values from your Knowledge Library — it will activate them in the Data Studio for you.
- **Build the canvas:** Once your data is set up, go to [Simulation Studio](Simulation-Studio) to build the calculation that runs on it.

---

*[← Back to Home](Home)*
