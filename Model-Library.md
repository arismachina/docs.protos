# Model Library

← [Back to Home](Home)

The **Model Library** is a registry of all computational models available in your workspace — physics models, ML models, kinetic models, domain-specific solvers, and custom scripts. Models are registered once and reusable across any project, fully versioned, and traceable.

---

## Why It Exists

Without a shared model library, teams rebuild the same models for each project. Protos makes models **reusable assets**: register once, call from anywhere, track every version, trace every result.

---

## Registering a Model

1. Go to **Model Library** in the sidebar.
2. Click **Register Model**.
3. Fill in the required fields:

| Field | Description |
|-------|-------------|
| **Name** | Clear, searchable name (e.g. `Doyle-Fuller-Newman Electrochemical Model`) |
| **Description** | What the model does, when to use it, known limitations |
| **Domain** | e.g. electrochemistry, thermal, mechanical, materials |
| **Inputs** | Each input: name, type, unit, required/optional |
| **Outputs** | Each output: name, type, unit |
| **Source** | Python script, COMSOL file, external API endpoint, etc. |
| **Version tag** | e.g. `v1.0.0` — use semantic versioning |

4. Save. The model is now available in [Simulation Studio](Simulation-Studio).

---

## Model Source Types

| Source type | How it works in Protos |
|-------------|----------------------|
| **Python script** | Upload the script; Protos executes it in a managed environment |
| **COMSOL / FEM file** | Upload the model file; Protos calls it with the specified input parameters |
| **External API** | Provide the endpoint URL and auth config; Protos calls it on run |
| **Wrapped solver** | Pre-packaged domain solvers (Aris-provided or community) |

---

## Finding a Model

- **Search** by name, domain, or keyword.
- **Filter** by domain, input type, or output type.
- **Sort** by most recently used or most recently updated.
- Click any model to see its full documentation, input/output schema, and version history.

---

## Model Versioning

Every update to a model creates a new version:

- Old simulation runs always reference the **exact model version** they used — results remain reproducible indefinitely.
- You can compare outputs from different model versions side-by-side in Simulation Studio.
- To update a model: open it in Model Library → click **New Version** → upload the updated source and update the description.

---

## Best Practices

- **Document inputs and outputs fully**: future users (including you, six months from now) will thank you. Include units, valid ranges, and edge case notes.
- **Use semantic versioning**: `v1.0.0` → `v1.1.0` for non-breaking updates, `v2.0.0` for breaking changes to inputs/outputs.
- **Tag models by domain**: makes them much easier to find in Simulation Studio.
- **Don't delete old versions**: even if a model is superseded, old versions are needed for reproducibility of past runs.

---

*← [Back to Home](Home)*
