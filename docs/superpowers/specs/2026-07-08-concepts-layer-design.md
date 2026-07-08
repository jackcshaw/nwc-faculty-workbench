# Workbench Concepts Layer — Design

Date: 2026-07-08
Status: Approved in brainstorm; awaiting implementation plan.
Owner: Jack Shaw

## Context

The workbench now has three layers: **do** (templates), **what** (the framework doc), and **where do I start** (diagnostic + README). It lacks the **why** layer — the design rationale connecting unfamiliar artifact types to things users already know. A faculty member at Phase 1–2 hits a wall of confident vocabulary (facilitation block, method card, source kit) with no bridge; an AI-fluent reader can't tell translation-with-governance from naive reinvention; and the AI assistant facilitating from the bundle confabulates when asked "why am I filling this out?"

The seed example: the method-card-vs-agent-skill explanation. A method card is what a SKILL.md is compiled *from* — harness-neutral on purpose, with governance fields (evidence gate, review points, revision log) that deployment formats drop. That explanation does three jobs at once: locates the unfamiliar against the familiar, justifies the design, and teaches a framework piece. Every concept note must do all three.

## Goals

- Give both audiences (faculty, AI-fluent readers) the "you already understand this" bridge.
- Meet the workbench's own standard: make the design reasoning visible.
- Ground the AI assistant so bundle-driven facilitation can answer *why* questions faithfully.
- Give pilot learnings a home in the why-layer (notes grow from real faculty questions).

## Non-goals

- No new site page; the repo and bundle carry the layer.
- No essay-argument content: the essay owns *why judgment must stay human*; concepts own *why this artifact has these fields*.
- No anticipatory notes: held-back topics wait for pilot demand.

## Scope: index + five notes

New `concepts/` directory:

| File | Serves | Industry twin | Framework tie |
| --- | --- | --- | --- |
| `README.md` (index) | Everything | — | The bridge itself |
| `method-cards-and-agent-skills.md` | Method card template | Agent skill / SKILL.md | Phase 5; knowledge outlasts the tool |
| `facilitation-blocks.md` | All nine templates | System prompt / agent instructions | Supervised AI-mediated work; assistant as runtime |
| `why-the-matrix-is-a-hypothesis.md` | Framework doc; after-action + calibration matrix checks | Eval-gated claims | Reliance calibration, modeled by the workbench itself |
| `source-kits-are-curated-context.md` | Source kit template | Curated context / RAG corpus | Phase 5–6; Level 6→8 lineage |
| `how-faculty-judgment-compounds.md` | After-action note + calibration protocol | Eval sets; inter-rater reliability | The staircase: reviewed practice → shared assets → evaluation |

**Index contents**, in order: the vocabulary bridge table (each row links artifact template ↔ concept note ↔ industry twin), a short "how to read these" paragraph, and a **backlog section** naming the two held-back notes (tool-chooser "what remains when the work is done"; traces/oral-defense "why not prompt logs") with the pull principle stated: *new concept notes get written when pilot questions ask for them, not before.*

## Note anatomy — scannability is the point

A faculty member decides in ten seconds whether a note earns their next two minutes. Every note, same shape, 300–500 words:

1. **H1 title**, then a **one-line bolded summary** (the whole note in one sentence).
2. `## What It Is` — two or three short sentences; names and links the template it serves in the first two sentences.
3. `## The Industry Twin` — what AI-fluent readers know this as.
4. `## What The Workbench Adds` — the design decisions and why (bullets welcome: governance fields, evidence gates, boundaries).
5. `## Where You'll Use It` — one concrete faculty moment.
6. Closing **framework line** — a single sentence, set off on its own (bold or blockquote), tying the concept to a phase or staircase step.

Formatting rules: paragraphs of two or three sentences maximum; bullets over prose for enumerations; no section longer than ~100 words; workbench voice (short declarative sentences, Title Case H2s).

## Integration points (deliberately light touch)

1. **Served templates** get one line under their intro: `Concept: [why this template works the way it does](../concepts/<note>.md)` — on the method card, source kit, after-action note, and calibration protocol (the last two both link `how-faculty-judgment-compounds.md`; the after-action note and calibration protocol matrix-check sections also reference `why-the-matrix-is-a-hypothesis.md` via the existing framework link — no extra edit needed there).
2. **Workbench source kit** links `facilitation-blocks.md` (it serves all templates, so it's anchored at the kit rather than in nine places) and adds `concepts/` to anchor materials.
2a. **Framework doc**: the matrix section's "Read the status legend first" paragraph gains a link to `concepts/why-the-matrix-is-a-hypothesis.md` — the matrix note's direct anchor.
3. **README** Start Here table gains one row: "Understand the design behind the tools → concepts/README.md".
4. **AGENTS.md** gains one bullet: use the concepts when the faculty member asks why an artifact works the way it does.
5. **Bundle**: new `SECTION: CONCEPTS` (index + five notes, concatenated in index order) placed immediately after `SECTION: FRAMEWORK` in `buildWorkbenchContext()`. Adds ~12–15KB; ceiling is 150KB.
6. **Site**: no page changes.

## Boundary guards

- Every note names its served template in its first two sentences, or it doesn't ship.
- No note argues the essay's thesis; operational rationale only.
- The matrix note repeats the exact string `Hypothesis — awaiting NWC validation` (it is a contract-test needle).
- Concepts describe current markdown practice; future-layer claims stay in the roadmap.

## Testing

- **Site contract tests**: bundle contains `SECTION: CONCEPTS` plus one distinctive needle per note (five needles + index bridge-table needle).
- **Workbench sweep**: all concept links resolve in both directions (template → note, note → template, index → both).
- **Scannability check** (manual, at review): each note passes the ten-second test — title + bold summary + section headings convey the gist without reading body text.

## Repos affected

- **workbench** (primary): `concepts/` + link edits to four templates, source kit, README, AGENTS.md.
- **site**: `buildWorkbenchContext()` section list + contract-test needles. Merge order: workbench PR first, site PR second (same dependency as before).
