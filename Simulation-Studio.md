# Simulation Studio

[← Home](Home) · **Simulation Studio**

Simulation Studio is where you run **physics-based models and first-principles calculations** directly inside your project. Every simulation input and output is linked to the canvas — results are always traceable to the design parameters that produced them.

---

## On This Page

- [Core Capabilities](#core-capabilities)
- [Running a Simulation](#running-a-simulation)
- [Design Space Exploration (Sweep)](#design-space-exploration-sweep)
- [Visualization](#visualization)
- [Connecting Simulations to the Canvas](#connecting-simulations-to-the-canvas)
- [Tips](#tips)

---

## Core Capabilities

| Capability | Description |
|-----------|-------------|
| **Physics-based models** | Deterministic, first-principles models across electrochemistry, thermodynamics, materials science, and more |
| **Design space exploration** | Vary parameters across a range and visualize how performance responds |
| **In-platform visualization** | Plots, charts, 3D views where applicable — no export required |
| **Full traceability** | Follow any output value back to the input that drove it, and back to the source that informed that input |

---

## Running a Simulation

1. Open a project and navigate to **Simulation Studio**.
2. Select a model from your [Model Library](Model-Library) — or [register a new one](Model-Library#registering-a-model).
3. Configure inputs — pull values directly from a schema entry or enter manually.
4. Click **Run**.
5. When complete, results appear as a new node on the canvas, linked to the input configuration used.

> **Tip:** Pull inputs from a schema entry rather than typing manually. This automatically links your simulation to the design it represents on the canvas, making the trace much richer.

---

## Design Space Exploration (Sweep)

Instead of running one simulation at a time, a **sweep** explores a range of parameter combinations in a single batch run. Use sweeps to map performance across a design space and identify optimal regions.

### Setting Up a Sweep

1. In Simulation Studio, click **New Sweep**.
2. Select the model to run.
3. Define the **parameters to vary** — for each:
   - Set min, max, and step size, or provide a list of discrete values.
4. Set a **performance target or constraint** (optional but recommended — helps filter results automatically).
5. Click **Launch Sweep**.

Protos runs all parameter combinations and returns an output surface.

### Reading Sweep Results

| View | What it shows |
|------|--------------|
| **Output surface** | Heatmap or scatter plot showing how performance changes across your parameter space |
| **Filter** | Narrow results to only the combinations that meet your target or constraint |
| **Select and promote** | Pick the best result and promote it to a design iteration on the canvas |

> **Note:** Sweep results are always linked to the specific model version and input schema they used. If you update a model, re-run the sweep to get updated results — old results remain valid and reproducible.

---

## Visualization

Each simulation result includes these views:

| View | What it shows |
|------|--------------|
| **Summary panel** | Key output values at a glance |
| **Charts** | Output values plotted against one or more input parameters |
| **Comparison view** | Side-by-side comparison of multiple simulation runs |
| **History** | How this run's inputs compare to previous runs of the same model |
| **Trace** | Follow any output value back to the input that drove it |

---

## Connecting Simulations to the Canvas

Every simulation run creates a **node on the project canvas**. This node:

- Links to the **input configuration** used: design parameters, model version, operating conditions
- Links to the **model** that was run, at the exact version used
- Is visible to all collaborators with project access
- Can be annotated with a note explaining the hypothesis being tested

This means any result in your project is always one click away from its full provenance.

---

## Tips

- **Run the happy path first** — use nominal parameter values before sweeping edge cases. A baseline run makes sweep results much easier to interpret.
- **Name your runs** — add a short note to each run explaining the hypothesis being tested. The history becomes unreadable without it.
- **Version your models** — if you update a model in the [Model Library](Model-Library), old simulation runs still reference the version they used. Results stay reproducible.
- **Use schema-linked inputs** — pulling inputs from a schema entry (rather than typing manually) means your simulation is automatically linked to the design it represents on the canvas.

---

## See Also

- [Model Library](Model-Library) — register and manage the models you run here
- [Schemas](Schemas) — link schema entries directly as simulation inputs
- [Knowledge Library](Knowledge-Library) — promote a simulation result to institutional knowledge
- [Glossary → Sweep](Glossary), [Glossary → Design space](Glossary)

---

*[← Back to Home](Home)*
