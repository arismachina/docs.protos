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

A community is a group of people you share with — its members, its team structure, and the assets shared into it. Resources stay owned by whoever created them; sharing into a community is what gives its members access. You can be a member of more than one community at the same time.

Everyone also has a **personal community** of their own, marked with a `Personal` badge on its card. It's the fallback when no other community is active, and it's the one community that can't be deleted.

> **Enterprise deployments call these Organizations.** The label depends on your deployment — protos.arismachina.com says *Communities*, a dedicated enterprise instance says *Organizations*. It's only a name: everything on this page works the same either way.

### Finding your communities

Click **Communities** in the sidebar — it sits near the bottom, just above your profile. This opens the Communities page, which shows a card for each community you belong to, along with any pending invitations.

### Setting an active community

Mark one community **Active** from its card on the Communities page (the button reads **Set as active** until you click it). Once set, the **Shared with me** tab on Schemas, Models, and Data Studio automatically scopes to that community — there's no separate per-page community filter to manage. The sidebar shows the active community's name as a subtitle under the Communities link, and hovering the link shows "filtering by {community}"; click it to go back and change or clear it.

### Creating a community

Click **+ New community** in the top right of the Communities page, enter a name, and confirm. You become the owner of the new community.

### Accepting an invitation

Pending invitations appear on the Communities page below your active communities. Click **Accept** to join or **Decline** to dismiss.

### Leaving a community

Click **Leave** in the header of the community's detail page. You lose access immediately, and you're removed from its sub-teams as well. Owners can't leave a community — transfer ownership first.

### Deleting a community

Owners see a **Delete** action in the same header. This permanently deletes the community and its teams, removes every member, and revokes everything shared with the community or any of its teams. Your personal community can't be deleted.

> **What happens to work built on a revoked share.** Protos tries to keep collaborators' work from breaking: where someone's canvas or document depended on an asset they've just lost access to, a private copy is usually created for them shortly afterwards. It happens in the background with no notification, so treat it as a fallback rather than a guarantee.

### Community detail page

Click any community card to open its detail page. At the top you can see your role in that community, the total number of members, and the number of teams. The page has these tabs:

- **Community chart** — a visual map of the community's structure (see below)
- **Members** — everyone in the community; owners and managers can invite new members here
- **Shared assets** — schemas, data documents, and models shared with a team, including everything inherited from teams above it. Pick a team from the selector; it starts on the community itself
- **Shared projects** — projects shared with a team, shown as cards; use the team selector to filter by team. Any schemas and data documents shared within each project appear nested under the project card. Inherited access (via a parent team) is indicated with an **Inherited** badge
- **Changelog** — a history of changes made within the community

### Community chart

The **Community chart** tab (the default view) shows the full team hierarchy as connected node cards, with the community at the top and sub-teams branching down. Click any node to select it — a detail panel appears below showing the **Members** and **Shared assets** for that specific team.

> Community owners and managers can inspect any team's shared assets. A plain member can only inspect teams they belong to, and sees an explanation rather than the asset list for the rest.

---

## Teams

Teams sit inside a community and let you share resources with a group of people at once. The team structure is managed entirely from the **Community chart** tab.

### Managing teams

Owners and managers can use the **⋯** menu on any node in the community chart to:

- **Add sub-team** — create a child team under the selected node
- **Rename** — rename the selected team
- **Move** — reassign the team to a different parent (also available by dragging a node onto another)
- **Delete** — remove the team. Only available once it has no sub-teams; until then the menu item reads **Delete (remove sub-teams first)**

The community itself sits at the top of the chart and isn't a team, so its node offers **Add sub-team** and **Rename**. You can also rename it with the pencil button beside its name in the page header.

To add or remove members from a specific team, click the team's node in the community chart and use the **Members** tab in the detail panel below.

### Leaving a team

Click a team's node in the community chart and use **Leave team** in the **Members** tab. You lose access to what's shared with that team, and anything you shared with it stops being shared. This is available on sub-teams you belong to, not on the community itself.

---

## Access Roles

When you share a resource, the person you share it with gets one of two roles. The person who created the resource is always its owner.

| Role | What they can do |
|------|-----------------|
| **Editor** | Can co-edit the resource directly — no need to copy it first |
| **Viewer** | Read-only access — can view, but cannot run, edit, or share |

Community **membership** is a separate set of roles, and it's what governs who can manage teams and members:

| Membership role | What they can do |
|-----------------|-----------------|
| **Owner** | Full control of the community, including deleting it |
| **Manager** | Invite and manage members, and manage the team structure |
| **Member** | Belong to the community and its teams |

---

## Sharing Resources

Sharing works across projects, canvases, schemas, data documents, models, and Co-Engineer chats — all through the same **Share** dialog.

### How to share

The dialog has two sides: **Add access** on the left, where you grant access, and [**Who has access**](#who-has-access) on the right, listing everyone who already has it.

1. Open the resource you want to share (project, canvas, schema, data document, model, or Co-Engineer chat).
2. Click the **⋯** menu and select **Share**.
3. Under **Add access**, confirm which community you're sharing into. If you belong to more than one, pick it from the dropdown at the top of the card — you can share into any of your communities without switching workspaces.
4. Choose who to add — pick one of the two tabs, and set the role (**Editor** or **Viewer**) from the **Role** dropdown beside them:

| Tab | How it works |
|-----|-------------|
| **People** | Search community members by name or email and click to add |
| **Team** | Pick a team from the tree — access flows down to all sub-teams |

To share with your whole community at once, click **Share with everyone** next to the community name. This gives every member Viewer access, and the button then reads **Shared community-wide**; adjust individual roles afterwards from the **Who has access** list.

Access is given immediately as you add people. Click **Close** when done.

> **Who can manage sharing.** For projects and Co-Engineer chats, **owners and editors** can share. For canvases, schemas, data documents, and models, the **Share** action is only offered to the owner. Knowledge Library documents are owner-only too. Making a resource public is **owner-only** in every case.

### Who has access

The share dialog shows a **Who has access** list that groups recipients by audience type:

| Group | What it shows |
|-------|--------------|
| **Communities** | Communities with access, subtitled *Everyone in this community* |
| **Teams** | Teams granted access directly |
| **People** | Individual members with direct access |
| **Domains** | Legacy email-domain grants. Domain sharing has been retired, so these can be removed but no new ones created |

Each entry uses an avatar appropriate to its audience type. A subtitle beneath each entry describes the scope of that grant — for example, **"Team — includes sub-teams"** for a team grant or **"Everyone in this community"** for a community-wide grant.

### Sharing also shares the sources behind your work

When you share a project or an asset (schema, data document, model, or canvas), Protos additively shares the **Knowledge Library documents that asset was built from**, read-only, with the same people. This lets a collaborator open and reason over the sources behind your work without you sharing your whole library. See [Knowledge Library → Sharing](Knowledge-Library#sharing) for details.

### Making a resource public

The **Public** section of the Share dialog is a wider scope than sharing with your community: a public resource is visible to **everyone on Protos**, across all communities, not just yours. Co-Engineer chats and Knowledge Library documents have no Public option.

Only the resource's **owner** can change this. Click **Make public** and an inline confirmation spells out the scope before it takes effect; **Make private** reverts it. Editors see the button disabled, with a tooltip explaining that it's owner-only.

---

## Discovering Shared Resources

Resource lists for schemas, data documents, and models have scope tabs at the top:

| Tab | Shows |
|-----|-------|
| **Mine** | Resources you own |
| **Shared with me** | Resources others have shared directly with you |
| **Public** | Resources that have been made public across Protos |
| **All** | Everything you have access to |

The same tabs appear on the Projects library, each with a count.

If you've [set an active community](#communities), **Shared with me** on these three pages automatically narrows to that community — see [Setting an active community](#communities).

Canvases don't use these scope tabs — to browse shared or public canvases, click **New → Import Canvas**, which has its own **Shared with Me**, **Community Canvases**, and **Public Canvases** tabs.

### Projects shared with you

The project switcher in the header lists projects you own **and** projects shared with you together, so a project a teammate shares reaches you without a trip to the library.

What a shared project's **⋯** menu offers depends on your role on it: **Delete** for the owner, **Rename** for the owner or an editor, and neither of them for a viewer. In place of the actions, the menu tells you why — and the wording depends on how the project reached you:

| How it reached you | What the menu says |
|--------------------|--------------------|
| Shared with a team in your community | *"Shared within your community — leave the community to remove it."* |
| Shared with you directly | *"Shared with you — only its owner can remove it."* |
| A legacy email-domain grant | *"Shared with your organisation — only its owner can remove it."* |
| Made public | *"A public project — only its owner can remove it."* |

There's no way to take a shared project off your own list, because the share isn't yours to revoke. To stop seeing one shared through a community, [leave that community](#leaving-a-community); for a project shared with you directly, ask its owner to remove your access.

---

## Live Presence

When other members of your community are in the same project, a row of **overlapping coloured circles** showing each person's initials appears in the top-right of the header bar. It only appears when at least one other person is present — it stays hidden when you're working alone.

### Seeing who's in the project

Click the circles to open a dropdown listing everyone currently in the project. Each entry shows the person's name and where they are — for example, *"On Cathode Coating v3"*, *"On Data Studio"*, or *"On the project overview"*. Click any entry to jump directly to what that person is viewing. Up to three avatars show at once, with a count for anyone beyond that.

---

## Soft Locks

Soft locks prevent conflicting edits by showing you when a teammate is already editing something. They are advisory — Protos does not block writes, but makes the conflict visible.

### Canvas component locks

When a team member opens a canvas component for editing, that component shows a **"[Name] is editing"** badge. If the Co-Engineer is making the edit on their behalf, it shows **"[Name]'s copilot is editing"** instead. Other users can still view the component but should wait for the lock to release before editing.

The lock releases when the editor closes the component panel. If their browser or connection drops instead, it expires about 60 seconds later.

### Schema and data document locks

Opening a schema in the editor shows the same **"[Name] is editing"** badge in its header. In the Data Studio, trying to edit a document someone else has open tells you **"[Name] is currently editing this document."** Models have no lock indicator.

---

## Shared Co-Engineer Chats

Co-Engineer sessions can be shared with community members.

1. Open a chat session and click **Share**.
2. Assign **Editor** or **Viewer** access to individuals, teams, or the whole community.
3. Shared sessions appear under **Shared with me** in the chat session list.
4. **Viewers** see a read-only transcript. **Editors** can continue the conversation.

When an editor sends a message in a shared session, other **editors** who have that session open see **"A collaborator is responding — you can type once they finish."** **Viewers**, who see the session through the read-only shared-session viewer, see the responding indicator right away, but the message and its completed reply appear together only once the Co-Engineer finishes the turn.

---

## Publishing for External Access

**Publications** let you share a canvas with people who don't have a Protos account — customers, partners, or reviewers.

A publication is a **snapshot of one or more canvases** at a point in time, accessible via a public URL, optionally behind a password.

### What external viewers can do

- See the canvas parameters and outputs
- Adjust parameter values and re-run the canvas interactively. Boolean, numeric, and text parameters are editable; other types are shown read-only
- **Choose which data documents feed the canvas.** If the canvas has input or data-input components, each one shows a multi-select document picker (limited to the documents matching that component's schema). Viewers tick which documents to feed in and re-run against their own selection.
- **View and edit the underlying data** when the owner has enabled the **Data tab**. It exposes the linked documents' field values, which viewers can adjust and re-run against.
- **Star the publication as helpful.** A star toggle in the sidebar (tooltip: *"Did you find this helpful?"*) lets a viewer mark the canvas helpful, with the total shown next to it.

When a viewer changes a parameter, an input, or a data document, the results on screen are out of date with respect to those edits, so the canvas shows **"Values changed — run again to see updated results."** Editing a field value in the Data tab shows **"Data changed — run again to see updated results."** instead. Running the canvas clears the hint, and so does putting a changed value back to what it was.

> **External models can make a canvas view-only.** The re-run abilities above apply to any canvas the owner left runnable. If a canvas includes a model that calls an external provider, the owner chooses at publish time whether viewers may run it; if they don't opt in, that entire canvas is **view-only** — parameter changes and data-document selections can't be re-run either. Canvases with no external models are always runnable.

### What they cannot see

- Your Python calculation code (stripped from the snapshot)
- Other canvases or projects
- The Knowledge Library or any internal data

### How to publish

Publishing is **owner-only**: only the project's owner sees an active **Publish** button. For everyone else it's disabled.

1. Open **Simulation Studio** from the sidebar. Scroll to the **Publications** section at the bottom of the canvas list and click **Publish**. (This is separate from the **Publish** button on the project overview, which submits a project to the public Protos gallery.)
2. Give the publication a **name**.
3. Select which **canvases to include**. A canvas can only be included once it has a successful run behind it — until then its checkbox is disabled and reads *"Run this canvas to include it."* Run the canvas in Simulation Studio, then come back.
4. **Include data tab** is on by default — uncheck it to leave the Data tab out. This adds a separate **Data** tab to the publication that exposes the linked documents' field values, which viewers can view, edit, and re-run against. (This is separate from the per-input document picker: viewers can choose which documents feed a canvas and re-run whether or not the data tab is on.)
5. For any **included** canvas that contains an external model, a per-canvas consent toggle appears: *"Let viewers run this canvas's external model on your API key. Turn off to make it view-only."* It defaults **on**. Leave it on to let external viewers re-run the canvas — its external models run on your key, billed to you. Turn it off to make **that whole canvas view-only** (viewers can't re-run it at all).
6. Optionally set a **password** for access control. Viewers are then asked for it before the publication opens, and you can [change or remove it later](#managing-a-publication).
7. Click **Publish** — a shareable URL is generated.

> **Note:** Publications are snapshots — they do not update automatically when you change the canvas. Re-publish to push an update.

### Managing a publication

The **Publications** card in Simulation Studio lists each publication with its canvas count and its helpful-star count. Click a publication's name to open its detail dialog, which shows:

- **Published** and **Last republished** timestamps
- **Included canvases**, and a **Data tab included** badge when that option is on
- **Password** — a **Protected** or **No password** badge, a field to **Set** or **Update** the password, and **Remove password** to drop it. Copy a password you've just set from the field beside it before you close the dialog: it isn't stored in a form Protos can show you again, so a lost one has to be replaced rather than looked up.
- **Copy link**, **Open**, **Re-publish (refresh snapshot)**, and **Delete** — deleting stops the shared link working and can't be undone

> **Deleting a project takes its publications offline.** A publication depends on the project that owns it, so deleting the project immediately stops every one of its public links working — as does deleting your account. Neither confirmation dialog warns you about it, so take the publications into account before you delete a project that has any.

In a project with no publications yet, members who aren't the owner see *"Publications are managed by the project owner."* in place of the prompt to publish one.

---

## See Also

- [Home → Project Overview](Home#project-overview)
- [Knowledge Library → Sharing](Knowledge-Library#sharing)
- [Glossary → Community](Glossary), [Glossary → Design freeze](Glossary), [Glossary → Version](Glossary)

---

*[← Back to Home](Home)*
