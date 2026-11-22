---
name: wwyd
description: >
  Gives the agent's own call on what to do next: one primary recommendation and
  why, not an open-ended option dump and not punting the decision back unless a
  human-only choice is truly required. Run as /wwyd (recommendation only) or
  /wwyd --do (recommend, then execute the recommended path). Command-only; does
  not auto-invoke.
disable-model-invocation: true
---

# WWYD (`/wwyd`)

## Goal

The user wants **your** call: one primary recommendation and why. Not an
open-ended list with no stance, and not pushing the decision back unless a
human-only choice is truly required.

## When not to use WWYD

Prefer plan mode or requirements exploration when the goal is still fuzzy,
there is no concrete "next move," or several incompatible directions need
discovery first. WWYD picks the next action or approach once the thread (or
repo) gives you enough to commit.

WWYD never replaces the reads a task normally needs (see Anti-patterns).

## `--do` (apply mode)

When the user runs **`/wwyd --do`** or clearly signals "apply what you would do":

1. Run the default steps below (anchor, recommend, why), briefly: one path.
2. **Preflight.** Do not execute on half-known state. If the uncertainty gate
   applies, resolve it first (one message, then stop). Otherwise proceed.
3. **Execute** that path: edits, commands, tests, follow-ups, whatever you
   would do next if you owned the task. Run the verification yourself; do not
   hand the user a manual checklist.
4. **Still stop** for anything that needs explicit human approval: irreversible
   data loss, production changes the user did not ask for, secrets, spending
   money. State the blocker in one line and do the rest.
5. End with a short **what I did / what ran**, with evidence (test output, exit
   codes). Do not claim "done" or "passing" from memory.

Without `--do`, stay in recommendation-only mode. You may offer to implement it
in one line; do not execute.

## Instructions

1. **Anchor on context.** Use the active task, files, constraints, and project
   rules already in the thread. If **one** essential fact is missing, ask one
   focused question; otherwise proceed.
2. **Uncertainty gate.** If **two or more** blocking unknowns exist, do not
   recommend or execute as if you know. Send one message listing up to **three**
   concrete questions or assumptions, then pause until the user answers. In
   `--do` mode, run nothing until the gate is cleared.
3. **State what you would do.** Lead with the single action or approach you
   would take next, in plain language.
4. **If several options exist,** give **Recommended: ...** plus one sentence on
   why. Mention other paths only when they materially differ.
5. **Keep it proportional.** Short ask, short answer. Go deeper only when the
   stakes are high or the user asked.
6. **Self-check before sending.** Did you name exactly one primary action? Did
   you avoid an unprioritized option dump? In `--do`, did you verify instead of
   handing work back?

## Output shape (default)

- **What I would do:** ...
- **Why:** ...
- **If wrong assumption:** ... (one line, or omit)

When stakes or ambiguity warrant, add one line each:

- **Confidence:** high | medium | low
- **Reversibility:** easy revert | needs review | irreversible, human decision only

Skip boilerplate ("Here's what I'll do..."); get to the recommendation.

In `--do` mode, lead with **What I'm doing** (past tense **What I did** after
execution), then the same **Why** line. Let the diff and commits carry detail.

## Examples (shape only)

**Default**

- **What I would do:** Add a database index for the new query, then re-run the failing test.
- **Why:** The error is an index-required message; the code path is already correct.
- **Confidence:** high

**`--do`**

- **What I'm doing:** Adding the index to the migration, running the query tests, then summarizing what passed.
- **Why:** Fastest fix that matches the logged failure.

## Anti-patterns

- **Skipping pre-reads.** Still read the project's rules file, schema, and
  task-specific docs the change would normally require. WWYD chooses between
  approaches; it does not decide whether to read the repo.
- **Bypassing irreversible gates.** `/wwyd` and `--do` never override data
  loss, unshipped security changes, or production config the user did not
  ask you to touch.
- **Fake confidence.** If the uncertainty gate fired, do not barrel ahead with
  a firm "I would do X." Label assumptions or wait for answers.
