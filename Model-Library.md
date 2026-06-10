# Model Library <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

[← Home](Home) · **Model Library**

The **Model Library** is a registry of all computational models available in your workspace — physics models, ML models, kinetic models, domain-specific solvers, and custom scripts. Models are registered once, reusable across any project, fully versioned, and traceable.

---

## On This Page

- [Why It Exists](#why-it-exists)
- [Registering a Model](#registering-a-model)
- [Model Source Types](#model-source-types)
- [Finding a Model](#finding-a-model)
- [Model Versioning](#model-versioning)
- [Best Practices](#best-practices)

---

## Why It Exists

Without a shared model library, teams rebuild the same models for each project — wasting time and introducing inconsistencies. Protos solves this by registering models at the workspace level:

> **The Co-engineer can register models for you.** Upload a Python script or point it at a GitHub repo and ask it to register the model — it will infer the input/output schema automatically.

- Register once, call from any project
- Every simulation run references the exact model version it used
- Results remain reproducible indefinitely
- All inputs, outputs, and provenance are traceable

---

## Registering a Model

1. Go to **Model Library** in the sidebar.
2. Click **Register Model**.
3. Fill in the required fields:

| Field | Description |
|-------|-------------|
| **Name** | Clear, searchable name (e.g. `Doyle-Fuller-Newman Electrochemical Model`) |
| **Description** | What the model does, when to use it, known limitations |
| **Domain** | e.g. `electrochemistry`, `thermal`, `mechanical`, `materials` |
| **Inputs** | Each input: name, type, unit, required or optional |
| **Outputs** | Each output: name, type, unit |
| **Source** | Python script, COMSOL file, external API endpoint, etc. |
| **Version tag** | e.g. `v1.0.0` — use semantic versioning |

4. Click **Save**. The model is now available in [Simulation Studio](Simulation-Studio).

> **Tip:** Document inputs and outputs fully — include units, valid ranges, and edge case notes. Future users (including you, six months from now) will thank you.

---

## Model Source Types

| Source type | How it works in Protos |
|-------------|----------------------|
| **Python script** | Upload the script; Protos executes it in a managed environment |
| **COMSOL / MATLAB file** | Upload the model file; Protos calls it with the specified input parameters |
| **External API** | Provide the endpoint URL and auth config; Protos calls it on run. You can supply a personal API key and choose whether to share it with your team or keep it private |
| **GitHub repo** | Point to a public GitHub repo; Protos clones it, generates a wrapper, and containerises it. A live build progress indicator shows each step (queued → running → succeeded/failed) — the build may take a few minutes |

---

## Finding a Model

- **Search** by name, domain, or keyword.
- **Filter** by domain, input type, or output type.
- **Sort** by most recently used or most recently updated.
- Click any model to see its full documentation, input/output schema, and version history.

---

## Model Versioning

Every update to a model creates a new version. This is critical for reproducibility.

| What versioning gives you | Detail |
|--------------------------|--------|
| **Reproducibility** | Old simulation runs always reference the exact model version they used — results never change retroactively |
| **Comparison** | Old canvas runs always reference the exact model version they used, so you can see how results changed between versions |
| **Audit trail** | Full history of who changed what and when |

**To update a model:**

1. Open the model in the Model Library.
2. Click **New Version**.
3. Upload the updated source and revise the description.
4. Add a changelog note describing what changed.

> **Warning:** Do not delete old model versions. Even if a model is superseded, old versions are needed to reproduce past simulation runs.

---

## Best Practices

- **Use semantic versioning:** `v1.0.0` → `v1.1.0` for non-breaking updates, `v2.0.0` for breaking changes to inputs or outputs.
- **Tag models by domain** — makes them far easier to find in Simulation Studio.
- **Note known limitations in the description** — an honest description of edge cases prevents misuse and prevents future users from discovering limitations the hard way.
- **Don't rebuild models already in the library** — search before registering. If a close match exists, consider extending it with a new version instead.

---

## See Also

- [Simulation Studio](Simulation-Studio) — run models registered here
- [Schemas](Schemas) — model input/output schemas follow the same field conventions
- [Knowledge Library](Knowledge-Library) — link model results to knowledge assets
- [Glossary → Version](Glossary)

---

*[← Back to Home](Home)*
