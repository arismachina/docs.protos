# Schemas <a href="https://protos.arismachina.com" class="try-protos">Try Protos</a>

[← Home](Home) · **Schemas**

Schemas define the **structure of your engineering data** in Protos. Rather than storing artifacts in disconnected spreadsheets or files, schemas give every data type a consistent, queryable shape — reusable across projects and connectable to external tools and integrations.

> **You don't have to build schemas manually.** The Co-engineer can create them for you — describe what you're working on and it will propose field names, types, and units. See [Tutorial: Working with the Co-engineer](Tutorial-Co-engineer).

---

## On This Page

- [Use Schemas For](#use-schemas-for)
- [Creating a Schema](#creating-a-schema)
- [Field Types](#field-types)
- [Using Reference Fields](#using-reference-fields)
- [Bringing External Data into Schemas](#bringing-external-data-into-schemas)
- [Best Practices](#best-practices)

---

## Use Schemas For

- Designs and design parameters
- Test data and experimental conditions
- Model parameterizations and input configurations
- Operating conditions and environmental variables
- Any structured data extracted from external files (SharePoint documents, uploaded PDFs, spreadsheets)

---

## Creating a Schema

For a full step-by-step walkthrough with screenshots, see the **[Tutorial: Creating Your First Schema](Tutorial-Schemas)**.

Quick reference:

1. Navigate to **Schemas** in the left sidebar → **New Schema**.
2. Name it: `[Domain] — [Artifact type]` (e.g. `Battery — Electrolyte Formulation`).
3. Add fields — set type, unit, and whether required.
4. Click **Save**.

> **Tip:** Keep schemas lean — only add fields that will actually be populated. A sparse schema with consistent data is far more useful than a dense schema half the team ignores.

---

## Field Types

| Type | Use for |
|------|---------|
| `number` | Measurable values — always set a unit |
| `string` | Free-form text — labels, notes, identifiers |
| `enum` | A fixed list of options (e.g. material grade, phase) |
| `boolean` | Yes / no flags |
| `date` | Timestamps for experiments, decisions, imports |
| `ref` | Link to another data document (e.g. test result → electrode formulation) |

---

## Using Reference Fields

Reference fields link schemas together, creating a **relational structure** across your engineering data. This is what makes trace powerful.

**Example:** A `Test Result` schema with a reference field pointing to `Design Iteration` means every test record is traceable to the exact design revision it validated.

To add a reference field:

1. In the schema editor, add a new field and set type to `ref`.
2. Select the **target schema**.
3. Set whether one or many references are allowed.

> **Note:** Use reference fields instead of duplicating data across schemas. Duplication breaks traceability and leads to inconsistencies as designs evolve.

---

## Bringing External Data into Schemas

Protos uses the **Co-engineer** to extract data from your existing files and create structured data documents that follow your schema.

### How it works

1. Define your schema first — the Co-engineer needs a schema to fill.
2. In the Co-engineer chat, upload a file or pull one in from SharePoint.
3. Ask the Co-engineer to create a data document from it — e.g. *"Create an Electrode Formulation document from this test report."*
4. The Co-engineer reads the file, maps what it finds to your schema fields, and creates the document. It will flag anything it couldn't find or wasn't sure about.

You review and correct the result before saving.

### Supported file sources

| Source | How to bring it in |
|--------|--------------------|
| **Local file** | Upload directly in the Co-engineer chat (PDF, Excel, TXT) |
| **SharePoint / OneDrive** | Browse your SharePoint from the Co-engineer and select a file |

> **Note:** GitHub is not used for data import. GitHub integration in Protos is for registering computational models only — see [Model Library](Model-Library).

### When to use this

This approach works well when you have existing reports, datasheets, or spreadsheets with values you want to capture as structured data. The Co-engineer handles the messy extraction; your schema guarantees the result is consistent and comparable with other documents of the same type.

---

## Best Practices

- **Standardize units before creating numeric fields** — changing units later requires a data migration.
- **Name schemas by domain + artifact type** for discoverability across the workspace.
- **Version your schemas** — create a new schema version for significant structural changes rather than editing in place. Editing in place can corrupt existing data linked to that schema.
- **Agree on a reference field convention** with your team before building out multiple related schemas.

---

## See Also

- [Model Library](Model-Library) — model input/output schemas follow the same conventions
- [Simulation Studio](Simulation-Studio) — pull schema values directly as simulation inputs
- [Knowledge Library](Knowledge-Library) — promote schema entries to reusable knowledge assets
- [Glossary → Reference field](Glossary)

---

*[← Back to Home](Home)*
