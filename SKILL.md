---
name: html-deliverable
description: Produce a single self-contained .html file (instead of a wall of markdown) when delivering substantive work meant for a human to read — plans, code-review writeups, design explorations, reports, prototypes, comparisons, research summaries, slide decks, diagrams, or interactive editors. Trigger when the result has multiple sections, options to compare side-by-side, visual/spatial structure, interactivity, or a TL;DR-then-drill-down shape — anything a reader would skim and jump around in rather than read top-to-bottom. Use on phrases like "write up the plan", "summarize the review", "compare these options", "give me a report", "make slides/a deck", "design exploration", "draw the architecture", or whenever you're handing off a meaningful artifact at the end of real work. Stay in markdown for short answers, single linear explanations, mid-conversation replies, or code-only output.
---

# html-deliverable

Substantive work meant for a human → self-contained `.html`, not markdown. Trade *"a document you'd skim"* for *"one you'd actually read."*

## Use HTML when content has

- Options to compare side-by-side
- Multiple sections to skim and jump between
- Visual structure (diagrams, tables, charts, timelines, swatches)
- Interactivity that aids understanding
- TL;DR → drill-down shape

Stay in markdown for short answers, single linear explanations, mid-conversation replies, code-only output. When unsure, lean markdown.

## Brevity budget

Visual structure does the work — not prose.

| Limit | Cap |
|---|---|
| Body prose, total | **≤ 300 words** |
| TL;DR | **one sentence** |
| Any paragraph | **≤ 2 sentences** |
| Section without a visual element (table / cards / tags / chart / code / SVG) | **forbidden** |
| Hedges, throat-clearing, restated headings | **cut** |

A 4th paragraph in a section = you're writing markdown. Convert to table / card grid / tag row.

## Visual style — warm editorial

Start from `templates/base.html`. Warm paper + serif headings + terracotta accent (modeled on *The Unreasonable Effectiveness of HTML*).

- **Bg** ivory `#FAF9F5`, paper `#FFFFFF`, warm-gray 1.5 px borders.
- **Text** slate `#141413`, prose `#3D3D3A`, muted `#87867F`.
- **Accent** clay `#D97757` — eyebrow tick, section index, links, one italic word in h1. Olive `#788C5D` for positive status.
- **Type** serif (`ui-serif, Georgia`) weight 500 for h1/h2/quotes. System sans for body. Mono uppercase for eyebrows, tags, table headers.
- **No dark mode.** Single warm-paper look.
- **Tokens only** — `var(--clay)`, `var(--g300)`, `var(--olive)`, etc. **Never hardcode hex.** Legacy aliases (`--accent`, `--card`, `--ok`) still resolve.
- **Fluid responsive.** Wrapper uses `clamp()` — up to 1400 px on big screens, tightens on phone. Prose `<p>` capped at 68 ch; tables / cards span the full wrap.

## Decision matrix — pick one

| Task | Pattern | Reference |
|---|---|---|
| Weighing options / roadmap | Exploration & Planning | `references/01-exploration-planning.md` |
| PR writeup, architecture | Code Review | `references/02-code-review.md` |
| Design system, palette | Design | `references/03-design.md` |
| Motion / interaction | Prototyping | `references/04-prototyping.md` |
| Process visualized | Illustrations & Diagrams | `references/05-illustrations-diagrams.md` |
| Slides | Decks | `references/06-decks.md` |
| Teaching, research | Research & Learning | `references/07-research-learning.md` |
| Status / weekly report | Reports | `references/08-reports.md` |
| Editor for user data | Custom Editing Interfaces | `references/09-custom-editing-interfaces.md` |

Read **only one** reference. If its snippet hardcodes a hex (`#2563eb`), swap for a token.

## Rules

1. **Self-contained** — one `.html`, all CSS/JS inline, no CDN/font/remote-image. Works offline.
2. **Start from `base.html`** — don't reinvent the chrome.
3. **Semantic HTML** — `<section>`, `<details>`, `<table>`, `<figure>`.
4. **No frameworks, no build.** Charts = hand-drawn inline SVG.
5. **Save to `notes/<slug>.html`.** Don't commit unless asked.
6. **Open it for the user.** After saving, run `open notes/<slug>.html` (macOS) / `xdg-open` (Linux) / `start` (Windows) via Bash so the file pops in the browser immediately. Don't make the user copy-paste a path.
7. **Reply in one line:** what + path. Never repeat content as markdown — the open browser tab is the deliverable.

## Anti-patterns

- Three-paragraph "introductions".
- Hardcoded hex (use tokens).
- Restating the heading in the body.
- Producing HTML *and* dumping the same content as markdown in chat.
- HTML for a 60-word answer.
