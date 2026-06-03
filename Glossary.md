# Glossary

[← Home](Home) · **Glossary**

Definitions for terms used throughout the Protos platform and this wiki. Terms are listed alphabetically.

---

| Term | Definition |
|------|-----------|
| **Canvas** | The visual graph workspace in a Protos project, where all nodes (designs, data, models, results) are connected and displayed. See [Home → Project Overview](Home#project-overview). |
| **Copilot** | The AI assistant embedded across all Protos features — authors schemas and data documents, surfaces knowledge, and recommends model configurations. Always traces its sources. See [Copilot](Copilot). |
| **Design freeze** | A concept in the PSPP framework referring to the point at which a validated design is locked for manufacturing handoff. In Protos, this is achieved by publishing a canvas snapshot. |
| **Design space** | The range of possible parameter combinations for a design, explored via sweeps in [Simulation Studio](Simulation-Studio). |
| **Node** | A block on the canvas. Node types are: parameter, input, calculation, model, and visualization. They are connected by arrows that define the data flow. |
| **PSPP** | Process → Structure → Property → Performance. The reasoning framework underlying Protos — used to trace how manufacturing choices propagate through to physical outcomes. |
| **Reference field** | A [schema](Schemas) field type that links one schema entry to another, creating relational structure across engineering data. See [Schemas → Using Reference Fields](Schemas#using-reference-fields). |
| **Schema** | A defined structure for a type of engineering data in Protos (e.g. a test result schema, a design parameter schema). See [Schemas](Schemas). |
| **Sweep** | A batch simulation run that varies one or more parameters across a range and returns an output surface. See [Simulation Studio → Design Space Exploration](Simulation-Studio#design-space-exploration-sweep). |
| **TRL** | Technology Readiness Level. Protos is optimized for TRL 0–3 — the early R&D phase where first-principles reasoning and traceability are most critical. |
| **Trace** | The ability to follow any value in Protos back through its chain of sources to the original reference, experiment, or decision. See [Knowledge Library → Traceability](Knowledge-Library#traceability). |
| **Version** | A named snapshot of a model at a point in time, managed in the [Model Library](Model-Library). Model versions are immutable once created, ensuring past simulation results remain reproducible. |

---

## See Also

- [Simulation Studio](Simulation-Studio) — where Canvas, Node, Sweep, and Design space are used in practice
- [Schemas](Schemas) — where Schema and Reference field are explained in depth
- [Data Studio](Data-Studio) — where data documents and the canvas connection are shown

---

*[← Back to Home](Home)*
