# 01 — Exploration & Planning

**Use when:** proposing 2+ approaches, weighing trade-offs, laying out a roadmap with risks, or asking the user to choose a direction before you commit code.

## What to put in the file

1. **TL;DR** — the recommended option in one sentence, plus why.
2. **Side-by-side option cards** — one card per approach. Each card has: name, one-line summary, pros, cons, effort estimate, risk level, and (if visual) a tiny inline mockup.
3. **Timeline / phased plan** — for the chosen option, a vertical or horizontal sequence of phases with durations.
4. **Risk table** — risks × likelihood × impact × mitigation.
5. **Open questions** — bullets of what you still need from the user.

## Snippet — option cards + risk table

```html
<section>
  <h2>Three approaches</h2>
  <div class="grid-3">
    <div class="card">
      <h3>A. Add a column</h3>
      <p class="muted">Minimal schema change, backfill nullable.</p>
      <p><span class="tag tag-ok">Low risk</span> <span class="tag">~2 days</span></p>
      <details><summary>Trade-offs</summary>
        <ul><li>+ no migration of existing rows</li><li>− query plan changes for two indexes</li></ul>
      </details>
    </div>
    <div class="card">
      <h3>B. New table + FK</h3>
      <p class="muted">Normalize, write-through during cutover.</p>
      <p><span class="tag tag-warn">Medium</span> <span class="tag">~1 week</span></p>
      <details><summary>Trade-offs</summary>
        <ul><li>+ clean model, future-proof</li><li>− dual-write window, rollback cost</li></ul>
      </details>
    </div>
    <div class="card">
      <h3>C. Event-sourced</h3>
      <p class="muted">Append-only log, project on read.</p>
      <p><span class="tag tag-bad">High</span> <span class="tag">~3 weeks</span></p>
      <details><summary>Trade-offs</summary>
        <ul><li>+ full audit trail</li><li>− team unfamiliar, infra cost</li></ul>
      </details>
    </div>
  </div>
</section>

<section>
  <h2>Risks (option B)</h2>
  <table>
    <thead><tr><th>Risk</th><th>Likelihood</th><th>Impact</th><th>Mitigation</th></tr></thead>
    <tbody>
      <tr><td>Dual-write drift</td><td>Medium</td><td>High</td><td>Reconciler job + shadow reads for 1 week</td></tr>
      <tr><td>Backfill exceeds maintenance window</td><td>Low</td><td>Medium</td><td>Chunked + resumable; can pause</td></tr>
    </tbody>
  </table>
</section>
```

## Pitfalls

- Don't list 5+ options — pick 2-3 real ones. More feels thorough but actually delays the decision.
- Don't bury the recommendation. State it in the TL;DR; let the cards justify it.
- If one option is clearly best, don't fake balance to make it look fair. Say so and explain why.
