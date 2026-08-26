# Simulation Studio <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

[← Home](Home) · **Simulation Studio**

Simulation Studio is where you build calculations, connect models to your data, and run simulations. You work on a visual canvas — a graph of connected blocks where data flows from inputs through to outputs.

> **The Co-Engineer can build canvases for you.** Describe what you want to calculate and it will create the blocks, write the code, and wire everything together. You then review the code and approve it before it runs.

---

## On This Page

- [What It Is](#what-it-is)
- [Navigating the Editor](#navigating-the-editor)
- [Building a Canvas](#building-a-canvas)
- [Running It](#running-it)
- [Design Space Exploration (Sweep)](#design-space-exploration-sweep)
- [Tips](#tips)

---

## What It Is

Simulation Studio shows a list of your canvases. Each canvas is a graph of connected blocks:

| Block | What it does |
|-------|-------------|
| **Data input** | Pulls in data documents from the Data Studio |
| **Parameter** | A value you can dial up or down (e.g. temperature, concentration) |
| **Calculation** | Python code that transforms upstream data |
| **Model** | Calls an external model registered in Models |
| **Visualization** | Plots the output as a chart |
| **Action** | Triggers a step in a sequence — marked *Soon*, not yet available |

Blocks are laid out left to right, in dependency order. You wire them up from inside a block rather than by dragging on the canvas: open a block and pick its inputs in the **Upstream Components** field. Data then flows through the chain automatically — when an input changes, everything downstream recalculates.

The Add Component dialog also splits **Parameter** into Numerical, Boolean, String, and Array.

> **Model blocks work differently.** A Model block is set up by the Co-Engineer rather than built by hand — the rail's **Model** entry hands you over to it. Adding models is a **Pro** feature; a canvas that already has one runs on any plan.

---

## Navigating the Editor

### Build and Results tabs

Open a canvas first by clicking on any simulation in the list — the Build and Results tabs appear once a canvas is open.

The editor has two top-level tabs:

- **Build** — where you add and connect blocks, write and approve calculations, and configure your canvas. This is where you set up how data flows.
- **Results** — where visualizations and outputs appear after a run. Before a run, Results shows a prompt to run the sequence; after a run it shows expandable charts and calculation outputs.

### Workspace tabs bar

You can have multiple canvases open at the same time. The tabs bar at the top of the editor lets you:

- Switch between open canvases
- Pin tabs you want to keep open
- Close tabs you're done with
- Create a new canvas from the tab bar

### Graph and List views

Next to the Build/Results switch is a small icon toggle between **Graph view** and **List view** (hover for the labels). Graph view is the node-and-arrow canvas described above; List view replaces it with the same blocks laid out as sections grouped by type — Parameter Components, Data Input Components, Model Components, Calculation Components, Visualization Components, and Action Components — useful when you want to scan everything at once instead of following the wiring.

### Exporting and sharing

The **More actions** button (vertical dots) in the Build toolbar opens a menu with **Share…** for the canvas owner, plus an **Export** group: **Graph as PNG**, **Visualization as PNG** (one entry per visualization), and **As PDF**. Importing a canvas isn't here — it's on the tab bar's **+** menu.

### Version History

Every meaningful change to a canvas's components or metadata creates a new version — renaming or moving a block doesn't. Open the version button in the header to browse past versions, compare any two, label one for later, or restore it — restoring creates a new version rather than erasing anything. If a schema, model, or data document this canvas depends on gets a newer version, a banner tells you **a newer version of a dependency is available**, and whether the changes are backward-compatible or include a breaking one; click **Update** to re-pin all outdated dependencies at once. Ask the Co-Engineer to check for you too — see [Versioning](Versioning) for the full picture.

### Components rail

Along the left side of the Build view is the **Components rail** — a palette of block types you can add to the canvas. Click or drag a block type onto the canvas to add it. Each type has a distinct icon so you can tell them apart at a glance.

### Node-details panel

Click any block on the canvas to open its detail panel on the right. Depending on the block type it has up to five sections:

| Section | What it shows |
|---------|--------------|
| **Preview** | An inline chart of the block's output — visualization blocks only. Before a run it reads *"Run the sequence to populate this visualization."* |
| **Details** | Varies by type: calculation and model → approval status, plus a code preview and an **Edit calculation** button for calculations; parameter → type and value, or range, points, and scale for an array; data input → the schema it reads, how many documents, and which keys |
| **Result** | Output values from the last run — or the error, if it failed. Calculation and model blocks only |
| **Sources** | The schema and documents this block reads |
| **Connections** | Upstream and downstream blocks this node is wired to |

The detail panel is available in Graph view.

### Open in Data Studio

Data input blocks carry an **Open in Data Studio** button — on the block's row in List view, and in its **Sources** section in Graph view. It jumps you to the [Data Studio](Data-Studio) on that block's schema, and directly to its document when the block reads exactly one. A block reading several documents takes you to the right schema with your own document selection intact.

---

## Building a Canvas

1. Open **Simulation Studio** from the sidebar — you land on the canvas list.
2. Click an existing canvas to open it, or click **New** (a dropdown with **New Canvas** and **Import Canvas** options) to create one.
3. The canvas opens in the **Build** tab. Add blocks from the **Components rail** on the left, then open each one and set its **Upstream Components** to wire it up — or describe what you're modelling and let the Co-Engineer build it for you.
4. For any **Calculation** or **Model** block, click **Approve & Run** before it will execute (this is a trust gate — you confirm the code is safe to run).

---

## Running It

Click **Start sequence** to run the canvas. Protos finds all **calculation and model** blocks that have no upstream calculation or model dependencies, runs those first, then cascades through downstream executables.

If any calculation or model blocks are unapproved, clicking Start sequence opens an **Unapproved components** dialog listing them. **Approve and run** approves them all and starts the sequence; **Cancel** backs out.

Switch to the **Results** tab to see visualizations and outputs after the run completes.

### Run progress indicators

While a run is in flight the toolbar shows **Running · X of Y done** with a spinner so you can see how far along it is. When all components settle it updates to **Completed X of Y**. If you're in the Build tab when results land, a dot appears on the **Results** tab — click it to see what's new.

### Component status

Each block shows where it has got to: **Queued** (waiting to run), **Running**, **Completed**, **Failed to run**, **Cancelled**, **Unapproved**, **Approved** (approved but not yet run), and **Ready**.

### Cancelling, and leaving mid-run

Runs happen on the server, so you can navigate away and come back — a run in flight will still be there when you return. To stop one, use the jobs menu in the header: it lists active jobs with a **Cancel** button for each. There's no way to cancel a whole sequence in one go.

---

## Design Space Exploration (Sweep)

A sweep means running your canvas across a range of values for a parameter instead of a single value — so instead of one result, you get a full curve.

You do this with an **array parameter**. Instead of setting temperature = 25, you set it as an array from 10 to 60 across 20 points. Protos generates the list automatically (linear or logarithmic spacing). Protos hands the whole list to the downstream calculation as a single input, so the calculation runs once and works across every value at once, returning all the results together.

**Example:** you want to see how dissolution rate changes with temperature. Set temperature as an array from 10°C to 60°C, run the canvas, and you get a chart of dissolution rate across the full range — one run instead of dozens.

To set up a sweep:
1. Add a **Parameter: Array** block from the Add Component dialog instead of a regular parameter.
2. Under **Array Configuration**, set **Min**, **Max**, **Number of Points**, and **Scale** (Linear or Log).
3. Start sequence — results come back as a full output surface you can plot.

### The Design of Experiments section

The sidebar carries a **Design of Experiments** section for the wider DOE workflow being built around sweeps: **Smart Sampling**, **Optimisation**, and **Design Candidates** pinned at the top, plus a **DOE Pipeline** group holding Exploration, Monitoring, Results, Handoff, and Library. Every entry carries a **Soon** badge and can't be opened yet.

Array parameters, above, are the part that works today — one run across a range of values. Batch runs and the planned DOE workflow around them are what that section is reserved for.

---

## Tips

- **Connect inputs from the Data Studio** rather than typing values manually — this links your results back to the exact document that produced them.
- **Name your canvases clearly** — you can have multiple canvases open as tabs for different calculations.
- **Use the Co-Engineer** to build calculations — describe what you want to compute and it will write the Python code and wire it up.

---

## See Also

- [Data Studio](Data-Studio) — manage which data documents feed into the canvas
- [Models](Model-Library) — register models you want to call from a canvas
- [Versioning](Versioning) — canvas version history, update-available banners, and restoring
- [Glossary → Sweep](Glossary), [Glossary → Canvas](Glossary)

---

*[← Back to Home](Home)*
