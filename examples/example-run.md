# Example run

A full pass on a real-shaped decision. This is what the skill produces when invoked.

---

**Invocation:**

> reflect on: should I rewrite the Braze webhook handler or keep patching it?

**1. GATE — undo cost.** Reversible. The handler is behind a feature flag; a bad rewrite reverts in an hour by flipping back to the current path. → Fast loop, ~70% info is enough.

**2. LOOK — the real numbers.** `git log --oneline` on the handler shows 9 commits in 4 months, 6 of them touching the same `normalizeEvent()` function; the other ~400 lines haven't changed since the initial commit. The bug tracker has 3 open issues, all in that one function. (Read from history, not assumed.)

**3. RESHAPE — the cheaper third option.** Not "rewrite the whole handler" (A) vs "keep patching blind" (B). **C: rewrite only `normalizeEvent()` behind the existing flag, leave the other 400 stable lines alone.** That's where 6 of 9 changes and 3 of 3 bugs live; the rest is carrying its weight.

**4. ATTACK — kill the answer.**
- *Requirement still real?* Yes — the event-shape churn is upstream from Braze and ongoing.
- *Arithmetic?* Full rewrite ≈ 2 days + reverify 400 lines; targeted rewrite ≈ 3 hours + reverify one function. ~5x cheaper for the same bug surface.
- *Kill threshold?* If the "stable" 400 lines turn out to share state with `normalizeEvent()`, the isolation assumption breaks and C collapses into A.

**5. CARD:**

> **THE CALL:** Rewrite only `normalizeEvent()` behind the existing flag; don't touch the rest of the handler.
> **HOW SURE:** ~80% — the git history is unambiguous about where the churn and bugs concentrate; the 20% doubt is whether the rest is as decoupled as it looks.
> **DO THIS NEXT:** Grep `normalizeEvent` for shared mutable state / closures over handler scope (15 min). If clean, start the targeted rewrite.
> **THE CASE AGAINST:** If a broader event-schema change is coming next quarter, a full rewrite now amortizes better than rewriting one function twice.
> **THIS FLIPS IF:** (1) `normalizeEvent()` shares state with the stable code, or (2) a known upstream schema overhaul lands within ~8 weeks.
