# Frequently Asked Questions

Common questions about Protos, grouped by topic.

---

## About Protos

??? faq "What is Protos?"
    An AI-native co-engineering workspace for industrial science and R&D.

    You describe what you're working on as structured data (schemas), connect models and reference material to it, then assemble and run the whole thing on a visual canvas.

    The Co-Engineer works alongside you throughout. It builds schemas, pulls data out of your documents, and writes the code behind the calculation components on your canvas. What it doesn't do is hand you the answer directly. The result comes from executing that code, and you can open the code, the inputs, and the source document behind any value in it.

    Docs: [Schemas](Schemas) · [Simulation Studio](Simulation-Studio)

??? faq "How is this different from using ChatGPT or Claude on our data?"
    A chat session gives you the model and nothing else. You paste in your context, get an answer back, and when the window closes the structure of the work goes with it.

    In Protos the model has somewhere to put things. It writes the calculation components, pulls structured data out of your documents and wires up canvases, and what it builds stays: your data in schemas, your models registered and ready to run.

    Three things that come from the workspace rather than the model:

    - **Traceability.** Every value is held against the document it came from, so a result can be audited.
    - **Collaboration.** Work is shared across the team with role-based access, instead of sitting in one person's session.
    - **Versioning.** Schemas, canvases and models are versioned, so every change is attributed and you can compare or roll back.

    The number itself comes from running those components, not from the model predicting it. You're still trusting the calculation, the same as code you wrote yourself. What you can do is open it.

    The AI isn't doing the engineering. It's making everything around it quicker to set up.

    Docs: [Simulation Studio](Simulation-Studio) · [Versioning](Versioning)

??? faq "Do I need to be a simulation engineer to use it?"
    No, but you need to know your domain. The Co-Engineer builds schemas, populates data documents from your files and assembles canvases, so most of the time you're reviewing structure rather than writing it. Reading a simulation result and judging whether it's sensible is still your job.

    Docs: [Co-Engineer](Co-engineer)

??? faq "What models ship with it?"
    Two kinds. Seven first-party physics models that we build and maintain (deterministic, PyBaMM-based, no learned weights), plus a public catalog you can browse and add from.

    By domain:

    - **Battery & cell design** (first-party physics): Cell Performance, Cell Optimizer, DFN Calendar Ageing, DFN Cyclic Ageing, SPMeT Power, SPMeT DCIR, SPMeT Dynamic Load
    - **Protein structure & design**: AlphaFold2, AlphaFold2-Multimer, OpenFold2, OpenFold3, Boltz2, ESM3 / ESMC (Biohub: fold, inverse-fold, generate, sample, encode/decode), ProteinMPNN, RFdiffusion, ColabFold MSA Search
    - **Drug discovery & chemistry**: DiffDock (docking), GenMol, MolMIM (molecular generation & optimization)
    - **Genomics**: Evo2 40B (DNA/RNA sequence generation)
    - **Logistics & operations**: NVIDIA cuOpt (vehicle routing)
    - **General-purpose AI**: Google Gemini Interactions (chat, tools, document understanding, image / TTS / music, deep research)

    The battery models are deterministic simulations, not trained models. Several of the catalog models are trained or foundation models: folding, generative chemistry, genomics, Gemini. Many run GPU-accelerated on NVIDIA NIM. You can also register your own model from a script, a repo or an existing API endpoint.

    Docs: [Models Library](Model-Library)

---

## Your models and simulations

??? faq "How are models executed, and where do they run?"
    You choose per model. It can run on our cloud, on your own machine, or behind an API you already host.

    Registering a model takes one of three intake options: **Local code** (upload a script), **Endpoint** (point at an existing API, with optional auth), or **Version control** (a public GitHub / GitLab / Bitbucket / Codeberg repo URL). Each registered model then runs one of three ways.

    - **Local runner.** The model runs on your own hardware, with your existing licences and toolboxes. Protos dispatches inputs and collects results, so there's no licence migration and your model code and data don't leave your network. MATLAB and COMSOL are supported as Local runner runtimes.
    - **Cloud.** You upload Python code, or point at a public repo, and Protos containerises and runs it on our infrastructure. Nothing for you to maintain.
    - **Endpoint.** Protos calls an API you already host.

    In every case the contract with Protos is a JSON input schema and a JSON output schema, not the solver file itself.

    **Licensed tools (MATLAB, COMSOL, Simulink, and so on)** run only via the Local runner or your own Endpoint, on a machine that already holds the licences. They aren't available in Cloud execution, and Aris Machina never ships, hosts or resells them. For Simulink you have three options: keep it local, wrap it behind REST (MATLAB Production Server / Compiler SDK), or export a co-simulation FMU and register a short Python runner as a Cloud model.

    One thing to expect when running a Local runner. Aris Machina isn't currently enrolled in the Apple Developer Program or the Microsoft equivalent, so the Local runner executable isn't OS-code-signed. The first time you launch it, your operating system will flag it as coming from an unidentified developer and you'll need to explicitly allow it to run.

    Docs: [Models Library](Model-Library)

??? faq "Are MATLAB toolbox dependencies supported, and are MATLAB licences required?"
    Toolboxes are supported, and licences stay with you. Aris Machina neither requires nor resells MATLAB entitlements.

    Toolbox support is a property of the execution environment rather than of Protos. Whatever MATLAB, Simscape or Powertrain Blockset licences the executing machine holds are the toolboxes available to the model. In Local runner and Endpoint modes that machine is yours, so your toolbox stack is available unchanged.

    The FMU route works differently. A co-simulation FMU is intended to embed the compiled solver, so it should execute without consuming a MATLAB licence at run time. Whether that holds for a given model depends on it being code-generation compatible. Simulink Compiler is needed to produce the FMU, and Simscape-derived FMUs carry MathWorks redistribution terms worth confirming with your legal team.

    Docs: [Models Library](Model-Library)

??? faq "How do we register a compiled or encapsulated model?"
    Five steps, and the Co-Engineer can do most of it for you.

    1. Models Library → Register Model.
    2. Choose the source: Local code (needs Name, Key, Description), Endpoint (URL plus auth), or Version control (repo URL).
    3. Define the input and output schemas. The JSON schema builders let you specify each parameter's name, type, and whether it's required.
    4. For repo-based models, Protos builds a container: *Preparing build context → Building image → Finalising*. This takes a few minutes.
    5. Drop the model onto a Simulation Studio canvas as a model node and wire up its inputs.

    You can skip most of step 3 by uploading a Python script or pointing the Co-Engineer at a repo and asking it to register the model. It will infer the input and output schemas.

    For an encapsulated Simulink model, register the thin Python runner (FMPy loading the FMU, or a client calling your MATLAB Production Server) rather than the .slx. Put that runner in a repo, let the Co-Engineer infer the schema, then hand-check the input schema against your parameter naming and units. The documented path uses a public repo URL. Private repos can be connected via a deploy token or mirror during onboarding.

    Docs: [Models Library](Model-Library)

??? faq "Can simulation criteria be modelled as structured requirements or KPIs, and linked to results?"
    Yes. Schemas define the structure of your engineering data with typed fields (string, number with units, boolean, enum, date) and Ref fields that link across schemas. In Data Studio, Design documents hold single values and Requirement documents hold min/max ranges, so a KPI corridor is a first-class object. Simulation Studio can pull schema values directly as simulation inputs, and the Co-Engineer will parse an uploaded spec document into structured targets and constraints, then compare design variants against those requirements and flag gaps.

    Any value can be followed back through its chain of sources to the original reference.

    One limit worth knowing: the comparison surfaces gaps through the Co-Engineer. It doesn't emit a formal signed verification matrix.

    Docs: [Schemas](Schemas) · [Data Studio](Data-Studio) · [Co-Engineer](Co-engineer)

??? faq "Can it handle batch runs and parameter sweeps (DoE)?"
    Not yet. Both are in development.

---

## Data and traceability

??? faq "How is data input handled, and which formats are supported?"
    Four routes in, covering documents, structured data, model I/O and live parameters.

    - **Knowledge Library**, for unstructured and reference material. PDF, DOCX, XLSX, CSV, TXT, Markdown, JSON and common image formats, up to 100 MB per file. Upload singly, as knowledge notes, or as bulk folders up to 500 files or 100 MB per batch. Content is parsed and chunked for the Co-Engineer.
    - **Data Studio**, for structured data. Data documents conforming to your schemas. The Co-Engineer can extract structured data from an uploaded file and create a data document following your schema. Spec and requirements ingest accepts files up to 32 MB.
    - **Canvas.** Parameter nodes and data input nodes feed simulations directly, wired up from data you enter yourself or that the Co-Engineer finds for you.
    - **Models.** JSON in, JSON out, against the schema you define at registration.

    For test and load-profile data, CSV or XLSX into Data Studio against a schema keeps every value traceable back to its source file. If a measurement file exceeds the document cap, pass it by reference to a Local runner or Endpoint model so the bulk data never has to move.

    Docs: [Knowledge Library](Knowledge-Library) · [Data Studio](Data-Studio)

??? faq "How do we know where a number came from?"
    Every value traces back to its source document, permanently.

    A value links through the knowledge chunk that produced it to the original source document, and each document shows every schema, data document, model and canvas built from it. The chain persists after people leave and projects are archived.

    Docs: [Data Studio](Data-Studio) · [Knowledge Library](Knowledge-Library)

??? faq "Can we roll back a mistake?"
    Yes, non-destructively and with attribution. Schemas, canvases, models and knowledge documents are versioned, changes are attributed to whoever made them, and consecutive work is grouped into sessions. Restoring writes a new version applying the old content rather than erasing what happened in between.

    You can also label versions, compare any two side by side, and ask the Co-Engineer what changed in plain language.

    Docs: [Versioning](Versioning)

??? faq "Can we show results to people without a Protos account?"
    Yes, by publishing a canvas to a public URL. Whoever opens the publication can change input parameters and re-run the canvas themselves.

    External viewers can't reach your Python code, your other projects, or your Knowledge Library, and you can block external model execution to avoid API charges. Publications are snapshots, so you republish when you want the public version to catch up. Only an owner can make something public.

    Docs: [Collaboration & Sharing](Collaboration-and-Sharing)

---

## Deployment and compute

??? faq "Where does the computation run, cloud or on-premise?"
    In our cloud, though you can keep the heavy solver work on your own machines.

    Self-serve accounts run on our shared cloud at [protos.arismachina.com](https://protos.arismachina.com){target="_blank"}. Enterprise Pod customers get a dedicated instance at their own web endpoint (for example `yourcompany.protos.arismachina.com`), gated by your own identity provider, with isolated storage. Your data stays inside your own pod. Pods are sized to your workload, and compute and storage both expand on request. Contact us to size one.

    The Local runner execution mode is separate from all that. It means the solver itself runs on your own hardware, with only inputs and results transiting the pod. Where IT policy requires the data plane inside your own infrastructure, that hybrid pattern meets it today: Protos pod for orchestration, Local runner for solver execution. A fully customer-hosted deployment isn't the standard offer, so talk to us if you need one.

    Docs: [Collaboration & Sharing](Collaboration-and-Sharing) · [Models Library](Model-Library)

??? faq "Which connectors are available?"
    MCP, OAuth, GitHub and SharePoint.

    Any MCP-compatible server can be attached under Profile → Connectors → MCP Servers, with four auth modes: OAuth with token refresh handled automatically, API key with a custom header name, custom headers where all values are encrypted at rest, or none. Notion, Linear and Sentry are documented examples. GitHub is supported as a model source via repo URL. Tools are enabled per conversation, so an operator explicitly opts a tool into each session.

    A typical pilot enables SSO against your IdP, SharePoint against nominated document libraries, GitHub for the model runner repo, and MCP only for systems you explicitly name.

    Docs: [MCP Connections](MCP-Connections)

---

## Security and compliance

??? faq "Where is our data hosted?"
    Google Cloud in the EU, in `europe-west4` (Eemshaven, Netherlands).

??? faq "Is our environment shared with other customers?"
    It depends on your deployment.

    **Enterprise Pod: no.** You get a dedicated Kubernetes namespace with its own MongoDB, PostgreSQL and Redis. There is no shared database holding several companies' data behind an application permission check. Databases run `ClusterIP`-only and aren't reachable from the public internet, with Kubernetes NetworkPolicies, Shielded VM nodes (Secure Boot on the primary pool), a custom VPC and Cloud NAT for controlled egress around them.

    **Self-serve accounts: shared infrastructure.** These run on our shared multi-tenant cloud at `protos.arismachina.com`. Your organisation's data is logically isolated and access-controlled per organisation, but the underlying databases and compute are shared with other self-serve customers. If you need physical isolation, meaning a dedicated namespace, your own databases and your own endpoint, that's the Enterprise Pod.

??? faq "Is our data encrypted?"
    Yes, in transit and at rest. It's on by default across the whole platform and there's no setting anyone can forget to switch on.

    **For technical reviewers:** TLS with HSTS in transit, AES-256 at rest. Public traffic terminates TLS at a GCP global HTTPS load balancer with a Google-managed certificate. HSTS (one year), Content-Security-Policy, X-Frame-Options and X-Content-Type-Options are enforced at the web tier. All persistent storage is AES-256 encrypted by default, including MongoDB, PostgreSQL, Redis and backups. Integration credentials get a second application-layer encryption before storage, and platform secrets live in GCP Secret Manager with keyless Workload Identity rather than long-lived service-account keys.

??? faq "Do you train AI models on our data?"
    No. Nothing you put into Protos trains or fine-tunes any model.

    The physics models are simulations with no learned weights, so there's nothing in them to train. The language models behind the Co-Engineer are called for inference only, on commercial API tiers where the no-training commitment is contractual rather than a setting someone can forget to switch on. Your documents are embedded and stored in our own database. We'll share the data processing agreements with our AI providers on request.

    **For technical reviewers:** Domain models are physics simulations on PyBaMM, executed inside Aris GCP/Kubernetes. Knowledge and RAG embeddings are stored in Aris PostgreSQL with pgvector. Customer-supplied model code runs sandboxed under gVisor. Provider-side retention is for abuse monitoring only, not model improvement, and doesn't depend on any per-conversation toggle. Anthropic currently retains data for up to 30 days for that purpose. Where you need a stricter guarantee, Zero Data Retention can be arranged, meaning the provider stores nothing at all.

??? faq "Which third parties see our data?"
    Three AI providers, each for a specific job.

    - **Anthropic** runs the Co-Engineer and receives prompts, project and schema context, retrieved knowledge and tool arguments.
    - **OpenAI** handles requirements and specification ingest.
    - **Mistral** does OCR and embeddings, so uploaded document text passes through it.

    If you register your own external model endpoint, inputs go wherever you point it. Everything else stays inside our cloud. The current sub-processor list is available in full on request.

??? faq "How is access controlled inside our environment?"
    An Organization is the top-level workspace. Teams sit inside it and can be nested.

    There are three roles. **Owner** has full control: view, edit, run, share and publish. **Editor** can edit and run, and can share within your organisation. **Viewer** is read-only, able to view and run but not to edit or share.

    Only an owner can make something public. Sharing an asset additively shares only the Knowledge Library documents it was built from, read-only, never the whole library. Public sharing can be disabled across your whole tenant.

    Docs: [Collaboration & Sharing](Collaboration-and-Sharing)

??? faq "Is there an audit trail?"
    Yes, for changes. Version history attributes every change to a user with timestamps and groups consecutive work into sessions.

    That describes the feature, not conformance to a specific regulatory standard. If you need the audit trail assessed against 21 CFR Part 11, ALCOA+ or similar, talk to us and we'll go through it.

    Docs: [Versioning](Versioning)

??? faq "Are you SOC 2 certified?"
    Not yet. A Type II audit is in progress. Google Cloud, which hosts the infrastructure, holds ISO/IEC 27001, 27017 and 27018 and SOC 2 Type II today.

    If you need a specific compliance position for a procurement process, ask us and we'll tell you where we stand.

??? faq "Are you GDPR compliant?"
    Yes, with a DPA available and SCCs in place.

    Personal data is processed under the GDPR. A Data Processing Agreement is available for enterprise customers. Standard Contractual Clauses under Article 46 apply where processing happens outside the EEA. Confirmed personal data breaches are notified without undue delay and within 72 hours of Aris becoming aware, per Article 33. Every incident gets a post-mortem with corrective actions tracked to completion.

??? faq "Can we review the architecture with our security team?"
    Yes. We can set up a session with your IT security team to walk through the architecture, auth flows and data residency before any data moves.

---

## Getting started

??? faq "Is there a free trial?"
    Yes. Sign up self-serve at [protos.arismachina.com](https://protos.arismachina.com){target="_blank"} and start straight away. No card, no call.

    If you're evaluating Protos for a team, we also run structured pilots on a dedicated Enterprise Pod, with a Forward Deployed Engineer working alongside you to get your models and data in. That's usually the quicker way to find out whether it fits your problem. Contact us to set one up.

??? faq "How does pricing work?"
    Self-serve pricing is listed at [protos.arismachina.com](https://protos.arismachina.com){target="_blank"} and you can subscribe there. Enterprise deployment is priced case by case, so contact us.
