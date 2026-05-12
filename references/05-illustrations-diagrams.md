# 05 — Illustrations & Diagrams

**Use when:** explaining a process, system architecture, data flow, state machine, or any concept that's faster to *see* than read. Also useful as figures inside a larger report.

## What to put in the file

1. **TL;DR** — one sentence of what the diagram shows.
2. **The diagram** — inline SVG, centered. Use real semantic shapes (`<rect>`, `<circle>`, `<line>`, `<text>`) — not screenshots.
3. **Legend / key** — colors, line styles, what they mean.
4. **Annotations** — clickable hotspots that expand to commentary, or numbered callouts beside the diagram.
5. **Failure / edge paths** — show the unhappy path too, dashed or in a warning color.

## Snippet — annotated flowchart with clickable steps

```html
<section>
  <h2>Request flow</h2>

  <figure style="background:var(--card); border:1px solid var(--border); border-radius:10px; padding:16px;">
    <svg viewBox="0 0 600 200" width="100%" font-family="ui-sans-serif" font-size="13">
      <defs>
        <marker id="arr" viewBox="0 0 10 10" refX="9" refY="5"
                markerWidth="6" markerHeight="6" orient="auto">
          <path d="M0,0 L10,5 L0,10 z" fill="currentColor"/>
        </marker>
      </defs>
      <g fill="none" stroke="currentColor" stroke-width="1.5">
        <rect x="20"  y="80" width="100" height="40" rx="6"/>
        <rect x="170" y="80" width="100" height="40" rx="6"/>
        <rect x="320" y="80" width="100" height="40" rx="6"/>
        <rect x="470" y="80" width="100" height="40" rx="6"/>
        <line x1="120" y1="100" x2="170" y2="100" marker-end="url(#arr)"/>
        <line x1="270" y1="100" x2="320" y2="100" marker-end="url(#arr)"/>
        <line x1="420" y1="100" x2="470" y2="100" marker-end="url(#arr)"/>
        <!-- failure path -->
        <path d="M 220 120 Q 220 170 370 170 Q 520 170 520 120"
              stroke="var(--bad)" stroke-dasharray="4 4" marker-end="url(#arr)"/>
      </g>
      <g fill="currentColor" text-anchor="middle">
        <text x="70"  y="105" data-step="1" style="cursor:pointer;">Client</text>
        <text x="220" y="105" data-step="2" style="cursor:pointer;">Edge</text>
        <text x="370" y="105" data-step="3" style="cursor:pointer;">Auth</text>
        <text x="520" y="105" data-step="4" style="cursor:pointer;">Origin</text>
        <text x="370" y="190" fill="var(--bad)" font-size="11">on auth fail → 401</text>
      </g>
    </svg>
    <figcaption class="muted" style="text-align:center; margin-top:8px;">Click a node for detail.</figcaption>
  </figure>

  <div id="step-detail" class="card" style="margin-top:12px; min-height:60px;">
    <p class="muted">Select a step…</p>
  </div>
</section>

<script>
  const detail = document.getElementById('step-detail');
  const notes = {
    1: '<strong>Client</strong> — adds <code>Authorization</code> header; retries with backoff on 5xx.',
    2: '<strong>Edge</strong> — TLS termination + per-IP rate limit (500 rps).',
    3: '<strong>Auth</strong> — validates JWT against rotating JWKS, ~3ms p50.',
    4: '<strong>Origin</strong> — application server pool, 12 instances.',
  };
  document.querySelectorAll('[data-step]').forEach(el => {
    el.addEventListener('click', () => detail.innerHTML = notes[el.dataset.step]);
  });
</script>

<section>
  <h3>Legend</h3>
  <p>
    <span style="display:inline-block; width:24px; border-top:1.5px solid currentColor; vertical-align:middle;"></span> happy path &nbsp;·&nbsp;
    <span style="display:inline-block; width:24px; border-top:1.5px dashed var(--bad); vertical-align:middle;"></span> failure path
  </p>
</section>
```

## Pitfalls

- Don't recreate a real screenshot in SVG. If it's a UI screenshot, embed a base64 PNG instead.
- Keep diagrams readable in dark mode — use `currentColor` for strokes so they invert.
- Don't over-label. If every node needs a paragraph, you wanted a doc with one figure, not a giant diagram.
