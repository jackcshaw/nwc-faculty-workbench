# Operationalizing the AI Fluency Framework in the Faculty Workbench — Design

Date: 2026-07-08
Status: Implemented 2026-07-08 (workbench branch ai-fluency-operationalization, site branch workbench-ai-integration).
Owner: Jack Shaw

## Context

The **Building AI Fluency** package (deck + four visuals, July 2026) presents a framework for integrating AI fluency into NDU/NWC operations:

- **Six phases**: Ask (Responsible Use) → Understand (AI for Learning) → Produce (Work Products) → Judge (Judgment/Revision) → Codify (Repeatable Practice) → Supervise (Supervised Systems). Scale begins at Phase 5.
- **Tool progression**: five system classes judged by what remains when the work is done — a chatbot leaves understanding, an agent leaves a work product, a skill leaves a method, an agent team leaves the harness, the knowledge layer leaves compounded knowledge.
- **Compounding staircase**: reviewed practice → shared assets → evaluation → governed systems → an AI-native institutional knowledge layer.
- **Reference matrix**: the full competency map — six phases × three personas (Learners, Faculty, Institution).

The framework is grounded in a deep research report on adult AI fluency (2024–2026 literature) and borrows vocabulary from OpenAI's Codex usage study (the asking→acting shift; the unit of work moving from a conversation to a delegated, reviewable workflow).

The **NWC Faculty Workbench** is a public-safe markdown toolkit organized around the *Irreducible Officer* spine (purpose, frame, reliance, accountability, transfer), with six ready-now templates and eight maturity levels. Its existing templates concentrate at framework Phases 2–4; Phases 5–6 and the Institution row are its thinnest coverage.

### Key judgments shaping this design

1. **Epistemic status is uneven.** The six-phase progression, the tool progression, and the compounding staircase are considered solid. The **matrix cells** (what each persona does at each phase) are the part that most needs NDU/NWC validation. The workbench therefore operationalizes the solid parts and *instruments* the matrix for validation — it does not implement the matrix as settled doctrine.
2. **Validation path**: not yet settled, most likely faculty pilot use. Artifacts must be fully self-serve.
3. **Success criterion (6 months)**: workbench adopted and re-used in live courses. Matrix evidence is a byproduct of use, never a burden on faculty.
4. **Dogfooding**: the workbench should demonstrate the framework by being itself AI-integrated — faculty's first contact with the progression is *experiencing* supervised, AI-mediated work with judgment kept human.

## Goals

- Turn the framework's solid parts into faculty-usable, self-serve artifacts.
- Fill the workbench's Phase 5–6 gap (Codify, Supervise) — where the framework says scale begins.
- Make every artifact runnable by any AI assistant, with zero install and no vendor dependency.
- Capture matrix-validation evidence as a near-free byproduct of routine use.
- Keep the Irreducible Officer spine primary; the phases are the on-ramp and navigation, not a replacement spine.

## Non-goals

- No restructuring of the workbench around the six phases (rejected: locks in the unvalidated matrix, dilutes the workbench's distinctive judgment-preservation identity).
- No app, web form, dashboard, database, or proposal queue (boundaries doc; Level 7/8 remain future).
- No private NWC course material in the public repo.
- No prompt-log compliance regime; facilitation stays facilitation, not surveillance.

## Design principles

1. **The AI assistant is the runtime; markdown is the program; the site is the distribution layer.** Interactivity comes from the faculty member's own assistant, not from built UI.
2. **Anchor on what is solid.** Artifacts organize around phases, tools, and the staircase. Matrix cells are hypotheses.
3. **Matrix cells carry validation status.** Every persona row/cell in the published matrix is marked `field-tested` or `hypothesis — awaiting NWC validation`, with a changelog as evidence arrives.
4. **Every visual has a markdown twin.** Each image ships alongside an equivalent markdown table or structured text so AI assistants can consume what humans see.
5. **Dual-reader artifacts.** Every template is designed for a faculty member and for the assistant facilitating them, in one markdown file.
6. **Feedback is nearly free.** Matrix checks are 3–4 lines inside instruments faculty already fill out.
7. **Tool-agnostic.** Plain text that works in ChatGPT, Claude, Gemini, Copilot, or whatever government IT permits.

## Architecture: the AI-runnable workbench

Every workbench artifact gains a **facilitation block** — a clearly marked section instructing any AI assistant how to run the artifact as a guided working session. A facilitation block specifies:

- **Role**: e.g., "You are facilitating an assignment-design session. The faculty member owns every judgment; you ask, structure, and challenge — you never fill in pedagogical decisions for them."
- **Inputs to collect**: course, learning objective, existing assignment if revising.
- **Process**: walk the sections in order, one at a time; push back on vague answers; flag where AI-free work should be preserved.
- **Stop rules**: what the assistant must not do (write the assignment itself, soften developmental friction, invent doctrine, proceed past unresolved judgment calls).
- **Output**: the completed worksheet as a markdown artifact the faculty member keeps.

Faculty experience: *open the template → give it to your assistant → the assistant interviews you through it → you leave with a completed, reviewable artifact.*

This is dogfooding in the precise sense: a template-with-facilitation-block **is** a Phase 5 method card (role, inputs, steps, review criteria, stop rules), and packaging the workbench for assistant navigation **is** a Level 6 source kit applied to the workbench itself.

## Artifact inventory

### New artifacts

| # | Artifact | Path (workbench repo) | Purpose |
| --- | --- | --- | --- |
| 1 | Framework reference | `framework/ai-fluency-progression.md` | Public-safe markdown rendition of the deck: six phases, tool progression, staircase, full matrix with validation-status markers and changelog. Embeds the four visuals with markdown twins. |
| 2 | Crosswalk | inside `framework/ai-fluency-progression.md` | Maps the six existing templates and eight maturity levels onto phases and matrix cells; makes the staircase ↔ Levels 5–8 correspondence explicit. One table answers "where am I, and what do I use?" |
| 3 | Phase placement diagnostic | `templates/phase-placement-diagnostic.md` | The front door. An assistant interviews the faculty member (~10 minutes) and places their assignment or course at a phase, then routes to the right templates. Also usable as a plain worksheet. |
| 4 | Method card template | `templates/method-card-template.md` | Phase 5. Turn a recurring AI-enabled task into a reusable method: task brief, inputs, steps, review criteria, stop rules, one good and one bad example. Raw material for source kits. |
| 5 | Supervised delegation exercise | `templates/supervised-delegation-exercise.md` | Phase 6. Design a bounded multi-step exercise where students direct AI: task briefs, intermediate-inspection points, stop/escalation rules, and assessment of whether judgment survives delegation. |
| 6 | Workbench source kit | `workbench-source-kit.md` (repo root) | The workbench packaged with its own source-kit template: tells any assistant what this toolkit is, what sources matter, what it should and should not do when facilitating. First worked example of Level 6. |

### Modified artifacts

| # | Artifact | Change |
| --- | --- | --- |
| 7 | All six existing templates | Add facilitation block (purely additive section). |
| 8 | After-action note; faculty calibration protocol | Add 3–4-line **matrix check**: which phase were you operating at, did the persona-row expectations hold, what would you change in that cell. |
| 9 | `README.md` | "Start here" gains the AI-assisted path (point your assistant at the source kit, run the diagnostic). Start Here table gains a phase column. Framework doc referenced. |

### Faculty journey (the compounding loop, enacted)

Diagnostic places you → crosswalk routes you → template runs as a facilitated session → after-action note captures what happened (matrix evidence for free) → calibration compares across faculty → method cards codify what works → source kits accumulate. This chain is the staircase: reviewed practice → shared assets → evaluation → governed reuse.

## Infrastructure

### Tier 1 — build as part of this work

1. **Visual assets pipeline.** Canonical home for the four framework visuals at `framework/assets/` (SVG source of truth; PNG export for decks). Embedded in the framework doc; every visual paired with its markdown twin (matrix as a real markdown table; progression as structured text).
2. **Workbench AI bundle.** Extend the site build (`site/scripts/build-site.mjs`) to emit `workbench-context.md` alongside the existing `companion-context.md`: framework doc + crosswalk + diagnostic + templates concatenated with section markers, served at a stable URL. One paste into any assistant.
3. **Site workbench mode refresh.** Diagnostic becomes the entry card; progression visual appears on the workbench page; new templates flow through the existing per-tool asset generation.

### Tier 2 — habits, not code

4. **Facilitation-block test transcripts.** Before a template ships, run it through at least two different assistants; keep one good transcript per template in the repo (e.g., `examples/transcripts/`) as a worked example. This is the staircase's "assets make evaluation possible" step.
5. **Feedback return path.** The after-action note gains a "send this to..." instruction (email at first). Deliberately manual; proposal queues are Level 8.

### Tier 3 — explicitly deferred

Interactive web diagnostic, dashboards, proposal queues, anything with a database. Under the assistant-as-runtime principle, the web diagnostic may never be needed.

## Build order

1. Framework doc + visuals + crosswalk (everything else references them).
2. Phase placement diagnostic + facilitation blocks on the two highest-traffic templates (assignment design worksheet, assessment/oral-defense rubric).
3. Method card template + supervised delegation exercise template.
4. Workbench source kit + AI bundle + site integration.
5. Remaining facilitation blocks + test transcripts + README refresh.

Each step leaves the workbench shippable; a pilot could begin after step 2.

## Repos affected

- **workbench** (primary): all artifacts above.
- **site**: bundle generation, workbench mode refresh, contract-test updates.
- **companion**: untouched. The workbench's AI integration facilitates faculty design work; the companion practices against the essay. Different purposes, per the ecosystem boundaries doc.

## Error handling and risks

| Risk | Mitigation |
| --- | --- |
| Two organizing schemes (maturity levels + phases) confuse users | Crosswalk is the single mapping; diagnostic makes phases the way *in* and templates the thing you *use*. Faculty never navigate both schemes at once. |
| Facilitation blocks behave differently across assistants | Conservative, explicit instructions; test transcripts from 2+ assistants per template before shipping. |
| Framework doc drifts from the deck | Framework doc is the canonical public rendition; the deck cites it. Changelog records matrix revisions. |
| Matrix checks become compliance theater | Keep to 3–4 lines, optional fields, paired with oral defense and calibration per existing guardrails. |
| Public repo absorbs private pilot material | Existing rule holds: templates, patterns, synthetic or approved examples only. Transcripts scrubbed before commit. |

## Testing

- **Site contract tests** extended to assert `workbench-context.md` exists, includes all expected sections, and stays within a sane size budget.
- **Template transcript review**: each facilitation block validated against 2+ assistants; transcript committed as the worked example.
- **Cold self-serve test**: someone unfamiliar with the project runs the diagnostic with their own assistant, unaided. Friction observed is friction fixed.

## Open questions

- Which email/return path for after-action notes (decide at implementation; placeholder is Jack's contact).
- Whether the framework doc also gets a mirror on the site as a readable page (lean yes — cheap, aids adoption; confirm during site integration).
