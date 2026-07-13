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
