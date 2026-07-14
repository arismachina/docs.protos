# Versioning <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

[← Home](Home) · **Versioning**

Protos keeps a version history for your key assets — so you can see what changed, when, and roll back if a change turns out to be wrong. Versioning is currently a pilot feature.

---

## On This Page

- [What Gets Versioned](#what-gets-versioned)
- [Viewing Version History](#viewing-version-history)
- [Comparing Versions](#comparing-versions)
- [Labeling a Version](#labeling-a-version)
- [Restoring a Previous Version](#restoring-a-previous-version)
- [Keeping Dependencies Up to Date](#keeping-dependencies-up-to-date)
- [Data Documents Version Differently](#data-documents-version-differently)
- [Co-Engineer and Versioning](#co-engineer-and-versioning)
- [Community Changelog](#community-changelog)
- [How This Relates to Publications](#how-this-relates-to-publications)

---

## What Gets Versioned

| Asset | When a new version is created |
|-------|-------------------------------|
| **[Schemas](Schemas)** | Every save |
| **[Canvases](Simulation-Studio)** | Changes to components or metadata |
| **[Models](Model-Library)** | Registering the model and every metadata edit |
| **Data documents** | Not on every save — see [below](#data-documents-version-differently) |
| **[Knowledge documents](Knowledge-Library)** | Certain edits to the entry |

An unchanged save never creates a new version — Protos compares content before minting one, so your history only shows real changes.

---

## Viewing Version History

Open the version button in the resource's header (schema editor, canvas, model detail page, or knowledge document) to see its history as a list, newest first. Each entry shows a summary of what changed from the version before it. Click any past version to open a **read-only preview** — nothing changes until you explicitly restore it.

---

## Comparing Versions

Beyond the automatic per-entry summary, you can compare any two versions directly: open a version's preview and click **Compare** to bring up two dropdowns (From / To) pre-filled with the two most recent versions — pick any pair you want. The comparison shows fields that were **added**, fields that were **removed**, and fields that **changed**, with the old and new value side by side.

---

## Labeling a Version

Click on any version's label in the history list — including the current one — to give it a short, memorable name (e.g. "thermal params" or "pre-review baseline"). There's no restriction on which versions you can label or how many; use it to mark checkpoints worth finding again later.

---

## Restoring a Previous Version

Restoring is non-destructive: it doesn't rewind history, it creates a **new version** that re-applies the old content on top of your current one. The full timeline — including the version you restored from — always stays intact.

A couple of asset-specific notes:

- **Models:** restoring only brings back name, description, tags, default parameters, input/output schema, endpoint, and job type — not other runtime or operational settings.
- **Data documents:** restoring requires editor access to the document.

---

## Keeping Dependencies Up to Date

Schemas and canvases can reference other versioned assets — a schema through a Ref field to another schema, a canvas through the schemas, models, and data documents it pins. If one of those referenced assets gets a newer version, an **"Update available"** banner appears in the schema editor or canvas header telling you a dependency has moved on, and whether the change is backward-compatible or a breaking (major) change worth reviewing first. Click **Update** to re-pin everything to the latest versions in one step — this itself mints a new version, so you can always see exactly when the update happened.

This banner currently only appears on schemas and canvases — not on models or data documents.

---

## Data Documents Version Differently

Unlike schemas, canvases, and models, a data document doesn't get a new version on every edit. Instead, it's versioned when you explicitly click **Save version**, or automatically when it's **referenced** somewhere that needs a fixed snapshot — for example when it's pinned into a canvas run or included in a publication. This keeps the history focused on the versions that actually matter, rather than every keystroke.

---

## Co-Engineer and Versioning

Ask the Co-Engineer about version history directly — things like *"What changed between v2 and v3 of this schema?"*, *"Show me this canvas's version history,"* or *"Is this canvas up to date with its dependencies?"* It can list a resource's versions, show a diff between any two, check whether a canvas's pinned dependencies are outdated, and — with your confirmation — restore a **schema**, **canvas**, or **data document** to a past version, or re-pin a canvas to the latest dependency versions. Model and knowledge-document versions can only be browsed, not restored, through the Co-Engineer; restore those from their own version-history UI instead.

---

## Community Changelog

The **Changelog** tab on a community's detail page (see [Collaboration & Sharing](Collaboration-and-Sharing)) shows a read-only feed of version events across schemas, data documents, models, and canvases shared with that community. Filter by asset type or by how significant a change was. Knowledge documents and publications don't appear in this feed.

---

## How This Relates to Publications

[Publications](Collaboration-and-Sharing#publishing-for-external-access) are a separate, purpose-built mechanism for sharing a canvas externally as a fixed snapshot at a point in time — publishing a canvas does not itself create a canvas version. It does, however, mint a version for any **data document** the canvas references, so the exact data behind a published result stays traceable even as those documents keep changing afterward.

---

## See Also

- [Schemas](Schemas) — version history lives in the schema editor header
- [Model Library](Model-Library) — model versioning and the Edit action
- [Collaboration & Sharing](Collaboration-and-Sharing) — the community Changelog and Publications
- [Glossary → Version](Glossary)

---

*[← Back to Home](Home)*
