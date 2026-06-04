# Tutorial: Creating Your First Schema

[← Home](Home) · [← Schemas](Schemas)

This tutorial walks you through creating a schema — the data structure that everything else in Protos is built on. It takes about 5 minutes.

---

## What you'll build

A schema for a simple design type — for example, an electrode coating experiment. By the end you'll have a reusable template that lets you create structured, comparable data documents across your project.

---

## Why you need a schema first

In Protos, you can't create a data document without a schema. The schema defines what fields that document has — what gets recorded, in what units, and what type of value each field holds.

Think of it like designing a form before anyone fills it in. You decide once what the fields are, and then every data document that follows that schema is guaranteed to have the same structure — which means you can compare them, filter them, and feed them into simulations reliably.

---

## Step 1 — Open the Schema Editor

Click **Schemas** in the left sidebar. You'll see a list of existing schemas (if any) and a **New Schema** button.

Click **New Schema**.

---

## Step 2 — Name your schema

Give it a clear name that describes the type of data it represents. The convention is `[Domain] — [Artifact type]`:

- `Battery — Electrode Coating`
- `Pharma — Tablet Formulation`
- `Thermal — Operating Conditions`

This naming makes schemas easy to find when your project grows.

---

## Step 3 — Add your fields

Now add the fields that every document of this type should have. For each field you need to choose:

**Type** — what kind of value this field holds:

| Type | Use for |
|------|---------|
| `number` | Any measured value — always set a unit |
| `string` | Free text — names, notes, identifiers |
| `enum` | A fixed list of choices (e.g. material type) |
| `boolean` | Yes / no |
| `date` | When something happened |
| `ref` | A link to another data document |

**Example fields for an Electrode Coating schema:**

| Field name | Type | Unit | Required? |
|-----------|------|------|-----------|
| coating_thickness | number | µm | yes |
| porosity | number | % | yes |
| active_material | enum | — | yes |
| mass_loading | number | mg/cm² | yes |
| double_sided | boolean | — | no |
| batch_notes | string | — | no |

Add each field using the field editor, set its type and unit, and mark it required or optional.

> **Tip:** Keep it lean — only add fields you will actually populate. A schema with 5 well-filled fields is far more useful than one with 20 fields that are mostly empty.

---

## Step 4 — Choosing required vs optional

Mark a field as **required** if a data document is meaningless without it. For example, `coating_thickness` is required because you can't compare coatings without knowing their thickness.

Mark it **optional** if it's useful when available but not always recorded — like batch notes.

If a required field is left empty, the document will fail validation and can't be used in a canvas input.

---

## Step 5 — When to use an enum

Use `enum` instead of `string` whenever the value comes from a fixed set. For example, `active_material` might be one of: `Graphite`, `NMC811`, `LFP`, `Silicon`.

If you use a plain string, engineers might write `"NMC 811"`, `"nmc811"`, or `"NMC-811"` — three different values that represent the same thing. An enum forces everyone to pick from the same list, which makes filtering and comparison work.

---

## Step 6 — When to use a ref field

Use a `ref` field when one document logically belongs to another. For example:

> A **Test Result** document that records what happened when you tested an **Electrode Coating** document.

Instead of copying the coating values into every test result, you add a `ref` field in the Test Result schema that points to the Electrode Coating document that was tested. Now the test result is permanently linked to the exact coating it validated.

This is what traceability is built on — you can follow the chain: test result → coating → what went into that coating.

---

## Step 7 — Save the schema

Click **Save**. The schema is now available across your project.

You can start creating data documents from it immediately — go to [Data Studio](Data-Studio) and the schema will appear in the picker.

---

## What you've learned

- A schema is a template that defines the fields every data document of that type must have
- Field types matter — use `enum` to enforce consistent values, `number` with units for measurements, `ref` to link documents together
- Mark fields required only if a document is meaningless without them
- Keep schemas lean — consistency beats completeness

---

## Next steps

- **Create data documents:** Go to [Data Studio](Data-Studio) and create your first documents following this schema.
- **Ask Co-engineer:** Describe the type of data you're working with and ask Co-engineer to create the schema for you — it will propose field names, types, and units based on your domain.
- **Edit later:** You can add new fields to a schema after documents have been created. Avoid removing or renaming fields that already have data — this can break existing documents.

---

*[← Back to Home](Home)*
