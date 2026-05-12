# html-deliverable

A Claude Code skill that produces a single self-contained `.html` file — warm editorial style, fluid responsive — instead of a markdown wall, when the agent finishes substantive work meant for a human to read.

> Inspired by [*The Unreasonable Effectiveness of HTML*](https://thariqs.github.io/html-effectiveness/) by Thariq Shihipar. The design tokens (warm ivory, serif headings, clay terracotta accent, olive positive) and the nine pattern categories are adopted directly from that essay.

## Install

```bash
git clone <this-repo> ~/.claude/skills/html-deliverable
```

Claude Code auto-discovers skills under `~/.claude/skills/`. No further setup.

## How it works

When the agent finishes work whose result has multiple sections, comparisons, visual structure, or a TL;DR-then-drill-down shape, it produces an `.html` file in `notes/<slug>.html`, **opens it in the browser for you**, and replies in one line with the path. The file works offline by double-click — all CSS, JS, and SVG are inline.

The agent stays in markdown for short answers, single linear explanations, or code-only output. HTML must earn its place.

## The nine patterns

| Pattern | For |
|---|---|
| 01 — Exploration & Planning | Weighing options, roadmaps |
| 02 — Code Review | PR writeups, architecture |
| 03 — Design | Palettes, type scales, components |
| 04 — Prototyping | Motion, interaction concepts |
| 05 — Illustrations & Diagrams | Visual process explanations |
| 06 — Decks | Slides |
| 07 — Research & Learning | Topic deep-dives |
| 08 — Reports | Status, weekly, analysis |
| 09 — Custom Editing Interfaces | Domain-specific editors |

The agent picks one pattern per deliverable and loads only that reference.

## Brevity budget

Every deliverable obeys these hard limits:

- Body prose ≤ 300 words total
- TL;DR is one sentence
- No paragraph longer than 2 sentences
- No section without a visual element
- No hardcoded hex — design tokens only

Visual structure does the work, not prose.

## Structure

```
html-deliverable/
├── SKILL.md          decision matrix, brevity rules, visual style
├── templates/
│   └── base.html     fluid wrapper, full design tokens, masthead/TL;DR/section chrome
└── references/       one per pattern, loaded on demand
    ├── 01-exploration-planning.md
    ├── 02-code-review.md
    ├── 03-design.md
    ├── 04-prototyping.md
    ├── 05-illustrations-diagrams.md
    ├── 06-decks.md
    ├── 07-research-learning.md
    ├── 08-reports.md
    └── 09-custom-editing-interfaces.md
```

## Credit

The pattern taxonomy, color palette (`#FAF9F5` ivory, `#D97757` clay, `#788C5D` olive, etc.), and the editorial aesthetic come from Thariq Shihipar's *The Unreasonable Effectiveness of HTML*: <https://thariqs.github.io/html-effectiveness/>. This skill operationalizes that essay's thesis for Claude Code.
