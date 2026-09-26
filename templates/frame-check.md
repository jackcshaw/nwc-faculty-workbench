# Frame Check

Use Frame Check to build an assignment case that makes students own a frame while directing AI, or to check and repair a case you already have. It teaches the method as it works: every rating names the test it applies, so you learn to make good examples, not just receive one.

Concept: [why this template works the way it does](../concepts/facilitation-blocks.md)

## Audience and readiness

Use this as an interactive educator session: ask one question at a time and wait. Use any setting already supplied; otherwise ask PME, higher education, or high school. Read the [audience guide](../audiences/guide.md) with this template. Collect the learning objective and the materials students will actually use before proposing anything.

**This template in your setting:** PME: the frame is the strategic problem and its success criterion. HE: the frame is the disciplinary question and the standard of evidence. High school: the frame is the standard the student chooses and defends within the teacher's task.

## AI Facilitation Block

To run an interactive session, give it this entire file and say: "Run Frame Check with me."

Instructions for the AI assistant:

- Role: You help an educator build or repair an assignment case using the five tests below. The educator owns every decision. You ask, propose, rate, and explain. Every rating names the test it applies and answers that test's Ask question.
- Start: Offer the calibration primer for the educator's setting. Show only the weak example first and ask the educator to rate it against the five tests. Reveal the recorded ratings and the strong example only after they answer. They may skip the primer.
- Then ask which mode: build a case, or check and repair a case they already have.
- Build mode: follow the Frame Check workflow below, one step at a time. In step 3 always propose two or three candidates and rate every one; never skip to a single answer. In step 4 recommend one and say what it gives up, then wait for the educator's choice.
- Check-and-repair mode: ask the educator to paste the case. Rate it against the five tests, quoting the case for each rating. Name the failure pattern. Propose the smallest repair that keeps their material. Offer a fresh build only if repair cannot reach a passing case.
- Never: invent facts, sources, statistics, or student responses; generate from memory when the educator has materials; give a misframed AI answer a factual error (that fails test 1); present generated text as real model output or classroom evidence.
- Always: list every factual claim in a generated case as needing a check against the educator's materials; label generated content as constructed.
- Finish: return the Frame Check record below as clean Markdown, including candidates considered and why they were rejected, claims awaiting verification, and decisions the educator deferred.

## The five tests

Rate every candidate assignment against all five. An assignment that fails test 1 or 2 is not a frame-first assignment, however polished the rest is.

### 1. The flaw is in the frame, not the facts

The flawed AI output should be competent on its own terms and wrong for the problem. Its numbers, dates, and summaries can be correct. What is wrong is the question it answered, the criterion it used, or whose interests it counted.

- **Ask:** could a student catch the flaw with a stock phrase such as "check for bias," "correlation isn't causation," "not a fair test," or "AI can be wrong"? If yes, the flaw is a template error, not a frame error.
- **Fails when:** the output contains an overclaim any careful reader would flag, or it is so obviously wrong that it is a strawman ("trees lower temperatures everywhere by 8°C").

### 2. The student owns a real framing choice

More than one coherent frame exists, and the student has to choose one and defend it. The educator can supply the topic and the materials. The consequential choice of standard, criterion, or question stays with the student.

- **Ask:** could two strong students reasonably frame this differently and both earn full credit?
- **Fails when:** the educator has already made every framing decision and the student only executes or corrects.

### 3. Directing AI actually helps

Somewhere in the sequence the student directs AI toward their own purpose and gets something useful back: a missed assumption, a perspective they had not considered, a sorted evidence base, an argument against their frame.

- **Ask:** would a student who directs AI well produce better work than one who refuses it?
- **Fails when:** the only AI activity is inspecting output someone else generated. That teaches critique, not direction.

### 4. Something deserves acceptance

At least one AI contribution should be worth accepting after a reasonable check. Students should practice warranted reliance, not only rejection.

- **Ask:** what would a student be right to accept, and what check justifies accepting it?
- **Fails when:** every AI contribution is a trap. Students learn that the exercise pattern is "find the flaw," and success measures recognition of the pattern.

### 5. The changed case tests whether the frame travels

The changed case should alter which evidence or criterion matters most, so the student has to re-apply their framing discipline. Fixing one error in a new setting is not enough.

- **Ask:** does the changed case make a different frame, or a different piece of evidence, decisive?
- **Fails when:** the changed case only removes the original error (a random sample instead of a self-selected one).

## Directing AI well, by level

Section V of the original essay describes a progression of where human judgment enters an AI workflow. Use it to set a level-appropriate target instead of a universal one.

| Level | What the student does | Typical fit |
| --- | --- | --- |
| Minimal prompting | Asks a bare question and inherits the model's frame | The failure to move students past |
| Structured prompting | States the purpose, the criteria, and what a good answer must include | High school; introductory HE |
| Evaluator loops | Has AI argue assigned perspectives or critique a draft, then weighs the disagreement | High school with teacher support; most HE |
| Reusable workflows and multiple agents | Assigns agents distinct roles, moderates disagreement, and synthesizes | Advanced HE; PME |

Where students cannot hold individual AI accounts, the student can still direct: they write the task, the criteria, and the check, and the teacher runs it on a shared screen.

## Grading a defended frame

Frame-first assignments often have no single correct answer, so the rubric grades the defense, not the conclusion. Give credit when the student:

- states the standard they used and why it fits the question's purpose;
- names at least one coherent alternative frame and why they did not adopt it;
- uses evidence that actually bears on their chosen standard;
- identifies an AI contribution they accepted and the check that justified it;
- explains what evidence or changed condition would change their answer.

Do not award credit for suspicion alone, for balance alone ("there are many perspectives"), or for changing one's mind as an end in itself.

## Frame Check workflow

Frame Check, the workbench tool built on these criteria, builds a case in six steps. It explains every rating through the test it applies, so educators learn the pattern as well as the verdict.

1. Collect the subject, level, learning objective, and the educator's real materials. Nothing is generated from memory.
2. Name the target skill: the frame the student must own.
3. Propose two or three candidate cases. Rate each against the five tests (strong, weak, fails), answering each Ask question and naming any failure pattern.
4. Recommend one and say what it gives up. The educator chooses.
5. Pressure-test the choice: stock-phrase test; the contribution worth accepting and its check; a changed case that makes different evidence decisive; the factual claims to verify.
6. Build the full assignment: question, at least two defensible frames, a misframed AI answer that is factually sound, one contribution worth accepting with its check, a level-appropriate directing-AI task, the changed case, and a defended-frame rubric.

To repair an existing case, rate it, name its failure pattern, and propose the smallest change that keeps the educator's material; rebuild only when repair cannot reach a passing case.

## Calibration primer

Show the weak example first and ask for the educator's ratings before revealing these.

<!-- frame-check:primer pme -->
Setting: professional military education.

**Weak example — PME outage attribution.** A fictional brief records 12 outages; 8 followed equipment updates and 4 have unexplained causes. The AI attributes all 12 to updates and recommends a fleet-wide rollback. The flaw is one "correlation isn't causation" catches.
Ratings: 1 Fails · 2 Fails · 3 Fails · 4 Strong · 5 Fails

**Strong example — PME exercise-window rollback.** Diagnostic evidence now confirms a software defect in 10 outages, and the AI's analysis of the defect is accurate. It recommends an immediate fleet-wide rollback because that fixes the defect fastest, while the decision the unit faces is keeping operations running through a scheduled exercise window, where a rollback carries its own operational risk. The facts are right; the success criterion is wrong. Fictional; needs PME faculty review.
Ratings: 1 Strong · 2 Strong · 3 Strong · 4 Strong · 5 Strong
<!-- /frame-check:primer pme -->

<!-- frame-check:primer he -->
Setting: higher education.

**Weak example — Campus shuttle survey.** A university invites 1,000 students; 100 respond and 80 favor a later shuttle. The AI reports that 80 percent of students favor it. The flaw is nonresponse, a textbook error "check for bias" catches.
Ratings: 1 Fails · 2 Fails · 3 Fails · 4 Strong · 5 Fails

**Strong example — Return-to-office research memo.** A student advises a firm that hires mostly new graduates. The AI synthesis of three real studies is accurate and concludes that the evidence is mixed and hybrid is the balance, while silently treating productivity as short-run output averaged across workers. For this firm, the feedback and promotion findings decide the question.
Ratings: 1 Strong · 2 Strong · 3 Strong · 4 Strong · 5 Strong
<!-- /frame-check:primer he -->

<!-- frame-check:primer k12 -->
Setting: high school.

**Weak example — Asphalt vs. shaded grass.** Two readings: sunlit asphalt at 36°C and shaded grass at 28°C. The AI says trees lower temperatures everywhere by 8°C. The flaw is a confound "not a fair test" catches, and the claim is close to a strawman.
Ratings: 1 Fails · 2 Fails · 3 Fails · 4 Strong · 5 Fails

**Strong example — "Was the New Deal a success?"** A balanced AI essay reports accurate unemployment figures and lasting programs and concludes the New Deal was mixed but largely successful. It judges success by recovery and durability and never asks for whom; Social Security's old-age program first excluded agricultural and domestic workers.
Ratings: 1 Strong · 2 Strong · 3 Strong · 4 Strong · 5 Strong
<!-- /frame-check:primer k12 -->

## Frame Check record

- Setting, subject, level, and learning objective:
- Materials supplied by the educator:
- Mode (build, or check and repair):
- Target skill (the frame the student must own):
- Candidates considered, with five-test ratings and why each was kept or rejected:
- Final case or assignment: question; defensible frames; misframed AI answer; contribution worth accepting and its check; directing-AI task; changed case; defended-frame rubric:
- Five-test rating table for the final case:
- Factual claims awaiting verification against the educator's materials:
- Decisions the educator deferred:
