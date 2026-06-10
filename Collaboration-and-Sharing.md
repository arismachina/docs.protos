# Collaboration & Sharing

[← Home](Home) · **Collaboration & Sharing**

Protos supports sharing canvases with teammates and publishing canvases for external stakeholders.

---

## On This Page

- [Organisations](#organisations)
- [Canvas Access Levels](#canvas-access-levels)
- [Sharing a Canvas](#sharing-a-canvas)
- [Publishing for External Access](#publishing-for-external-access)

---

## Organisations

An organisation is the top-level workspace in Protos. Projects, models, and team members all belong to an organisation. You can be a member of more than one organisation and switch between them at any time.

### Joining an organisation

Joining an organisation is an explicit action — you won't be added automatically. You'll receive an invitation or a join link from an existing member.

### Switching organisations

The **active-org switcher** is available in the navigation. Click it to see all organisations you belong to and switch between them. Everything you see in Protos — projects, the Model Library, team members — is scoped to your active organisation.

---

## Canvas Access Levels

Sharing in Protos is at the **canvas level**, not the project level. Projects have a single owner; canvases within that project can be shared individually.

| Level | What they can do |
|-------|-----------------|
| **Owner** | View, edit, run, share, and publish the canvas |
| **View only** | View the canvas and its results — cannot edit or run |

Non-owners you share a canvas with get read-only access. To make changes, they would need to copy the canvas into their own project.

---

## Sharing a Canvas

From within a canvas, you can share access in three ways:

| Method | How it works |
|--------|-------------|
| **Individual share** | Add a specific user by their user ID or email address |
| **Domain share** | Anyone with the same email domain as you gets access automatically |
| **Public** | Anyone with a Protos account can find and view the canvas |

All shared access is read-only. Only the owner can edit.

---

## Publishing for External Access

**Publications** let you share a canvas with people who don't have a Protos account — customers, partners, or reviewers.

A publication is a **snapshot of a canvas** at a point in time, accessible via a public URL.

### What external viewers can do

- See the canvas parameters and outputs
- Adjust parameter values and re-run the canvas interactively
- View linked data documents

### What they cannot see

- Your Python calculation code (stripped from the snapshot)
- Other canvases or projects
- The Knowledge Library or any internal data

### How to publish

1. Open **Simulation Studio** from the sidebar. Click **Publish** in the top right of the canvas list.
2. A dialog opens.
3. Give the publication a **name**.
4. Select which **canvases to include**.
5. Optionally check **Include data tab** to expose the underlying data to viewers.
6. Optionally set a **password** for access control.
7. Click **Publish** — a shareable URL is generated. Share it with whoever needs access.

> **Note:** Publications are snapshots — they do not update automatically when you change the canvas. Re-publish to push an update.

---

## See Also

- [Home → Project Overview](Home#project-overview)
- [Glossary → Design freeze](Glossary), [Glossary → Version](Glossary)

---

*[← Back to Home](Home)*
