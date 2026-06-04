# Tutorial: Registering Your First Model

[← Home](Home) · [← Model Library](Model-Library)

This tutorial walks you through registering an external model so it can be used in your canvases. It takes about 10 minutes.

---

## What you'll do

Register a Python script as a model in the Model Library, then verify it's ready to use in a canvas.

---

## Why register a model

A model block in a canvas calls out to external code — a Python script, a MATLAB simulation, a COMSOL file, or an API. Before you can use it in a canvas, you need to register it: tell Protos what the model is called, what inputs it expects, and what outputs it returns.

Once registered, any canvas in any project can call it. You register once and reuse everywhere.

---

## Before you start

Have your model file ready — a Python script is the simplest case. The script just needs a function named `run` (or `main`, `execute`, `predict`, or `simulate`) that takes your inputs as arguments and returns a dict.

A minimal example:

```python
def run(coating_thickness: float, porosity: float, temperature: float):
    result = coating_thickness * porosity * (1 + 0.002 * temperature)
    return {"adjusted_capacity": result}
```

---

## Step 1 — Open the Model Library

Click **Models Library** in the left sidebar. You'll see a list of registered models and a **Register Model** button.

---

## Step 2 — Upload your script and let Protos infer the schema

Click **Register Model** and select **Upload file**.

Upload your Python script. Protos reads the file and automatically infers:
- The input fields (from your function's argument names and type hints)
- The output fields (from what the function returns)
- Suggested units where it can infer them

Review the inferred schema. If it looks right, you can proceed. If anything is wrong — missing units, wrong type, an input it missed — edit it before saving.

> **This is the most important step.** Clear input/output documentation is what makes a model usable by others (and by you, six months from now). Add units for every numeric field and a description for anything non-obvious.

---

## Step 3 — Fill in the model details

Complete the registration form:

- **Name:** Something searchable. `Electrode Capacity Model` is better than `model_v2`.
- **Description:** What the model does, when to use it, and any known limitations. Be honest about edge cases — this prevents misuse.
- **Domain:** e.g. `electrochemistry`, `thermal`, `mechanical`. Used for filtering.
- **Version tag:** Start with `v1.0.0`. Use semantic versioning — `v1.1.0` for non-breaking updates, `v2.0.0` if you change the inputs or outputs.

Click **Save**. The model is now registered and available in Simulation Studio.

---

## Step 4 — Test it in a canvas

Go to **Simulation Studio**, open or create a canvas, and add a **Model** block.

In the model block, search for the model you just registered. Select it — the input fields you defined will appear as connection points. Wire them up to parameter blocks or input blocks on the canvas.

Run the canvas. If the model executes and returns results, registration was successful.

If it fails, open the model block's detail panel — the error message will tell you what went wrong (wrong input type, missing required field, runtime error in the script, etc.).

---

## Registering from GitHub

If your model lives in a GitHub repo:

1. Click **Register Model → GitHub**.
2. Paste the repo URL.
3. Protos clones the repo, detects the entry point, and drafts a wrapper.
4. Review the inferred input/output schema and the wrapper code.
5. Confirm to containerise and register.

This works for public repos. The repo needs a function named `run`, `main`, `execute`, `predict`, or `simulate` at the top level, or a `protos.toml` file that declares the model's interface.

---

## Updating a model

When your model code changes:

1. Open the model in the Model Library.
2. Click **New Version**.
3. Upload the updated script and add a changelog note.
4. Save.

Old canvases continue to reference the old version — their results stay reproducible. New canvases will pick up the latest version by default.

> **Never delete old versions.** If you delete a version that an existing canvas references, that canvas can no longer reproduce its results.

---

## What you've learned

- Register a model once and use it in any canvas across any project
- Protos infers inputs and outputs from your code — review and add units before saving
- Good documentation (name, description, units, limitations) is what makes a model reusable
- Versioning keeps old results reproducible when your model evolves

---

## Next steps

- **Use it in a canvas:** Go to [Simulation Studio](Simulation-Studio) and add this model as a block in a canvas.
- **Ask Co-engineer:** You can ask the Co-engineer to register a model for you — upload the file in the chat and say "register this as a model called X."

---

*[← Back to Home](Home)*
