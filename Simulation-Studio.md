# Simulation Studio

← [Back to Home](Home)

Simulation Studio is where you run **physics-based models and first-principles calculations** directly inside your project. Every simulation input and output is linked to the canvas — results are always traceable to the design parameters that produced them.

---

## Core Capabilities

- Deterministic physics-based models (electrochemistry, thermodynamics, materials science, and more)
- **Design space exploration**: vary parameters across a range and visualize how performance responds
- In-platform visualization: plots, charts, 3D views where applicable
- Full trace from output → input → source reference

---

## Running a Simulation

1. Open a project and navigate to **Simulation Studio**.
2. Select a model from your [Model Library](Model-Library) (or register a new one).
3. Configure inputs — pull values directly from your schema or enter manually.
4. Click **Run**.
5. When complete, results appear as a new node on the canvas, linked to the input configuration used.

---

## Design Space Exploration (Sweep)

Instead of running one simulation at a time, a **sweep** lets you explore a range of parameter combinations in a single batch run.

1. In Simulation Studio, click **New Sweep**.
2. Select the model to run.
3. Define the **parameters to vary**:
   - Set min, max, and step size (or provide a list of values) for each parameter.
4. Set a **performance target or constraint** (optional but recommended — helps filter results).
5. Click **Launch Sweep**.
6. Protos runs all combinations and returns an output surface.

### Reading sweep results

- **Output surface**: a heatmap or scatter showing how performance changes across your parameter space.
- **Filter**: narrow results to only the combinations that meet your target.
- **Select and promote**: pick the best result and promote it to a design iteration on the canvas.

---

## Visualization

Each simulation result includes:

| View | What it shows |
|------|--------------|
| **Summary panel** | Key output values at a glance |
| **Charts** | Output values plotted against one or more input parameters |
| **Comparison view** | Side-by-side comparison of multiple simulation runs |
| **History** | How this run's inputs compare to previous runs of the same model |
| **Trace** | Follow any output value back to the input that drove it |

---

## Connecting Simulations to the Canvas

Every simulation run creates a node on the project canvas. This node:

- Links to the **input configuration** used (design parameters, model version, conditions)
- Links to the **model** that was run
- Is visible to all collaborators with project access
- Can be annotated with a note explaining why this run was done

---

## Tips

- **Run the happy path first**: use nominal parameter values before sweeping edge cases.
- **Name your runs**: add a short note to each run explaining the hypothesis being tested — it makes the history much easier to read later.
- **Use schema-linked inputs**: pulling inputs from a schema entry (rather than typing manually) means your simulation is automatically linked to the design it represents on the canvas.
- **Version your models**: if you update a model in the Model Library, old simulation runs will still reference the model version they used — results stay reproducible.

---

*← [Back to Home](Home)*
