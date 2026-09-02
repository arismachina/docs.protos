# Glossary

[← Home](Home) · **Glossary**

Definitions for terms used throughout the Protos platform and this wiki. Terms are listed alphabetically.

---

| Term | Definition |
|------|-----------|
| **Canvas** | The visual graph workspace in a Protos project, where all nodes are connected and displayed. See [Home → Project Overview](Home#project-overview). |
| **Co-Engineer** | The AI assistant embedded across all Protos features — authors schemas and data documents, surfaces knowledge, and recommends model configurations. Always traces its sources. See [Co-Engineer](Co-engineer). |
| **Community** | A group of people you share with in Protos, together with its members and team structure. Resources stay owned by whoever created them; sharing into a community is what gives its members access. You can belong to several at once. See [Collaboration & Sharing](Collaboration-and-Sharing#communities). |
| **Design freeze** | A concept in the PSPP framework referring to the point at which a validated design is locked for manufacturing handoff. In Protos, this is achieved by publishing a canvas snapshot. |
| **Design space** | The range of possible parameter combinations for a design, explored via sweeps in [Simulation Studio](Simulation-Studio). |
| **DOE (Design of Experiments)** | Choosing which experiments to run so a limited budget of runs buys the most information. In Protos it is a **loop**: each round's results decide the next round's experiments. See [Design of Experiments](Design-of-Experiments). |
| **KPI** | In a [DOE](Design-of-Experiments) loop, a field the loop is chasing. It tries to move the KPI toward a target, taken from the requirement document or stated when the loop is set up. |
| **Lever** | In a [DOE](Design-of-Experiments) loop, a field the loop may vary. It chooses values for each lever within a range, taken from the schema or set on the loop, and the range can be widened later. |
| **Node** | A block on the canvas. Node types are: parameter, data input, calculation, model, and visualization. They are connected by arrows that define the data flow. |
| **PSPP** | Process → Structure → Property → Performance. The reasoning framework underlying Protos — used to trace how manufacturing choices propagate through to physical outcomes. |
| **Ref node** | A [schema](Schemas) field type (shown as "Ref" in the editor) that links one schema entry to another, creating relational structure across engineering data. See [Schemas → Using Reference Fields](Schemas#using-reference-fields). |
| **Round** | One batch of experiments in a [DOE](Design-of-Experiments) loop — recommended together, run together, and used together to plan the next batch. |
| **Schema** | A defined structure for a type of engineering data in Protos (e.g. a test result schema, a design parameter schema). See [Schemas](Schemas). |
| **Sweep** | A single run that varies one or more parameters across a range and returns an output surface. See [Simulation Studio → Sweep](Simulation-Studio#design-space-exploration-sweep). |
| **TRL** | Technology Readiness Level. Protos is optimized for TRL 0–3 — the early R&D phase where first-principles reasoning and traceability are most critical. |
| **Trace** | The ability to follow any value in Protos back through its chain of sources to the original reference, experiment, or decision. See [Knowledge Library → Traceability](Knowledge-Library#traceability-and-connections). |
| **Version** | A snapshot of a schema, canvas, model, data document, or knowledge document at a point in time. See [Versioning](Versioning). |

---

## See Also

- [Simulation Studio](Simulation-Studio) — where Canvas, Node, Sweep, and Design space are used in practice
- [Schemas](Schemas) — where Schema and Ref node are explained in depth
- [Data Studio](Data-Studio) — where data documents and the canvas connection are shown
- [Design of Experiments](Design-of-Experiments) — where KPI, Lever, and Round are used in practice

---

*[← Back to Home](Home)*
