# PM AI Usage & Prompt Library

A practical reference library for applying AI to the end-to-end work of a Senior Technical Program Manager. This repository contains a prompt engineering guide, an AI use-case reference for program management, and a production-ready prompt library covering the full PM lifecycle — from project scoping and planning through execution, stakeholder communication, and risk management.

The prompts in this library are designed to be used with Claude (claude.ai), but the principles and structures translate to other large language model tools.

---

## What This Repository Demonstrates

This library is the product of hands-on AI integration work — applying prompt engineering techniques to real, complex PM workflows rather than toy examples. Specifically, it reflects:

- **Prompt engineering depth** — System prompts are structured using role-based framing, chain-of-thought (precognition) instructions, XML delimiters, structured output templates, and explicit quality guardrails. Each prompt is designed to produce deterministic, reviewable output rather than open-ended AI responses.
- **PM domain breadth** — The library covers the full TPM lifecycle: WBS and backlog generation, OKR drafting and cascade alignment, RACIA governance, risk register construction, PI Planning (SAFe), sprint planning and backlog refinement, meeting notes summarization, weekly status reporting, and executive/stakeholder communications.
- **AI governance and responsible use** — Every prompt separates AI-inferred content from explicitly provided inputs, flags statements requiring human validation before use, and includes guidance on where AI should not substitute for human judgment (e.g., sprint goal-setting, PI commitment votes, final communications review).
- **Practical tooling integration** — Prompts are designed to accept real-world PM tool formats (Jira Excel exports, Confluence/Notion/Google Docs, Outlook email, Slack messages) and produce output compatible with those same tools, reducing friction in actual workflows.
- **Versioned, documented artifacts** — Each prompt file includes usage instructions, output section references, parameter tables, design notes, and a changelog — the same documentation standards expected of any production-grade engineering artifact.

---

## Repository Structure

```
PM-AI-Usage-and-Prompt-Library/
├── AI-Prompt-Engineering-Guide.md       # Reference guide to prompt engineering techniques
├── AI-Use-Cases-for-PMs.md              # AI use-case catalog for all PM types
└── Prompt Library/
    ├── Epics-Stories-WBS_AI-Prompt.md
    ├── OKR-Drafting_AI-Prompt.md
    ├── RACIA-Matrix_AI-Prompt.md
    ├── Risk-Register_AI-Prompt.md
    ├── SAFe-PI-Planning_AI-Prompt.md
    ├── Backlog-Refinement_Sprint-Planning_AI-Prompt.md
    ├── Executive-Stakeholder-Comms_AI-Prompt.md
    ├── Summarize-Organize-Meeting-Notes_AI-Prompt.md
    └── Weekly-Status-Report-Summary_AI-Prompt.md
```

---

## Reference Materials

### AI Prompt Engineering Guide
`AI-Prompt-Engineering-Guide.md`

A concise but thorough guide to the prompt engineering techniques used throughout this library. Written for Claude but applicable to other LLMs. Covers:

| Technique | Description |
|---|---|
| User/Assistant model | How conversational structure affects model behavior |
| System prompts | Establishing context and behavioral rules before user input |
| Role-based prompting | Using role assignment to improve relevance and tone |
| Prompt templates with variables | Building reusable, parameterized prompts |
| XML tag delimiters | Structuring multi-part inputs and outputs |
| Output format control | JSON, structured text, and format-prefilling techniques |
| Precognition (chain-of-thought) | Guiding the model through logical steps before answering |
| One-shot and few-shot examples | Using examples to steer model behavior and output format |
| Hallucination prevention | Granting permission to say "I don't know"; evidence-first reasoning |
| Complex prompt construction | A 10-step framework for building production-grade prompts |

### AI Use Cases for PMs
`AI-Use-Cases-for-PMs.md`

A strategic reference document cataloging AI use cases relevant to Portfolio, Program, Project, and Product Managers across four phases: Scoping, Planning, Execution, and Monitoring. Includes automation opportunities such as requirements intake and analysis, timeline estimation, workflow automation, and predictive risk identification.

---

## Prompt Library

Each prompt below is a production-ready artifact: it includes a system prompt, a user message template, an output section reference, parameter table, design notes, and a changelog. Prompts are designed to be pasted into a Claude Project's instructions for persistent use across sessions.

---

### Work Breakdown Structure — Epics & User Stories
`Prompt Library/Epics-Stories-WBS_AI-Prompt.md`

Generates a structured WBS organized as Epics and User Stories from any combination of project briefs, meeting notes, PRDs, or existing Jira backlogs. All content is derived from inputs — no default activities or Epics are pre-loaded.

**Key design decisions:**
- Three-section output: WBS Summary (portfolio view), WBS Outline (hierarchical navigation), WBS Detail (full descriptions with acceptance criteria)
- Consistent numeric/alphanumeric IDs (Epics: 1, 2, 3 / Stories: 1A, 1B, 2A) cross-referenced across all three sections
- Non-Functional Requirements Epic always included as the final Epic — security, performance, observability, and compliance are treated as first-class scope
- Jira export handling: existing ticket IDs are incorporated; all existing entries are preserved before new ones are added
- Validation section separates inferred scope from explicit scope and surfaces scope gaps

---

### OKR Drafting — Business Function & Team Level
`Prompt Library/OKR-Drafting_AI-Prompt.md`

Generates OKR sets at the Business Function or Team level from strategic inputs, parent OKRs, or existing drafts. Enforces strict Key Result quality rules and supports cascade alignment from company → function → team.

**Key design decisions:**
- Strict Key Result validity: every KR must have a numeric target and measure an outcome, not an activity. Activity-based KRs are automatically reframed and the original is documented in the Health Check.
- High/Medium/Low confidence scoring with mix enforcement — a uniformly High set is flagged as too conservative, a uniformly Low set is flagged as likely unrealistic
- All targets without baseline data carry a `[PM REVIEW]` flag so the team knows exactly what to validate before adopting the set
- Eight-criterion OKR Health Check included with every output
- Cascade alignment: each Objective is mapped to a parent OKR; unmapped Objectives are flagged

---

### RACIA Matrix
`Prompt Library/RACIA-Matrix_AI-Prompt.md`

Generates a fully input-driven RACIA matrix — Responsible, Accountable, Consulted, Informed, and Approval — from project briefs, meeting notes, org descriptions, or existing RACI documents.

**Key design decisions:**
- Fully input-driven: no default activities or roles. Works for any program type or domain.
- Accountability conflict detection: zero-A and multiple-A violations are detected before output is produced and surfaced with recommended resolutions
- Column name hierarchy: individual names → team names → role labels. Generic placeholders are never used.
- N/A signals a deliberate non-assignment rather than an oversight — no blank cells
- "Other Activities to Consider" and "Other Roles to Consider" sections surface governance gaps not visible in the inputs

---

### Program Risk Register
`Prompt Library/Risk-Register_AI-Prompt.md`

Generates a structured risk register using a Likelihood × Impact × Detectability scoring framework (scale of 1–27). Separates identified risks from AI-inferred risks to maintain full transparency about what is known vs. predicted.

**Key design decisions:**
- Three-dimension scoring: Detectability (borrowed from FMEA) distinguishes a high-impact risk that announces itself from one that hits without warning
- Source-critical override: risks explicitly labeled CRITICAL in source notes are always surfaced in the Top Risks section, regardless of calculated score — team-escalated risks are never visually downgraded by the framework
- Register Summary table provides portfolio-level exposure (Critical/Elevated/Monitor × Identified/Inferred) before the detail tables
- Eight risk categories covering Technical, Data, Security & Compliance, Resourcing, Dependency, Timeline & Scope, Operational, and Organizational risks
- Suggested mitigation paths are explicitly labeled as directional recommendations requiring human validation

---

### SAFe PI Planning Preparation — Multi-Team Program Increment
`Prompt Library/SAFe-PI-Planning_AI-Prompt.md`

Generates comprehensive PI Planning preparation materials for a multi-team Agile Release Train (ART) following the Scaled Agile Framework (SAFe). Accepts an Epic and User Story list and produces sprint plans per team, dependency maps, Team PI Objectives, a Program Board, and a risks and impediments register.

**Key design decisions:**
- SAFe-faithful 5+1 sprint structure: five 2-week development sprints plus an Innovation and Planning (IP) sprint. The IP sprint is explicitly reserved for stabilization — the prompt enforces this structurally, one of the most commonly violated SAFe principles.
- Three-tier story classification: Critical Path / Stretchable / Nice-to-Have with sequencing logic applied per tier
- Cross-team dependency buffer: upstream story must complete at least one full sprint before the downstream story starts, building in slippage absorption
- Flexible capacity handling: velocity-adjusted sprint capacity, capacity-only sequencing, or a default (6 people × 65 days = 390 person-days) with explicit flagging of which was applied
- Shared services teams (DevOps, Database, Security) receive capacity-based sequencing with a reactive buffer rather than being forced into a velocity model
- AI-inferred items (team assignments, dependencies, added stories) are marked `⚠️` throughout — nothing is silently assumed
- Designed as a first-draft generator before the event and a refinement tool after teams have added stories

---

### Sprint Planning & Backlog Refinement
`Prompt Library/Backlog-Refinement_Sprint-Planning_AI-Prompt.md`

Two-mode prompt covering Backlog Refinement and Sprint Planning. The two modes are never combined in a single session — Refinement is the prerequisite for Planning.

**Backlog Refinement mode** produces a readiness assessment per Story against five criteria: Description, Acceptance Criteria, Dependencies, Size, and Shared Understanding. Failures produce specific, actionable recommendations rather than generic observations.

**Sprint Planning mode** uses an adjusted sprint capacity calculation (Team capacity in days × Average points per day) rather than raw average velocity, producing a velocity-adjusted ceiling that accounts for holidays and reduced team availability.

**Key design decisions:**
- Five-criterion Story Readiness framework with a Ready/Not Ready verdict per Story and a Refinement Summary table
- Systemic Observations section surfaces patterns across multiple Stories (e.g., "acceptance criteria are consistently missing across all Stories in Epic 3")
- Easy-to-Complete Stories and Critical Path Stories subsections provide two planning levers without reordering the PM's priority list
- The prompt never estimates story points — pointing is a team exercise. Unestimated Stories are flagged and cannot be confidently included in the sprint plan.
- Sprint goal generation is intentionally excluded — sprint goals require team commitment that AI cannot substitute

---

### Executive Summary & Stakeholder Communications
`Prompt Library/Executive-Stakeholder-Comms_AI-Prompt.md`

Four-type prompt covering the most common high-stakes PM communication scenarios. Each type produces draft language calibrated to the specified audience, with inline `[PM REVIEW]` flags on statements requiring validation before sending.

| Type | Name | Audience |
|---|---|---|
| Type 1 | Program Status Communication | Executive, steering committee, board |
| Type 2 | Decision Request | Decision makers — exec or leadership |
| Type 3a | Incident/Issue Communication (Internal) | Leadership, vendor partners |
| Type 3b | Incident/Issue Communication (External) | Customers, external teams |

**Key design decisions:**
- Audience-aware tone inference across six audience profiles (Board/C-suite through Vendor Partner and Customer) — no separate tone parameter needed
- Type 3b exclusion logic: the precognition step explicitly identifies internal details (team names, root cause speculation, infrastructure references) and excludes them before drafting, not as a post-draft edit
- Type 3 is explicitly designed to work with incomplete, rapidly evolving documentation — raw Slack messages, partial incident logs, and unverified facts are all valid inputs
- All outputs include a PM-only Gaps & Flags section listing information gaps — this section must be removed before sending

---

### Summarizing and Organizing Meeting Notes
`Prompt Library/Summarize-Organize-Meeting-Notes_AI-Prompt.md`

Transforms raw meeting notes into a structured summary formatted for distribution via Microsoft Outlook. Produces an Executive Summary, Decisions Made, Decisions Required, Action Items (with inferred owner flagging), Risks, Dependencies, Teams to Consult, and Recommended Distribution — in a single pass.

**Key design decisions:**
- Strict distinction between Decisions Made (discrete choices about scope, architecture, or deployment) and status updates — completed tasks do not qualify as decisions
- Action item owner inference is explicit: inferred owners are flagged with `(inferred)` so the author can verify before sending
- Jira ticket handling: ticket numbers and URLs are both supported; the prompt never invents ticket references
- Output is formatted for Outlook HTML compose mode (bold, bullets, numbered lists) without markdown separators that would render as literal text

---

### Weekly Engineering Status Report
`Prompt Library/Weekly-Status-Report-Summary_AI-Prompt.md`

Transforms raw status call notes into a polished, executive-ready weekly engineering status report. Supports three audience levels, two output formats, and produces per-team RAG status with inferred status flagging.

**Key design decisions:**
- Audience-aware depth adjustment: C-suite/Board output minimizes technical detail and focuses on business impact; Engineering leads output preserves full technical specifics
- RAG status inference with explicit flagging: `*(status inferred)*` is appended whenever status is derived from context rather than stated outright
- Cross-Team Dependencies & Escalations section is produced as a standalone section — items blocking multiple teams are surfaced separately, not buried in individual team tables
- Output format branching: checkbox format (`[ ]`) for Confluence/Notion/Google Docs; standard bullets for Outlook compatibility

---

## Prompt Engineering Approach

The prompts in this library follow a consistent structure informed by the prompt engineering principles documented in `AI-Prompt-Engineering-Guide.md`:

1. **Role assignment** — Every system prompt opens with a specific expert persona (e.g., "Technical Program Manager with 15+ years of experience at high-growth B2B SaaS companies") to anchor the model's knowledge and tone.
2. **Precognition (chain-of-thought)** — Each prompt includes a numbered list of silent reasoning steps the model must work through before producing output, reducing shortcut reasoning on complex tasks.
3. **XML input delimiters** — User-provided inputs (`<context>`, `<input_materials>`, `<backlog>`, etc.) are enclosed in XML tags to prevent formatting issues in pasted content from corrupting prompt instructions.
4. **Explicit output templates** — Every prompt specifies the exact output structure, section names, and table formats. The model fills in the template rather than inventing its own structure.
5. **Transparency guardrails** — AI-inferred content is always separated from explicitly provided content and flagged for human review. The model is instructed to surface gaps and unknowns rather than silently fill them.
6. **Preamble suppression** — Production prompts suppress introductory commentary so output begins directly with the requested artifact.

---

## Getting Started

1. Open [claude.ai](https://claude.ai) and create a new Project (recommended for persistent system prompts)
2. Paste the **System Prompt** section from any prompt file into the Project's instructions
3. Start a new conversation and send the **User Message** section with your inputs filled in
4. Review the output against the **Output Sections Reference** table in the prompt file

Each prompt file includes a **Usage** section with step-by-step instructions specific to that prompt's workflow.
