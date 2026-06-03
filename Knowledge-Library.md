# Knowledge Library

[← Home](Home) · **Knowledge Library**

The **Knowledge Library** is Protos's institutional memory. Every decision, data point, and reference is captured here and linked to the design artifacts that used it — so nothing is ever lost between projects, teams, or people.

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
| **Academic literature** | Papers, standards, patents — imported by DOI or PDF upload |
| **Internal reports** | Experimental summaries, design reviews, test reports |
| **Decisions** | Why a parameter value was chosen; why an approach was rejected |
| **Experimental results** | Test data promoted to reusable knowledge assets |
| **AI-surfaced connections** | [Copilot](Copilot) surfaces relevant prior work as you design |

---

## Finding Knowledge

1. Open **Knowledge Library** from the sidebar.
2. **Search** by keyword, author, domain, or date.
3. **Filter** by type: paper, internal doc, dataset, or decision.
4. Click any result to see its full content and **all the places in Protos where it has been used**.

> **Tip:** Before starting a new project or design iteration, search for prior experiments and decisions in the same domain. [Copilot](Copilot) can also surface relevant entries automatically as you work on the canvas.

---

## Adding to the Library

### Import a paper

1. Click **Add → Paper**.
2. Paste the DOI or upload the PDF.
3. Protos extracts key data: authors, year, domain, key values — all made searchable.
4. Tag relevant parameters and add a note about why this paper is relevant to your work.

> **Best practice:** Link papers to the specific values you're extracting from them, not just the paper itself. A link to a paper is less useful than a link to a paper *at the claim that informed your decision*.

### Capture a decision

1. On the canvas, right-click any node and select **Add to Knowledge Library**.
2. Write a short rationale:
   > *"Chose 1.2 mol/L based on [Reference] showing peak conductivity at this concentration under our temperature range."*
3. The decision is now discoverable by future users and permanently linked to the design node it annotated.

### Promote experimental results

1. From a test data entry, click **Promote to Knowledge Library**.
2. Add a summary and relevant tags.
3. Future projects can discover this result during search and link it to their designs.

---

## Traceability

Every value in a design or simulation can be traced back to its knowledge source. Click the **trace icon** on any parameter to see the full chain:

```
Design value
  └── Model parameter
        └── Literature reference / experimental result
              └── Original source (paper, test, decision)
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

- [Copilot](Copilot) — surfaces Knowledge Library entries automatically as you work
- [Schemas](Schemas) — promote schema entries to knowledge assets
- [Simulation Studio](Simulation-Studio) — link simulation results back to knowledge sources
- [Glossary → Trace](Glossary)

---

*[← Back to Home](Home)*
