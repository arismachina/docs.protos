# Collaboration & Sharing

← [Back to Home](Home)

Protos is built for cross-functional R&D teams. This page covers access control, sharing projects, and the design freeze workflow for manufacturing handoff.

---

## Access Levels

| Role | View | Edit canvas | Share | Freeze / publish |
|------|------|-------------|-------|-----------------|
| **Viewer** | ✓ | | | |
| **Contributor** | ✓ | ✓ | | |
| **Editor** | ✓ | ✓ | ✓ | |
| **Admin** | ✓ | ✓ | ✓ | ✓ |

---

## Sharing a Project

1. Open the project.
2. Click **Share** in the top-right.
3. Add team members by email or Aris Machina username.
4. Set their access level.
5. Click **Send invite**.

### External sharing (customers, partners)
- Use a **view-only shareable link** — confirm with the project lead before sending.
- Remove external access when the review period ends.
- Do not grant Contributor or Editor access to external parties without explicit approval.

---

## Design Freeze

When a design is ready for handoff to manufacturing, pilot, or qualification:

1. Open the project.
2. Click **Freeze Design** (top-right menu → **Freeze**).
3. Add a freeze note:
   - What's included in this freeze
   - What's intentionally excluded or deferred
   - Decision owner and date
4. Confirm. A **frozen snapshot** is created:
   - Immutable — no one can edit it
   - Permanently linkable via a stable URL
   - Shareable with ERP, PLM, and MES systems for production handoff

### After a freeze
- A new **draft iteration** is automatically created so work can continue.
- The frozen version remains accessible in the project's version history.
- Frozen versions can be compared side-by-side with later iterations.

---

## Version History

Every project maintains a full version history:

- Access it via **Project → History** in the left sidebar.
- Each version shows: who made the change, when, and a summary of what changed.
- You can **restore** any previous version as a new draft (the history is never deleted).

---

## Notifications

Team members receive notifications (in-app and optionally by email) when:

- They are added to a project
- A node they are watching is updated
- A design is frozen
- A Copilot suggestion is awaiting their review
- A comment or annotation is added to a node they own

Configure notification preferences in **Account Settings → Notifications**.

---

*← [Back to Home](Home)*
