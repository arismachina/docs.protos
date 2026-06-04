# Tutorial: Working with the Co-engineer

[← Home](Home) · [← Co-engineer](Co-engineer)

This tutorial shows you how to use the Co-engineer effectively — what to ask it, how it takes action in your project, and how to verify what it creates. It takes about 10 minutes.

---

## What the Co-engineer actually does

The Co-engineer is not just a chat window — it can take real actions in your project. When you ask it to create a schema, it creates it. When you ask it to populate a data document from a file, it reads the file, extracts values, and creates the document.

Everything it creates can be traced back to a source — if it drew a value from your Knowledge Library, it records which chunk it came from.

---

## Before you start

The Co-engineer works best when you have:
- At least one document in your **Knowledge Library** — so it has reference material to draw on
- At least one **Schema** — so it knows what structure to fill when creating data documents

If you haven't done those yet, do them first:
- [Tutorial: Creating Your First Schema](Tutorial-Schemas)
- [Tutorial: Setting Up Your Knowledge Library](Tutorial-Knowledge-Library)

---

## Step 1 — Open the Co-engineer

The Co-engineer panel is on the right side of the screen. Click the chat icon to open it.

You'll see a text input at the bottom and any previous conversation history above.

---

## Step 2 — Ask it to create a schema

Try this first request:

> *"Create a schema for electrode coating experiments. It should capture coating thickness, porosity, active material type, and mass loading."*

The Co-engineer will:
1. Search existing schemas to check if one already exists
2. Propose the schema with field names, types, and units
3. Ask you to confirm before creating it

Once you confirm, the schema appears in your Schema Editor. Go check it — you can edit any field before creating your first documents.

> **Always confirm before it creates.** The Co-engineer will describe what it's about to do and wait for your go-ahead. It won't create or change anything without your approval.

---

## Step 3 — Ask it to create a data document from a file

Upload a file in the chat — a test report, a datasheet, or any document with values you want to capture.

Then ask:

> *"Create an Electrode Coating document from this file."*

The Co-engineer will:
1. Read the file
2. Search the Knowledge Library for relevant context
3. Map what it finds to your schema fields
4. Show you the proposed document values
5. Create the document once you confirm

If it can't find a value for a required field, it will tell you rather than guessing. You can fill in the gaps manually.

---

## Step 4 — Check the provenance

Open the data document that was just created. Each field value that the Co-engineer filled in from a Knowledge Library source has a provenance marker — a small indicator showing which document and passage it came from.

Click it to see the original source. This is the traceability chain: field value → Knowledge Library chunk → your uploaded document.

If a value came from the file you uploaded rather than the Knowledge Library, that's recorded too.

---

## Step 5 — Ask it a question

The Co-engineer can answer questions using your project context:

> *"What does the Knowledge Library say about optimal porosity for NMC811 electrodes?"*

It will search your uploaded documents, find the relevant passages, and answer — citing exactly where the information came from.

Or ask for a recommendation:

> *"Based on what's in the Knowledge Library, what concentration should I use for the electrolyte formulation?"*

It will search, synthesise, and give a grounded answer — not a guess.

---

## Step 6 — Ask it to set up the Data Studio

Once you have a few data documents, ask:

> *"Activate the three most recent electrode coating documents in the Data Studio."*

The Co-engineer will update the Data Studio selection for you. Go to the Data Studio to confirm the documents are now active and visible in the table.

---

## Step 7 — Build a canvas with it

Ask:

> *"Build a canvas that takes my electrode coating data as input and calculates the theoretical capacity."*

The Co-engineer will:
1. Create the canvas in Simulation Studio
2. Add an Input block connected to your schema
3. Write the calculation code
4. Wire everything together

Go to Simulation Studio to review the canvas it built. The calculation block will need your approval before it runs — read the code, confirm it looks right, then approve it.

---

## What the Co-engineer won't do

- It won't create or change anything without your confirmation
- It won't fill in values it can't find or justify — it will flag missing information instead
- It won't speculate beyond what's in the Knowledge Library or what can be calculated

---

## What you've learned

- The Co-engineer takes real actions — it creates schemas, documents, and canvases
- It always asks for confirmation before making changes
- Every value it creates from a Knowledge Library source is cited and traceable
- It's most useful when the Knowledge Library is populated first
- You can ask it questions, ask it to create things, or ask it to set up the Data Studio

---

## Next steps

- **Keep the conversation going:** The Co-engineer remembers the full conversation history in a session. You don't need to re-explain context on every message.
- **Use it throughout:** The Co-engineer is useful at every stage — not just setup. Ask it to interpret simulation results, suggest next steps, or compare designs against your requirements.

---

*[← Back to Home](Home)*
