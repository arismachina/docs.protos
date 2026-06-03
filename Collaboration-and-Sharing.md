# Collaboration & Sharing

[← Home](Home) · **Collaboration & Sharing**

Protos is built for cross-functional R&D teams. This page covers access control, sharing projects, design freeze for manufacturing handoff, and version history.

---

## On This Page

- [Access Levels](#access-levels)
- [Sharing a Project](#sharing-a-project)
- [Design Freeze](#design-freeze)
- [Version History](#version-history)
- [Notifications](#notifications)

---

## Access Levels

| Role | View | Edit canvas | Share | Freeze / publish |
|------|------|-------------|-------|-----------------|
| **Viewer** | ✓ | | | |
| **Contributor** | ✓ | ✓ | | |
| **Editor** | ✓ | ✓ | ✓ | |
| **Admin** | ✓ | ✓ | ✓ | ✓ |

Set a collaborator's role when you invite them, or change it later via **Project → Members**.

---

## Sharing a Project

### Internal sharing

1. Open the project.
2. Click **Share** in the top-right.
3. Add team members by email or Aris Machina username.
4. Set their access level (see table above).
5. Click **Send invite**.

### External sharing (customers, partners)

> **Before sharing externally:** confirm with the project lead that external access is appropriate for this project and stage.

- Use a **view-only shareable link** — this gives read access without requiring an Aris Machina account.
- **Remove external access** when the review period ends.
- Do not grant Contributor or Editor access to external parties without explicit approval from an Admin.

---

## Design Freeze

A design freeze captures an **immutable, permanently linkable snapshot** of a validated design. Use it for handoff to manufacturing, pilot, or qualification.

### How to freeze a design

1. Open the project.
2. Click **Freeze Design** (top-right menu → **Freeze**).
3. Add a freeze note covering:
   - What's included in this freeze
   - What's intentionally excluded or deferred
   - Decision owner and date
4. Confirm.

The frozen snapshot is:

| Property | Detail |
|----------|--------|
| **Immutable** | No one can edit it, including Admins |
| **Permanently linkable** | Stable URL that will never change |
| **Shareable with downstream systems** | Push to ERP, PLM, and MES for production handoff |

### After a freeze

- A new **draft iteration** is automatically created so work can continue.
- The frozen version remains accessible in the project's **Version History**.
- Frozen versions can be compared side-by-side with later iterations.

> **Note:** Design freeze is the only action in Protos that is irreversible by design. The frozen snapshot cannot be edited or deleted — this is intentional, as it provides the audit trail required for manufacturing and qualification processes.

---

## Version History

Every project maintains a full, non-destructive version history.

- Access it via **Project → History** in the left sidebar.
- Each version shows: who made the change, when, and a summary of what changed.
- **Restore** any previous version as a new draft — the history is never deleted.

Version history is separate from design freeze. Versions are rolling snapshots of work in progress; a frozen design is a deliberate, named checkpoint for external handoff.

---

## Notifications

Team members receive notifications (in-app and optionally by email) when:

- They are added to a project
- A node they are watching is updated
- A design is frozen
- A [Copilot](Copilot) suggestion is awaiting their review
- A comment or annotation is added to a node they own

Configure notification preferences in **Account Settings → Notifications**.

---

## See Also

- [Home → Project Overview](Home#project-overview) — creating projects and setting initial access
- [Copilot](Copilot) — Copilot suggestions may be routed to specific team members for review
- [Glossary → Design freeze](Glossary), [Glossary → Version](Glossary)

---

*[← Back to Home](Home)*
