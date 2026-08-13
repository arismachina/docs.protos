# Frequently Asked Questions

Common questions about Protos, grouped by topic.

---

## About Protos

??? faq "What is Protos?"
    An AI co-engineer for R&D teams working on complex physical systems.

    You capture what you're working on as structured data (schemas), connect models and reference material to it, and assemble and run the whole thing as a visual canvas. Results come from deterministic, first-principles models rather than a language model's best guess, so an output is something you can open up, explain and defend later — not a number you take on trust. And every value stays traceable to the source it came from, so when someone asks where a figure came from months later, you can show them.

    Docs: [Schemas](Schemas) · [Simulation Studio](Simulation-Studio)

??? faq "How is this different from using ChatGPT or Claude on our data?"
    Deterministic physics versus plausible prose.

    A chat model can give you a sliver of a simulation. You can't put it in a design review. Protos runs real simulations, so the number comes from solved equations rather than from prediction, and it keeps the structure of your work in a schema instead of a conversation, so it survives after the chat window closes.

    Three things a chat window can't give you:

    - **Traceability** — every value is held against the document it came from, so the result is auditable.
    - **Collaboration** — work is shared across the team with role-based access rather than trapped in one person's session.
    - **Versioning** — schemas, canvases and models are versioned, so every change is attributed and you can compare or roll back.

    The language model is there to build and connect things faster, not to produce the engineering result.

    Docs: [Simulation Studio](Simulation-Studio) · [Versioning](Versioning)

??? faq "Do I need to be a simulation engineer to use it?"
    No, though you need to know your domain.

    The Co-Engineer builds schemas, populates data documents from your files and assembles canvases, so you're reviewing structure rather than writing it from scratch. Reading a simulation result and judging whether it's sensible is still your job.

    Docs: [Co-Engineer](Co-engineer)

??? faq "What models ship with it?"
    Two kinds: seven first-party physics models we build and maintain (deterministic, PyBaMM-based, with no learned weights), plus a growing public catalog you can browse and add with a click. Grouped by domain:

    - **Battery & cell design** (first-party physics) — Cell Performance, Cell Optimizer, DFN Calendar Ageing, DFN Cyclic Ageing, SPMeT Power, SPMeT DCIR, SPMeT Dynamic Load
    - **Protein structure & design** — AlphaFold2, AlphaFold2-Multimer, OpenFold2, OpenFold3, Boltz2, ESM3 / ESMC (Biohub: fold, inverse-fold, generate, sample, encode/decode), ProteinMPNN, RFdiffusion, ColabFold MSA Search
    - **Drug discovery & chemistry** — DiffDock (docking), GenMol, MolMIM (molecular generation & optimization)
    - **Genomics** — Evo2 40B (DNA/RNA sequence generation)
    - **Logistics & operations** — NVIDIA cuOpt (vehicle routing)
    - **General-purpose AI** — Google Gemini Interactions (chat, tools, document understanding, image / TTS / music, deep research)

    The battery models are deterministic simulations, not trained models; several catalog models (folding, generative chemistry, genomics, Gemini) are trained or foundation models, and many run GPU-accelerated on NVIDIA NIM. You can also register your own from a script, a repo or an existing API endpoint.

    Docs: [Models Library](Model-Library)

---

## Your models and simulations

??? faq "How are models executed, and where do they run?"
    You choose how each model runs — on our cloud, on your own machine, or behind an API you already host.

    The Models Library takes three intake options — Local code (upload a script, Protos runs it in a managed environment), Endpoint (point at an existing API, with optional auth), and Version control (public GitHub / GitLab / Bitbucket / Codeberg repo URL). Each registered model runs in one of two execution modes: **Local runner**, where your model runs on your own machine and the platform dispatches inputs, or **Cloud**, where you upload Python code and Protos containerises and runs it. MATLAB and COMSOL are supported as Local runner runtimes. In every case the contract with Protos is a JSON input schema and a JSON output schema, not the solver file itself.

    The three routes:

    - **Local runner** — the model runs on your own hardware with your existing licences and toolboxes; Protos dispatches inputs and collects results, so no licence migration and no model or data IP leaves your network.
    - **Cloud** — upload Python code (or point at a public GitHub / GitLab / Bitbucket / Codeberg repo) and Protos containerises and runs it on our infrastructure, with no runner machine to maintain.
    - **Endpoint** — point Protos at an existing API you already host, with optional auth.

    **Licensed tools (MATLAB, COMSOL, Simulink, …):** these run only via the Local runner or your own Endpoint, on a machine that already holds the licences — they are not available in Cloud execution, and Aris Machina never ships, hosts or resells them. A licence-bound solver therefore stays on your hardware with its existing toolboxes. For Simulink specifically: keep it local, wrap it behind REST (MATLAB Production Server / Compiler SDK), or export a co-simulation FMU and register a short Python runner as a Cloud model.

    **Running a Local runner — one trust step:** Aris Machina is not currently enrolled in the Apple Developer Program or the equivalent Microsoft signing program, so the Local runner executable is not OS-code-signed. The first time you launch it, your operating system will flag it as coming from an unidentified developer and you'll need to explicitly allow it to run.

    Docs: [Models Library](Model-Library)

??? faq "Are MATLAB toolbox dependencies supported, and are MATLAB licences required?"
    Supported. Licences stay with you — Aris Machina neither requires nor resells MATLAB entitlements.

    Toolbox support is a property of the execution environment, not of Protos: whatever MATLAB, Simscape or Powertrain Blockset licences the executing machine holds are the toolboxes available to the model. With Local runner and Endpoint modes, that machine is yours, so your full toolbox stack is available unchanged.

    With the FMU route, a co-simulation FMU is intended to embed the compiled solver, so it would execute without consuming a MATLAB licence at run time. Whether that holds for a given model depends on it being code-generation compatible. Simulink Compiler is needed to produce the FMU, and Simscape-derived FMUs carry MathWorks redistribution terms worth confirming with your legal team.

    Docs: [Models Library](Model-Library)

??? faq "How do we register a compiled or encapsulated model?"
    Five steps, and the Co-Engineer can do most of it for you.

    1. Models Library → Register Model.
    2. Choose the source: Local code (needs Name, Key, Description), Endpoint (URL plus auth), or Version control (repo URL).
    3. Define the input and output schemas. The JSON schema builders let you specify each parameter's name, type, and whether it's required.
    4. For repo-based models Protos builds a container: *Preparing build context → Building image → Finalising*. This takes a few minutes.
    5. Drop the model onto a Simulation Studio canvas as a model node and wire inputs.

    Shortcut: upload a Python script or point the Co-Engineer at a repo and ask it to register the model. It will infer the input and output schema automatically.

    For an encapsulated Simulink model, register the thin Python runner (FMPy loading the FMU, or a client calling your MATLAB Production Server) rather than the .slx. Put that runner in a repo, let the Co-Engineer infer the schema, then hand-check the input schema against your parameter naming and units. The documented path uses a public repo URL; private repos can be connected via a deploy token or mirror during onboarding.

    Docs: [Models Library](Model-Library)

??? faq "Can simulation criteria be modelled as structured requirements or KPIs, and linked to results?"
    Yes. This is core to the product, not an add-on.

    Schemas define the structure of your engineering data with typed fields (string, number with units, boolean, enum, date) and Ref fields that link across schemas. In Data Studio, Design documents use single values and Requirement documents use min/max ranges, so a KPI corridor is a first-class object. Simulation Studio can pull schema values directly as simulation inputs, and the Co-Engineer will parse an uploaded spec document into structured targets and constraints, then compare design variants against requirements and surface gaps.

    Everything stays traceable: you can follow any value in Protos back through its chain of sources to the original reference.

    Today that comparison surfaces gaps through the Co-Engineer rather than emitting a formal signed verification matrix.

    Docs: [Schemas](Schemas) · [Data Studio](Data-Studio) · [Co-Engineer](Co-engineer)

??? faq "Can it handle batch runs and parameter sweeps (DoE)?"
    Coming soon.

---

## Data and traceability

??? faq "How is data input handled, and which formats are supported?"
    Four routes in, covering documents, structured data, model I/O and live parameters.

    - **Knowledge Library** (unstructured and reference material) — PDF, DOCX, XLSX, CSV, TXT, Markdown, JSON and common image formats, up to 100 MB per file. Upload singly, as knowledge notes, or as bulk folders up to 500 files or 100 MB per batch. Content is parsed and chunked for the Co-Engineer.
    - **Data Studio** (structured) — data documents conforming to your schemas. The Co-Engineer can extract structured data from an uploaded file and create a data document following your schema. Spec and requirements ingest accepts files up to 32 MB.
    - **Canvas** — parameter nodes and data input nodes feed simulations directly, wired up from data you enter yourself or that the Co-Engineer finds for you.
    - **Models** — JSON in, JSON out, against the schema you define at registration.

    For test and load-profile data, CSV or XLSX into Data Studio against a schema makes every value traceable back to its source file. For very large measurement files that exceed the document cap, pass them by reference to a Local runner or Endpoint model so the bulk data never has to move.

    Docs: [Knowledge Library](Knowledge-Library) · [Data Studio](Data-Studio)

??? faq "How do we know where a number came from?"
    Every value traces back to its source document, permanently.

    A value links through the knowledge chunk that produced it to the original source document, and each document shows every schema, data document, model and canvas built from it. The chain survives people leaving and projects being archived, which is usually the real reason teams want it.

    Docs: [Data Studio](Data-Studio) · [Knowledge Library](Knowledge-Library)

??? faq "Can we roll back a mistake?"
    Yes, non-destructively, with attribution.

    Schemas, canvases, models and knowledge documents are versioned. Changes are attributed to whoever made them and grouped into sessions. Restoring writes a new version applying the old content rather than erasing what happened in between. You can label versions, compare any two side by side, and ask the Co-Engineer what changed in plain language.

    Docs: [Versioning](Versioning)

??? faq "Can we show results to people without a Protos account?"
    Yes, by publishing a canvas to a public URL.

    Whoever opens the publication can change input parameters and re-run the canvas themselves, which tends to land better than a PDF of your conclusions. External viewers cannot reach your Python code, your other projects, or your Knowledge Library, and you can block external model execution to avoid API charges. Publications are snapshots, so you republish when you want the public version to catch up. Making anything public is owner-only.

    Docs: [Collaboration & Sharing](Collaboration-and-Sharing)

---

## Deployment and compute

??? faq "Where does the computation run — cloud or on-premise?"
    In our cloud, with the option to keep the heavy solver work on your own machines.

    **Self-serve accounts** run on our shared cloud at [protos.arismachina.com](https://protos.arismachina.com){target="_blank"}, with nothing to set up. **Enterprise Pod customers** get a dedicated instance at their own web endpoint (for example `yourcompany.protos.arismachina.com`), gated by your own identity provider, with isolated storage. Your data stays inside your own pod.

    Enterprise Pods are sized to your workload, and compute and storage both expand on request. Contact us to size one.

    Separately, the Local runner execution mode means the solver itself can run on your own hardware while only inputs and results transit the pod. Where IT policy requires the data plane inside your own infrastructure, that hybrid pattern — Protos pod for orchestration, Local runner for solver execution — satisfies it today. A fully customer-hosted deployment is not the standard offer; talk to us if you need one.

    Docs: [Collaboration & Sharing](Collaboration-and-Sharing) · [Models Library](Model-Library)

??? faq "Which connectors are available?"
    MCP, OAuth, GitHub and SharePoint.

    Any MCP-compatible server can be attached under Profile → Connectors → MCP Servers, with four auth modes: OAuth with token refresh handled automatically, API key with a custom header name, custom headers where all values are encrypted at rest, or none. Notion, Linear and Sentry are documented examples. GitHub is supported as a model source via repo URL. Tools are enabled per conversation, so an operator explicitly opts a tool into each session.

    A typical pilot enables SSO against your IdP, SharePoint against nominated document libraries, GitHub for the model runner repo, and MCP only for systems you explicitly name.

    Docs: [MCP Connections](MCP-Connections)

---

## Security and compliance

??? faq "Where is our data hosted?"
    Google Cloud in the EU — `europe-west4`, Eemshaven, Netherlands.

??? faq "Is our environment shared with other customers?"
    It depends on your deployment.

    **Enterprise Pod — no.** You get a dedicated Kubernetes namespace with its own MongoDB, PostgreSQL and Redis; there is no shared database holding several companies' data behind an application permission check. Databases run `ClusterIP`-only and aren't reachable from the public internet, with Kubernetes NetworkPolicies, Shielded VM nodes (Secure Boot on the primary pool), a custom VPC and Cloud NAT for controlled egress around them.

    **Self-serve accounts — shared infrastructure.** These run on our shared multi-tenant cloud at `protos.arismachina.com`. Your organisation's data is logically isolated and access-controlled per organisation, but the underlying databases and compute are shared with other self-serve customers. If you need physical isolation — a dedicated namespace, your own databases and your own endpoint — that's the Enterprise Pod.

??? faq "Is our data encrypted?"
    Yes, always, in both directions. Anything travelling between you and Protos is encrypted in transit, and everything we store is encrypted at rest. It is on by default across the whole platform and there is no setting anyone can forget to switch on.

    **For technical reviewers:** TLS with HSTS in transit, AES-256 at rest. Public traffic terminates TLS at a GCP global HTTPS load balancer with a Google-managed certificate. HSTS (one year), Content-Security-Policy, X-Frame-Options and X-Content-Type-Options are enforced at the web tier. All persistent storage — MongoDB, PostgreSQL, Redis and backups — is AES-256 encrypted by default. Integration credentials get a second application-layer encryption before storage, and platform secrets live in GCP Secret Manager with keyless Workload Identity rather than long-lived service-account keys.

??? faq "Do you train AI models on our data?"
    No. Nothing you put into Protos trains or fine-tunes any model.

    The physics models are simulations with no learned weights, so there is nothing in them to train in the first place. The language models behind the Co-Engineer are called for inference only, on commercial API tiers where the no-training commitment is contractual rather than a setting someone can forget to switch on. Your documents are embedded and stored in our own database. We'll share the data processing agreements with our AI providers on request.

    **For technical reviewers:** domain models are physics simulations on PyBaMM, executed inside Aris GCP/Kubernetes. Knowledge and RAG embeddings are stored in Aris PostgreSQL with pgvector. Customer-supplied model code runs sandboxed under gVisor. Provider-side retention is for abuse monitoring only, not model improvement, and does not depend on any per-conversation toggle: Anthropic currently retains data for up to 30 days for that purpose. Where you need a stricter guarantee, Zero Data Retention — the provider stores nothing at all — can be arranged.

??? faq "Which third parties see our data?"
    Three AI providers, each for a specific job.

    - **Anthropic** runs the Co-Engineer and receives prompts, project and schema context, retrieved knowledge and tool arguments.
    - **OpenAI** handles requirements and specification ingest.
    - **Mistral** does OCR and embeddings, so uploaded document text passes through it.

    If you register your own external model endpoint, inputs go wherever you point it. Everything else stays inside our cloud. The current sub-processor list is available in full on request.

??? faq "How is access controlled inside our environment?"
    An Organization is the top-level workspace. Teams sit inside it and can be nested.

    Three roles: **Owner** has full control — view, edit, run, share and publish. **Editor** can edit and run, and can share within your organisation. **Viewer** has read-only access — can view and run, but cannot edit or share.

    Only an owner can make something public. Sharing an asset additively shares only the Knowledge Library documents it was built from, read-only, never the whole library. Public sharing can be disabled across your whole tenant.

    Docs: [Collaboration & Sharing](Collaboration-and-Sharing)

??? faq "Is there an audit trail?"
    Yes, for changes.

    Version history attributes every change to a user with timestamps and groups consecutive work into sessions.

    This describes the feature rather than conformance to a specific regulatory standard. If you need the audit trail assessed against 21 CFR Part 11, ALCOA+ or similar, talk to us and we'll go through it.

    Docs: [Versioning](Versioning)

??? faq "Are you SOC 2 certified?"
    Not yet. A Type II audit is in progress.

    Google Cloud, which hosts the infrastructure, holds ISO/IEC 27001, 27017 and 27018 and SOC 2 Type II today. If you need a specific compliance position for a procurement process, talk to us and we'll tell you.

??? faq "Are you GDPR compliant?"
    Yes, with a DPA available and SCCs in place.

    Personal data is processed under the GDPR. A Data Processing Agreement is available for enterprise customers. Standard Contractual Clauses under Article 46 apply where processing happens outside the EEA. Confirmed personal data breaches are notified without undue delay and within 72 hours of Aris becoming aware, per Article 33. Every incident gets a post-mortem with corrective actions tracked to completion.

??? faq "Can we review the architecture with our security team?"
    Yes. A dedicated session with your IT security team to walk through the architecture, auth flows and data residency can be arranged before any data moves.

---

## Getting started

??? faq "Is there a free trial?"
    Yes. You can sign up self-serve at [protos.arismachina.com](https://protos.arismachina.com){target="_blank"} and start straight away. No card, no call.

    If you're evaluating Protos for a team, we also run structured pilots on a dedicated Enterprise Pod, with a Forward Deployed Engineer working alongside you to get your models and data in. That's the faster route to knowing whether it works for your problem. Contact us to set one up.

??? faq "How does pricing work?"
    You can subscribe to a Protos account at [protos.arismachina.com](https://protos.arismachina.com){target="_blank"} and see the pricing there.

    Contact us for enterprise deployment and pricing.
