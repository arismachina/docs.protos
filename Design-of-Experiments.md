# Design of Experiments <a href="https://protos.arismachina.com" class="try-protos" target="_blank" rel="noopener noreferrer">Try Protos</a>

[← Home](Home) · **Design of Experiments**

**Design of Experiments** is for when you have a limited budget of runs — simulations or bench experiments — and need to spend them where they buy the most information. You say what you're chasing and what you're free to vary; each round recommends the next batch of experiments from everything measured so far.

> **A loop, not a one-off design.** You don't get a single table of experiments to go and run. You run a batch, record what came back, and the recommender uses it to choose the next batch. Each pass is a **round**, and the loop keeps going until the requirement is met or it stops learning anything new.

---

## On This Page

- [What You Need First](#what-you-need-first)
- [Starting a Loop](#starting-a-loop)
- [The Loop Screen](#the-loop-screen)
- [Recording Results](#recording-results)
- [Reading the Model](#reading-the-model)
- [Keeping a Design](#keeping-a-design)
- [Changing What the Loop Varies](#changing-what-the-loop-varies)
- [Where Measurements Live](#where-measurements-live)

---

## What You Need First

A loop starts from data you already have, in the [Data Studio](Data-Studio). You need two documents on the same [schema](Schemas):

- **A design** — the starting point. Its fields are what the loop can vary and what it measures.
- **A requirement** — what the design is judged against. Its bounds are the targets each KPI chases, and the only place those targets can be changed.

If you have no requirement yet, create one from the Data Studio's **Create type** toggle. Without both, the **Start a DOE** button stays disabled.

---

## Starting a Loop

1. In the [Data Studio](Data-Studio), pick the schema and put your design and requirement on the table.
2. Click **Start a DOE**. The table switches into marking mode.
3. **Mark each field** you care about, on the field itself:
   - **KPI** — *chase this.* The loop tries to move it toward its requirement.
   - **Lever** — *vary this.* The loop chooses values for it each round.

   A field the schema computes can't be a lever on a simulated loop: a canvas run would overwrite whatever the design set. Vary it on a lab loop instead, where a person sets the value.
4. Click **Continue**. The setup dialog asks only what the sheet couldn't answer:
   - **What this loop chases** — each KPI's target, read from the requirement. A KPI with no target there needs a goal stated here: **Hit target**, **Maximise**, or **Minimise**. A target typed here belongs to this loop alone; put it in a requirement document if other loops should chase it too.
   - **What it may vary** — each lever's range. A lever with no range in the schema can't be sampled until you give it one.
   - **Experiments per round** — how many you can run in parallel. The loop recommends this many each round.
5. Click **Plan the first round**. Protos opens the loop screen with the opening batch ready.

Loops are listed under **Design of Experiments** in the sidebar, and each one is linkable — the URL carries the loop, so you can share a running loop with a colleague who can read it.

---

## The Loop Screen

Four tabs:

| Tab | What's on it |
|-----|--------------|
| **Experiments** | This round's batch — the values to set, and where results go |
| **Gaps** | How far the best design still is from each requirement, and whether that distance is closing |
| **Model** | What the loop has learned: each lever's effect, what's still uncertain, and what drives the variation |
| **Data** | Every measurement this project holds on this system, whichever loop recorded it |

### Why an experiment was chosen

Each row on the **Experiments** tab carries the reason it's there — which KPI it's chasing, what the model predicts for it, and whether it was picked to improve on the best design so far or to reduce uncertainty somewhere the model is guessing. A prediction shown against a row is the model's current belief, never a result.

---

## Recording Results

Each experiment can be filled however suits it, and one loop can mix all three:

- **Type a measured value** straight into the row. You can also give a **±** — one standard deviation in the KPI's own units. Supply it and the model treats that reading as less certain instead of chasing the scatter; leave it out and the value is taken as exact.
- **Run it on a canvas.** Pick which [Simulation Studio](Simulation-Studio) canvas runs the row and it's dispatched against a temporary copy of that canvas, so your canvas is never modified. Rows show as *queued* and *computing* while they run, and a failed row can be retried. If no canvas suits the loop yet, the Co-Engineer can build one for it.
- **Compute it from a formula**, when the schema field carries one.

Mark each row **Physical experiment** or **Simulated** under **Run as**. The loop keeps the two apart: where simulation and measurement separate is exactly what its correction term is fitted on, so a loop can learn from cheap simulated runs and a smaller number of physical ones together.

### Bulk entry

- **Attach measured results** takes a CSV of the whole round. Each row is matched to the experiment it names by its input values, so the sheet can be in any order — but a row matching no planned experiment, or two of them, stops the attach rather than being filed on a guess.
- **Upload experimental data** records past experiments the loop never planned. Name each column after a schema field — its label, its name, or its full path.
- **Export** downloads this round, or all measured data, as CSV. The columns match the upload format, so an export from one loop can seed another.

### Letting it run itself

**Next batch** recommends the following round when you're ready. **Run automatically** does it for a number of rounds without stopping — useful when every row is a canvas run and nothing needs a person.

**Finish this loop** ends the current round with whatever has been recorded; anything that hasn't started is cancelled.

---

## Reading the Model

The **Model** tab is fitted from the data, not configured.

- **What the model believes** — the predicted response across each lever's range, with a ±2σ band. A wide band is the model saying it doesn't know yet.
- **What the model learned about each lever** — each lever's effect graded *strong*, *clear*, *gentle*, or *no detected effect*.
- **What is still uncertain** — which KPIs are pinned down across the whole lever range and which are still learning. If everything is pinned down, more rounds in these bounds buy little.
- **What drives the variation** — how much of what a response does across the lever ranges each lever accounts for, alone and in combination. A lever swung narrowly accounts for little because it was barely moved, not because it doesn't matter.

> **The model refuses rather than guessing.** If the runs so far can't support a model — too few of them, a lever that never varied on its own, failures clustered in one region, or a fit that is over-confident about its own uncertainty — the loop says so and tells you what would fix it. It does not produce a smooth response surface you shouldn't trust.

### The Gaps tab

**Gap against requirements** shows each KPI as a distance from its target, and **Is the gap closing?** plots that distance after each round. Bars that stop shrinking mean more rounds aren't buying anything — widen the bounds or revise the requirement rather than running more batches.

**Original design vs best run so far** answers two separate questions side by side: did this move from where you started, and does it satisfy the requirement right now. A design can do either without the other. **Best in project** is the same KPI's best value anywhere on this schema, even from a different loop.

**How much each KPI matters** sets a multiplier on a shortfall when the loop chooses the next batch — at ×2, being 10% short counts like 20%. It changes what gets recommended from here on, never what has already been measured. Set it to 0 to keep measuring a KPI without chasing it.

---

## Keeping a Design

**Which design to keep**, on the Gaps tab, ranks the measured experiments by how many requirements they meet, then by the strongest weakest KPI. **Use this design** writes it into the Data Studio as a data document, beside the design the loop started from — which stays exactly as it was, so you can still compare them. The settings and results are already recorded on the experiment; adopting it names it and marks it saved.

When every requirement is met the loop says so, and you can save the design you want or keep going to look for a better one.

**Abandon** gives up on a loop for good and deletes every draft design it created that you haven't adopted. This can't be undone.

---

## Changing What the Loop Varies

**Change what this loop varies** lets you widen a bound, add a lever, or retire one. The next round runs in the new space and keeps every past experiment the new space can still hold — the dialog tells you how much survives before you commit. If nothing does, it says so rather than silently starting from nothing.

---

## Where Measurements Live

Measurements belong to the **project and the schema**, not to the loop that recorded them. So a new loop on a system this project has already measured starts from that whole history instead of from nothing, and the **Data** tab shows every measurement on the system regardless of which loop produced it.

Each value keeps where it came from — measured, simulated, or computed — so the loop can weigh a number without ever having to guess at its provenance.

Access is by loop: if a project has loops you can't read, the Data tab tells you there is measured data it isn't showing rather than quietly leaving it out.

---

## See Also

- [Data Studio](Data-Studio) — where a loop starts, and where an adopted design lands
- [Schemas](Schemas) — field ranges and units are what the loop samples within
- [Simulation Studio](Simulation-Studio) — the canvases that can evaluate a round
- [Co-Engineer](Co-engineer) — can build a canvas for a loop, and work with campaigns from chat

---

*[← Back to Home](Home)*
