# Workbench Concepts Layer — Design

Date: 2026-07-08 (revised 2026-07-09 after structural review and site usefulness review)
Status: Approved; implementation plan at `../plans/2026-07-08-concepts-layer.md`.
Owner: Jack Shaw

## Context

The workbench has three layers: **do** (templates), **what** (the framework doc), and **where do I start** (diagnostic + README). It lacks the **why** layer — the design rationale connecting unfamiliar artifact types to things users already know. A faculty member at Phase 1–2 hits a wall of confident vocabulary (facilitation block, method card, source kit) with no bridge; and the AI assistant facilitating from the bundle confabulates when asked "why am I filling this out?"

**Audience (settled 2026-07-09):** one audience, one voice. The evaluator vetting this work at judgmentlab.net *is* a faculty member or his boss — not a separate tech persona. Jack works with NWC directly, so access ≈ commitment: **the site's job is use, not conversion**. Plain language leads everywhere; the industry mapping is the credibility bridge inside each note, never the lead. Everything must be DEAD SIMPLE: every user-facing step is one obvious action.

The concept notes' jobs, ranked: (1) **retain the faculty user** — mid-use confusion finds a two-minute bridge instead of a wall; (2) **orient the evaluator** — the design reads as translation-with-governance, not naive reinvention.

The seed example: the method-card-vs-agent-skill explanation. A method card is what a SKILL.md is compiled *from* — harness-neutral on purpose, with governance fields (evidence gate, review points, revision log) that deployment formats drop. That explanation does three jobs at once: locates the unfamiliar against the familiar, justifies the design, and teaches a framework piece. Every concept note must do all three.

## Goals

- Give faculty (at every fluency level) the "you already understand this" bridge.
- Meet the workbench's own standard: make the design reasoning visible.
- Ground the AI assistant so bundle-driven facilitation can answer *why* questions faithfully.
- Give pilot learnings a home in the why-layer (notes grow from real faculty questions).
- Make the whole reading surface readable on-site — no raw markdown as a reading experience.
- Make extension constant-cost: adding the Nth artifact costs the same as the 5th (see Extensibility).

## Non-goals

- No essay-argument content: the essay owns *why judgment must stay human*; concepts own *why this artifact has these fields*.
- No anticipatory notes: held-back topics wait for pilot demand (the pull principle).
- No CTA / contact capture on the site: the NWC channel is direct; access ≈ commitment.
- No CMS, no framework migration, no client-side markdown parsing: build-time rendering only.

## Scope: index + five notes

New `concepts/` directory:

| File | Serves | Industry equivalent | Framework tie |
| --- | --- | --- | --- |
| `README.md` (index) | Everything | — | The bridge itself |
| `method-cards-and-agent-skills.md` | Method card template | Agent skill / SKILL.md | Phase 5; knowledge outlasts the tool |
| `facilitation-blocks.md` | All templates | System prompt / agent instructions | Supervised AI-mediated work; assistant as runtime |
| `why-the-matrix-is-a-hypothesis.md` | Framework doc; after-action + calibration matrix checks | Eval-gated claims | Reliance calibration, modeled by the workbench itself |
| `source-kits-are-curated-context.md` | Source kit template | Curated context / RAG corpus | Phase 5–6; Level 6→8 lineage |
| `how-faculty-judgment-compounds.md` | After-action note + calibration protocol | Eval sets; inter-rater reliability | The staircase: reviewed practice → shared assets → evaluation |

**Index contents**, in order:

1. H1 + one-line bolded summary.
2. `## Start From Your Question` — five question → note bullets, faculty phrasing ("Why does a method card have a revision log?" → the note). This leads because faculty recognize their question faster than a taxonomy.
3. `## The Vocabulary Bridge` — the table (concept note ↔ served template ↔ industry equivalent), for readers scanning by term.
4. `## How To Read These` — short paragraph on the shared shape.
5. `## Backlog` — the two held-back notes (tool-chooser "what remains when the work is done"; traces/oral-defense "why not prompt logs") with the pull principle stated: *new concept notes get written when pilot questions ask for them, not before.*

## Note anatomy — scannability is the point

A faculty member decides in ten seconds whether a note earns their next two minutes. Every note, same shape, 250–500 words:

1. **H1 title**, then a **one-line bolded summary** (the whole note in one sentence).
2. `## What It Is` — two or three short sentences; names and links the template it serves in the first two sentences.
3. `## Where You'll Use It` — one concrete faculty moment. Placed second (settled 2026-07-09): faculty recognize situations faster than definitions; the moment does the hooking, the mapping and rationale follow.
4. `## The Industry Equivalent` — what AI-fluent readers know this as.
5. `## What The Workbench Adds` — the design decisions and why (bullets welcome: governance fields, evidence gates, boundaries).
6. Closing **framework line** — a single sentence blockquote beginning `**Framework tie:**`, tying the concept to a phase or staircase step.

Formatting rules: paragraphs of two or three sentences maximum; bullets over prose for enumerations; no section longer than ~100 words; workbench voice (short declarative sentences, Title Case H2s). Section reordering during drafting is a **rewrite of transitions, not a block swap** — each section must read correctly given what precedes it.

## Workbench integration points (deliberately light touch)

1. **Served templates** get one line under their intro: `Concept: [why this template works the way it does](../concepts/<note>.md)` — on the method card, source kit, after-action note, and calibration protocol (the last two both link `how-faculty-judgment-compounds.md`).
2. **Workbench source kit** links `facilitation-blocks.md` and adds `concepts/` to anchor materials. Its "The nine templates" prose becomes "the templates in [templates/](templates/)" — no count to rot.
3. **Framework doc**: the matrix section's "Read the status legend first" paragraph gains a link to `concepts/why-the-matrix-is-a-hypothesis.md`.
4. **README** Start Here table gains one row: "Understand the design behind the tools → concepts/README.md".
5. **AGENTS.md** gains one bullet: use the concepts when the faculty member asks why an artifact works the way it does.

## Site scope — two PRs, split by what gates the goal

The bundle is the gating artifact (transcript testing starts the moment it is final); the reading-surface work is not. They ship separately.

### Site PR A (gating, small): bundle

- `buildWorkbenchContext()` gains `SECTION: CONCEPTS` immediately after `SECTION: FRAMEWORK`: `concepts/README.md` first, then the remaining `concepts/*.md` **discovered by directory glob** (sorted), not a hardcoded list.
- Contract tests become **filesystem-derived**: the test walks `concepts/*.md` and `templates/*.md` in the workbench checkout and asserts each file's content appears in the bundle. Hardcoded per-file needle lists are replaced; content-specific needles (below) remain.
- Merges immediately after the workbench PR. Deploy follows. **The transcript gate starts here** — it does not wait for PR B.

### Site PR B (non-gating): reading surface

- **Rendered display**: the workbench mode's selected-tool panel shows build-time-rendered HTML (reusing `renderMarkdown()`, as `buildSourcesMode()` already does) instead of raw markdown in a `<pre>`. Raw markdown stays in the embedded tool JSON for the Copy button and downloads — copy/download behavior is unchanged.
- **Renderer additions**: `renderMarkdown()` gains pipe-table and blockquote support (the concepts index has a table; notes end in blockquotes; the diagnostic has a placement-logic table).
- **Link rewriting convention** (build-time pre-pass, before rendering): `../concepts/<name>.md` and `../templates/<name>.md` (also non-prefixed sibling links inside `concepts/`) → in-mode navigation to that rendered document; `../framework/ai-fluency-progression.md` → anchor to the progression-visual section; any other `.md` target → link stripped to plain text. No dead links, no external jumps for internal content.
- **Concepts real estate**: a "The Design Behind The Tools" group in workbench mode — concept cards (glob-driven) opening rendered notes in the same selected-document panel, with per-note download links.
- **Two equal doors** entry on both workbench and companion modes: the user self-selects "my assistant reads the web" (copy prompt) vs. "attach a file instead" (download + copy prompt) and sees only their path. Replaces the prose fallback branches.
- **Status caption**: one line under the progression visual naming the persona-row content as `Hypothesis — awaiting NWC validation`, linking to the rendered matrix concept note.
- **Completeness assertion** in the build: every `templates/*.md` has a `getWorkbenchTools()` card and vice versa — extension can't silently ship half-wired.

## Extensibility standard

Adding the Nth artifact costs the same as the 5th: constant editorial work (the file itself, one index row or card copy), zero mechanical bookkeeping, silent drift impossible.

- Bundle sections for templates and concepts: glob-derived, never enumerated in the build script.
- Contract tests: derived from the workbench filesystem, never a hand-maintained list.
- Renderer + concepts cards: glob-driven; a new note renders and appears the moment the file exists.
- Build fails loudly when editorial touchpoints are missed (completeness assertion).
- Prose never states counts of things that can grow.

Deliberately manual (editorial content, not bookkeeping): README rows, AGENTS.md bullets, card copy, index question bullets.

## Boundary guards

- Every note names its served template in its first two sentences, or it doesn't ship.
- No note argues the essay's thesis; operational rationale only.
- The matrix note repeats the exact string `Hypothesis — awaiting NWC validation` (contract-test needle; em dash).
- Concepts describe current markdown practice; future-layer claims stay in the roadmap.
- **Jack reviews all five notes before the workbench PR merges** — specifically the industry-equivalent claims, which will be read by AI-fluent NWC staff and cannot be delegated to automated review.

## Testing

- **Site contract tests (PR A)**: filesystem-derived section coverage for every `templates/*.md` and `concepts/*.md`; plus content needles: `SECTION: CONCEPTS`, `Vocabulary Bridge`, `Start From Your Question`, `harness-neutral`, `assistant is the runtime`, `eval-gated`, `curated context`, `inter-rater reliability`, `Hypothesis — awaiting NWC validation`.
- **Site contract tests (PR B)**: rendered-surface assertions (a rendered table from the index, a rendered blockquote framework line, no raw `../` markdown links in displayed HTML); two-doors presence on both modes; status caption; completeness assertion firing on a deliberate mismatch (test-of-the-test).
- **Workbench sweep**: all concept links resolve in both directions (template → note, note → template, index → both).
- **Scannability check** (manual, at Jack's review): each note passes the ten-second test.

## Sequencing and repos

1. **workbench PR** (`concepts-layer` branch): `concepts/` + link edits + count-prose fix. Gate: Jack's note review. Merge first.
2. **site PR A** (bundle): merge immediately after; deploy. **Bundle final — transcript gate begins** (next package; right-sized to the pilot path first: diagnostic, assignment-design worksheet, and the pilot candidate's template).
3. **site PR B** (reading surface): proceeds in parallel with transcript work; deploy on merge. Nothing on the critical path waits for it.

NWC-dependent decisions (assistant environment, pilot faculty, distribution) live in `nwc/NWC-DISCUSSION-LEDGER.md`; their answers may simplify the two doors and retarget the transcript scope.
