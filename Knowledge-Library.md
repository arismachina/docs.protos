# Knowledge Library

← [Back to Home](Home)

The **Knowledge Library** is Protos's institutional memory. Every decision, data point, and reference is captured here and linked to the design artifacts that used it — so nothing is ever lost between projects, teams, or people.

---

## What Lives Here

| Type | Examples |
|------|---------|
| **Academic literature** | Papers, standards, patents — imported by DOI or PDF upload |
| **Internal reports** | Experimental summaries, design reviews, test reports |
| **Decisions** | Why a parameter value was chosen; why an approach was rejected |
| **Experimental results** | Test data promoted to reusable knowledge assets |
| **AI-surfaced connections** | Protos Copilot surfaces relevant prior work as you design |

---

## Finding Knowledge

1. Open **Knowledge Library** from the sidebar.
2. **Search** by keyword, author, domain, or date.
3. **Filter** by type: paper, internal doc, dataset, decision.
4. Click any result to see its full content and **all the places in Protos where it has been used**.

---

## Adding to the Library

### Import a paper
1. Click **Add → Paper**.
2. Paste the DOI or upload the PDF.
3. Protos extracts key data points (authors, year, domain, key values) and makes them searchable.
4. Tag relevant parameters or add a note about why this paper is relevant to your work.

### Capture a decision
1. On the canvas, right-click any node and select **Add to Knowledge Library**.
2. Write a short rationale: *"Chose 1.2 mol/L based on [Reference] showing peak conductivity at this concentration under our temperature range."*
3. The decision is now discoverable by future users and linked to the design node it annotated.

### Promote experimental results
1. From a test data entry, click **Promote to Knowledge Library**.
2. Add a summary and any relevant tags.
3. Future projects can discover this result during search and link it to their designs.

---

## Traceability

Every value in a design or simulation can be traced back to its knowledge source.

Click the **trace icon** (🔗) on any parameter to see the full chain:

```
Design value → Model parameter → Literature reference / experimental result
```

This chain is preserved permanently — even if team members leave or projects are archived.

---

## Best Practices

- **Promote decisions as they're made**, not retrospectively. The rationale is clearest in the moment.
- **Always link papers to the specific values you're taking from them** — not just the paper itself. This makes trace far more useful.
- **Use tags consistently**: agree on a tag taxonomy with your team (e.g. by domain, material, property type).
- **Review the library at project kickoff**: before starting new work, search for relevant prior experiments and decisions. Protos Copilot can also surface suggestions automatically.

---

*← [Back to Home](Home)*
