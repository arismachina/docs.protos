# Collaboration & Sharing

[← Home](Home) · **Collaboration & Sharing**

Protos supports sharing resources with teammates and publishing canvases for external stakeholders.

---

## On This Page

- [Organisations](#organisations)
- [Teams](#teams)
- [Access Roles](#access-roles)
- [Sharing Resources](#sharing-resources)
- [Discovering Shared Resources](#discovering-shared-resources)
- [Live Presence](#live-presence)
- [Soft Locks](#soft-locks)
- [Shared Co-Engineer Chats](#shared-co-engineer-chats)
- [Publishing for External Access](#publishing-for-external-access)

---

## Organisations

An organisation is the top-level workspace in Protos. Projects, models, canvases, and team members all belong to an organisation. You can be a member of more than one organisation at the same time.

### Finding your organisations

Click **Organizations** in the sidebar — it sits near the bottom, just above your profile. This opens the Organisations page, which shows a card for each organisation you belong to, along with any pending invitations.

### Creating an organisation

Click **+ New organization** in the top right of the Organisations page, enter a name, and confirm. You become the owner of the new organisation.

### Accepting an invitation

Pending invitations appear on the Organisations page below your active organisations. Click **Accept** to join or **Decline** to dismiss.

### Organisation detail page

Click any organisation card to open its detail page. At the top you can see your role in that org, the total number of members, and the number of teams. The page has four tabs:

- **Org chart** — a visual map of the organisation's structure (see below)
- **Members** — everyone in the organisation; owners and managers can invite new members here
- **Shared assets** — all assets shared at the organisation level, including projects, schemas, data documents, and models
- **Shared projects** — projects shared with a team, shown as cards; use the team selector to filter by team. Any schemas and data documents shared within each project appear nested under the project card. Inherited access (via a parent team) is indicated with an **Inherited** badge

### Org chart

The **Org chart** tab (the default view) shows the full team hierarchy as connected node cards, with the organisation at the top and sub-teams branching down. Click any node to select it — a detail panel appears below showing the **Members** and **Shared assets** for that specific team.

> The shared assets within a node are visible to all org members; if you are not a member of that specific team, you will see an access error rather than the asset list.

---

## Teams

Teams sit inside an organisation and let you share resources with a group of people at once. The team structure is managed entirely from the **Org chart** tab.

### Managing teams

Owners and managers can use the **⋯** menu on any node in the org chart to:

- **Add sub-team** — create a child team under the selected node
- **Rename** — rename the selected team
- **Move** — reassign the team to a different parent (also available by dragging a node onto another)
- **Delete** — remove the team (only possible if it has no sub-teams)

To add or remove members from a specific team, click the team's node in the org chart and use the **Members** tab in the detail panel below.

---

## Access Roles

When you share a resource, the person you share it with gets one of three roles:

| Role | What they can do |
|------|-----------------|
| **Owner** | Full control — view, edit, run, share, and publish |
| **Editor** | Can co-edit the resource directly — no need to copy it first |
| **Viewer** | Read-only access — can view and run, but cannot edit or share |

---

## Sharing Resources

Sharing works across projects, canvases, schemas, data documents, models, and co-engineer chats — all through the same **Share** dialog. Sharing is currently in beta.

### How to share

1. Open the resource you want to share (project, canvas, schema, data document, model, or co-engineer chat).
2. Click the **⋯** menu and select **Share**.
3. Select a role (**Editor** or **Viewer**) from the role dropdown.
4. Choose who to add access for — pick one of the three tabs:

| Tab | How it works |
|-----|-------------|
| **People** | Search org members by name or email and click to add |
| **Team** | Pick a team from the tree — access flows down to all sub-teams |
| **Domain** | Enter an email domain (e.g. `example.com`) — any org member with that domain gets access |

To share with your entire organisation at once, click **Share with everyone** above the tabs. This gives every org member Viewer access; you can adjust the role afterwards from the **Who has access** list.

Access is given immediately as you add people. Click **Close** when done.

> **Owners and editors** can manage sharing. Viewers cannot reshare.

### Who has access

The share dialog shows a **Who has access** list that groups recipients by audience type:

| Group | What it shows |
|-------|--------------|
| **Organizations** | Orgs with access; a cascade subtitle explains the inherited role |
| **Teams** | Teams granted access directly |
| **People** | Individual members with direct access |
| **Domains** | Email-domain grants, showing the matched domain |

Each entry uses an avatar appropriate to its audience type. A subtitle beneath each entry describes the scope of that grant — for example, **"Team — includes sub-teams"** for a team grant or **"Everyone in this organization"** for an org-wide grant.

If you belong to more than one organisation, an **org-context picker** lets you share the resource into any of your orgs without switching workspaces.

### Making a resource public

The Share dialog includes **Make public** and **Make private** buttons. Clicking **Make public** shows an inline confirmation before taking effect. Click **Make private** to revert.

---

## Discovering Shared Resources

All resource lists (canvases, schemas, data documents, models) have scope tabs at the top:

| Tab | Shows |
|-----|-------|
| **All** | Everything you have access to |
| **Mine** | Resources you own |
| **Shared with me** | Resources others have shared directly with you |
| **Public** | Resources that have been made public across Protos |

---

## Live Presence

When other members of your organisation are in the same project, a row of **overlapping coloured circles** showing each person's initials appears in the top-right of the header bar. It only appears when at least one other person is present — it stays hidden when you're working alone.

### Seeing who's in the project

Click the circles to open a dropdown listing everyone currently in the project. Each entry shows the person's name and where they are — for example, *"On a simulation canvas"* or *"On a schema"*. Click any entry to jump directly to what that person is viewing.

---

## Soft Locks

Soft locks prevent conflicting edits by showing you when a teammate is already editing something. They are advisory — Protos does not block writes, but makes the conflict visible.

### Canvas component locks

When a team member opens a canvas component for editing, that component shows a **"[Name] is editing"** badge. If the Co-engineer is making the edit on their behalf, it shows **"[Name]'s copilot is editing"** instead. Other users can still view the component but should wait for the lock to release before editing.

The lock releases automatically when the editor closes the component panel, navigates away, or after roughly 60 seconds of inactivity.

### Schema, data document, and model locks

The same lock badge appears on schemas, data documents, and models. When someone opens one of these resources for editing, all other users see the **"[Name] is editing"** indicator on that resource in the list. The lock releases the same way — on close, navigation, or timeout.

---

## Shared Co-Engineer Chats

Co-engineer sessions can be shared with org members.

1. Open a chat session and click **Share**.
2. Assign **Editor** or **Viewer** access to individuals, teams, or the whole org.
3. Shared sessions appear under **Shared with me** in the chat session list.
4. **Viewers** see a read-only transcript. **Editors** can continue the conversation.

When an editor sends a message in a shared session, other participants see it appear immediately — the editor's message bubble appears along with a **"Co-Engineer is responding…"** indicator. The completed reply replaces the placeholder when the Co-engineer finishes. Participants do not need to refresh or wait for the reply before seeing that a message was sent.

---

## Publishing for External Access

**Publications** let you share a canvas with people who don't have a Protos account — customers, partners, or reviewers.

A publication is a **snapshot of a canvas** at a point in time, accessible via a public URL.

### What external viewers can do

- See the canvas parameters and outputs
- Adjust parameter values and re-run the canvas interactively
- View and edit linked data documents (if the owner enabled the data tab when publishing)

### What they cannot see

- Your Python calculation code (stripped from the snapshot)
- Other canvases or projects
- The Knowledge Library or any internal data

### How to publish

1. Open **Simulation Studio** from the sidebar. Scroll to the **Publications** section at the bottom of the canvas list and click **Publish**.
2. Give the publication a **name**.
3. Select which **canvases to include**.
4. Optionally check **Include data tab** to expose the underlying data to viewers.
5. Optionally set a **password** for access control.
6. Click **Publish** — a shareable URL is generated.

> **Note:** Publications are snapshots — they do not update automatically when you change the canvas. Re-publish to push an update.

---

## See Also

- [Home → Project Overview](Home#project-overview)
- [Glossary → Design freeze](Glossary), [Glossary → Version](Glossary)

---

*[← Back to Home](Home)*
