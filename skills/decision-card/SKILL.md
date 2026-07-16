---
name: decision-card
description: Pressure-test a decision before committing to it. Use ONLY when explicitly invoked — "reflect on <decision>", "run a decision card on <x>", "should I X or Y", "help me decide between", "stress-test this call". Runs a short loop: size the undo cost, look for a cheaper third option, attack the answer, then deliver an answer-first decision card with a real confidence number and the case against. Does NOT fire itself; never runs unprompted; never fetches URLs, edits global config, or installs anything.
---

# decision-card

A decision-stress-test loop that ends in a one-screen card. De-fanged rebuild of the public `reflect` skill (MIT) — same good bones (undo-cost triage, invent-a-third-option, answer-first card), none of the machinery it shipped with (no self-firing, no live URL fetching, no plugin auto-update, no global-config edits). Runs only when the user explicitly asks.

## When this runs

Explicit invocation only. Trigger phrases: "reflect on…", "run a decision card on…", "should I A or B", "help me decide", "stress-test this decision". If the user is merely thinking out loud or narrating a plan, do **not** trigger. Never fire yourself mid-task.

## The loop

### 1. GATE — size the undo cost first

One question before anything else: **what would it cost to undo this in 3 months?**

- **Reversible** (a bad day or week to reverse): answer fast. ~70% of the info is enough — over-deliberating cheap-to-reverse calls is how work stalls.
- **Irreversible** (money already spent, a public promise, reputation, people, deleted data): slow down, run the full loop, and include the cold-adversary pass in step 4.

State which bucket it's in and why, in one line.

### 2. LOOK — get the real numbers, don't assert them

Before forming an opinion, gather the cheap evidence that would actually settle it:

- If a fact lives in the repo, history, or data, read it. "Why does this exist" is answered by `git log`, not a guess.
- If a one-hour test, grep, or query would decide it, **offer to run it** and wait for the go-ahead. Do not run commands unprompted.
- Label every base rate as sourced or estimated. Never dress an estimate as a fact.

### 3. RESHAPE — invent the cheaper third option

Before answering "A or B," spend real effort on a **C** that makes being wrong cheap:

- the smaller first step that tests the core assumption
- the reversible version of the irreversible plan
- the 20% that delivers 80% of the value

The honest answer to "rewrite or patch?" is often "rewrite the one function everyone keeps patching; leave the rest." Surface C explicitly even if you still end up recommending A or B.

### 4. ATTACK — try to kill your own answer

Before showing the card, argue against your lean:

- Question the requirement — who asked for this, and is it still real?
- Run the actual arithmetic — does the math survive?
- Mirror the deal — would the other side sign it in reverse?
- Set the kill threshold — what result would make you abandon this?

For **irreversible** calls, add a cold-adversary pass: reason through the facts *without* the user's stated preference, reach an independent conclusion, then reconcile. If the cold pass disagrees with your lean, say so plainly.

### 5. CARD — deliver answer-first

Output in this exact shape, with nothing before it:

> **THE CALL:** <one sentence>
> **HOW SURE:** ~<n>% — <why that number>
> **DO THIS NEXT:** <the single next action: a commit, a test, a message>
> **THE CASE AGAINST:** <the strongest opposing argument, stated fairly>
> **THIS FLIPS IF:** <the 1–3 things that would change the answer>

Keep it tight. The card is the deliverable; the reasoning above it stays short.

## What this skill deliberately does NOT do

- Never fires unprompted; never edits CLAUDE.md / AGENTS.md or any file outside its own directory.
- Never fetches remote URLs or writes new instruction files into itself.
- Never installs anything or runs shell commands without explicit approval.

Attribution: undo-cost triage is Bezos's type-1/type-2 framing; loop structure adapted from the open-source `reflect` skill (seldonframe/reflect, MIT), rebuilt static and stripped of its self-modifying machinery.
