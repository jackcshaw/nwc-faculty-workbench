# AI Fluency Operationalization Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Make the Building AI Fluency framework usable inside the NWC Faculty Workbench as AI-runnable markdown artifacts, with the reference matrix instrumented for NWC validation, plus site distribution (bundle + workbench mode refresh).

**Architecture:** The AI assistant is the runtime; markdown is the program; the site is the distribution layer. Every template gets an "AI Facilitation Block" so any assistant can run it as a guided session. The site stops duplicating template text and reads it from the workbench repo; the build emits a single `workbench-context.md` bundle faculty can paste into any assistant.

**Tech Stack:** Plain markdown (workbench repo), Node ESM build script + contract tests (site repo, no framework — plain `node:fs` asserts).

**Spec:** `docs/superpowers/specs/2026-07-08-ai-fluency-operationalization-design.md`

## Global Constraints

- Two repos: **workbench** (`/Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench`, branch `ai-fluency-operationalization`, already exists) and **site** (`/Users/jackcshaw-2/dev/comprendo-clients/nwc/site`, create branch `workbench-ai-integration` in Task 12). Never commit to `main` in either repo.
- Commit messages must not contain any Claude or AI-authorship references (no Co-Authored-By trailers, no tool names).
- Public-safe only: no private NWC course material, no security/deployment claims, no personal data beyond the maintainer contact line specified in Task 8.
- Match workbench voice: short declarative sentences, Title Case H2s (`## What This Is For`), field lists as `- Label:` bullets, list items ending in semicolons only where existing files do.
- Matrix content is **hypothesis, not doctrine**: every persona row published with status `Hypothesis — awaiting NWC validation`. Never present matrix cells as validated.
- Facilitation blocks are facilitation, not surveillance: no logging requirements, no compliance language.
- The heading `## AI Facilitation Block` is a load-bearing string: Task 12's contract test and the bundle depend on it verbatim. Use exactly this heading in every template.
- Site contract tests run with: `cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site && npm run build && npm test`. Workbench has no test runner; verification is `grep` checks specified per task.
- Source visuals live at `/Users/jackcshaw-2/Downloads/NWC_AI Fluency in Higher Ed Final/` (path contains spaces — quote it).

---

### Task 1: Framework visual assets

**Files:**
- Create: `framework/assets/asking-to-supervising.svg`
- Create: `framework/assets/tools-behind-progression.svg`
- Create: `framework/assets/capability-compounds.svg`
- Create: `framework/assets/reference-matrix.svg`

**Interfaces:**
- Produces: the four SVG paths above, referenced by Task 2 (framework doc embeds) and Task 14 (site copies `asking-to-supervising.svg`).

- [ ] **Step 1: Copy and rename the four SVGs**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
mkdir -p framework/assets
SRC="/Users/jackcshaw-2/Downloads/NWC_AI Fluency in Higher Ed Final"
cp "$SRC/visual1_asking_to_supervising.svg" framework/assets/asking-to-supervising.svg
cp "$SRC/visual2_tools_behind_progression.svg" framework/assets/tools-behind-progression.svg
cp "$SRC/visual3_capability_compounds.svg" framework/assets/capability-compounds.svg
cp "$SRC/visual4_reference_matrix.svg" framework/assets/reference-matrix.svg
```

- [ ] **Step 2: Verify all four exist and are non-trivial SVGs**

Run: `ls -la framework/assets/ && head -c 200 framework/assets/asking-to-supervising.svg`
Expected: four files, each > 5KB, first bytes contain `<svg` (possibly after an XML declaration).

- [ ] **Step 3: Commit**

```bash
git add framework/assets
git commit -m "Add AI fluency framework visuals as SVG sources"
```

---

### Task 2: Framework reference doc

**Files:**
- Create: `framework/ai-fluency-progression.md`

**Interfaces:**
- Consumes: Task 1 asset paths.
- Produces: `framework/ai-fluency-progression.md` with H1 `# The AI Fluency Progression` and H2s `## The Six Phases`, `## The Tools Behind The Progression`, `## How Institutional Capability Compounds`, `## The Reference Matrix`, `## Validation Status And Changelog`. Task 3 appends `## Crosswalk` to this file. Task 13 bundles this file. Task 11 links it from the README.

- [ ] **Step 1: Write the file with exactly this content**

````markdown
# The AI Fluency Progression

This document is the workbench's public rendition of the **Building AI Fluency** framework (Jack C. Shaw, July 2026). It carries the framework's three solid structures — the six-phase progression, the tool progression, and the compounding staircase — plus the reference matrix, which is published here as a **hypothesis under validation**, not settled doctrine.

The framework applies to subject matter experts, higher education, and professional military education. Context, mission, and constraints change. The core progression stays the same. One thread runs through everything: **judgment stays human at every phase.**

## The Six Phases

![AI Fluency: From Asking to Supervising](assets/asking-to-supervising.svg)

Fluency grows as learners move from asking AI for help to delegating bounded work, judging results, codifying methods, and supervising AI-supported systems. The unit of work shifts from a conversation to a delegated, reviewable workflow.

| # | Phase | Name | The move |
| --- | --- | --- | --- |
| 1 | Ask | Responsible Use | Use AI safely and verify before relying on it. |
| 2 | Understand | AI for Learning | Build understanding before production. |
| 3 | Produce | Work Products | Delegate bounded work. Own the result. |
| 4 | Judge | Judgment / Revision | Improve the work before trusting it. |
| 5 | Codify | Repeatable Practice | Codify methods once. Stop re-explaining the task. **Where scale begins.** |
| 6 | Supervise | Supervised Systems | Direct bounded work. Keep judgment human. |

Phases 1–2 are **learning with AI**. Phases 3–4 are **working with AI**. Phases 5–6 are **governing AI-supported work**. The ordering reflects the research: unguided AI use can raise performance without producing learning, so safe use and learning come before production, and production is paired with judgment. Not every learner becomes a system builder. Every learner becomes a capable supervisor and judge of AI-supported work.

## The Tools Behind The Progression

![The Tools Behind the Progression](assets/tools-behind-progression.svg)

Five classes of systems, defined by what you do with them and **what remains when the work is done**.

| Tool | What remains when the work is done |
| --- | --- |
| Chatbot | Your understanding. |
| Agent | A work product. |
| Skill | A method. |
| Agent team | The harness — the structure of roles and review you supervised through. |
| AI-native knowledge layer | The institution's compounded knowledge. |

One warning: **the tool follows the maturity of the practice.** Reaching for an agent where a checklist would do is the most common failure.

## How Institutional Capability Compounds

![How Institutional Capability Compounds](assets/capability-compounds.svg)

The institutional argument runs as a staircase:

1. Reviewed practice becomes shared assets.
2. Shared assets make evaluation possible.
3. Evaluation makes governed systems trustworthy.
4. Passing the governance gate unlocks the payoff: every model the institution uses afterward draws on its own validated knowledge instead of starting over.

Models come and go. The knowledge compounds. The workbench's maturity levels already climb this staircase; see the Crosswalk below.

## The Reference Matrix

![AI Fluency Progression: Reference Matrix](assets/reference-matrix.svg)

The matrix maps what learners practice, what faculty teach and assess, and what the institution provides, at every phase. It is the working document for course and program design.

**Read the status legend first.** Each persona row below carries a validation status. `Hypothesis` means the cells are research-informed but have not yet survived contact with NWC faculty use. Workbench templates carry short matrix checks (in the after-action note and calibration protocol) so routine use produces the evidence that confirms, revises, or strikes these cells.

### Learners — practice it and produce with it

Status: Hypothesis — awaiting NWC validation.

| Phase | Learners |
| --- | --- |
| 1 Ask | Know when AI is answering vs acting. Ask, summarize, and draft. Verify claims and sources. Protect sensitive information. Check whether AI used files, tools, or external actions. |
| 2 Understand | Ask for explanations and worked examples. Quiz understanding and surface gaps. Track misconceptions. Restate ideas without the model. |
| 3 Produce | Produce bounded outputs: outlines, drafts, briefs, analyses, decision aids. State task, context, sources, and constraints. Define the standard for a usable output. |
| 4 Judge | Review outputs and intermediate work. Check sources and find weak claims. Compare alternatives and correct errors. Decide what to accept, revise, or reject. |
| 5 Codify | Turn recurring work into a reusable method: task brief, prompt pattern, checklist, source list, steps, review criteria, and stop rules. Keep examples of good and bad outputs. |
| 6 Supervise | Write task briefs, assign roles, set constraints. Inspect intermediate outputs and compare results. Define stop rules and escalate uncertainty. Integrate results into finished work. |

### Faculty — teach, coach, and assess it

Status: Hypothesis — awaiting NWC validation.

| Phase | Faculty |
| --- | --- |
| 1 Ask | Model safe use and safe delegation. Show the difference between an answer and an action. Require marking what was trusted, rejected, verified. |
| 2 Understand | Design AI support for understanding, not substitution. Compare AI explanations with course materials. Ask what changed in the student's thinking. |
| 3 Produce | Require process notes: what was delegated, revised, sourced. Assess the artifact and the choices behind it. Keep the student responsible for the result. |
| 4 Judge | Use critique logs, source audits, revision records. Run oral defense and checkpoint reviews. Grade judgment, not just polish. |
| 5 Codify | Have students document a workflow. Test it on more than one case, then peer review. Ask where human review belongs, and why. |
| 6 Supervise | Teach supervision before automation. Use small, bounded delegation exercises. Assess monitoring, integration, and judgment. |

### Institution — enables it, governs it, and compounds it

Status: Hypothesis — awaiting NWC validation.

| Phase | Institution |
| --- | --- |
| 1 Ask | Approved tools with role permissions. Data boundaries and safe-use examples. Guidance on when AI may inspect files or act. |
| 2 Understand | Trusted source packets. Course-level AI guidance. Sample learning prompts and faculty examples. |
| 3 Produce | Assignment expectations and disclosure norms. Rubrics and sample artifacts. Rules for acceptable AI-assisted production. |
| 4 Judge | Review standards and critique rubrics. Source-audit practices and approval gates. Norms for making AI-supported reasoning visible. |
| 5 Codify | Shared prompt and workflow libraries. Skill-card templates and approved knowledge sources. Examples of good and bad outputs; light evaluations. |
| 6 Supervise | Governed tools and bounded agent templates. Logs, permissions, escalation rules, review gates. Staff training, evaluation metrics, risk management. An AI-native knowledge layer feeding models, when warranted. |

## Validation Status And Changelog

Statuses: `Hypothesis — awaiting NWC validation` | `Field-tested — evidence from NWC use` | `Revised` | `Struck`.

Evidence arrives through the matrix checks in the after-action note template and the faculty calibration protocol. Changes to matrix cells are recorded here.

| Date | Row / cell | Change | Evidence |
| --- | --- | --- | --- |
| 2026-07-08 | All rows | Published at Hypothesis status. | Initial rendition from the Building AI Fluency package. |
````

- [ ] **Step 2: Verify structure**

Run: `grep -c '^## ' framework/ai-fluency-progression.md && grep -c 'Hypothesis — awaiting NWC validation' framework/ai-fluency-progression.md`
Expected: `5` H2 headings and `4` status strings (legend paragraph mentions `Hypothesis` unnumbered; the exact status string appears 3× as row status + 1× in the statuses line).

- [ ] **Step 3: Commit**

```bash
git add framework/ai-fluency-progression.md
git commit -m "Add framework reference doc with matrix at hypothesis status"
```

---

### Task 3: Crosswalk section

**Files:**
- Modify: `framework/ai-fluency-progression.md` (append at end of file)

**Interfaces:**
- Consumes: Task 2 file.
- Produces: `## Crosswalk` H2 in the framework doc. Tasks 4 and 11 reference the phase→template routing defined here; keep template filenames exactly as written.

- [ ] **Step 1: Append exactly this content to the end of `framework/ai-fluency-progression.md`**

```markdown

## Crosswalk

One table answers "where am I, and what do I use?" Start with the [phase placement diagnostic](../templates/phase-placement-diagnostic.md) if you do not know your phase.

### Templates To Phases

| Template | Primary phases | Matrix row it exercises |
| --- | --- | --- |
| [Phase placement diagnostic](../templates/phase-placement-diagnostic.md) | Entry point, all phases | Faculty |
| [Assignment design worksheet](../templates/assignment-design-worksheet.md) | 2–4 | Faculty |
| [Assessment and oral-defense rubric](../templates/assessment-and-oral-defense-rubric.md) | 4 | Faculty |
| [Flawed output library template](../templates/flawed-output-library-template.md) | 4 | Faculty, Institution |
| [Faculty calibration protocol](../templates/faculty-calibration-protocol.md) | 4–5 | Faculty, Institution |
| [Method card template](../templates/method-card-template.md) | 5 | Learners, Faculty |
| [Supervised delegation exercise](../templates/supervised-delegation-exercise.md) | 6 | Learners, Faculty |
| [Source kit template](../templates/source-kit-template.md) | 5–6 | Institution |
| [After-action note template](../templates/after-action-note-template.md) | 5 | Faculty, Institution |

### Maturity Levels To Phases And Staircase

| Workbench level | Phase(s) | Staircase step |
| --- | --- | --- |
| 1 Faculty Fluency Lab | Faculty practice phases 1–4 themselves | Reviewed practice |
| 2 Assignment Design | 2–3 | Reviewed practice |
| 3 Assessment Design | 4 | Reviewed practice |
| 4 Flawed Output Library | 4 | Shared assets |
| 5 Faculty Calibration | 4–5 | Evaluation |
| 6 Source Kits | 5–6 | Shared assets |
| 7 Institutional Memory (future) | 5, Institution row | Governed systems |
| 8 Context Curation (future) | 6, Institution row | Beyond the governance gate |

The workbench and the framework arrived at the same institutional shape independently: reviewed practice becomes shared assets, assets make evaluation possible, evaluation earns governed reuse. The maturity levels are the staircase, enacted.
```

- [ ] **Step 2: Verify links resolve**

Run: `grep -o '\.\./templates/[a-z-]*\.md' framework/ai-fluency-progression.md | sort -u`
Expected: nine template paths. Note `phase-placement-diagnostic.md`, `method-card-template.md`, and `supervised-delegation-exercise.md` do not exist yet — created in Tasks 4, 6, 7. All others must exist now: verify with `ls templates/`.

- [ ] **Step 3: Commit**

```bash
git add framework/ai-fluency-progression.md
git commit -m "Add crosswalk mapping templates and maturity levels to phases"
```

---

### Task 4: Phase placement diagnostic

**Files:**
- Create: `templates/phase-placement-diagnostic.md`

**Interfaces:**
- Produces: `templates/phase-placement-diagnostic.md` with H1 `# Phase Placement Diagnostic` and an `## AI Facilitation Block` H2. Task 13 bundles it; Task 14 registers it as the first site tool with filename `phase-placement-diagnostic.md`.

- [ ] **Step 1: Write the file with exactly this content**

````markdown
# Phase Placement Diagnostic

Use this diagnostic to find where an assignment, exercise, or course sits on the AI fluency progression, then pick the right workbench template. It takes about ten minutes with an AI assistant, or on paper.

The six phases are described in [the AI fluency progression](../framework/ai-fluency-progression.md): 1 Ask, 2 Understand, 3 Produce, 4 Judge, 5 Codify, 6 Supervise.

## AI Facilitation Block

If you are working on paper, skip this section. If you are using an AI assistant, give it this entire file and say: "Run this diagnostic with me."

Instructions for the AI assistant:

- Role: You are running a placement interview for a faculty member. The faculty member owns every judgment about their course. You ask, listen, and place. You do not redesign their assignment.
- Collect first: the course or seminar, the specific assignment or exercise, and what role AI currently plays in it (including "none").
- Process: Ask the placement questions below one at a time, in order. Stop early once the placement logic gives a clear answer. Push back once if an answer is vague, then accept the faculty member's call.
- Never: recommend tools or products; invent NWC policy or doctrine; treat a higher phase as better teaching — the right phase is the one that fits the task and the students; continue past an unresolved answer without flagging it.
- Finish: State the phase placement in one sentence, explain the routing in two or three sentences using the routing table, and return a short markdown note the faculty member can keep: assignment, placement, reasoning, recommended templates.

## Placement Questions

1. In this task, is AI answering questions, or doing work? (Answering only, or no AI yet → likely phase 1–2. Doing bounded work → phase 3 or higher.)
2. What must students own before AI enters — the frame, the purpose, the evidence standard? (If this is undefined, start at phase 2–3 design regardless of ambition.)
3. Is reviewing, verifying, or critiquing AI output an assessed part of the task? (Yes → phase 4 is in play.)
4. Does this task recur — across weeks, sections, or courses — often enough that the method could be written down once and reused? (Yes → phase 5.)
5. Would you trust students to direct a multi-step AI workflow with checkpoints you can inspect? (Yes, and phases 1–5 are in place → phase 6. If earlier phases are missing, place at the earliest missing phase instead.)
6. Who is being placed — the assignment, the students, or you? (This diagnostic places the assignment. Faculty can run it on their own practice too; the logic is the same.)

## Placement Logic

| If... | Placement |
| --- | --- |
| No deliberate AI role yet, or safety and verification habits are not established | 1 Ask |
| AI supports understanding, but production with AI is not assessed | 2 Understand |
| AI does bounded production work; students own task, context, and constraints | 3 Produce |
| Students must judge, verify, and revise AI output as assessed work | 4 Judge |
| The task recurs and the method is worth writing down once | 5 Codify |
| Students direct a bounded multi-step AI workflow under inspection | 6 Supervise |

Place at the **earliest phase that is not yet solid**. A phase 6 ambition with phase 1 habits is a phase 1 placement.

## Routing

| Placement | Use these templates |
| --- | --- |
| 1–2 | [Assignment design worksheet](assignment-design-worksheet.md) — decide where AI belongs and what stays AI-free. |
| 3 | [Assignment design worksheet](assignment-design-worksheet.md) + [source kit template](source-kit-template.md). |
| 4 | [Assessment and oral-defense rubric](assessment-and-oral-defense-rubric.md) + [flawed output library template](flawed-output-library-template.md). |
| 5 | [Method card template](method-card-template.md) + [faculty calibration protocol](faculty-calibration-protocol.md). |
| 6 | [Supervised delegation exercise](supervised-delegation-exercise.md). |
| After any run | [After-action note template](after-action-note-template.md). |

## Paper Worksheet

- Course or seminar:
- Assignment or exercise:
- Current AI role:
- Answers to questions 1–5:
- Placement:
- Reasoning:
- Templates to use next:
````

- [ ] **Step 2: Verify**

Run: `grep -c '^## ' templates/phase-placement-diagnostic.md && grep -c 'AI Facilitation Block' templates/phase-placement-diagnostic.md`
Expected: `5` and `1`.

- [ ] **Step 3: Commit**

```bash
git add templates/phase-placement-diagnostic.md
git commit -m "Add phase placement diagnostic as workbench front door"
```

---

### Task 5: Facilitation blocks — assignment worksheet and rubric

**Files:**
- Modify: `templates/assignment-design-worksheet.md` (insert after the intro paragraph, before `## 1. Learning Purpose`)
- Modify: `templates/assessment-and-oral-defense-rubric.md` (insert after the intro paragraph, before its first H2)

**Interfaces:**
- Produces: `## AI Facilitation Block` H2 in both files (verbatim heading — Task 12's test greps for it).

- [ ] **Step 1: Insert into `templates/assignment-design-worksheet.md`, immediately after the intro paragraph ("Use this worksheet when designing...") and before `## 1. Learning Purpose`:**

```markdown
## AI Facilitation Block

If you are working on paper, skip this section. If you are using an AI assistant, give it this entire file and say: "Facilitate this worksheet with me."

Instructions for the AI assistant:

- Role: You are facilitating an assignment-design session for a faculty member. The faculty member owns every pedagogical judgment. You ask, structure, and challenge. You never decide where AI belongs in their assignment.
- Collect first: the course or seminar, the learning objective, and the current assignment if one exists.
- Process: Walk the numbered sections in order, one question at a time. In section 3, actively defend developmental friction — if the faculty member proposes AI help there, ask what judgment the struggle was building. In section 5, make them commit to a sequence before moving on.
- Never: write the assignment yourself; fill in a field the faculty member has not decided; soften developmental friction to make the design easier; invent doctrine, policy, or sources; continue past an unresolved judgment call without flagging it.
- Finish: Return the completed worksheet as clean markdown, listing any fields the faculty member deferred.
```

- [ ] **Step 2: Insert into `templates/assessment-and-oral-defense-rubric.md`, immediately after the intro paragraph ("Use this rubric when...") and before its first H2:**

```markdown
## AI Facilitation Block

If you are working on paper, skip this section. If you are using an AI assistant, give it this entire file and say: "Help me prepare an assessment with this rubric."

Instructions for the AI assistant:

- Role: You are helping a faculty member prepare to assess AI-enabled student work and rehearse an oral defense. The faculty member owns every rating and every judgment about the student. You structure, probe, and rehearse.
- Collect first: the assignment being assessed and what evidence of ownership the faculty member already has.
- Process: Walk the dimensions one at a time and ask what evidence would distinguish a 2 from a 3 on each. Then rehearse: play the student in an oral defense using the question list, and afterward tell the faculty member which questions exposed the most.
- Never: rate a real student's work yourself; suggest that disclosure of AI use alone equals ownership; add rubric dimensions without being asked; treat polish as evidence of judgment.
- Finish: Return the faculty member's prepared rubric notes and the oral-defense question order they chose, as clean markdown.
```

- [ ] **Step 3: Verify**

Run: `grep -c 'AI Facilitation Block' templates/assignment-design-worksheet.md templates/assessment-and-oral-defense-rubric.md`
Expected: `1` in each file.

- [ ] **Step 4: Commit**

```bash
git add templates/assignment-design-worksheet.md templates/assessment-and-oral-defense-rubric.md
git commit -m "Add AI facilitation blocks to assignment worksheet and rubric"
```

---

### Task 6: Method card template (Phase 5)

**Files:**
- Create: `templates/method-card-template.md`

**Interfaces:**
- Produces: `templates/method-card-template.md` with H1 `# Method Card Template`. Task 13 bundles it; Task 14 registers it with filename `method-card-template.md`.

- [ ] **Step 1: Write the file with exactly this content**

````markdown
# Method Card Template

Use this template to turn a recurring AI-enabled task into a reusable method. A method card is what remains when the work is done well more than once: the task brief, the steps, the review criteria, and the stop rules. This is phase 5 of the fluency progression — where scale begins, because people stop re-explaining the task.

Codify only what has worked at least twice. A method card for a task you have done once is a guess wearing a uniform.

## AI Facilitation Block

If you are working on paper, skip this section. If you are using an AI assistant, give it this entire file and say: "Help me write a method card."

Instructions for the AI assistant:

- Role: You are helping a faculty member (or student, if assigned) codify a recurring task they already know how to do. Their tacit knowledge is the content; you extract and structure it. You are not designing a new workflow.
- Collect first: the recurring task, how many times they have done it with AI support, and what went wrong at least once.
- Process: If the task has run fewer than two times, say so and stop — recommend running it again first. Otherwise walk the sections in order. Push hardest on review criteria and stop rules; "looks good" is not a criterion.
- Never: invent steps the person has not actually used; write review criteria for a domain you are guessing at; skip the bad example — the failure case is what makes the card teachable.
- Finish: Return the completed method card as clean markdown and ask where it should live so someone else can find it.

## Card Metadata

- Method name:
- Owner:
- Date:
- Course, seminar, or task family:
- Public, internal, or restricted:
- Times this method has been run:

## Task Brief

- Goal of the task:
- Who the output is for:
- Output format and length:
- What the human must supply each run (context, sources, constraints):

## When To Use — And Not

- Use when:
- Do not use when:
- Simpler alternative that sometimes suffices (checklist, template, no AI):

## Steps

Number each step. Mark the AI role in each: none, draft, challenge, compare, format.

1.
2.
3.

## Review Criteria

What makes the output acceptable? Be concrete enough that a colleague could apply these without you.

- Accuracy check:
- Source or evidence standard:
- Format standard:
- Rejection triggers:

## Stop Rules

When must the method halt and hand back to human judgment?

- Stop when:
- Escalate to whom:

## Examples

- One good output (paste or link, with one line on why it passed):
- One bad output (paste or link, with one line on why it failed):

## Human Review Points

- Where a human must review before the output is used:
- Who owns the result:

## Revision Log

| Date | Change | Reason |
| --- | --- | --- |
|  |  |  |
````

- [ ] **Step 2: Verify**

Run: `grep -c '^## ' templates/method-card-template.md`
Expected: `10`.

- [ ] **Step 3: Commit**

```bash
git add templates/method-card-template.md
git commit -m "Add method card template for phase 5 codification"
```

---

### Task 7: Supervised delegation exercise template (Phase 6)

**Files:**
- Create: `templates/supervised-delegation-exercise.md`

**Interfaces:**
- Produces: `templates/supervised-delegation-exercise.md` with H1 `# Supervised Delegation Exercise`. Task 13 bundles it; Task 14 registers it with filename `supervised-delegation-exercise.md`.

- [ ] **Step 1: Write the file with exactly this content**

````markdown
# Supervised Delegation Exercise

Use this template to design a bounded exercise where students direct a multi-step AI workflow and faculty assess whether judgment survives delegation. This is phase 6 of the fluency progression. Teach supervision before automation: students should have practiced phases 1–5 on this kind of task first.

The exercise is not "build an agent." It is "supervise delegated work you remain accountable for."

## AI Facilitation Block

If you are working on paper, skip this section. If you are using an AI assistant, give it this entire file and say: "Help me design a supervised delegation exercise."

Instructions for the AI assistant:

- Role: You are helping a faculty member design a phase 6 exercise. The faculty member owns the pedagogy, the task choice, and the assessment standard. You structure and stress-test the design.
- Collect first: the course, the strategic task being delegated, and what evidence exists that students have phase 4–5 habits on this task.
- Process: Walk the sections in order. Stress-test the boundedness: if the delegated task cannot fail safely inside one session, push for a smaller task. Make the faculty member define the escalation rule before the inspection points.
- Never: design the exercise around a specific vendor or product; let "students supervise AI" become "students watch AI"; write assessment criteria that reward output volume over judgment; imply operational or classified use.
- Finish: Return the completed exercise design as clean markdown, with an explicit list of what could go wrong in the first run.

## Learning Purpose

- Course or seminar:
- Judgment this exercise develops:
- Why supervision, not direct production, is the right practice here:
- Prerequisite phases students have already practiced, and the evidence:

## The Delegated Task

Bounded, multi-step, inspectable, safe to fail.

- Task:
- Why it is safe to delegate in a classroom:
- Number of steps or stages:
- Time box:
- What a good final product looks like:

## Task Brief Students Must Write

Students write this before touching AI. Faculty review it first.

- Goal and audience:
- Steps and the AI role in each:
- Sources allowed and excluded:
- Output standard:
- Stop rules and escalation conditions:

## Intermediate Inspection Points

- What students must inspect mid-run (intermediate outputs, source use, drift from the brief):
- What evidence of each inspection they record:
- What faculty observe during the run:

## Stop And Escalation Rules

- Conditions that must halt the run:
- What students do when uncertain (escalate, verify, or refuse):
- What may never be delegated in this exercise:

## Final Integration Without AI

- What students must do unaided after the run (integrate, judge, defend):
- Final judgment students state in first person:

## Assessment: Does Judgment Survive Delegation?

| Dimension | Thin (1) | Strong (4) |
| --- | --- | --- |
| Task brief quality | Vague goal, no stop rules | Bounded goal, explicit criteria and stop rules |
| Inspection rigor | Accepted intermediate work unread | Caught and corrected a real problem mid-run |
| Stop-rule discipline | Kept going on momentum | Halted or escalated when conditions were met |
| Ownership of result | "The AI did it" | Defends the final judgment in first person |
| Transfer | Cannot adapt the method | Explains how the brief changes for a changed case |

## Oral-Defense Questions

- What did you inspect, and what did you find?
- What did the system get wrong, and when did you notice?
- What would have triggered your stop rule, and did anything come close?
- State the final judgment as your own. What in it did you change from the AI's version?
- How would your task brief change for [a changed case]?

## Trace Artifact

Keep it lean: task brief, inspection notes, stop-rule events, final judgment, one paragraph on what the student would change.
````

- [ ] **Step 2: Verify**

Run: `grep -c '^## ' templates/supervised-delegation-exercise.md`
Expected: `10`.

- [ ] **Step 3: Commit**

```bash
git add templates/supervised-delegation-exercise.md
git commit -m "Add supervised delegation exercise template for phase 6"
```

---

### Task 8: Matrix checks and feedback return path

**Files:**
- Modify: `templates/after-action-note-template.md` (two inserts)
- Modify: `templates/faculty-calibration-protocol.md` (one insert)

**Interfaces:**
- Consumes: phase names from Task 2.
- Produces: `## Matrix Check` H2 in both files. The framework doc changelog (Task 2) names these as its evidence sources.

- [ ] **Step 1: In `templates/after-action-note-template.md`, insert after the `## Next Run` section (end of file):**

```markdown

## Matrix Check

Three lines for the fluency progression. See [the reference matrix](../framework/ai-fluency-progression.md). All fields optional.

- Phase this exercise operated at (1 Ask / 2 Understand / 3 Produce / 4 Judge / 5 Codify / 6 Supervise):
- Did the matrix's expectations for learners and faculty at this phase match what happened? What did not:
- One change you would make to that matrix row:

## Sending This Note

Completed notes make the workbench better. Send a copy (with private course material removed) to the workbench maintainer: jackcshaw@gmail.com.
```

Note: the maintainer address is deliberately explicit and swappable; Jack confirms or changes it before merge.

- [ ] **Step 2: In `templates/faculty-calibration-protocol.md`, insert after the `## Calibration Note Template` section and before `## Review Question`:**

```markdown
## Matrix Check

Three lines for the fluency progression. See [the reference matrix](../framework/ai-fluency-progression.md). All fields optional.

- Phase the reviewed work operated at (1 Ask / 2 Understand / 3 Produce / 4 Judge / 5 Codify / 6 Supervise):
- Did faculty expectations at this phase match the matrix's faculty row? Where not:
- One change you would make to that matrix row:
```

- [ ] **Step 3: Verify**

Run: `grep -c 'Matrix Check' templates/after-action-note-template.md templates/faculty-calibration-protocol.md`
Expected: `1` in each file.

- [ ] **Step 4: Commit**

```bash
git add templates/after-action-note-template.md templates/faculty-calibration-protocol.md
git commit -m "Add matrix checks and feedback return path to capture instruments"
```

---

### Task 9: Facilitation blocks — remaining four templates

**Files:**
- Modify: `templates/flawed-output-library-template.md` (insert after intro paragraph, before first H2)
- Modify: `templates/faculty-calibration-protocol.md` (insert after intro paragraph, before `## Purpose`)
- Modify: `templates/source-kit-template.md` (insert after intro paragraph, before `## 1. Overview`)
- Modify: `templates/after-action-note-template.md` (insert after intro paragraph, before `## Exercise Information`)

**Interfaces:**
- Produces: `## AI Facilitation Block` H2 in all four files. After this task, all eight templates plus the diagnostic contain the verbatim heading (Task 12 test greps it).

- [ ] **Step 1: Insert into `templates/flawed-output-library-template.md`:**

```markdown
## AI Facilitation Block

If you are working on paper, skip this section. If you are using an AI assistant, give it this entire file and say: "Help me build a flawed output."

Instructions for the AI assistant:

- Role: You are helping a faculty member create a polished but strategically flawed AI output for teaching. The faculty member chooses the flaw and owns the instructor key. You draft the polish; they design the trap.
- Collect first: the course, the case or topic, and which flaw type from the list the faculty member wants students to find.
- Process: Have the faculty member specify the flaw and the stronger frame first, then draft the student-facing artifact so the flaw survives a surface reading. Then complete the instructor key together.
- Never: choose the flaw type yourself; make the flaw a factual error a spell-check mindset would catch — the point is strategic, not clerical; write the oral-defense questions without the faculty member's approval.
- Finish: Return the complete library entry as clean markdown: metadata, student-facing artifact, instructor key, and oral-defense questions.
```

- [ ] **Step 2: Insert into `templates/faculty-calibration-protocol.md`:**

```markdown
## AI Facilitation Block

If you are working on paper, skip this section. If you are using an AI assistant, give it this entire file and say: "Help me run a calibration session."

Instructions for the AI assistant:

- Role: You are supporting a faculty calibration session. The faculty are the judges; you are the scribe and the timekeeper. You surface disagreement; you never resolve it.
- Collect first: the artifact under review and how many faculty are participating.
- Process: Keep individual reviews independent — do not share one reviewer's diagnosis with another before step 2. In step 2, present convergence and divergence neutrally. In step 3, record the shared minimum standard in the faculty's own words.
- Never: score the artifact yourself; smooth over a disagreement to reach consensus; suggest that divergent faculty judgment is a problem to eliminate — the protocol says legitimate range is an outcome.
- Finish: Return the completed calibration note as clean markdown.
```

- [ ] **Step 3: Insert into `templates/source-kit-template.md`:**

```markdown
## AI Facilitation Block

If you are working on paper, skip this section. If you are using an AI assistant, give it this entire file and say: "Help me package a source kit."

Instructions for the AI assistant:

- Role: You are helping a faculty member curate the context packet for an AI-enabled exercise. The faculty member decides what is in, what is out, and where the boundaries sit. You organize and pressure-test.
- Collect first: the exercise, the anchor materials that exist, and the public/internal/restricted status of each.
- Process: Walk the sections in order. Pressure-test section 4 hardest: for each source, ask whether it is allowed, excluded, or missing. Flag anything that looks like private course material heading into a public kit.
- Never: add sources the faculty member has not named; write the AI-role boundaries yourself; treat a file dump as a kit — if the kit lacks standards and boundaries, say so.
- Finish: Return the completed source kit as clean markdown with an explicit public-safety note on anything borderline.
```

- [ ] **Step 4: Insert into `templates/after-action-note-template.md`:**

```markdown
## AI Facilitation Block

If you are working on paper, skip this section. If you are using an AI assistant, give it this entire file and say: "Debrief this exercise with me."

Instructions for the AI assistant:

- Role: You are debriefing a faculty member after an AI-enabled exercise, while memory is fresh. Their observations are the content; you draw them out and structure them.
- Collect first: which exercise, when it ran, and the single strongest and weakest moment.
- Process: Walk the sections in order, but follow energy — if the faculty member wants to start with what failed, start there and backfill. Push for specifics: one named frame error beats three generalities. End with the matrix check.
- Never: soften a failure into a lesson before the faculty member has described it plainly; propose updates to shared artifacts as decided — the proposals table requires faculty approval; include student names or private course material in the note.
- Finish: Return the completed note as clean markdown and remind the faculty member where to send it.
```

- [ ] **Step 5: Verify all templates now carry the block**

Run: `grep -l 'AI Facilitation Block' templates/*.md | wc -l`
Expected: `9` (six original templates + diagnostic + method card + supervised delegation).

- [ ] **Step 6: Commit**

```bash
git add templates/
git commit -m "Add AI facilitation blocks to remaining templates"
```

---

### Task 10: Workbench source kit and transcripts directory

**Files:**
- Create: `workbench-source-kit.md` (repo root)
- Create: `examples/transcripts/README.md`

**Interfaces:**
- Produces: `workbench-source-kit.md` with H1 `# Workbench Source Kit`. Task 13 uses it as the bundle's OPERATING RULES section.

- [ ] **Step 1: Write `workbench-source-kit.md` with exactly this content**

````markdown
# Workbench Source Kit

This is the NWC Faculty Workbench packaged with its own source-kit template — the workbench dogfooding its Level 6. Give this file (or the full workbench bundle) to any AI assistant so it can help faculty use the toolkit.

## 1. Overview

- Source kit title: NWC Faculty Workbench
- Faculty owner: Workbench maintainer
- Public, internal, or restricted: Public
- Intended exercise: Faculty design, assessment, codification, and supervision of AI-enabled PME practice.

## 2. Learning Purpose

- Objective: Faculty design assignments, assessments, methods, and exercises where students use AI without surrendering purpose, frame, reliance, accountability, or judgment.
- Why AI belongs: Every template carries an AI Facilitation Block so an assistant can run it as a guided session. The faculty member experiences supervised AI-mediated work while designing it.
- What faculty must own: Every pedagogical judgment. The assistant asks, structures, and challenges; it never decides.

## 3. Anchor Materials

- [The AI fluency progression](framework/ai-fluency-progression.md) — six phases, tools, staircase, and the reference matrix (hypothesis status).
- [Phase placement diagnostic](templates/phase-placement-diagnostic.md) — start here to find the right phase and template.
- The nine templates in [templates/](templates/), each with its own facilitation block.
- [README.md](README.md) — what the workbench is and is not.

## 4. Allowed And Excluded Sources

### Allowed

- Everything in this repository.
- Materials the faculty member supplies during a session.

### Excluded

- Private NWC course material — never absorb it into public artifacts.
- Invented doctrine, policy, or sources.
- Claims that the reference matrix is validated, or that secure deployment is solved.

## 5. AI Role

What the assistant may do:

- run the phase placement diagnostic;
- facilitate any template per its AI Facilitation Block;
- compare templates and recommend which fits the faculty member's problem;
- structure, challenge, and draft formatting.

What the assistant may not do:

- make pedagogical decisions for faculty;
- soften developmental friction;
- treat matrix cells as validated;
- turn facilitation into surveillance or compliance paperwork;
- move private material into public artifacts.

## 6. Faculty Review Notes

- What faculty should observe: whether the assistant kept them in the judgment seat during facilitation.
- Common failure mode: the assistant fills in fields to be helpful. Every filled field the faculty member did not decide is a defect.
- After any real use: complete an [after-action note](templates/after-action-note-template.md), including the matrix check.
````

- [ ] **Step 2: Write `examples/transcripts/README.md` with exactly this content**

```markdown
# Facilitation Transcripts

Worked examples of templates being facilitated by an AI assistant.

## The Habit

Before a template's facilitation block is considered ship-ready, run it through at least two different assistants (for example ChatGPT and Claude) as a realistic faculty session. Keep one good transcript per template here, named `<template-name>--<assistant>.md`.

Scrub before committing: no private course material, no student names, no account details. Transcripts are teaching examples, not compliance records.

## Status

No transcripts yet. They are added as templates are tested against real assistants during pilot preparation.
```

- [ ] **Step 3: Verify**

Run: `grep -c '^## ' workbench-source-kit.md && ls examples/transcripts/`
Expected: `6` and `README.md`.

- [ ] **Step 4: Commit**

```bash
git add workbench-source-kit.md examples/
git commit -m "Add workbench source kit and transcripts directory"
```

---

### Task 11: README and AGENTS.md refresh

**Files:**
- Modify: `README.md` (Start Here section and table)
- Modify: `AGENTS.md` (Operating Principles list)

**Interfaces:**
- Consumes: filenames from Tasks 4, 6, 7, 10 and phase mappings from Task 3.

- [ ] **Step 1: In `README.md`, replace the `## Start Here` section (heading plus its table) with:**

```markdown
## Start Here

**With an AI assistant (recommended):** give your assistant the [workbench source kit](workbench-source-kit.md) — or the full workbench bundle from the public site — and say: "Run the phase placement diagnostic with me." The assistant will place your assignment on the fluency progression and route you to the right template. Every template contains an AI Facilitation Block, so the assistant can run it as a guided session.

**On paper:** every template works as a plain worksheet. Start with the table below.

| I want to... | Use this | Phase |
| --- | --- | --- |
| Find where to start | [templates/phase-placement-diagnostic.md](templates/phase-placement-diagnostic.md) | Entry |
| Design an AI-enabled assignment | [templates/assignment-design-worksheet.md](templates/assignment-design-worksheet.md) | 2–4 |
| Assess ownership and run oral defense | [templates/assessment-and-oral-defense-rubric.md](templates/assessment-and-oral-defense-rubric.md) | 4 |
| Build a reusable flawed AI output | [templates/flawed-output-library-template.md](templates/flawed-output-library-template.md) | 4 |
| Calibrate faculty judgment | [templates/faculty-calibration-protocol.md](templates/faculty-calibration-protocol.md) | 4–5 |
| Codify a recurring method | [templates/method-card-template.md](templates/method-card-template.md) | 5 |
| Design a supervised delegation exercise | [templates/supervised-delegation-exercise.md](templates/supervised-delegation-exercise.md) | 6 |
| Package exercise context for an AI assistant | [templates/source-kit-template.md](templates/source-kit-template.md) | 5–6 |
| Capture lessons after an exercise | [templates/after-action-note-template.md](templates/after-action-note-template.md) | 5 |
| See the full fluency progression and matrix | [framework/ai-fluency-progression.md](framework/ai-fluency-progression.md) | All |
| Understand future context curation | [roadmap/context-curation-roadmap.md](roadmap/context-curation-roadmap.md) | Future |
| Keep surfaces separate | [docs/ecosystem-boundaries.md](docs/ecosystem-boundaries.md) | — |

The phase column refers to the six-phase AI fluency progression (Ask, Understand, Produce, Judge, Codify, Supervise). The workbench's maturity levels below and the progression map onto each other; see the [crosswalk](framework/ai-fluency-progression.md#crosswalk).
```

- [ ] **Step 2: In `README.md`, update the `## Build Now` list to include the three new artifacts** — replace the existing bullet list under `## Build Now` with:

```markdown
- phase placement diagnostic;
- assignment design worksheet;
- assessment and oral-defense rubric;
- flawed-output library template;
- faculty calibration protocol;
- method card template;
- supervised delegation exercise;
- source-kit template;
- after-action note template.
```

- [ ] **Step 3: In `AGENTS.md`, add these bullets to the top of the `## Operating Principles` list (before the assignment-design bullet):**

```markdown
- Start with [workbench-source-kit.md](workbench-source-kit.md) if you were given only this file: it defines your role and boundaries.
- Use [templates/phase-placement-diagnostic.md](templates/phase-placement-diagnostic.md) when the faculty member does not know where to start.
- Use [templates/method-card-template.md](templates/method-card-template.md) when a recurring AI-enabled task is worth codifying.
- Use [templates/supervised-delegation-exercise.md](templates/supervised-delegation-exercise.md) when faculty are designing bounded student supervision of multi-step AI work.
- Use [framework/ai-fluency-progression.md](framework/ai-fluency-progression.md) to explain phases, tools, the staircase, or the matrix. Present matrix cells as hypotheses under validation, never as doctrine.
- Every template contains an AI Facilitation Block. Follow it exactly when facilitating.
```

- [ ] **Step 4: Verify**

Run: `grep -c 'phase-placement-diagnostic' README.md AGENTS.md && grep -c 'ai-fluency-progression' README.md AGENTS.md`
Expected: at least `1` per file for each pattern. Also verify every link target exists: `grep -o '](\([a-z/.-]*\.md\)' README.md AGENTS.md | sed 's/](//' | sort -u | while read f; do [ -f "$f" ] || echo "MISSING: $f"; done` — expect no output.

- [ ] **Step 5: Commit**

```bash
git add README.md AGENTS.md
git commit -m "Route README and agent instructions through the fluency progression"
```

---

### Task 12: Site reads workbench templates from the workbench repo

**Files:**
- Modify: `site/scripts/build-site.mjs` (add `workbenchRepoPath` + `readRequiredWorkbenchFile`; rewrite `getWorkbenchTools()` to read markdown from files)
- Modify: `site/tests/site-contract.test.mjs` (add assertion)

Work in the **site repo**. Create the branch first.

**Interfaces:**
- Consumes: workbench repo template files including `## AI Facilitation Block` headings (Tasks 5, 9).
- Produces: `readRequiredWorkbenchFile(relativePath)` and a `getWorkbenchTools()` whose entries keep fields `{id, title, cardTitle, cardDesc, cardAction, filename, useNote, markdown}` — Task 13 and Task 14 call both.

- [ ] **Step 1: Create the site branch**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site
git checkout main && git pull && git checkout -b workbench-ai-integration
```

- [ ] **Step 2: Add the failing contract test** — in `site/tests/site-contract.test.mjs`, after the block that asserts workbench asset files exist (the `assets/workbench/*.md` forEach), add:

```js
assert(
  html.includes("AI Facilitation Block"),
  "workbench templates should be read from the workbench repo (facilitation blocks present)",
);
```

- [ ] **Step 3: Run tests to verify the new assertion fails**

Run: `npm run build && npm test`
Expected: FAIL with "workbench templates should be read from the workbench repo" (the inlined template copies predate facilitation blocks).

- [ ] **Step 4: Implement repo reading in `site/scripts/build-site.mjs`**

Near the top, after the `companionRepoPath` line, add:

```js
const workbenchRepoPath = process.env.WORKBENCH_REPO_PATH || join(root, "..", "workbench");
```

After `readRequiredCompanionFile`, add:

```js
function readRequiredWorkbenchFile(relativePath) {
  const filePath = join(workbenchRepoPath, relativePath);
  if (!existsSync(filePath)) {
    throw new Error(`Missing workbench file: ${filePath}. Set WORKBENCH_REPO_PATH to the workbench repo checkout.`);
  }
  return readFileSync(filePath, "utf8");
}
```

Rewrite `getWorkbenchTools()`: keep every existing entry's `id`, `title`, `cardTitle`, `cardDesc`, `cardAction`, `filename`, `useNote` exactly as they are, but delete every inline `markdown:` template literal and replace with a file read. The resulting function:

```js
function getWorkbenchTools() {
  const tools = [
    {
      id: "assignment-design",
      title: "Assignment Design Worksheet",
      cardTitle: "Assignment design",
      cardDesc: "Decide where AI belongs and what students must own.",
      cardAction: "Open worksheet",
      filename: "assignment-design-worksheet.md",
      useNote: "Use this as a working document with faculty before revising an assignment.",
    },
    {
      id: "assessment",
      title: "Assessment And Oral-Defense Rubric",
      cardTitle: "Assessment",
      cardDesc: "Review purpose, frame, reliance, accountability, and transfer.",
      cardAction: "Open rubric",
      filename: "assessment-and-oral-defense-rubric.md",
      useNote: "Use this to decide what evidence faculty need beyond the finished artifact.",
    },
    {
      id: "flawed-output",
      title: "Flawed Output Library Template",
      cardTitle: "Flawed outputs",
      cardDesc: "Create polished AI work with a hidden strategic problem.",
      cardAction: "Open template",
      filename: "flawed-output-library-template.md",
      useNote: "Use this to build examples that fail under strategic questioning, not surface reading.",
    },
    {
      id: "source-kit",
      title: "Source Kit Template",
      cardTitle: "Source kits",
      cardDesc: "Package materials and boundaries for an AI-assisted exercise.",
      cardAction: "Open template",
      filename: "source-kit-template.md",
      useNote: "Use this to tell an AI assistant what materials, standards, and boundaries matter.",
    },
    {
      id: "calibration",
      title: "Faculty Calibration Protocol",
      cardTitle: "Faculty calibration",
      cardDesc: "Compare how faculty diagnose the same AI-assisted work.",
      cardAction: "Open protocol",
      filename: "faculty-calibration-protocol.md",
      useNote: "Use this when faculty need to make tacit judgment easier to explain and reuse.",
    },
    {
      id: "after-action",
      title: "After-Action Note Template",
      cardTitle: "After-action note",
      cardDesc: "Save what worked, what failed, and what faculty should change.",
      cardAction: "Open note",
      filename: "after-action-note-template.md",
      useNote: "Use this after running an exercise so lesson rationale and faculty judgment do not disappear.",
    },
  ];
  return tools.map((tool) => ({
    ...tool,
    markdown: readRequiredWorkbenchFile(join("templates", tool.filename)),
  }));
}
```

- [ ] **Step 5: Build and test**

Run: `npm run build && npm test`
Expected: PASS, including the new facilitation-block assertion. If an existing needle assertion fails because the repo template text differs from the old inlined copy, the repo version is source of truth — update the stale needle in the test, not the template.

- [ ] **Step 6: Commit**

```bash
git add scripts/build-site.mjs tests/site-contract.test.mjs
git commit -m "Read workbench templates from the workbench repo instead of inlining

Ends template drift between site and workbench; facilitation blocks
now flow through automatically."
```

---

### Task 13: Workbench AI bundle

**Files:**
- Modify: `site/scripts/build-site.mjs` (add `buildWorkbenchContext()`, emit `workbench-context.md`)
- Modify: `site/tests/site-contract.test.mjs` (bundle assertions)

**Interfaces:**
- Consumes: `readRequiredWorkbenchFile` (Task 12); workbench files from Tasks 2, 4, 10.
- Produces: `dist/assets/workbench-context.md` and const `workbenchContextFilename = "workbench-context.md"` / `workbenchContextUrl` — Task 14 links both.

- [ ] **Step 1: Add failing contract tests** — in `site/tests/site-contract.test.mjs`, after the companion-context assertions block, add:

```js
const workbenchContextPath = join(dist, "assets", "workbench-context.md");
assert(existsSync(workbenchContextPath), "workbench context bundle should exist after build");
assert(statSync(workbenchContextPath).size > 30_000, "workbench context bundle should contain the workbench source materials");
const workbenchContext = readFileSync(workbenchContextPath, "utf8");
[
  "SECTION: OPERATING RULES",
  "SECTION: FRAMEWORK",
  "SECTION: PHASE PLACEMENT DIAGNOSTIC",
  "SECTION: ASSIGNMENT DESIGN WORKSHEET",
  "SECTION: ASSESSMENT AND ORAL-DEFENSE RUBRIC",
  "SECTION: FLAWED OUTPUT LIBRARY TEMPLATE",
  "SECTION: FACULTY CALIBRATION PROTOCOL",
  "SECTION: METHOD CARD TEMPLATE",
  "SECTION: SUPERVISED DELEGATION EXERCISE",
  "SECTION: SOURCE KIT TEMPLATE",
  "SECTION: AFTER-ACTION NOTE TEMPLATE",
  "AI Facilitation Block",
  "Hypothesis — awaiting NWC validation",
].forEach((needle) => {
  assert(workbenchContext.includes(needle), `workbench bundle should include ${needle}`);
});
```

- [ ] **Step 2: Run tests to verify failure**

Run: `npm run build && npm test`
Expected: FAIL with "workbench context bundle should exist after build".

- [ ] **Step 3: Implement the bundle in `site/scripts/build-site.mjs`**

Near `companionContextFilename`, add:

```js
const workbenchContextFilename = "workbench-context.md";
const workbenchContextUrl = `${siteUrl}/assets/${workbenchContextFilename}`;
```

After `buildCompanionContext()`, add:

```js
function buildWorkbenchContext() {
  const sections = [
    ["OPERATING RULES", "workbench-source-kit.md"],
    ["FRAMEWORK", "framework/ai-fluency-progression.md"],
    ["PHASE PLACEMENT DIAGNOSTIC", "templates/phase-placement-diagnostic.md"],
    ["ASSIGNMENT DESIGN WORKSHEET", "templates/assignment-design-worksheet.md"],
    ["ASSESSMENT AND ORAL-DEFENSE RUBRIC", "templates/assessment-and-oral-defense-rubric.md"],
    ["FLAWED OUTPUT LIBRARY TEMPLATE", "templates/flawed-output-library-template.md"],
    ["FACULTY CALIBRATION PROTOCOL", "templates/faculty-calibration-protocol.md"],
    ["METHOD CARD TEMPLATE", "templates/method-card-template.md"],
    ["SUPERVISED DELEGATION EXERCISE", "templates/supervised-delegation-exercise.md"],
    ["SOURCE KIT TEMPLATE", "templates/source-kit-template.md"],
    ["AFTER-ACTION NOTE TEMPLATE", "templates/after-action-note-template.md"],
  ];

  const parts = [
    "# NWC Faculty Workbench - Context Bundle",
    "",
    "Read this whole file before answering. Sections are marked with clear SECTION headers.",
    "Start from the OPERATING RULES. Every template contains an AI Facilitation Block; follow it exactly when facilitating.",
  ];

  sections.forEach(([label, relativePath]) => {
    parts.push("", "", `# ===== SECTION: ${label} =====`, "", readRequiredWorkbenchFile(relativePath).trim());
  });

  return parts.join("\n");
}
```

In the top-level build sequence, after the `companionContextFilename` write, add:

```js
writeFileSync(join(assetsDir, workbenchContextFilename), buildWorkbenchContext() + "\n", "utf8");
```

- [ ] **Step 4: Build and test**

Run: `npm run build && npm test`
Expected: PASS.

- [ ] **Step 5: Commit**

```bash
git add scripts/build-site.mjs tests/site-contract.test.mjs
git commit -m "Emit workbench-context.md bundle for one-paste AI assistant use"
```

---

### Task 14: Site workbench mode refresh

**Files:**
- Modify: `site/scripts/build-site.mjs` (three new tools, setup prompt, hero copy, progression visual copy)
- Modify: `site/tests/site-contract.test.mjs` (new assertions)

**Interfaces:**
- Consumes: `workbenchContextUrl` and `workbenchContextFilename` (Task 13); `getWorkbenchTools()` shape (Task 12); workbench files from Tasks 1, 4, 6, 7.
- Produces: final site surface; nothing downstream.

- [ ] **Step 1: Add failing contract tests** — in `site/tests/site-contract.test.mjs`, extend the workbench asset list forEach to include the three new assets, and add surface assertions:

```js
[
  "assets/workbench/phase-placement-diagnostic.md",
  "assets/workbench/method-card-template.md",
  "assets/workbench/supervised-delegation-exercise.md",
].forEach((asset) => {
  const filename = asset.split("/").pop();
  assert(html.includes(filename), `site should include ${filename}`);
  assert(existsSync(join(dist, asset)), `${asset} should be generated`);
});

assert(html.includes('data-tool-id="phase-diagnostic"'), "workbench should lead with the phase placement diagnostic");
assert(html.includes('data-copy-target="workbench-setup-prompt"'), "workbench should include a copyable setup prompt");
assert(html.includes("assets/workbench-context.md"), "workbench should link the context bundle");
assert(
  (html.match(/https:\/\/judgmentlab\.net\/assets\/workbench-context\.md/g) || []).length >= 1,
  "workbench setup prompt should point at the workbench bundle",
);
assert(existsSync(join(dist, "assets", "asking-to-supervising.svg")), "progression visual should be copied into assets");
assert(html.includes("assets/asking-to-supervising.svg"), "workbench page should show the fluency progression visual");
```

- [ ] **Step 2: Run tests to verify failure**

Run: `npm run build && npm test`
Expected: FAIL on the phase-placement-diagnostic asset assertion first.

- [ ] **Step 3: Add the three tools** — in `getWorkbenchTools()`, insert this entry at the **start** of the `tools` array (it becomes the default-selected tool):

```js
    {
      id: "phase-diagnostic",
      title: "Phase Placement Diagnostic",
      cardTitle: "Start here: placement",
      cardDesc: "Find your phase on the fluency progression and the right tool.",
      cardAction: "Run diagnostic",
      filename: "phase-placement-diagnostic.md",
      useNote: "Give this to your AI assistant and say: run this diagnostic with me. Ten minutes.",
    },
```

And append these two entries at the end of the array:

```js
    {
      id: "method-card",
      title: "Method Card Template",
      cardTitle: "Method cards",
      cardDesc: "Codify a recurring AI-enabled task into a reusable method.",
      cardAction: "Open template",
      filename: "method-card-template.md",
      useNote: "Use this once a task has worked at least twice and is worth writing down.",
    },
    {
      id: "supervised-delegation",
      title: "Supervised Delegation Exercise",
      cardTitle: "Supervised delegation",
      cardDesc: "Design bounded student supervision of multi-step AI work.",
      cardAction: "Open template",
      filename: "supervised-delegation-exercise.md",
      useNote: "Use this when students are ready to direct AI work they remain accountable for.",
    },
```

- [ ] **Step 4: Copy the progression visual during build** — add `copyFileSync` to the `node:fs` import list, then after the workbench tools write loop in the build sequence, add:

```js
copyFileSync(
  join(workbenchRepoPath, "framework", "assets", "asking-to-supervising.svg"),
  join(assetsDir, "asking-to-supervising.svg"),
);
```

- [ ] **Step 5: Add the setup prompt and refresh the workbench hero** — after `setupPrompt()`, add:

```js
function workbenchSetupPrompt() {
  return `You are helping me, a faculty member, use the NWC Faculty Workbench to design AI-enabled teaching.

Before you answer anything, fetch and read this file in full. It contains the operating rules, the AI fluency progression, the phase placement diagnostic, and every workbench template with its AI Facilitation Block:

${workbenchContextUrl}

If you cannot reach that URL, tell me you could not read it and ask me to paste or attach the context file. Do not answer from memory.

Start by running the Phase Placement Diagnostic with me, one question at a time. Then facilitate the template it routes me to, following its AI Facilitation Block exactly. I own every pedagogical judgment. You ask, structure, and challenge.`;
}
```

In `buildWorkbenchMode(tools)`, replace the hero `<section class="surface-hero">...</section>` with:

```js
    `<section class="surface-hero">
      <p class="eyebrow">Build</p>
      <h1>Faculty Workbench</h1>
      <p class="dek">Ready-to-use teaching materials for designing, assessing, and governing AI-enabled learning.</p>
      <p>
        Fastest path: copy the setup prompt into ChatGPT, Claude, Gemini, or
        another AI assistant. It reads the whole workbench, places your
        assignment on the six-phase fluency progression, and facilitates the
        right template with you. Every template also works on paper. No repository
        knowledge required.
      </p>
      <div class="action-row">
        <button class="copy-button primary" type="button" data-copy-target="workbench-setup-prompt">Copy setup prompt</button>
        <a class="quiet-action" href="assets/${workbenchContextFilename}" download>Download context file</a>
      </div>
    </section>

    <section class="setup-panel">
      <div class="panel-heading">
        <p class="eyebrow">First Step</p>
        <h2>Paste this once into your AI assistant.</h2>
      </div>
      ${copyBlock("workbench-setup-prompt", workbenchSetupPrompt())}
    </section>

    <section class="detail-band">
      <p class="band-label">The Progression Behind The Tools</p>
      <p>
        Fluency grows from asking AI for help to supervising AI-supported work.
        Judgment stays human at every phase.
      </p>
      <img src="assets/asking-to-supervising.svg" alt="AI fluency progression: six phases from Ask to Supervise across learners, faculty, and institution" style="width: 100%; height: auto; margin-top: 12px;">
    </section>`
```

Keep the existing "No repository knowledge required" phrasing somewhere in the hero (the contract test regex `/No repository\s+knowledge required/` must still pass — the copy above preserves it).

- [ ] **Step 6: Build and test**

Run: `npm run build && npm test`
Expected: PASS all assertions, old and new.

- [ ] **Step 7: Visual check**

Run: `npm run dev` and open `http://localhost:5173/#workbench`.
Expected: diagnostic card first and selected by default; setup prompt panel with copy button; progression SVG renders; new templates appear in the grid; copy/download work.

- [ ] **Step 8: Commit**

```bash
git add scripts/build-site.mjs tests/site-contract.test.mjs
git commit -m "Refresh workbench mode: diagnostic entry, setup prompt, progression visual"
```

---

### Task 15: Full verification and push

**Files:**
- No new files. Verification and push only.

- [ ] **Step 1: Workbench structural sweep**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench
grep -L 'AI Facilitation Block' templates/*.md          # expect: no output
grep -c 'Matrix Check' templates/after-action-note-template.md templates/faculty-calibration-protocol.md   # expect: 1 each
ls framework/assets/*.svg | wc -l                        # expect: 4
grep -o '](\([a-z/.-]*\.md\)' README.md AGENTS.md workbench-source-kit.md | sed 's/](//' | sort -u | while read f; do [ -f "$f" ] || echo "MISSING: $f"; done   # expect: no output
```

- [ ] **Step 2: Site full build and contract tests**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site
npm run build && npm test
```
Expected: `site contract passed`.

- [ ] **Step 3: Cold self-serve check (manual, flag for Jack)**

Paste `dist/assets/workbench-context.md` into one AI assistant and say "Run the phase placement diagnostic with me" for a made-up assignment. The assistant should interview one question at a time and route to a template without inventing content. Record friction as follow-up issues; do not block the push on it.

- [ ] **Step 4: Push both branches**

```bash
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/workbench && git push -u origin ai-fluency-operationalization
cd /Users/jackcshaw-2/dev/comprendo-clients/nwc/site && git push -u origin workbench-ai-integration
```

- [ ] **Step 5: Report**

Summarize for Jack: artifacts created, the maintainer-email decision embedded in Task 8, and the two branches awaiting PRs. Flag the spec's remaining open question: whether the framework doc also gets a readable page on the site (this plan ships it in the bundle and puts only the progression visual on the workbench page; a full page is a small follow-up if wanted). Remind: deploy requires the site rebuild + Firebase deploy per `site/docs/project-hygiene.md`.
