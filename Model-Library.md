# Models Library <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

[← Home](Home) · **Models Library**

The **Models Library** is a registry of all computational models available in your workspace — physics models, ML models, kinetic models, domain-specific solvers, and custom scripts. Models are registered once, reusable across any project, fully versioned, and traceable.

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

1. Go to **Models Library** in the sidebar.
2. Click **Register Model**.
3. Fill in the required fields:

| Field | Description |
|-------|-------------|
| **Name** | Clear, searchable name (e.g. `Doyle-Fuller-Newman Electrochemical Model`) |
| **Description** | What the model does, when to use it, known limitations |

Define the **Input schema** and **Output schema** — the JSON schema builders let you specify each parameter's name, type, and whether it's required.

4. Click **Register model**. After registering, a launcher token download step appears for local-runner models.

> **Tip:** Document inputs and outputs fully — include units, valid ranges, and edge case notes. Future users (including you, six months from now) will thank you.

---

## Model Source Types

The first step is to choose an **Execution type**: **Local runner** (your model runs on your own machine; the platform dispatches inputs) or **Cloud** (upload Python code; the platform containerises and runs it).

The dialog opens with three intake options — **Local code** (upload or paste code), **Endpoint** (point to an existing API endpoint), and **GitHub/GitLab/etc.** (build from a repo).

| Source type | How it works in Protos |
|-------------|----------------------|
| **Python script** | Upload the script; Protos executes it in a managed environment |
| **COMSOL / MATLAB** | Select as the runtime under Local runner. Protos dispatches inputs to a launcher script that runs on your machine. |
| **External API** | Provide the endpoint URL and auth config; Protos calls it on run. You can supply a personal API key and choose whether to share it with your team or keep it private |
| **Public repo** | Paste a public repo URL (GitHub, GitLab, Bitbucket, or Codeberg). Protos builds a container from it; you can optionally auto-draft the wrapper with AI. A live build progress indicator shows three steps: **Preparing build context → Building image → Finalising** — the build may take a few minutes |

---

## Finding a Model

Protos includes a set of ready-to-use physics models you can add to any canvas without registering anything:

| Model | What it does |
|-------|-------------|
| [Cell Performance](Model-Cell-Performance) | Equilibrium KPIs (capacity, energy, N/P ratio, mass) from electrode design; optional SPMe electrochemical simulation for DCIR, power, and drive cycles |
| [Cell Optimizer](Model-Cell-Optimizer) | Multi-objective optimisation (NSGA-II + PyBaMM) to find Pareto-optimal cell designs against up to 5 targets |
| [DFN Calendar Ageing](Model-DFN-Calendar-Ageing) | Capacity fade and SoH prediction during storage over days to years using the Doyle-Fuller-Newman model |
| [DFN Cyclic Ageing](Model-DFN-Cyclic-Ageing) | Cycle-life simulation with SEI growth, particle cracking, swelling, and loss of active material |
| [SPMeT Power](Model-SPMeT-Power) | Maximum power envelope across SOC, temperature, and pulse duration |
| [SPMeT DCIR](Model-SPMeT-DCIR) | Direct Current Internal Resistance at configurable time points across a SOC/temperature/C-rate sweep |
| [SPMeT Dynamic Load](Model-SPMeT-Dynamic-Load) | Drive cycle simulation with full voltage, current, temperature, and energy timeseries |

Beyond the built-in models, you can search and add any model your team has registered:

- **Search** by name, domain, or keyword.
- **Filter** by tags or scope (mine, shared with me, public).
- **Sort** by Last updated, Name A-Z, or Newest first.
- Click any model to see its full documentation, input/output schema, and version history.

---

## Model Versioning

Every update to a model creates a new version. This is critical for reproducibility.

| What versioning gives you | Detail |
|--------------------------|--------|
| **Reproducibility** | Old simulation runs always reference the exact model version they used — results never change retroactively |
| **Comparison** | Old canvas runs always reference the exact model version they used, so you can see how results changed between versions |
| **Audit trail** | Full history of who changed what and when |

**To update a model:** Open the model and click **Edit** to update its name, description, or tags. To update the model's code, you must re-register it.

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
