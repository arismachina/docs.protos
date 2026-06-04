# Tutorial: Creating Your First Schema

[← Home](Home) · [← Schemas](Schemas)

> For a full reference on field types, naming conventions, and best practices, see [Schemas](Schemas).

This tutorial walks you through building a schema and explains the decisions you make along the way. Takes about 5 minutes.

---

![Schemas page showing a list of schemas](images/02-schema-library.png)

## Step 1 — Create the schema

Click **Schemas** in the sidebar → **New Schema**.

Name it using the domain + artifact convention: `Battery — Electrode Coating` or `Pharma — Tablet Formulation`. This makes it findable as your project grows.

---

## Step 2 — Add fields and make the key decisions

For each field, you choose a type and whether it's required. The types are listed in [Schemas → Field Types](Schemas#field-types) — but here's what matters in practice:

**Use `enum` over `string` whenever the value comes from a fixed set.** If you use a plain string for material type, you'll end up with `"NMC811"`, `"nmc 811"`, and `"NMC-811"` as three separate values. An enum forces everyone to pick from the same list — filtering and comparison work reliably.

**Use `ref` instead of duplicating values.** If your Test Result schema needs to reference which electrode formulation was tested, add a `ref` field pointing to the Electrode Coating schema instead of copying the formulation values into every test result. This is what makes traceability work. See [Schemas → Using Reference Fields](Schemas#using-reference-fields).

**Mark a field required only if the document is meaningless without it.** A missing `coating_thickness` on an electrode coating document makes it unusable. Missing `batch_notes` doesn't.

---

## Step 3 — Save and test it

Click **Save**. Now go to [Data Studio](Data-Studio) and try creating a document from this schema. If a required field is blocking you that shouldn't be required, go back and change it — it's easier to adjust now than after documents exist.

---

## What to avoid

- Don't add fields you won't consistently fill in. Empty fields in half your documents break comparisons.
- Don't rename or remove fields once data documents exist against this schema — it can corrupt those documents.

---

*[← Back to Home](Home)*
