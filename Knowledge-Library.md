# Knowledge Library <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

[← Home](Home) · **Knowledge Library**

The **Knowledge Library** is Protos's institutional memory. Every decision, data point, and reference is captured here and linked to the design artifacts that used it — so nothing is ever lost between projects, teams, or people.

> **Set this up before using the Co-Engineer.** The Co-Engineer draws on the Knowledge Library when creating schemas, filling data documents, and answering questions. The richer the library, the more grounded and traceable its output.

---

## On This Page

- [What Lives Here](#what-lives-here)
- [Source Categories](#source-categories)
- [Finding Knowledge](#finding-knowledge)
- [Adding to the Library](#adding-to-the-library)
- [Tags](#tags)
- [Traceability and Connections](#traceability-and-connections)
- [Sharing](#sharing)
- [Best Practices](#best-practices)

---

## What Lives Here

| Type | Examples |
|------|---------|
| **Academic literature** | Papers, standards, patents — uploaded as PDF, DOCX, XLSX, XLS, CSV, TXT, Markdown, JSON, and common image formats up to about 16 MB |
| **Internal reports** | Experimental summaries, design reviews, test reports |
| **Decisions** | Why a parameter value was chosen; why an approach was rejected |
| **Experimental results** | Test reports and datasets uploaded as files or captured as text notes |
| **AI-surfaced connections** | [Co-Engineer](Co-engineer) surfaces relevant prior work as you design |

---

## Source Categories

Every document is automatically classified by where it came from, so you can tell a peer-reviewed paper apart from a forum thread at a glance. Each category has its own icon in the library table:

| Category | What it means |
|----------|---------------|
| **Research** | Peer-reviewed papers, standards, patents |
| **Article** | Articles and other published web writing |
| **Web** | General web pages |
| **Forum** | Forum and discussion threads |
| **Datasheet** | Manufacturer datasheets and spec tables |
| **Uploaded** | Files you uploaded directly |
| **Memory** | Knowledge notes you (or the Co-Engineer) captured as text |
| **Model Run** | Content generated from a model run |
| **Conversation** | Knowledge captured from a Co-Engineer conversation |
| **SharePoint** | Content the Co-Engineer imported from SharePoint |

Protos assigns a category on ingestion, and the owner can change it at any time: open the **⋯** row actions and use **Set category** (or select several rows to set the category in bulk).

> Files you import yourself through **Add → From SharePoint** are recorded as **Uploaded**. The **SharePoint** category applies to imports the Co-Engineer makes on your behalf.

---

## Finding Knowledge

1. Open **Knowledge** from the sidebar.
2. Switch between views with the tabs at the top: **List** (the default flat, paginated view), **Knowledge Graph**, and **Project Graph** — see [Traceability and Connections](#traceability-and-connections).
3. In the **List** view, narrow things down with the **category filter chips** (Research, Article, Web, …), each showing a count — only categories actually present in your library appear. Combine them with the **tag filter**, the search box, and a sort dropdown offering **Newest first**, **Oldest first**, **Name A–Z**, or **Format**.
4. If documents have been shared with you, **Mine** and **Shared** chips also appear so you can switch between your own documents and ones others shared. A marker on each shared row shows it came from someone else.
5. Use the search box to find items by **title, folder path, or tag**, then press Enter — it doesn't search document content or notes.
6. Click any item to see its full content, its [source categories and tags](#tags), the sources that informed it, and what it is [used by](#traceability-and-connections). From here the owner can also **download** the original file (or the extracted text, for conversation-sourced entries), and for entries created from a Co-Engineer conversation you can jump back to that conversation.

> **Tip:** Before starting a new project or design iteration, browse the library for prior experiments and decisions in the same domain. [Co-Engineer](Co-engineer) can also surface relevant entries automatically as you work on the canvas.

---

## Adding to the Library

The **Add** menu offers four ways in, described below in the order they appear.

### Upload a document

1. Click **Add → Upload Document**.
2. Choose a file — PDF, DOCX, Excel, CSV, TXT, images, and more. Up to **100 MB** per file.
3. The title is auto-filled from the filename — edit it if needed.
4. Click **Upload**. Protos parses and chunks the content, making it available to the Co-Engineer.
5. If that file is already in the library, Protos tells you so instead of creating a duplicate, and offers **View document** to open the copy you already have.

### Upload a folder

For bulk ingestion of many files at once:

1. Click **Add → Upload Folder**.
2. Select a folder — Protos processes the files in the background. Batches are capped at **100 MB** and **500 files** maximum. Individual files above the single-upload size limit are reported as failures rather than ingested.

Upload a second folder before the first has finished and both run — the second no longer waits behind the first.

### Import from SharePoint

1. Click **Add → From SharePoint**. The **Browse SharePoint** dialog opens.
2. If your Microsoft account isn't connected yet, connect it here — or from **Integrations** in the sidebar. In a tenant where individual users can't grant access themselves, a Microsoft administrator approves Protos once for the whole organisation, and everyone connects normally afterwards.
3. Browse sites, then document libraries and folders, or switch to **Search** to find a file by name. Tick anything you want to bring in: individual files, whole folders including everything nested inside them, or an entire document library. One selection can mix all three within a site — moving to a different site clears it.
4. Click **Import**. Protos scans your selection, then ingests on the server, reporting how many files were added, how many were already in the library, and how many failed.
5. Closing the dialog only stops the progress display — **the import keeps running on the server**. Reload the page to see files that landed after you closed it.

A SharePoint import is not held to the folder-upload batch caps — it can carry up to **12,000 files** and about **22 GB** in one go, and a selection larger than a single ingestion batch is split into several that run in turn rather than being refused. Individual files above the per-file size limit are still reported as failures. Unlike a folder upload, the count includes unsupported file types, which are reported as failures rather than skipped up front.

> **Why the two differ.** A folder upload stages every byte in your browser before ingestion starts, which is what holds it to 100 MB. A SharePoint import is fetched server-side and never stages, so it can be much larger.

### Add a knowledge note

To capture a decision, insight, or observation as text:

1. Click **Add → Add Knowledge**.
2. Enter a **Title** and the **Content**.
3. Click **Save Knowledge**.

### Managing documents and folders

Use the **⋯** menu on a document row to **Rename**, **Set category**, **Share**, or **Delete** it. Folder rows offer **Rename** and **Delete**. From a document's detail page you can also edit its title inline, and edit the content of a knowledge note you created. (Tags are edited from the document's row in the library table — see [Tags](#tags).)

---

## Tags

Protos tags documents automatically at ingestion, and you can add or remove tags yourself inline from a document's row in the library table. Click a tag to filter the library down to matching documents, and combine the tag filter with the category chips and search. Only the first tag shows as a chip on a row; the rest collapse into a **+N** marker. Tags are the flexible, cross-cutting complement to the fixed source categories — use them for your own groupings (a project code, a material, a review status).

---

## Traceability and Connections

When the Co-Engineer creates or updates a data document using information from the Knowledge Library, it records which specific chunks of which documents it drew on. This means you can see exactly where a field value came from — not just "the Co-Engineer said so" but the specific source passage, with a citation that links straight back to it.

```
Field value in data document
  └── Knowledge Library chunk
        └── Original uploaded document (paper, report, note)
```

### Used by

Each document's detail page has a **Used by** panel — the reverse of a citation. It lists the schemas, data documents, models, and canvases that were built from that document, so you can see the downstream impact of a source before you change or remove it. For a document that came from a Co-Engineer conversation, this panel is instead titled **"Modified in this conversation"** and lists what was created or edited during that chat.

### Graph views

Two graph views let you explore these relationships visually:

- **Knowledge Graph** — how your documents and the assets that cite them connect across the library.
- **Project Graph** — the same connections scoped to a single project.

This chain is preserved permanently — even if team members leave or projects are archived.

---

## Sharing

Knowledge documents are private to you by default, and can be **shared read-only** with other people — no one you share with can edit or delete them. There are two ways a document gets shared:

- **Directly** — open a document and use its **Share** dialog to grant read access to specific people or teams.
- **Automatically, with the work built on it** — when you share a project or an asset (schema, data document, model, or canvas), Protos additively shares the Knowledge Library documents that asset cites, so your collaborator can open and reason over the same sources.

Shared documents appear in the recipient's library under the **Shared** filter, marked with a "shared with you" indicator. Access is always evaluated live, so revoking a share takes effect immediately. See [Collaboration & Sharing](Collaboration-and-Sharing#sharing-resources) for how sharing works across Protos.

---

## Best Practices

- **Capture decisions as they're made**, not retrospectively. The rationale is clearest in the moment and becomes harder to reconstruct over time.
- **Link papers to specific claims**, not just to the paper itself. Trace is only useful if it points to the exact piece of evidence that informed a decision.
- **Use descriptive titles and tags** so documents are easy to find — search matches titles, folder paths, and tags (not document content), and tags also let you group documents across categories.
- **Keep categories aligned with how you work**: adjust a document's category with **Set category** whenever a different one fits your workflow better, so filters and the graph stay meaningful.
- **Review the library at project kickoff**: search for prior experiments and decisions before starting new work. Don't repeat work that's already been done.

---

## See Also

- [Co-Engineer](Co-engineer) — surfaces Knowledge Library entries automatically as you work
- [Schemas](Schemas) — data documents created from knowledge sources link back to their chunks
- [Simulation Studio](Simulation-Studio) — link simulation results back to knowledge sources
- [Collaboration & Sharing](Collaboration-and-Sharing#sharing-resources) — how read-only sharing propagates to sources
- [Glossary → Trace](Glossary)

---

*[← Back to Home](Home)*
