# Knowledge Library <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

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
| **Academic literature** | Papers, standards, patents — uploaded as PDF, DOCX, XLSX, CSV, TXT, Markdown, JSON, and common image formats up to 100 MB |
| **Internal reports** | Experimental summaries, design reviews, test reports |
| **Decisions** | Why a parameter value was chosen; why an approach was rejected |
| **Experimental results** | Test reports and datasets uploaded as files or captured as text notes |
| **AI-surfaced connections** | [Co-engineer](Co-engineer) surfaces relevant prior work as you design |

---

## Finding Knowledge

1. Open **Knowledge Library** from the sidebar.
2. **Filter** by type using the tabs: All, Custom Knowledge, Reference Knowledge, or Conversations.
3. Click any item to see its full content and the source documents that informed it.

> **Tip:** Before starting a new project or design iteration, browse the library for prior experiments and decisions in the same domain. [Co-engineer](Co-engineer) can also surface relevant entries automatically as you work on the canvas.

---

## Adding to the Library

### Upload a document

1. Click **Add → Upload Document**.
2. Choose a file — PDF, DOCX, Excel, CSV, TXT, images, and more. Up to 100 MB.
3. The title is auto-filled from the filename — edit it if needed.
4. Click **Upload**. Protos parses and chunks the content, making it available to the Co-engineer.

### Add a knowledge note

To capture a decision, insight, or observation as text:

1. Click **Add → Add Knowledge**.
2. Enter a **Title** and the **Content**.
3. Click **Save Knowledge**.

### Upload a folder

For bulk ingestion of many files at once:

1. Click **Add → Upload Folder**.
2. Select a folder — Protos processes the files in the background. Batches are capped at **100 MB** and **500 files** maximum.

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

- **Capture decisions as they're made**, not retrospectively. The rationale is clearest in the moment and becomes harder to reconstruct over time.
- **Link papers to specific claims**, not just to the paper itself. Trace is only useful if it points to the exact piece of evidence that informed a decision.
- **Use descriptive titles** so documents are easy to find via search — the Knowledge Library does not have a tagging feature, so the title is the primary handle for discovery.
- **Review the library at project kickoff**: search for prior experiments and decisions before starting new work. Don't repeat work that's already been done.

---

## See Also

- [Co-engineer](Co-engineer) — surfaces Knowledge Library entries automatically as you work
- [Schemas](Schemas) — data documents created from knowledge sources link back to their chunks
- [Simulation Studio](Simulation-Studio) — link simulation results back to knowledge sources
- [Glossary → Trace](Glossary)

---

*[← Back to Home](Home)*
