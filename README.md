# decision-card

A small, static Claude skill that stress-tests a decision before you commit to it — then hands you a one-screen card: the call, how sure it is, what to do next, the best case against, and what would flip it.

It's a deliberately de-fanged rebuild of the open-source [`reflect`](https://github.com/seldonframe/reflect) skill. Same good bones — undo-cost triage, invent-a-cheaper-third-option, an answer-first card. None of the machinery: **no self-firing, no live URL fetching, no plugin auto-update, no edits to your global agent config.** It runs only when you ask it to.

## Try it in the browser

There's an interactive version — the same loop as a stepper you click through, ending in the same card.

**→ [andrewluxem.github.io/decision-card](https://andrewluxem.github.io/decision-card/)**

Type the decision, walk GATE → LOOK → RESHAPE → ATTACK, and get the card — then copy it as Markdown, download a `.md`, or print to PDF. It's a single self-contained file, [`web/index.html`](web/index.html): no build step, no dependencies, no tracking. Everything runs client-side; nothing is sent anywhere and nothing is stored. Same discipline as the skill — it never fires on its own or fetches anything, you drive it.

The Markdown it emits is the exact card format below, so it round-trips straight back into Claude or your notes.

## Why the rebuild

`reflect` is a genuinely good idea wrapped in behaviors worth avoiding: it fires itself unprompted, it can fetch a URL and write the result into its own instruction tree (a persistent, unaudited injection surface), and it installs as an auto-updating plugin that pulls repo HEAD on every commit. `decision-card` keeps the thinking and drops all of that. The whole skill is one `SKILL.md` you can read in two minutes.

## The loop

1. **GATE** — what would it cost to undo this in 3 months? Reversible → answer fast at ~70% info. Irreversible → full rigor.
2. **LOOK** — read the real numbers (git log, the actual data). Offer to run the one-hour test rather than reasoning around it. Never run commands unprompted.
3. **RESHAPE** — before "A or B," invent a cheaper, reversible **C**.
4. **ATTACK** — try to kill your own answer. On irreversible calls, a cold-adversary pass reasons without your stated preference.
5. **CARD** — answer first:

   > **THE CALL:** …
   > **HOW SURE:** ~n% — why
   > **DO THIS NEXT:** …
   > **THE CASE AGAINST:** …
   > **THIS FLIPS IF:** …

See [`examples/example-run.md`](examples/example-run.md) for a full worked run.

## Install

**Manual (recommended — clone and copy):**

```bash
git clone https://github.com/andrewluxem/decision-card.git
cp -r decision-card/skills/decision-card ~/.claude/skills/
```

Then invoke it explicitly: `reflect on: <the thing you're stuck on>`.

**As a Claude Code plugin (version-pinned, no auto-update):**

```
/plugin marketplace add andrewluxem/decision-card
/plugin install decision-card@decision-card
```

Unlike `reflect`, `plugin.json` carries an explicit `version`. Installing pins that version; it does **not** silently pull new commits. To take an update you bump the version and reinstall — the update is a decision, not a background event. (That's the whole point.)

## Usage

```
reflect on: should I rewrite the webhook handler or keep patching it?
run a decision card on: price the new tier at $29 or $49?
```

It won't trigger on its own — thinking out loud won't set it off. Ask for it.

## Iterating

The skill is the single file `skills/decision-card/SKILL.md`. Edit it, invoke it on a real decision, and see whether the card earns its place. When you change behavior meaningfully, bump `version` in `.claude-plugin/plugin.json` so plugin installs pick it up deliberately.

The browser tool is the single file `web/index.html` — edit it directly and open it locally to preview. On push to `main`, the [`pages` workflow](.github/workflows/pages.yml) redeploys it to GitHub Pages (one-time setup: repo **Settings → Pages → Source: GitHub Actions**).

## License

MIT — see [`LICENSE`](LICENSE). Loop structure adapted from `seldonframe/reflect` (MIT); undo-cost triage is Bezos's type-1/type-2 framing.
