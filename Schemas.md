# Schemas

← [Back to Home](Home)

Schemas define the **structure of your engineering data** in Protos. Rather than storing artifacts in disconnected spreadsheets or files, schemas give every data type a consistent, queryable shape that can be reused across projects and connected to external tools.

---

## What You Can Schema

- Designs and design parameters
- Test data and experimental conditions
- Model parameterizations and input configurations
- Operating conditions and environmental variables
- Any structured data imported from external sources

---

## Creating a Schema

1. Navigate to **Schemas** in the left sidebar.
2. Click **New Schema**.
3. Name it clearly: `[Domain] — [Artifact type]`
   - Example: `Battery — Electrolyte Formulation`
   - Example: `Thermal — Operating Conditions`
4. Add fields:

| Field property | Options |
|---------------|---------|
| **Type** | text, number, select, date, reference, file |
| **Required** | yes / no |
| **Unit** | e.g. `°C`, `mol/L`, `MPa` |
| **Description** | short help text shown to users filling the field |

5. Save. The schema is now available across your project.

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

## Connecting External Data Sources

Protos ingests data from tools your team already uses and maps it to your schemas automatically.

### SharePoint / OneDrive
1. Go to **Integrations → SharePoint**.
2. Authenticate and select the folder or file to link.
3. In the import wizard, map columns or fields to your schema fields.
4. Set a sync frequency (manual, daily, on change).

### GitHub
1. Go to **Integrations → GitHub**.
2. Authenticate and select the repository and file paths to watch.
3. Protos monitors for changes and imports new data on push.
4. Useful for: model configs, simulation parameters, dataset CSVs.

### Google Drive
1. Go to **Integrations → Google Drive**.
2. Authenticate and select the spreadsheet or folder.
3. Map columns to schema fields in the import wizard.
4. Run **Import** — Protos creates structured entries for each row.

---

## Using Reference Fields

Reference fields link schemas together, creating a relational structure across your engineering data.

**Example:** A `Test Result` schema with a reference field pointing to `Design Iteration` means every test record is traceable to the exact design revision it validated.

To add a reference field:
1. In the schema editor, add a new field and set type to `reference`.
2. Select the **target schema**.
3. Set whether one or many references are allowed.

---

## Best Practices

- **Keep schemas lean**: only add fields that will actually be populated.
- **Standardize units** before creating numeric fields — changing units later requires a data migration.
- **Use reference fields** instead of duplicating data across schemas.
- **Name schemas by domain + artifact type** for discoverability.
- **Version your schemas**: create a new schema version for significant structural changes rather than editing in place.

---

*← [Back to Home](Home)*
