# Collaboration & Sharing

[← Home](Home) · **Collaboration & Sharing**

Protos supports sharing resources with teammates and publishing canvases for external stakeholders.

---

## On This Page

- [Communities](#communities)
- [Teams](#teams)
- [Access Roles](#access-roles)
- [Sharing Resources](#sharing-resources)
- [Discovering Shared Resources](#discovering-shared-resources)
- [Live Presence](#live-presence)
- [Soft Locks](#soft-locks)
- [Shared Co-Engineer Chats](#shared-co-engineer-chats)
- [Publishing for External Access](#publishing-for-external-access)

---

## Communities

A community is the top-level workspace in Protos. Projects, models, canvases, and team members all belong to a community. You can be a member of more than one community at the same time.

### Finding your communities

Click **Communities** in the sidebar — it sits near the bottom, just above your profile. This opens the Communities page, which shows a card for each community you belong to, along with any pending invitations.

### Setting an active community

Mark one community **Active** from its card on the Communities page (the button reads **Set as active** until you click it). Once set, the **Shared with me** tab on Schemas, Models, and Data Studio automatically scopes to that community — there's no separate per-page community filter to manage. The sidebar shows the active community's name as a subtitle under the Communities link, and hovering the link shows "filtering by {community}"; click it to go back and change or clear it.

### Creating a community

Click **+ New community** in the top right of the Communities page, enter a name, and confirm. You become the owner of the new community.

### Accepting an invitation

Pending invitations appear on the Communities page below your active communities. Click **Accept** to join or **Decline** to dismiss.

### Community detail page

Click any community card to open its detail page. At the top you can see your role in that community, the total number of members, and the number of teams. The page has these tabs:

- **Community chart** — a visual map of the community's structure (see below)
- **Members** — everyone in the community; owners and managers can invite new members here
- **Shared assets** — all assets shared at the community level, including projects, schemas, data documents, and models
- **Shared projects** — projects shared with a team, shown as cards; use the team selector to filter by team. Any schemas and data documents shared within each project appear nested under the project card. Inherited access (via a parent team) is indicated with an **Inherited** badge
- **Changelog** — a history of changes made within the community

### Community chart

The **Community chart** tab (the default view) shows the full team hierarchy as connected node cards, with the community at the top and sub-teams branching down. Click any node to select it — a detail panel appears below showing the **Members** and **Shared assets** for that specific team.

> The shared assets within a node are visible to all community members; if you are not a member of that specific team, you will see an access error rather than the asset list.

---

## Teams

Teams sit inside a community and let you share resources with a group of people at once. The team structure is managed entirely from the **Community chart** tab.

### Managing teams

Owners and managers can use the **⋯** menu on any node in the community chart to:

- **Add sub-team** — create a child team under the selected node
- **Rename** — rename the selected team
- **Move** — reassign the team to a different parent (also available by dragging a node onto another)
- **Delete** — remove the team (only possible if it has no sub-teams)

To add or remove members from a specific team, click the team's node in the community chart and use the **Members** tab in the detail panel below.

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
| **People** | Search community members by name or email and click to add |
| **Team** | Pick a team from the tree — access flows down to all sub-teams |
| **Domain** | Enter an email domain (e.g. `example.com`) — any community member with that domain gets access |

To share with your entire community at once, click **Share with everyone** above the tabs. This gives every community member Viewer access; you can adjust the role afterwards from the **Who has access** list.

Access is given immediately as you add people. Click **Close** when done.

> For projects and co-engineer chats, **owners and editors** can manage sharing. For canvases, schemas, data documents, and models, only the **owner** can manage sharing — editors cannot reshare.

### Who has access

The share dialog shows a **Who has access** list that groups recipients by audience type:

| Group | What it shows |
|-------|--------------|
| **Communities** | Communities with access; a cascade subtitle explains the inherited role |
| **Teams** | Teams granted access directly |
| **People** | Individual members with direct access |
| **Domains** | Email-domain grants, showing the matched domain |

Each entry uses an avatar appropriate to its audience type. A subtitle beneath each entry describes the scope of that grant — for example, **"Team — includes sub-teams"** for a team grant or **"Everyone in this community"** for a community-wide grant.

If you belong to more than one community, a **community-context picker** lets you share the resource into any of your communities without switching workspaces.

### Sharing also shares the sources behind your work

When you share a project or an asset (schema, data document, model, or canvas), Protos additively shares the **Knowledge Library documents that asset was built from**, read-only, with the same people. This lets a collaborator open and reason over the sources behind your work without you sharing your whole library. See [Knowledge Library → Sharing](Knowledge-Library#sharing) for details.

### Making a resource public

The Share dialog includes **Make public** and **Make private** buttons. Clicking **Make public** shows an inline confirmation before taking effect. Click **Make private** to revert.

---

## Discovering Shared Resources

Resource lists for schemas, data documents, and models have scope tabs at the top:

| Tab | Shows |
|-----|-------|
| **All** | Everything you have access to |
| **Mine** | Resources you own |
| **Shared with me** | Resources others have shared directly with you |
| **Public** | Resources that have been made public across Protos |

If you've [set an active community](#communities), **Shared with me** on these three pages automatically narrows to that community — see [Setting an active community](#communities).

Canvases don't use these scope tabs — to browse shared or public canvases, click **New → Import Canvas**, which has its own Shared / Community / Public tabs.

---

## Live Presence

When other members of your community are in the same project, a row of **overlapping coloured circles** showing each person's initials appears in the top-right of the header bar. It only appears when at least one other person is present — it stays hidden when you're working alone.

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

Co-engineer sessions can be shared with community members.

1. Open a chat session and click **Share**.
2. Assign **Editor** or **Viewer** access to individuals, teams, or the whole community.
3. Shared sessions appear under **Shared with me** in the chat session list.
4. **Viewers** see a read-only transcript. **Editors** can continue the conversation.

When an editor sends a message in a shared session, other **editors** who have that session open see the message bubble and a **"Co-Engineer is responding…"** indicator appear right away. **Viewers**, who see the session through the read-only shared-session viewer, see the message and the completed reply appear together only once the Co-engineer finishes the turn.

---

## Publishing for External Access

**Publications** let you share a canvas with people who don't have a Protos account — customers, partners, or reviewers.

A publication is a **snapshot of a canvas** at a point in time, accessible via a public URL.

### What external viewers can do

- See the canvas parameters and outputs
- Adjust parameter values and re-run the canvas interactively
- **Choose which data documents feed the canvas.** When a canvas has input or data-input components and the owner enabled the data tab at publish time, the Visualization tab shows a checkbox for each data document. Viewers pick which documents to feed in and re-run against their own selection, rather than being locked to the snapshot's defaults.
- **Re-run the canvas's external models** — *if the owner allows it.* When the owner opts in per canvas at publish time, viewers can re-run model components that call external providers, executed on the owner's API key. If the owner leaves this off, those models stay view-only.
- **Star the publication as helpful.** A star toggle in the sidebar (tooltip: *"Did you find this helpful?"*) lets a viewer mark the canvas helpful, and the running total is shown next to it — compact once past 999 (e.g. `2K`, `1.2M`). On a password-protected publication, viewers enter the password before they can star.

### What they cannot see

- Your Python calculation code (stripped from the snapshot)
- Other canvases or projects
- The Knowledge Library or any internal data

### How to publish

1. Open **Simulation Studio** from the sidebar. Scroll to the **Publications** section at the bottom of the canvas list and click **Publish**.
2. Give the publication a **name**.
3. Select which **canvases to include**.
4. Optionally check **Include data tab**. This exposes the underlying data to viewers *and* lets them choose which data documents feed the canvas and re-run against their selection — it is no longer a read-only data view.
5. For any canvas that contains an external model, a per-canvas consent toggle appears: *"Let viewers run this canvas's external model on your API key. Turn off to make it view-only."* It defaults **on**. Leave it on to let external viewers re-run those models (billed to your key); turn it off to keep them view-only.
6. Optionally set a **password** for access control.
7. Click **Publish** — a shareable URL is generated.

> **Note:** Publications are snapshots — they do not update automatically when you change the canvas. Re-publish to push an update.

The **Publications** card in Simulation Studio shows each publication's helpful-star count read-only, so you can see how many viewers found a published canvas useful.

---

## See Also

- [Home → Project Overview](Home#project-overview)
- [Knowledge Library → Sharing](Knowledge-Library#sharing)
- [Glossary → Community](Glossary), [Glossary → Design freeze](Glossary), [Glossary → Version](Glossary)

---

*[← Back to Home](Home)*
