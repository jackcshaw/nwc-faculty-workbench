# Workbench Concepts Layer Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add the "why" layer (concepts/ index + five notes) to the workbench and bundle, then upgrade the site's reading surface — rendered documents, two-doors entry, status caption — split so the bundle (the gating artifact for transcript testing) ships first.

**Architecture:** Three PRs. Workbench PR: concepts content + link wiring (gated by Jack's note review). Site PR A (gating, small): glob-driven `SECTION: CONCEPTS` in the bundle + filesystem-derived contract tests — bundle final, transcripts unblocked. Site PR B (non-gating): `renderMarkdown()` gains tables/blockquotes, all workbench docs render as HTML (raw markdown retained for copy/download), concepts get "The Design Behind The Tools" real estate, both entry flows become two equal doors, the progression visual gains a hypothesis-status caption, and a build-time completeness assertion makes silent drift impossible.

**Tech Stack:** Plain markdown (workbench); Node ESM build script + `node:fs` contract tests (site, no framework).

**Spec:** `docs/superpowers/specs/2026-07-08-concepts-layer-design.md` (revised 2026-07-09 — read it first; it records the audience reframe, PR split rationale, and extensibility standard).

## Global Constraints

- **Repos and branches.** workbench: `/Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench`, branch `concepts-layer` (exists). site: `/Users/jackcshaw-2/dev/comprendo-clients/nwc/site` — PR A branch `workbench-concepts-bundle` (Task 10), PR B branch `workbench-reading-surface` (Task 11). Never commit to `main` in either repo.
- **Merge order:** workbench PR (after Task 9's Jack review) → site PR A → deploy (**bundle final; transcript gate may begin**) → site PR B → deploy. PR B never blocks the bundle.
- **No AI/Claude authorship references in commit messages** — no Co-Authored-By trailers, no tool names, ever.
- **Public-safe only:** no private NWC course material, no student names, no security/deployment claims.
- **Workbench voice:** short declarative sentences; Title Case H2s; bullets over prose; two-to-three-sentence paragraphs.
- **Note anatomy (every note, this exact section order):**
  1. `#` H1 title, then one **bold** one-line summary sentence.
  2. `## What It Is` — names and links its served template in the first two sentences.
  3. `## Where You'll Use It` — one concrete faculty moment.
  4. `## The Industry Equivalent`.
  5. `## What The Workbench Adds`.
  6. Closing framework line as a `>` blockquote beginning `**Framework tie:**`.
  250–500 words; no section over ~100 words. If `wc -w` reports under 250, expand `## What The Workbench Adds` with another concrete design decision — never pad the summary or the framework line.
- **Load-bearing strings (verbatim):** `## AI Facilitation Block` (untouched in templates); `Hypothesis — awaiting NWC validation` (em dash) in the matrix note; bundle section headers `# ===== SECTION: <LABEL> =====`; content needles listed in Task 10.
- **Served-template link format:** `Concept: [why this template works the way it does](../concepts/<note>.md)` — plain paragraph line.
- **Extensibility standard (spec §Extensibility):** bundle sections and tests derive from the filesystem/tools list — no hand-enumerated file lists in build script or tests; no counts in prose.
- **Verification commands:** workbench — per-task `grep`/`wc -w`/sweep checks. site — `cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site && npm run build && npm test`.
- **Deploy:** after each site PR merges, deploy per `site/docs/project-hygiene.md` (read that file for the exact steps before deploying).

---

## Workbench PR — Tasks 1–9

### Task 1: Concepts index (`concepts/README.md`)

**Files:**
- Create: `concepts/README.md`

**Interfaces:**
- Produces: the question-led index. Contract needles `Start From Your Question` and `Vocabulary Bridge` (Task 10). Links the five note files created in Tasks 2–6: `method-cards-and-agent-skills.md`, `facilitation-blocks.md`, `why-the-matrix-is-a-hypothesis.md`, `source-kits-are-curated-context.md`, `how-faculty-judgment-compounds.md`.

- [ ] **Step 1: Write the index file**

Create `concepts/README.md` with exactly this content:

```markdown
# Workbench Concepts

**The concepts layer explains why each workbench artifact has the fields it does — bridging unfamiliar tools to things you already understand.**

The templates tell you *what to do*. The framework doc tells you *what the phases are*. These notes tell you *why*. Each takes two minutes and stands alone.

## Start From Your Question

- Why does a method card have a revision log and stop rules? → [Method cards and agent skills](method-cards-and-agent-skills.md)
- Why is the assistant so scripted when I hand it a template? → [Facilitation blocks](facilitation-blocks.md)
- Why does the matrix say "hypothesis — awaiting validation"? → [Why the matrix is a hypothesis](why-the-matrix-is-a-hypothesis.md)
- Why isn't a source kit just a folder of readings? → [Source kits are curated context](source-kits-are-curated-context.md)
- Why write an after-action note when the exercise went fine? → [How faculty judgment compounds](how-faculty-judgment-compounds.md)

## The Vocabulary Bridge

Each row links a concept note to the template it explains and the industry idea it already resembles.

| Concept note | The template it serves | Industry equivalent |
| --- | --- | --- |
| [Method cards and agent skills](method-cards-and-agent-skills.md) | [Method card](../templates/method-card-template.md) | Agent skill / SKILL.md |
| [Facilitation blocks](facilitation-blocks.md) | [All templates](../templates/) | System prompt / agent instructions |
| [Why the matrix is a hypothesis](why-the-matrix-is-a-hypothesis.md) | [Framework doc](../framework/ai-fluency-progression.md) | Evals; claims gated on results |
| [Source kits are curated context](source-kits-are-curated-context.md) | [Source kit](../templates/source-kit-template.md) | Curated context / RAG corpus |
| [How faculty judgment compounds](how-faculty-judgment-compounds.md) | [After-action note](../templates/after-action-note-template.md) · [Calibration protocol](../templates/faculty-calibration-protocol.md) | Eval sets; inter-rater reliability |

## How To Read These

Every note has the same shape: a one-line summary, what the artifact is, one moment you would use it, the industry idea it maps to, and what the workbench adds and why. Skim the headings first. Read the body only if the summary earns your next two minutes.

## Backlog

Two notes are deliberately unwritten until faculty ask for them:

- **A tool-chooser note** — "what remains when the work is done," on picking the right artifact for a task.
- **A traces / oral-defense note** — "why not prompt logs," on evidence of ownership over compliance logging.

New concept notes get written when pilot questions ask for them, not before.
```

- [ ] **Step 2: Verify structure and needles**

Run:
```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
grep -c "Start From Your Question" concepts/README.md
grep -c "Vocabulary Bridge" concepts/README.md
grep -c "^- Why" concepts/README.md
```
Expected: `1`, `1`, `5`.

- [ ] **Step 3: Commit**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
git add concepts/README.md
git commit -m "Add question-led concepts index with vocabulary bridge"
```

---

### Task 2: Note — Method cards and agent skills

**Files:**
- Create: `concepts/method-cards-and-agent-skills.md`

**Interfaces:**
- Consumes: `../templates/method-card-template.md` (exists).
- Produces: contract needle `the card is the source` (Task 10); back-link target for Task 7.

- [ ] **Step 1: Write the note**

Create `concepts/method-cards-and-agent-skills.md` with exactly this content:

```markdown
# Method Cards And Agent Skills

**A method card captures a working AI-enabled task completely enough that a skill file — the industry's format for reusable AI procedures — can be created straight from it.**

## What It Is

A [method card](../templates/method-card-template.md) captures a recurring AI-enabled task once you have run it well more than once: the task brief, the steps, the review criteria, and the stop rules. It is the Phase 5 artifact — where scale begins, because people stop re-explaining the task.

## Where You'll Use It

A faculty member has walked three cohorts through the same AI-assisted intelligence-estimate critique. The steps are in her head. She fills out a method card so the next instructor runs it the same way — and so the review criteria she learned the hard way do not leave with her.

## The Industry Equivalent

Software teams package repeatable AI tasks as **skills**. A skill is a folder built around one markdown file: a short description that tells the assistant when to act, step-by-step instructions, and examples. The assistant finds it and follows it automatically when a matching task appears.

## What The Workbench Adds

A completed method card contains everything a skill file needs — plus what the skill format has no field for:

- **The instructions travel.** The card's steps, review criteria, and examples map straight into a skill file. Hand a finished card to an assistant and ask it to package one; that is the whole conversion.
- **The accountability stays.** Owner, times run, human review points, and the revision log have no home in a skill file. They live on the card because they are for the faculty managing the method, not the machine running it.
- **The evidence gate comes first.** Codify only what has worked twice. No skill gets created from a method that has not earned it.

> **Framework tie:** Method cards are Phase 5 (Codify). The skill file is the deployment; the card is the source — and the record of judgment that outlasts any one tool.
```

- [ ] **Step 2: Verify needle, section order, served-template link, word count**

Run:
```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
grep -c "the card is the source" concepts/method-cards-and-agent-skills.md
grep -n "^## " concepts/method-cards-and-agent-skills.md
head -8 concepts/method-cards-and-agent-skills.md | grep -c "method-card-template.md"
wc -w concepts/method-cards-and-agent-skills.md
```
Expected: needle ≥1; H2 order is exactly `What It Is`, `Where You'll Use It`, `The Industry Equivalent`, `What The Workbench Adds`; served-template link within first 8 lines; 250–500 words.

- [ ] **Step 3: Commit**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
git add concepts/method-cards-and-agent-skills.md
git commit -m "Add concept note: method cards and agent skills"
```

---

### Task 3: Note — Facilitation blocks

**Files:**
- Create: `concepts/facilitation-blocks.md`

**Interfaces:**
- Consumes: `../templates/phase-placement-diagnostic.md`, `../templates/source-kit-template.md` (exist).
- Produces: contract needle `assistant is the runtime` (Task 10); anchored from the source kit in Task 8.

- [ ] **Step 1: Write the note**

Create `concepts/facilitation-blocks.md` with exactly this content:

```markdown
# Facilitation Blocks

**A facilitation block turns a worksheet into a script an AI assistant can run — the workbench's system prompt, written into the document itself.**

## What It Is

Every workbench template carries an `## AI Facilitation Block` — see the [phase placement diagnostic](../templates/phase-placement-diagnostic.md) for the pattern. It tells an assistant how to run that template as a guided session: its role, what to collect first, how to walk the sections, what never to do, and how to finish.

## Where You'll Use It

A faculty member pastes the [source kit template](../templates/source-kit-template.md) into an assistant and says "help me package a source kit." The facilitation block makes the assistant pressure-test her boundaries instead of dumping a generic checklist. She never reads the block; she just gets a better session because it is there.

## The Industry Equivalent

This is a system prompt — the agent instructions that shape how an assistant behaves for a task. AI-fluent readers already write these to steer tone, scope, and guardrails.

## What The Workbench Adds

The block lives *inside* the worksheet, not in a separate config. That makes every template dual-reader: a human fills it out on paper, or hands the whole file to an assistant and the assistant is the runtime. Three design choices:

- **Collect-first.** The assistant gathers the faculty member's own material before producing anything — judgment stays with the human.
- **Never-list.** Each block names what the assistant must not do: invent steps, smooth over disagreement, turn work into surveillance.
- **Bounded finish.** The session ends with a clean artifact and a handoff, not an open-ended chat.

The block also stays visible on paper. Faculty can read exactly what the assistant was told to do — the instructions are never hidden from the person being facilitated.

> **Framework tie:** Facilitation blocks are supervised, AI-mediated work in miniature (Phase 6). The assistant is the runtime; the markdown is the program; the faculty member keeps the judgment seat.
```

- [ ] **Step 2: Verify needle, section order, served-template link, word count**

Run:
```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
grep -c "assistant is the runtime" concepts/facilitation-blocks.md
grep -n "^## " concepts/facilitation-blocks.md
head -8 concepts/facilitation-blocks.md | grep -c "phase-placement-diagnostic.md"
wc -w concepts/facilitation-blocks.md
```
Expected: needle ≥1; H2 order per anatomy; link within first 8 lines; 250–500 words.

- [ ] **Step 3: Commit**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
git add concepts/facilitation-blocks.md
git commit -m "Add concept note: facilitation blocks"
```

---

### Task 4: Note — Why the matrix is a hypothesis

**Files:**
- Create: `concepts/why-the-matrix-is-a-hypothesis.md`

**Interfaces:**
- Consumes: `../framework/ai-fluency-progression.md`, `../templates/after-action-note-template.md`, `../templates/faculty-calibration-protocol.md` (exist).
- Produces: contract needles `gated on evals` and exact string `Hypothesis — awaiting NWC validation` (Task 10); anchored from the framework doc in Task 8 and the site status caption in Task 15.

- [ ] **Step 1: Write the note**

Create `concepts/why-the-matrix-is-a-hypothesis.md` with exactly this content:

```markdown
# Why The Matrix Is A Hypothesis

**The reference matrix ships as a set of claims awaiting evidence, not as doctrine — the workbench modeling the reliance calibration it teaches.**

## What It Is

The [framework doc](../framework/ai-fluency-progression.md) ends in a reference matrix: what learners practice, what faculty teach, and what the institution provides, at every phase. Each persona row carries a status line, and today that status is `Hypothesis — awaiting NWC validation`.

## Where You'll Use It

A faculty member reads a matrix cell that does not match her classroom. Instead of dismissing the framework, she notes the mismatch in her after-action matrix check. Her disagreement becomes data — exactly what the status line invites.

## The Industry Equivalent

AI teams call them evals: test sets that measure a capability against success criteria before anyone relies on it. A claim gated on evals stays a hypothesis until the results back it. The matrix status line is that same discipline applied to a teaching framework.

## What The Workbench Adds

The matrix could have been printed as settled doctrine. It is not, for two reasons:

- **Honesty about evidence.** The cells are research-informed but have not survived contact with NWC faculty use. Marking them as hypotheses says so plainly.
- **A path to evidence.** The [after-action note](../templates/after-action-note-template.md) and [calibration protocol](../templates/faculty-calibration-protocol.md) carry short matrix checks. Routine use produces the evidence that confirms, revises, or strikes each cell.

As evidence arrives, a row's status moves — to `Field-tested — evidence from NWC use`, `Revised`, or `Struck` — and the change is recorded in the framework doc's changelog. The workbench asks students to calibrate how far they rely on AI. The matrix holds itself to the same standard.

> **Framework tie:** This is reliance calibration turned on the framework itself — trust the matrix exactly as far as the evidence goes, and no further.
```

- [ ] **Step 2: Verify needles, section order, served-template link, word count**

Run:
```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
grep -c "gated on evals" concepts/why-the-matrix-is-a-hypothesis.md
grep -c "Hypothesis — awaiting NWC validation" concepts/why-the-matrix-is-a-hypothesis.md
grep -n "^## " concepts/why-the-matrix-is-a-hypothesis.md
head -8 concepts/why-the-matrix-is-a-hypothesis.md | grep -c "ai-fluency-progression.md"
wc -w concepts/why-the-matrix-is-a-hypothesis.md
```
Expected: both needles ≥1 (em dash exact); H2 order per anatomy; link within first 8 lines; 250–500 words.

- [ ] **Step 3: Commit**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
git add concepts/why-the-matrix-is-a-hypothesis.md
git commit -m "Add concept note: why the matrix is a hypothesis"
```

---

### Task 5: Note — Source kits are curated context

**Files:**
- Create: `concepts/source-kits-are-curated-context.md`

**Interfaces:**
- Consumes: `../templates/source-kit-template.md` (exists).
- Produces: contract needle `curated context` (Task 10); back-link target for Task 7.

- [ ] **Step 1: Write the note**

Create `concepts/source-kits-are-curated-context.md` with exactly this content:

```markdown
# Source Kits Are Curated Context

**A source kit is a curated context packet with boundaries — the teaching version of the corpus an AI-fluent team assembles before letting a model work.**

## What It Is

A [source kit](../templates/source-kit-template.md) is the curated context for an AI-enabled exercise. It tells an assistant what materials matter, what standards apply, what outputs faculty will inspect, and what boundaries must hold. A source kit is not a file dump.

## Where You'll Use It

A faculty member building a wargame-analysis exercise assembles the readings, the assessment rubric, and a note that one restricted case is excluded. The kit lets any assistant run the exercise without ever seeing what it should not.

## The Industry Equivalent

This is curated context — the material a team assembles so a model answers from the right sources instead of guessing. At scale it becomes a retrieval corpus (the "RAG" an AI team maintains); in a single session, it is the files you attach to the chat. Either way, deciding what goes in is the real work.

## What The Workbench Adds

A source kit is curated context with the governance a teaching setting needs:

- **Explicit boundaries.** Every source is marked allowed, excluded, or restricted. Private course material does not leak into a public kit by accident.
- **Standards and inspection points.** The kit names what "good" looks like and what faculty will actually check — not just what to feed the model.
- **Faculty ownership of the cut.** The faculty member decides what is in and what is out. The assistant organizes and pressure-tests; it does not curate for them.

> **Framework tie:** Source kits are Phase 5–6 (Codify → Supervise) — and the seed of the Level 6→8 lineage: today's curated packet is what a future faculty-governed context vault would grow from.
```

- [ ] **Step 2: Verify needle, section order, served-template link, word count**

Run:
```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
grep -c "curated context" concepts/source-kits-are-curated-context.md
grep -n "^## " concepts/source-kits-are-curated-context.md
head -8 concepts/source-kits-are-curated-context.md | grep -c "source-kit-template.md"
wc -w concepts/source-kits-are-curated-context.md
```
Expected: needle ≥1; H2 order per anatomy; link within first 8 lines; 250–500 words.

- [ ] **Step 3: Commit**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
git add concepts/source-kits-are-curated-context.md
git commit -m "Add concept note: source kits are curated context"
```

---

### Task 6: Note — How faculty judgment compounds

**Files:**
- Create: `concepts/how-faculty-judgment-compounds.md`

**Interfaces:**
- Consumes: `../templates/after-action-note-template.md`, `../templates/faculty-calibration-protocol.md` (exist).
- Produces: contract needle `inter-rater reliability` (Task 10); back-link target for Task 7 (both after-action and calibration).

- [ ] **Step 1: Write the note**

Create `concepts/how-faculty-judgment-compounds.md` with exactly this content:

```markdown
# How Faculty Judgment Compounds

**Reviewed practice becomes shared assets becomes evaluation — the same staircase that turns individual notes into inter-rater reliability.**

## What It Is

Two templates capture judgment so it accumulates instead of evaporating: the [after-action note](../templates/after-action-note-template.md), which preserves lesson rationale after an exercise, and the [calibration protocol](../templates/faculty-calibration-protocol.md), which compares how faculty diagnose the same AI-assisted work.

## Where You'll Use It

Three instructors grade the same flawed AI estimate and diverge on whether the reliance was justified. The calibration protocol captures where they agree, where they do not, and the question that exposes the difference — ready for the next rotation instead of lost to the hallway.

## The Industry Equivalent

AI teams build eval sets and measure inter-rater reliability — do independent reviewers agree on what counts as good? Faculty already do this work; calibration just makes it explicit and reusable.

## What The Workbench Adds

Individual judgment is perishable. The workbench compounds it in three moves:

- **Reviewed practice.** An after-action note turns one exercise into a durable record of what worked and why.
- **Shared assets.** Calibration turns private standards into a shared minimum — recorded in the faculty's own words, disagreement preserved rather than smoothed away.
- **Evaluation.** The matrix checks inside both templates feed evidence back to the framework, so the whole system learns.

Legitimate disagreement is an outcome, not a failure. The protocol records the range instead of forcing consensus.

> **Framework tie:** This is the staircase — reviewed practice → shared assets → evaluation. It is how a faculty's judgment outlives any one instructor.
```

- [ ] **Step 2: Verify needle, section order, served-template links, word count**

Run:
```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
grep -c "inter-rater reliability" concepts/how-faculty-judgment-compounds.md
grep -n "^## " concepts/how-faculty-judgment-compounds.md
head -8 concepts/how-faculty-judgment-compounds.md | grep -Ec "after-action-note-template.md|faculty-calibration-protocol.md"
wc -w concepts/how-faculty-judgment-compounds.md
```
Expected: needle ≥1; H2 order per anatomy; ≥1 served-template link within first 8 lines; 250–500 words.

- [ ] **Step 3: Commit**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
git add concepts/how-faculty-judgment-compounds.md
git commit -m "Add concept note: how faculty judgment compounds"
```

---

### Task 7: Served-template Concept links

**Files:**
- Modify: `templates/method-card-template.md`
- Modify: `templates/source-kit-template.md`
- Modify: `templates/after-action-note-template.md`
- Modify: `templates/faculty-calibration-protocol.md`

**Interfaces:**
- Consumes: note files from Tasks 2–6.
- Produces: bidirectional template↔note links verified by Task 8's sweep.

- [ ] **Step 1: Add the Concept line to each template**

In each file, insert a blank line then the Concept line immediately after the sentence shown, keeping `## AI Facilitation Block` after it:

`templates/method-card-template.md` — after `Codify only what has worked at least twice. A method card for a task you have done once is a guess wearing a uniform.`:

```markdown
Concept: [why this template works the way it does](../concepts/method-cards-and-agent-skills.md)
```

`templates/source-kit-template.md` — after `A source kit is not a file dump.`:

```markdown
Concept: [why this template works the way it does](../concepts/source-kits-are-curated-context.md)
```

`templates/after-action-note-template.md` — after `Use this note after running an AI-enabled exercise. The goal is to preserve lesson rationale, faculty judgment, and useful revisions before they disappear.`:

```markdown
Concept: [why this template works the way it does](../concepts/how-faculty-judgment-compounds.md)
```

`templates/faculty-calibration-protocol.md` — after `Use this protocol when faculty need to compare how they diagnose the same AI-assisted work. The goal is to make tacit judgment explicit without forcing false agreement.`:

```markdown
Concept: [why this template works the way it does](../concepts/how-faculty-judgment-compounds.md)
```

- [ ] **Step 2: Verify placement**

Run:
```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
grep -c "^Concept: \[why this template works the way it does\]" \
  templates/method-card-template.md templates/source-kit-template.md \
  templates/after-action-note-template.md templates/faculty-calibration-protocol.md
for f in method-card-template source-kit-template after-action-note-template faculty-calibration-protocol; do
  awk '/^Concept: /{c=NR} /^## AI Facilitation Block/{if(c && c<NR) print FILENAME" OK"}' "templates/$f.md"
done
```
Expected: each file reports `1`; each prints `templates/<name>.md OK`.

- [ ] **Step 3: Commit**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
git add templates/method-card-template.md templates/source-kit-template.md templates/after-action-note-template.md templates/faculty-calibration-protocol.md
git commit -m "Link served templates to their concept notes"
```

---

### Task 8: Navigation surfaces, count-prose fix, link sweep

**Files:**
- Modify: `workbench-source-kit.md` (§3 Anchor Materials — includes the count-prose fix)
- Modify: `framework/ai-fluency-progression.md` (status-legend paragraph, ~line 59)
- Modify: `README.md` (Start Here table)
- Modify: `AGENTS.md` (Operating Principles)

**Interfaces:**
- Consumes: all files from Tasks 1–7.
- Produces: anchors for `facilitation-blocks.md`, `why-the-matrix-is-a-hypothesis.md`, and the index; count-free prose; sweep-verified link graph.

- [ ] **Step 1: Source kit — anchors + count-prose fix**

In `workbench-source-kit.md` §3 Anchor Materials, replace the line:

```markdown
- The nine templates in [templates/](templates/), each with its own facilitation block.
```

with:

```markdown
- The templates in [templates/](templates/), each with its own facilitation block.
- [Facilitation blocks](concepts/facilitation-blocks.md) — the pattern behind every AI Facilitation Block, so you know how the assistant runs a template.
- [The concepts layer](concepts/README.md) — why each artifact has the fields it does.
```

- [ ] **Step 2: Framework doc — status-legend anchor**

Append one sentence to the `**Read the status legend first.**` paragraph (ends `...confirms, revises, or strikes these cells.`):

```markdown
 For why the matrix ships as a hypothesis rather than doctrine, see [the concept note](../concepts/why-the-matrix-is-a-hypothesis.md).
```

- [ ] **Step 3: README — Start Here row**

Immediately after the row `| See the full fluency progression and matrix | [framework/ai-fluency-progression.md](framework/ai-fluency-progression.md) | All |` insert:

```markdown
| Understand the design behind the tools | [concepts/README.md](concepts/README.md) | — |
```

- [ ] **Step 4: AGENTS.md — concepts bullet**

After `- Every template contains an AI Facilitation Block. Follow it exactly when facilitating.` insert:

```markdown
- Use [concepts/](concepts/README.md) when the faculty member asks why an artifact works the way it does — each note bridges a template to a familiar industry idea.
```

- [ ] **Step 5: Verify anchors and count-prose removal**

Run:
```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
grep -c "concepts/facilitation-blocks.md" workbench-source-kit.md
grep -c "concepts/README.md" workbench-source-kit.md README.md AGENTS.md
grep -c "../concepts/why-the-matrix-is-a-hypothesis.md" framework/ai-fluency-progression.md
grep -rn "nine templates" . --include='*.md' | grep -v docs/superpowers | wc -l
```
Expected: `1`; `1` in each of the three files; `1`; `0` (count-prose gone).

- [ ] **Step 6: Bidirectional link sweep**

Run:
```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
for f in concepts/*.md; do
  dir=$(dirname "$f")
  grep -oE "\]\(([^)]+\.md)\)" "$f" | sed -E 's/\]\((.*)\)/\1/' | while read -r link; do
    [ -e "$dir/$link" ] || echo "BROKEN: $f -> $link"
  done
done
grep -q "concepts/method-cards-and-agent-skills.md" templates/method-card-template.md && echo "method-card back-link OK"
grep -q "concepts/source-kits-are-curated-context.md" templates/source-kit-template.md && echo "source-kit back-link OK"
grep -q "concepts/how-faculty-judgment-compounds.md" templates/after-action-note-template.md && echo "after-action back-link OK"
grep -q "concepts/how-faculty-judgment-compounds.md" templates/faculty-calibration-protocol.md && echo "calibration back-link OK"
```
Expected: no `BROKEN:` lines; four `back-link OK` lines.

- [ ] **Step 7: Commit**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
git add workbench-source-kit.md framework/ai-fluency-progression.md README.md AGENTS.md
git commit -m "Wire concepts layer into workbench navigation surfaces"
```

---

### Task 9: Jack's note review — HUMAN GATE, then open the workbench PR

**Files:** none created; review of Tasks 1–6 output.

**This gate cannot be delegated.** Jack reviews the six concept files for:

- [ ] **Industry-equivalent claims are accurate** — every statement about SKILL.md files, system prompts, eval gates, RAG corpora, and inter-rater reliability will be read by AI-fluent NWC staff; each must survive a skeptical practitioner.
- [ ] **Ten-second scannability** — title + bold summary + headings convey the gist without body text.
- [ ] **Voice** — reads as workbench prose, not AI-diligent filler.
- [ ] **Boundary guards hold** — no essay-thesis argument; no future-layer claims; served template named in first two sentences.

Apply any edits as review-fix commits on `concepts-layer`. Then:

- [ ] **Open the workbench PR**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
git push -u origin concepts-layer
gh pr create --title "Add concepts layer: the why behind the workbench artifacts" --body "$(cat <<'EOF'
## Summary
- New concepts/ directory: question-led index + five design-rationale notes (method cards, facilitation blocks, matrix-as-hypothesis, source kits, judgment compounding)
- Served templates link their concept notes; source kit, framework doc, README, AGENTS.md gain anchors
- Count-prose removed (extensibility standard)

## Test plan
- Bidirectional link sweep passes (template → note, note → template, index → both)
- Per-note needle, anatomy-order, and word-count checks pass
- Site contract tests pick up the bundle section in the follow-up site PR
EOF
)"
```

Merge per the standard PR pipeline. **Site PR A (Task 10) starts only after this merges.**

---

## Site PR A — the bundle (gating)

### Task 10: Glob-driven CONCEPTS section + filesystem-derived contract tests

**Files:**
- Modify: `scripts/build-site.mjs` (imports, new helper, `buildWorkbenchContext()`)
- Modify: `tests/site-contract.test.mjs` (imports, needle list, new derived checks)

**Interfaces:**
- Consumes: merged workbench `concepts/` directory (via `WORKBENCH_REPO_PATH`, default `../workbench`).
- Produces: `# ===== SECTION: CONCEPTS =====` in `dist/assets/workbench-context.md` after FRAMEWORK; tests that extend automatically with new workbench files. Task 12 reuses `listWorkbenchFiles()`.

- [ ] **Step 1: Create the branch**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site
git checkout main && git pull
git checkout -b workbench-concepts-bundle
```

- [ ] **Step 2: Write the failing tests**

In `tests/site-contract.test.mjs`: add `readdirSync` to the `node:fs` import. Replace the workbench-bundle needle block (the array currently listing `SECTION: OPERATING RULES` through `Hypothesis — awaiting NWC validation`) with:

```javascript
[
  "SECTION: OPERATING RULES",
  "SECTION: FRAMEWORK",
  "SECTION: CONCEPTS",
  "Start From Your Question",
  "Vocabulary Bridge",
  "the card is the source",
  "assistant is the runtime",
  "gated on evals",
  "curated context",
  "inter-rater reliability",
  "AI Facilitation Block",
  "Hypothesis — awaiting NWC validation",
].forEach((needle) => {
  assert(workbenchContext.includes(needle), `workbench bundle should include ${needle}`);
});

const workbenchRepoPath = process.env.WORKBENCH_REPO_PATH || join(root, "..", "workbench");
["templates", "concepts"].forEach((dir) => {
  const dirPath = join(workbenchRepoPath, dir);
  assert(existsSync(dirPath), `workbench ${dir}/ directory should exist`);
  const files = readdirSync(dirPath).filter((name) => name.endsWith(".md"));
  assert(files.length > 0, `workbench ${dir}/ should contain markdown files`);
  files.forEach((name) => {
    const probe = readFileSync(join(dirPath, name), "utf8").trim().slice(0, 200);
    assert(workbenchContext.includes(probe), `workbench bundle should include content of ${dir}/${name}`);
  });
});

const conceptsAt = workbenchContext.indexOf("SECTION: CONCEPTS");
assert(
  workbenchContext.indexOf("SECTION: FRAMEWORK") < conceptsAt &&
    conceptsAt < workbenchContext.indexOf("SECTION: PHASE PLACEMENT DIAGNOSTIC"),
  "CONCEPTS section should sit between FRAMEWORK and the templates",
);
```

If `root` is not already defined in the test file, mirror how it derives `dist` (`join(__dirname, "..")`).

- [ ] **Step 3: Run to verify failure**

Run: `cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site && npm run build && npm test`
Expected: FAIL at `workbench bundle should include SECTION: CONCEPTS`. Build itself succeeds (concept files exist in `../workbench` after the workbench PR merge).

- [ ] **Step 4: Implement the glob-driven sections**

In `scripts/build-site.mjs`: add `readdirSync` to the `node:fs` import. Below `readRequiredWorkbenchFile`, add:

```javascript
function listWorkbenchFiles(relativeDir) {
  const dirPath = join(workbenchRepoPath, relativeDir);
  if (!existsSync(dirPath)) {
    throw new Error(`Missing workbench directory: ${dirPath}. Set WORKBENCH_REPO_PATH to the workbench repo checkout.`);
  }
  return readdirSync(dirPath).filter((name) => name.endsWith(".md")).sort();
}
```

Replace `buildWorkbenchContext()`'s hardcoded `sections` array and forEach with:

```javascript
function buildWorkbenchContext() {
  const conceptFiles = [
    "concepts/README.md",
    ...listWorkbenchFiles("concepts")
      .filter((name) => name !== "README.md")
      .map((name) => `concepts/${name}`),
  ];
  const sections = [
    ["OPERATING RULES", "workbench-source-kit.md"],
    ["FRAMEWORK", "framework/ai-fluency-progression.md"],
    ["CONCEPTS", conceptFiles],
    ...workbenchTools.map((tool) => [tool.title.toUpperCase(), `templates/${tool.filename}`]),
  ];

  const parts = [
    "# NWC Faculty Workbench - Context Bundle",
    "",
    "Read this whole file before answering. Sections are marked with clear SECTION headers.",
    "Start from the OPERATING RULES. Every template contains an AI Facilitation Block; follow it exactly when facilitating.",
    "Relative links inside sections refer to files in the workbench repository; their contents appear as SECTIONs of this bundle.",
  ];

  sections.forEach(([label, relativePath]) => {
    const body = Array.isArray(relativePath)
      ? relativePath.map((p) => readRequiredWorkbenchFile(p).trim()).join("\n\n")
      : readRequiredWorkbenchFile(relativePath).trim();
    parts.push("", "", `# ===== SECTION: ${label} =====`, "", body);
  });

  return parts.join("\n");
}
```

(Template sections now derive from `workbenchTools` — the labels `tool.title.toUpperCase()` reproduce the existing section headers exactly, e.g. `PHASE PLACEMENT DIAGNOSTIC`.)

- [ ] **Step 5: Run to verify pass, order, and size**

Run:
```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site
npm run build && npm test
grep -n "SECTION: FRAMEWORK\|SECTION: CONCEPTS\|SECTION: PHASE PLACEMENT" dist/assets/workbench-context.md
wc -c dist/assets/workbench-context.md
```
Expected: PASS; CONCEPTS between FRAMEWORK and PHASE PLACEMENT DIAGNOSTIC; ~61KB (well under the 150,000 ceiling).

- [ ] **Step 6: Commit, open PR A**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site
git add scripts/build-site.mjs tests/site-contract.test.mjs
git commit -m "Add glob-driven concepts section to workbench bundle"
git push -u origin workbench-concepts-bundle
gh pr create --title "Bundle: concepts section, filesystem-derived contract tests" --body "$(cat <<'EOF'
## Summary
- workbench-context.md gains SECTION: CONCEPTS (index + notes, glob-driven) after FRAMEWORK
- Template sections derive from the tools list; contract tests derive coverage from the workbench filesystem — adding a workbench file requires zero build/test edits

## Test plan
- npm run build && npm test (needles + derived coverage + section order + size ceiling)
EOF
)"
```

- [ ] **Step 7: After merge — deploy**

Deploy per `site/docs/project-hygiene.md` (read it for exact steps). Verify: `curl -s https://judgmentlab.net/assets/workbench-context.md | grep -c "SECTION: CONCEPTS"` returns `1`.

**→ Bundle is final. The transcript gate (next package) may begin now — it does not wait for PR B.**

---

## Site PR B — the reading surface (non-gating)

### Task 11: `renderMarkdown()` — pipe tables and blockquotes

**Files:**
- Modify: `scripts/build-site.mjs` (`renderMarkdown()` at ~line 782; new `renderTable()` helper)
- Modify: `tests/site-contract.test.mjs` (renderer assertions)

**Interfaces:**
- Produces: `renderMarkdown` handling `|`-tables and `>`-blockquotes. Tasks 12–13 render documents containing both.

- [ ] **Step 1: Create the branch (requires PR A merged)**

PR B builds on Task 10's `listWorkbenchFiles()` — do not start until `workbench-concepts-bundle` has merged to main.

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site
git checkout main && git pull
git checkout -b workbench-reading-surface
```

- [ ] **Step 2: Extend renderMarkdown**

In `renderMarkdown()`, add state `let tableLines = [];` and `let quoteLines = [];` beside `let listType = null;`. Add two flush helpers beside `closeList()`:

```javascript
  function flushTable() {
    if (tableLines.length) {
      html.push(renderTable(tableLines));
      tableLines = [];
    }
  }

  function flushQuote() {
    if (quoteLines.length) {
      html.push(`<blockquote><p>${renderInline(quoteLines.join(" "))}</p></blockquote>`);
      quoteLines = [];
    }
  }
```

In the line loop, after the code-fence handling and before the blank-line check, insert:

```javascript
    if (line.trim().startsWith("|")) {
      flushParagraph();
      closeList();
      flushQuote();
      tableLines.push(line.trim());
      continue;
    }
    flushTable();

    const quoted = line.match(/^>\s?(.*)$/u);
    if (quoted) {
      flushParagraph();
      closeList();
      if (quoted[1].trim()) quoteLines.push(quoted[1].trim());
      continue;
    }
    flushQuote();
```

Add `flushTable(); flushQuote();` calls inside the blank-line branch and before the final `return` (next to the existing `flushParagraph(); closeList();`). Add the helper below `renderMarkdown`:

```javascript
function renderTable(lines) {
  const rows = lines.map((line) =>
    line.replace(/^\|/u, "").replace(/\|$/u, "").split("|").map((cell) => cell.trim()),
  );
  const header = rows[0] ?? [];
  const body = rows.slice(1).filter((cells) => !cells.every((cell) => /^:?-{3,}:?$/u.test(cell)));
  const head = `<thead><tr>${header.map((cell) => `<th>${renderInline(cell)}</th>`).join("")}</tr></thead>`;
  const rowsHtml = body
    .map((cells) => `<tr>${cells.map((cell) => `<td>${renderInline(cell)}</td>`).join("")}</tr>`)
    .join("");
  return `<table>${head}<tbody>${rowsHtml}</tbody></table>`;
}
```

- [ ] **Step 3: Verify existing pages unaffected**

Run: `cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site && npm run build && npm test`
Expected: PASS — the renderer additions change nothing until Tasks 12–13 feed them table/blockquote content.

- [ ] **Step 4: Commit**

```bash
git add scripts/build-site.mjs
git commit -m "Add table and blockquote support to the markdown renderer"
```

---

### Task 12: Link rewriting + rendered document display (templates)

**Files:**
- Modify: `scripts/build-site.mjs` — new `rewriteWorkbenchLinks()`; `renderInline()` internal-anchor handling; `getWorkbenchTools()` gains `html` per tool; `buildWorkbenchMode()` selected-tool panel; embedded client JSON + switch handler in the inline script (~line 931 and ~1161).

**Interfaces:**
- Consumes: Task 11 renderer.
- Produces: every tool has `tool.html` (rendered, link-rewritten); the selected-tool panel displays HTML while Copy/Download keep raw markdown; `data-wb-link` anchors navigate between rendered documents. Task 13 reuses all of it for concepts.

- [ ] **Step 1: Add the link-rewrite pre-pass**

Below `renderTable`, add:

```javascript
function rewriteWorkbenchLinks(markdown) {
  return markdown.replace(/\[([^\]]+)\]\(([^)]+)\)/gu, (match, label, href) => {
    if (/^https?:/u.test(href) || href.startsWith("#")) return match;
    const target = href.replace(/^(?:\.\.\/)+/u, "").replace(/^\.\//u, "");
    const mdName = (target.match(/^(?:templates\/|concepts\/)?([A-Za-z0-9-]+)\.md$/u) || [])[1];
    if (mdName && (target.startsWith("templates/") || target.startsWith("concepts/") || !target.includes("/"))) {
      return `[${label}](#wb-doc-${mdName})`;
    }
    if (target === "framework/ai-fluency-progression.md") {
      return `[${label}](#workbench-progression)`;
    }
    return label; // unresolvable internal link: render as plain text, never a dead link
  });
}
```

- [ ] **Step 2: Internal anchors in renderInline**

Replace `renderInline`'s link replacement with:

```javascript
  value = value.replace(/\[([^\]]+)\]\(([^)]+)\)/gu, (_match, label, href) => {
    if (href.startsWith("#")) {
      return `<a href="${href}" data-wb-link>${label}</a>`;
    }
    return `<a href="${href}" target="_blank" rel="noreferrer">${label}</a>`;
  });
```

- [ ] **Step 3: Render each tool at build time**

In `getWorkbenchTools()`, where each tool's `markdown` is loaded, add alongside it:

```javascript
      html: renderMarkdown(rewriteWorkbenchLinks(markdown), { skipFirstH1: true }),
```

(Match the function's existing shape — if it maps over entries to attach `markdown`, attach `html` in the same map.)

- [ ] **Step 4: Display rendered HTML; keep raw for copy**

In `buildWorkbenchMode()`, give the progression band an anchor: change `<section class="detail-band">` (the one containing the progression `<img>`) to `<section class="detail-band" id="workbench-progression">`. Replace the template-layout block:

```html
      <div class="template-layout">
        <article class="template-rendered article-body" id="workbench-doc-view">${selected.html}</article>
        <pre hidden><code id="workbench-template">${escapeHtml(selected.markdown.trim())}</code></pre>
        <aside class="use-note">
          <p class="eyebrow">How To Use It</p>
          <p id="selected-tool-note">${escapeHtml(selected.useNote)}</p>
        </aside>
      </div>
```

(The Copy button's `data-copy-target="workbench-template"` keeps working — it reads the hidden raw markdown.)

- [ ] **Step 5: Client-side switching carries html**

In the embedded JSON (~line 931), add `html: tool.html` to the mapped fields. In the tool-switch handler (~line 1161), when a tool is selected, set the rendered view too:

```javascript
    document.getElementById("workbench-doc-view").innerHTML = tool.html;
```

Add one delegated handler for cross-document links (place near the tool-switch wiring):

```javascript
document.addEventListener("click", (event) => {
  const link = event.target.closest("[data-wb-link]");
  if (!link) return;
  const id = link.getAttribute("href").replace("#wb-doc-", "");
  const doc = workbenchTools.find((tool) => tool.filename === `${id}.md`);
  if (doc) {
    event.preventDefault();
    selectWorkbenchTool(doc.id);
  }
});
```

(Adapt `selectWorkbenchTool` to whatever the existing switch function is named; if selection is inline in the click handler, extract it into a named function first. `#workbench-progression` links need no handler — native anchor scroll works.)

- [ ] **Step 6: Minimal CSS**

In `css()`, add alongside the existing `.template-block` styles:

```css
.template-rendered { max-height: 640px; overflow-y: auto; padding: 20px 24px; background: var(--panel, #f4f1ea); border: 1px solid rgba(10,34,66,0.12); }
.template-rendered table { width: 100%; border-collapse: collapse; font-size: 0.92em; }
.template-rendered th, .template-rendered td { border: 1px solid rgba(10,34,66,0.15); padding: 6px 10px; text-align: left; }
.template-rendered blockquote { border-left: 3px solid #d82032; margin: 12px 0; padding: 4px 14px; }
```

(Match variable names/colors to the existing `css()` — reuse its palette, don't invent one.)

- [ ] **Step 7: Verify**

Run:
```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site
npm run build && npm test
grep -c 'id="workbench-doc-view"' dist/index.html
grep -c '](\.\./' dist/index.html
```
Expected: build+tests pass; view present; `0` raw relative markdown links in the HTML.

- [ ] **Step 8: Commit**

```bash
git add scripts/build-site.mjs
git commit -m "Render workbench templates as readable documents with rewritten links"
```

---

### Task 13: Concepts real estate — "The Design Behind The Tools"

**Files:**
- Modify: `scripts/build-site.mjs` — new `getWorkbenchConcepts()`; concepts emitted to `dist/assets/workbench/concepts/`; new section in `buildWorkbenchMode()`; concepts in embedded JSON + switch handler.

**Interfaces:**
- Consumes: Tasks 11–12 (renderer, rewrite, doc view, `listWorkbenchFiles` from Task 10).
- Produces: glob-driven concept cards opening rendered notes in the shared doc view, with downloads.

- [ ] **Step 1: Build the concepts collection**

Below `getWorkbenchTools()`, add:

```javascript
function getWorkbenchConcepts() {
  const files = ["README.md", ...listWorkbenchFiles("concepts").filter((name) => name !== "README.md")];
  return files.map((name) => {
    const markdown = readRequiredWorkbenchFile(`concepts/${name}`);
    const title = (markdown.match(/^#\s+(.+)$/mu) || [, name])[1].trim();
    const summary = (markdown.match(/^\*\*(.+)\*\*$/mu) || [, ""])[1].trim();
    return {
      id: name.replace(/\.md$/u, ""),
      title: name === "README.md" ? "Start Here: The Concepts Index" : title,
      summary,
      filename: name,
      markdown,
      html: renderMarkdown(rewriteWorkbenchLinks(markdown), { skipFirstH1: true }),
    };
  });
}
```

At top level (near `const workbenchTools = getWorkbenchTools();`), add `const workbenchConcepts = getWorkbenchConcepts();`, and emit downloads next to the existing tool emit:

```javascript
mkdirSync(join(workbenchAssetsDir, "concepts"), { recursive: true });
workbenchConcepts.forEach((note) => {
  writeFileSync(join(workbenchAssetsDir, "concepts", note.filename), note.markdown.trim() + "\n", "utf8");
});
```

- [ ] **Step 2: Add the section to workbench mode**

In `buildWorkbenchMode()` (thread `workbenchConcepts` in as a second parameter `concepts`, and update the call site in the `buildHtml({ ... workbenchHtml: buildWorkbenchMode(workbenchTools) ... })` block to pass it), insert between the tool grid and the selected-tool section:

```html
    <section class="detail-band">
      <p class="band-label">The Design Behind The Tools</p>
      <p>Why each artifact has the fields it does — each note bridges a workbench tool to an idea you may already know.</p>
    </section>

    <section id="workbench-concepts" class="tool-grid" aria-label="Workbench concept notes">
      ${concepts.map((note) => `<button class="tool-card" type="button" data-concept-id="${note.id}">
        <span class="tool-title">${escapeHtml(note.title)}</span>
        <span class="tool-desc">${escapeHtml(note.summary)}</span>
        <span class="tool-action">Read note &rarr;</span>
      </button>`).join("\n      ")}
    </section>
```

- [ ] **Step 3: Wire client-side selection**

Embed `workbenchConcepts` in the client JSON (id, title, filename, summary, markdown, html — same shape as tools). Extend the doc-selection logic: `data-concept-id` clicks and `#wb-doc-<id>` links resolve against tools first, then concepts; selection renders `note.html` into `#workbench-doc-view`, puts raw markdown into the hidden copy block, sets the download href to `assets/workbench/concepts/${note.filename}`, and sets the use-note to `Read it here, or download it to share with a colleague.`.

- [ ] **Step 4: Verify**

Run:
```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site
npm run build && npm test
grep -c "The Design Behind The Tools" dist/index.html
ls dist/assets/workbench/concepts/
```
Expected: pass; band present; six concept files emitted.

- [ ] **Step 5: Commit**

```bash
git add scripts/build-site.mjs
git commit -m "Add rendered concepts section to workbench mode"
```

---

### Task 14: Two equal doors — workbench and companion entries (attach-first, proof-of-read handshake)

**Files:**
- Modify: `scripts/build-site.mjs` — `buildWorkbenchMode()` hero/action-row, `buildCompanionMode()` hero paragraph + action-row, `workbenchSetupPrompt()`, `setupPrompt()` (companion), `css()`.

**Interfaces:**
- Consumes: `buildWorkbenchContext()`/`buildCompanionContext()` section counts (compute at build time — see Step 3).
- Produces: door-choice UI on both modes, attach door FIRST (field learning: institutional networks filter the site; attach always works); setup prompts gain a proof-of-read handshake. Contract needles `Attach the file` ×2 and `SECTION:` handshake copy.

- [ ] **Step 1: Workbench doors — attach door leads**

In `buildWorkbenchMode()`, replace the hero's "Fastest path…" paragraph and `action-row` div with:

```html
      <p>
        The setup prompt reads the whole workbench, places your assignment on the
        six-phase fluency progression, and facilitates the right template with you.
        Every template also works on paper. No repository knowledge required.
      </p>
      <p class="eyebrow">How will your assistant get the file?</p>
      <div class="door-grid">
        <div class="door">
          <h3>Attach the file — works everywhere</h3>
          <p>Download the context file, attach it to a new chat, then paste the setup prompt. Works on filtered networks and with assistants that cannot browse.</p>
          <a class="copy-button primary" href="assets/${workbenchContextFilename}" download>Download context file</a>
          <button class="quiet-action" type="button" data-copy-target="workbench-setup-prompt">Copy the prompt</button>
        </div>
        <div class="door">
          <h3>My assistant reads the web</h3>
          <p>Copy the setup prompt and paste it into ChatGPT, Claude, or Gemini. It fetches the workbench itself.</p>
          <button class="copy-button" type="button" data-copy-target="workbench-setup-prompt">Copy setup prompt</button>
        </div>
      </div>
```

- [ ] **Step 2: Companion doors**

In `buildCompanionMode()`, replace the "Running this needs an assistant that can read a web page…" paragraph and its `action-row` with the same door structure (`data-copy-target` and download href pointing at the companion prompt/`companionContextFilename`; door copy otherwise identical, attach door first).

- [ ] **Step 3: Proof-of-read handshake in both setup prompts**

"Say ready" is unverifiable — an assistant can summarize a large fetched file and believe it read it. Both setup prompt functions gain a handshake whose expected answer the BUILD computes, so it never rots as sections are added.

Make the section count available: have `buildWorkbenchContext()` and `buildCompanionContext()` each expose their section count (e.g., return `{ text, sectionCount }` or compute the count where the prompts are built by counting `# ===== SECTION:` occurrences in the generated bundle text — implementer's choice, but the count must come from the generated artifact, not a hardcoded number).

In `workbenchSetupPrompt()`, after the existing fetch instruction and before the diagnostic instruction, insert (with `${count}` interpolated from the build):

```text
After reading, tell me exactly how many "===== SECTION:" headers the file contains and the name of the last section — it should be ${count}. If your count differs or you cannot see the whole file, say so and ask me to attach the file instead; do not continue from a partial read.
```

Apply the same pattern to the companion `setupPrompt()` with the companion bundle's count.

- [ ] **Step 3: CSS**

In `css()`:

```css
.door-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 16px; margin-top: 8px; }
.door { border: 1px solid rgba(10,34,66,0.15); padding: 18px 20px; background: rgba(255,255,255,0.5); }
.door h3 { margin: 0 0 6px; font-size: 1.05rem; }
.door p { margin: 0 0 12px; font-size: 0.95rem; }
```

- [ ] **Step 4: Verify**

Run:
```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site
npm run build
grep -c "How will your assistant get the file?" dist/index.html
grep -c "Attach the file — works everywhere" dist/index.html
grep -c 'headers the file contains' dist/index.html
```
Expected: `2`, `2`, `2` (workbench + companion each).

- [ ] **Step 5: Commit**

```bash
git add scripts/build-site.mjs
git commit -m "Replace prose fallback with two-door entry on workbench and companion"
```

---

### Task 15: Status caption, completeness assertion, PR B tests, ship

**Files:**
- Modify: `scripts/build-site.mjs` (caption in `buildWorkbenchMode()`; assertion near `getWorkbenchTools()` call site)
- Modify: `tests/site-contract.test.mjs` (PR B assertions)

- [ ] **Step 1: Status caption under the progression visual**

Immediately after the progression `<img …>` in `buildWorkbenchMode()`:

```html
      <p class="visual-status">The persona rows above are reference-matrix content, published as
        <a href="#wb-doc-why-the-matrix-is-a-hypothesis" data-wb-link>Hypothesis — awaiting NWC validation</a> — the concept note explains why.</p>
```

CSS: `.visual-status { font-size: 0.9rem; margin-top: 8px; opacity: 0.85; }`

- [ ] **Step 2: Completeness assertion**

In `scripts/build-site.mjs`, immediately after `const workbenchTools = getWorkbenchTools();`:

```javascript
const templateFiles = listWorkbenchFiles("templates");
const toolFilenames = new Set(workbenchTools.map((tool) => tool.filename));
templateFiles.forEach((name) => {
  if (!toolFilenames.has(name)) throw new Error(`templates/${name} has no getWorkbenchTools() card — add one`);
});
toolFilenames.forEach((name) => {
  if (!templateFiles.includes(name)) throw new Error(`getWorkbenchTools() lists ${name} but templates/${name} does not exist`);
});
```

- [ ] **Step 3: Test the assertion fires (test-of-the-test)**

Run:
```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site
touch ../workbench/templates/zz-assertion-probe.md
npm run build; echo "exit: $?"
rm ../workbench/templates/zz-assertion-probe.md
```
Expected: build FAILS with `templates/zz-assertion-probe.md has no getWorkbenchTools() card`; probe removed after.

- [ ] **Step 4: PR B contract tests**

Append to `tests/site-contract.test.mjs`:

```javascript
[
  "How will your assistant get the file?",
  "The Design Behind The Tools",
  'id="workbench-doc-view"',
  "Hypothesis — awaiting NWC validation",
  "Industry equivalent",
  "Framework tie:",
].forEach((needle) => {
  assert(html.includes(needle), `site should include ${needle}`);
});
assert((html.match(/How will your assistant get the file\?/gu) || []).length === 2, "both modes should offer the two doors");
assert(!/\]\(\.\.\//u.test(html), "rendered HTML should contain no raw relative markdown links");
assert(existsSync(join(dist, "assets", "workbench", "concepts", "README.md")), "concept downloads should be generated");
```

- [ ] **Step 5: Full verify + visual spot-check**

Run: `npm run build && npm test` — expected PASS. Then open `dist/index.html` in a browser: workbench mode shows doors, rendered diagnostic (with its placement-logic table as a real table), concepts cards, caption; click a `Concept:` link inside a rendered template — the note opens in the doc view.

- [ ] **Step 6: Commit, open PR B**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site
git add scripts/build-site.mjs tests/site-contract.test.mjs
git commit -m "Add status caption, completeness assertion, and reading-surface tests"
git push -u origin workbench-reading-surface
gh pr create --title "Reading surface: rendered documents, two-door entry, concepts real estate" --body "$(cat <<'EOF'
## Summary
- Templates and concept notes render as readable documents (raw markdown retained for copy/download); internal links navigate between documents, never dead
- Two-door entry (web-reading assistant vs attach-a-file) on workbench and companion modes
- Concepts get "The Design Behind The Tools" section; progression visual gains hypothesis-status caption
- Build fails loudly if a template lacks a card (completeness assertion)

## Test plan
- npm run build && npm test (rendered-surface needles, door count, no-dead-links check, download emission)
- Manual: doc navigation via concept links; doors on both modes; caption links to the matrix note
EOF
)"
```

- [ ] **Step 7: After merge — deploy**

Deploy per `site/docs/project-hygiene.md`. Verify live: workbench mode shows the doors and rendered documents.

---

## Execution Notes

- **Critical path:** Tasks 1–9 (workbench) → Task 10 (PR A) → deploy → **transcript gate begins** (next package: pilot-path templates first — diagnostic, assignment-design worksheet, the pilot candidate's template; inspection method in project memory). Tasks 11–15 (PR B) proceed in parallel with transcript work.
- **Task 9 is a human gate** — no PR opens until Jack has reviewed the notes' industry-equivalent claims.
- **NWC ledger dependency:** ledger item 1 (assistant environment) may later collapse the two doors to one — that is a cheap copy revision, not a reason to wait.
- **Renderer caution (Tasks 12–13):** the inline client script's exact function names around tool switching (~lines 1161–1200) must be read before editing — the plan describes the required behavior; match the code's actual naming.
- **Scannability check** happens inside Task 9's review, per spec.
