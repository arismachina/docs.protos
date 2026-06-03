# Schemas

[← Home](Home) · **Schemas**

Schemas define the **structure of your engineering data** in Protos. Rather than storing artifacts in disconnected spreadsheets or files, schemas give every data type a consistent, queryable shape — reusable across projects and connectable to external tools and integrations.

---

## On This Page

- [What You Can Schema](#what-you-can-schema)
- [Creating a Schema](#creating-a-schema)
- [Field Types](#field-types)
- [Using Reference Fields](#using-reference-fields)
- [Connecting External Data Sources](#connecting-external-data-sources)
- [Best Practices](#best-practices)

---

## What You Can Schema

- Designs and design parameters
- Test data and experimental conditions
- Model parameterizations and input configurations
- Operating conditions and environmental variables
- Any structured data imported from external sources (SharePoint, GitHub, Google Drive)

---

## Creating a Schema

1. Navigate to **Schemas** in the left sidebar.
2. Click **New Schema**.
3. Name it: `[Domain] — [Artifact type]`
   - Example: `Battery — Electrolyte Formulation`
   - Example: `Thermal — Operating Conditions`
4. Add fields using the field editor:

| Field property | Options |
|---------------|---------|
| **Type** | `text`, `number`, `select`, `date`, `reference`, `file` |
| **Required** | yes / no |
| **Unit** | e.g. `°C`, `mol/L`, `MPa` |
| **Description** | Short help text shown to users filling the field |

5. Click **Save**. The schema is now available across your project.

> **Tip:** Keep schemas lean — only add fields that will actually be populated. A sparse schema with consistent data is far more useful than a dense schema half the team ignores.

---

## Field Types

| Type | Use for |
|------|---------|
| `text` | Labels, descriptions, free-form notes |
| `number` | Measurable values — always set a unit |
| `select` | Categorical choices (e.g. material grade, phase) |
| `date` | Timestamps for experiments, decisions, imports |
| `reference` | Link to another schema entry (e.g. test → design) |
| `file` | Attach raw data files, PDFs, or scripts |

---

## Using Reference Fields

Reference fields link schemas together, creating a **relational structure** across your engineering data. This is what makes trace powerful.

**Example:** A `Test Result` schema with a reference field pointing to `Design Iteration` means every test record is traceable to the exact design revision it validated.

To add a reference field:

1. In the schema editor, add a new field and set type to `reference`.
2. Select the **target schema**.
3. Set whether one or many references are allowed.

> **Note:** Use reference fields instead of duplicating data across schemas. Duplication breaks traceability and leads to inconsistencies as designs evolve.

---

## Connecting External Data Sources

Protos ingests data from tools your team already uses and maps columns/fields to your schemas automatically.

### SharePoint / OneDrive

1. Go to **Integrations → SharePoint**.
2. Authenticate and select the folder or file to link.
3. In the import wizard, map columns or fields to your schema fields.
4. Set a sync frequency: manual, daily, or on change.

### GitHub

1. Go to **Integrations → GitHub**.
2. Authenticate and select the repository and file paths to watch.
3. Protos monitors for changes and imports new data on push.
4. Most useful for: model configs, simulation parameters, dataset CSVs.

### Google Drive

1. Go to **Integrations → Google Drive**.
2. Authenticate and select the spreadsheet or folder.
3. Map columns to schema fields in the import wizard.
4. Click **Import** — Protos creates a structured entry for each row.

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
