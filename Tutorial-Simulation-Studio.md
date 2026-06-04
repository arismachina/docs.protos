# Tutorial: Building Your First Canvas

[← Home](Home) · [← Simulation Studio](Simulation-Studio)

This tutorial walks you through building a canvas from scratch — connecting your data, running a calculation, and reading the results. It takes about 10 minutes.

---

## What you'll build

A canvas that takes an electrode coating data document, runs a calculation on it, and plots the output. By the end you'll understand how data flows through a canvas and how to run it.

---

## Before you start

You need:
- A project with at least one schema and one data document. If you don't have these yet, go to [Schemas](Schemas) first and create a simple one, then create a data document from it in the [Data Studio](Data-Studio).

---

## Step 1 — Open Simulation Studio

Click **Simulation Studio** in the left sidebar. You'll land on the canvas home page where all your canvases are listed.

Click **New Canvas**, give it a name (e.g. "My first calculation"), and open it.

You'll see an empty graph workspace — this is your canvas.

---

## Step 2 — Add an Input block

An Input block is how you bring data from the Data Studio into the canvas.

1. Click **Add block** (or the `+` button) and choose **Input**.
2. Select your schema from the dropdown.
3. Select the data document you want to use.
4. Choose which fields from the document you want to expose — these become the outputs of this block that other blocks can use.

You'll see the Input block appear on the canvas with your chosen fields listed on it.

> **Why this matters:** You're not hardcoding values — you're pointing at a live data document. If you change the document in the Data Studio later, the canvas will automatically pick up the new values.

---

## Step 3 — Add a Parameter block

A Parameter is a value you control directly on the canvas — not pulled from a document.

1. Click **Add block** → **Parameter**.
2. Choose **Number** as the type.
3. Give it a name (e.g. "temperature") and set a value (e.g. 25).
4. Set a unit (e.g. °C).

The parameter block appears on the canvas. You can change its value any time and the canvas will recalculate.

---

## Step 4 — Add a Calculation block

A Calculation block runs Python code on the inputs you connect to it.

1. Click **Add block** → **Calculation**.
2. Give it a name (e.g. "compute output").
3. Write your Python code in the editor. The inputs from upstream blocks are available as variables. For example:

```python
porosity = inputs["porosity"]
temperature = inputs["temperature"]

result = porosity * (1 + 0.002 * temperature)

return {"adjusted_porosity": result}
```

4. Connect the Input block and the Parameter block to this Calculation block by drawing arrows from them to it.

> **The code gets its inputs from whatever blocks are connected upstream.** The variable names match the field names you exposed in the Input block and the parameter name you set.

---

## Step 5 — Approve the calculation

Calculation blocks require your approval before they run. This is a safety check — you're confirming the code is safe to execute.

Look for the **Approve** button on the calculation block and click it.

Once approved, the block will run automatically whenever its inputs change. Before approval, it stays paused.

> **Note:** If you edit the code later, it goes back to needing approval. This happens every time the code changes.

---

## Step 6 — Add a Visualization block

1. Click **Add block** → **Visualization**.
2. Connect the Calculation block to it.
3. Write the visualization code — for example, a simple chart of the output.

Or skip this for now and just read the raw result directly from the calculation block.

---

## Step 7 — Run the canvas

There are two ways to run:

**Automatic:** Change the value of your parameter (e.g. set temperature to 30) and the canvas runs the calculation automatically because its input changed.

**Manual — Start sequence:** Click the **Start sequence** button at the top of the canvas. This forces a full run from scratch — it finds all blocks with no upstream connections, runs those first, then cascades through everything downstream in order.

> **When to use Start sequence:** Use it the first time you run a canvas, or any time you want to force a full re-run regardless of what changed. It also runs unapproved calculations — if you click Start sequence and have unapproved blocks, Protos will warn you and ask you to confirm before proceeding.

---

## Step 8 — Read the results

Click on the Calculation block to open its detail panel. You'll see:
- The inputs that were used
- The output values your code returned
- The execution status (completed, failed, running)

If it failed, the error message is shown here. Fix the code, save, re-approve, and run again.

---

## Step 9 — Try swapping the data

Go to the **Data Studio**, activate a different data document of the same schema, and come back to the canvas. The Input block will show the new values and the calculation will re-run automatically.

This is the core of the Data Studio → Canvas flow: the canvas doesn't care *which* document it's running on — it runs on whatever is active. Swap the data, get new results.

---

## What you've learned

- Canvases are graphs of connected blocks: Input → Parameter → Calculation → Visualization
- Inputs pull live data from the Data Studio — they're not hardcoded
- Calculation blocks need to be approved before they run automatically
- Start sequence forces a full manual run
- Swapping active documents in the Data Studio is how you test different variants

---

## Next steps

- **Sweeps:** Instead of a single parameter value, set it as an array (min, max, number of points) to run the calculation across a range and get a full output curve. See [Simulation Studio → Sweeps](Simulation-Studio#design-space-exploration-sweep).
- **Models:** Replace the Calculation block with a Model block to call an external registered model instead of inline Python code. See [Model Library](Model-Library).
- **Co-engineer:** Ask the Co-engineer to build the canvas for you — describe what you want to calculate and it will write the code and wire it up.

---

*[← Back to Home](Home)*
