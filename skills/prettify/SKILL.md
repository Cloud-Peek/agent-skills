---
name: prettify
description: Use when the user wants to turn a message, document, or provided text into a polished, self-contained visual HTML infographic saved to a local file.
---

# Prettify

Turn source content into a single, self-contained HTML infographic that looks like
it was designed on purpose — then hand back the command to open it.

This skill deliberately gives you **no fixed template**. Don't fill in a prescribed
layout. Read the content, decide what it's really about, and design the page that
best communicates *that*. Trust your taste. The bar is "a polished product/landing
infographic," not "markdown with nicer fonts."

## Source content

- If the user provides or points at text/a file, that's the source.
- Otherwise use the last substantive assistant message.

Preserve every meaningful point. You may reorder, group, retitle, and condense for
visual flow — but never invent facts or quietly drop real content.

## The one hard constraint: self-contained & offline

- **One `.html` file.** All CSS in a single inline `<style>`. No external anything —
  no CDNs, no web fonts, no remote images, no network requests. It must render
  perfectly with no internet.
- Favor pure HTML + CSS + inline SVG. Reach for a little inline JS only if it
  genuinely serves the content (most infographics need none). Keep it print-safe and
  responsive (stack gracefully on narrow screens).

Everything below is about *quality*, not rules to check off.

## What makes one of these good

Think about what elevated the best versions of this:

- **Design, not decoration.** A cohesive visual system — a small palette (CSS
  variables help), consistent spacing, a clear type hierarchy (eyebrow → headline →
  subhead → body), generous whitespace, restrained rounded cards/shadows. **Always
  use a dark colour scheme** — a deep, near-black background (not pure `#000`),
  light/desaturated body text, slightly raised dark cards with subtle borders, and
  one vivid accent chosen to pop against the dark. Stay dark regardless of subject;
  tune the accent to the mood. One accent, used with intent.

- **Diagrams that carry real information** — this is what separates a great
  infographic from a styled list, and it's the bar you're most likely to miss.
  **Build at least one diagram that is bespoke to *this* content** — one that would
  make no sense pasted onto a different topic. Let the content choose its shape:
  - a process or decision → a stepped flow with the *branch conditions* labeled, not
    just boxes;
  - "X calls/maps to Y" → a two-column wiring/mapping diagram with connectors;
  - categories or impact → a color-coded map **with a legend so color means
    something**;
  - tradeoffs → before/after or side-by-side panels;
  - magnitudes → honest stat cards.
  Build these from HTML/CSS/SVG; make them legible, not ornamental. **Whenever you
  color-code, draw the legend** — undefined color is decoration.

  Anti-pattern to refuse: a row of numbered badges + a two-column tick-list + a few
  generic cards as the centerpiece. Those are reusable *widgets* — they look
  identical regardless of topic, so the page reads as a template even though you
  wrote no template. A checklist of the page's own section titles (or of this
  skill's own constraints) is decoration, not a diagram. Litmus test: if your main
  visual would survive a find-and-replace of the subject, it isn't carrying
  information — redesign it.

- **Scannable hierarchy.** Someone should grasp the gist in ten seconds and be able
  to drill in. Sections with clear headers, lead-ins, and visual rhythm.

- **Semantic color & labeling.** If you color-code, define the meaning. Use chips/
  tags/`<code>` for identifiers, paths, and statuses.

- **Restraint and polish.** Cohesion beats novelty. Give the page **one clear focal
  point** — decide what the eye should hit first, and keep everything else calm
  enough that it can. On a dark canvas saturated colour is *loud*; spend it like
  money. When you colour-code a field whose values are mostly "baseline/unchanged,"
  render those quietly (low saturation, close to the background, a small dot or thin
  edge) and reserve full-strength colour for the exceptional or changed value. If
  every cell is bright, nothing stands out and the reader doesn't know where to
  look. Align things. Don't crowd. Make it feel finished.

Match ambition to the material: a dense plan deserves several custom diagrams; a
short note might be one elegant hero plus a single sharp diagram of its core idea.
Scale the *amount*, never the craft — even a thin source earns one genuinely
bespoke visual, not a grid of stock cards. Don't force structure the content
doesn't have, and don't flatten structure it does.

## Output

Save to `/tmp` with a descriptive, timestamped name:

```bash
TS=$(date +%Y%m%d-%H%M%S)
OUT="/tmp/prettify-${TS}.html"
```

## Final response

After writing the file, keep it short:

1. One sentence naming the file and what you made.
2. A fenced `bash` block with exactly the open command, e.g.:

```bash
open /tmp/prettify-<timestamp>.html
```

Don't paste the HTML into chat and don't re-summarize the source.
