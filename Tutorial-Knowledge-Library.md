# Tutorial: Setting Up Your Knowledge Library

[← Home](Home) · [← Knowledge Library](Knowledge-Library)

This tutorial walks you through uploading your reference material and understanding how the Co-engineer uses it. It takes about 5 minutes.

---

## What you'll do

Upload a document into the Knowledge Library and understand how it becomes available to the Co-engineer when it creates schemas, data documents, and answers questions.

---

## Why do this early

The Co-engineer is only as good as the knowledge you give it. If you upload your reference papers, test reports, and specs before you start designing, the Co-engineer can draw on them when creating schemas and filling in data documents — and it will cite exactly which document and passage it used.

If you skip this step, the Co-engineer still works but it will rely on general knowledge rather than your specific project context.

---

## Step 1 — Open the Knowledge Library

Click **Knowledge Library** in the left sidebar.

You'll see a list of existing documents (empty if this is a new project) and an **Add** button.

---

## Step 2 — Upload a document

Click **Add** and choose one of three options:

**Upload a file** — the most common option. Supports PDF, TXT, and JSON. Good for:
- Academic papers
- Internal test reports
- Spec documents and datasheets
- Design review summaries

Click **Add → File**, select your file, give it a name and optional description, add relevant tags (e.g. `electrode`, `electrolyte`, `NMC811`), and click **Upload**.

Protos splits the document into chunks and embeds them — this is what makes the content searchable and usable by the Co-engineer.

**Type a note directly** — for capturing decisions, rationale, or anything that doesn't exist as a file:

> *"Chose 1.2 mol/L concentration based on the Q1 conductivity study — peak conductivity at this value under our temperature range."*

Click **Add → Text**, type your note, tag it, and save. It's stored exactly like an uploaded file — searchable and available to the Co-engineer.

**Upload a folder** — for bulk ingestion of many files at once. Protos processes them in the background and shows a progress indicator.

---

## Step 3 — Use tags

Tags are how you filter and find documents later. Agree on a simple taxonomy with your team before you start — for example:

- By material: `graphite`, `nmc811`, `lfp`
- By type: `paper`, `internal-report`, `spec`, `decision`
- By property: `conductivity`, `capacity`, `thermal`

Inconsistent tags make search unreliable later.

---

## Step 4 — Search to confirm it's working

Type a keyword in the search bar — something you know is in the document you just uploaded. If the document appears in the results, the library is ready.

---

## Step 5 — See how the Co-engineer uses it

Now open the Co-engineer (the chat panel on the right side of the screen) and ask something that requires knowledge from your uploaded document:

> *"Based on the documents in the Knowledge Library, what concentration should I use for the electrolyte?"*

The Co-engineer will search the library, find the relevant passage, and answer — citing the specific document and chunk it drew from. You can click the citation to read the original source.

This is the traceability chain: Co-engineer answer → Knowledge Library chunk → your uploaded document.

---

## What makes a good Knowledge Library

- **Upload before you start designing** — not after. The Co-engineer uses the library when creating schemas and documents, so populate it first.
- **Capture decisions as text notes as you make them** — the rationale is clearest in the moment and the hardest to reconstruct later.
- **Tag everything** — untagged documents get lost in a large project.
- **Link papers to specific claims** — a paper that says "see Johnson et al. 2022" is less useful than a note that says "Johnson et al. 2022 shows peak conductivity at 1.2 mol/L under our conditions (Table 3)."

---

## What you've learned

- The Knowledge Library stores your reference material — papers, reports, decisions, notes
- Documents are chunked and embedded, making them searchable and usable by the Co-engineer
- The Co-engineer cites its sources — you can always trace a value back to the document it came from
- Populating the library early makes the Co-engineer dramatically more useful

---

## Next steps

- **Use Co-engineer with this context:** Ask it to create a schema or populate a data document — it will now draw on your uploaded knowledge.
- **Keep adding as you work:** Every decision worth remembering should be captured as a text note. Don't wait until the project is over.

---

*[← Back to Home](Home)*
