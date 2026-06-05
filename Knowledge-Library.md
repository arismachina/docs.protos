# Knowledge Library

[← Home](Home) · **Knowledge Library**

The **Knowledge Library** is Protos's institutional memory. Every decision, data point, and reference is captured here and linked to the design artifacts that used it — so nothing is ever lost between projects, teams, or people.

> **Set this up before using the Co-engineer.** The Co-engineer draws on the Knowledge Library when creating schemas, filling data documents, and answering questions. The richer the library, the more grounded and traceable its output.

---

## On This Page

- [What Lives Here](#what-lives-here)
- [Finding Knowledge](#finding-knowledge)
- [Adding to the Library](#adding-to-the-library)
- [Traceability](#traceability)
- [Best Practices](#best-practices)

---

## What Lives Here

| Type | Examples |
|------|---------|
| **Academic literature** | Papers, standards, patents — uploaded as PDF or TXT files |
| **Internal reports** | Experimental summaries, design reviews, test reports |
| **Decisions** | Why a parameter value was chosen; why an approach was rejected |
| **Experimental results** | Test reports and datasets uploaded as files or captured as text notes |
| **AI-surfaced connections** | [Co-engineer](Co-engineer) surfaces relevant prior work as you design |

---

## Finding Knowledge

1. Open **Knowledge Library** from the sidebar.
2. **Search** by keyword, author, domain, or date.
3. **Filter** by type: paper, internal doc, dataset, or decision.
4. Click any result to see its full content and **all the places in Protos where it has been used**.

> **Tip:** Before starting a new project or design iteration, search for prior experiments and decisions in the same domain. [Co-engineer](Co-engineer) can also surface relevant entries automatically as you work on the canvas.

---

## Adding to the Library

### Upload a file

1. Click **Add** in the Knowledge Library.
2. Upload a PDF, TXT, or JSON file — a paper, test report, spec document, or any reference material.
3. Protos chunks and embeds the content, making it searchable and available as context for the Co-engineer.
4. Add tags and a description to make it easier to find later.

### Type a note directly

If you want to capture a decision or rationale without a file:

1. Click **Add → Text**.
2. Type the content — e.g. *"Chose 1.2 mol/L based on the conductivity data from the Q1 electrolyte study — peak conductivity at this concentration under our temperature range."*
3. The note is stored, embedded, and searchable just like an uploaded file.

### Upload a folder

For bulk ingestion of many files at once:

1. Click **Add → Folder**.
2. Upload a folder of files — Protos processes them in the background.
3. A progress indicator tracks succeeded and failed files.

---

## Traceability

When the Co-engineer creates or updates a data document using information from the Knowledge Library, it records which specific chunks of which documents it drew on. This means you can see exactly where a field value came from — not just "the Co-engineer said so" but the specific source passage.

```
Field value in data document
  └── Knowledge Library chunk
        └── Original uploaded document (paper, report, note)
```

This chain is preserved permanently — even if team members leave or projects are archived.

---

## Best Practices

- **Capture decisions as they're made**, not retrospectively. The rationale is clearest in the moment and grows harder to reconstruct over time.
- **Link papers to specific claims**, not just to the paper itself. Trace is only useful if it points to the exact piece of evidence that informed a decision.
- **Agree on a tag taxonomy with your team** before tagging — e.g. by domain, material, or property type. Inconsistent tags make search unreliable.
- **Review the library at project kickoff**: search for prior experiments and decisions before starting new work. Don't repeat work that's already been done.

---

## See Also

- [Co-engineer](Co-engineer) — surfaces Knowledge Library entries automatically as you work
- [Schemas](Schemas) — data documents created from knowledge sources link back to their chunks
- [Simulation Studio](Simulation-Studio) — link simulation results back to knowledge sources
- [Glossary → Trace](Glossary)

---

*[← Back to Home](Home)*
