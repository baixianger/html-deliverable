# 08 — Reports

**Use when:** a recurring or structured report — weekly status, incident postmortem, experiment writeup, financial summary, monitoring digest, anything where the reader's first question is "what changed?"

## What to put in the file

1. **TL;DR box** — 2-3 sentences. The headline reader stops here.
2. **Key metrics row** — 3-5 big numbers with delta vs. previous period (green up / red down — context-dependent on which direction is good).
3. **Mini charts** — inline SVG sparklines or simple bar/line charts. No Chart.js.
4. **Timeline** — colored horizontal bar showing the period at a glance (incidents, releases, milestones).
5. **Sections per area** — one short subsection per topic, with a status tag.
6. **Appendix / details** — links, raw numbers, follow-ups, in `<details>`.

## Snippet — metrics row + sparkline + timeline

```html
<section class="tldr">
  <div class="label">TL;DR</div>
  <p>P95 latency improved 18% after the cache rollout on Tuesday. One brief incident Thursday (auth provider, 4 min). Sign-ups flat.</p>
</section>

<section>
  <h2>Headline metrics</h2>
  <div class="grid-3">
    <div class="card">
      <div class="muted" style="font-size:12px; text-transform:uppercase; letter-spacing:.08em;">P95 latency</div>
      <div style="font-size:32px; font-weight:600; margin:6px 0;">142 ms</div>
      <div style="color:var(--ok); font-size:13px;">▼ 18% vs last week</div>
    </div>
    <div class="card">
      <div class="muted" style="font-size:12px; text-transform:uppercase; letter-spacing:.08em;">Sign-ups</div>
      <div style="font-size:32px; font-weight:600; margin:6px 0;">2,341</div>
      <div class="muted" style="font-size:13px;">— flat</div>
    </div>
    <div class="card">
      <div class="muted" style="font-size:12px; text-transform:uppercase; letter-spacing:.08em;">Error rate</div>
      <div style="font-size:32px; font-weight:600; margin:6px 0;">0.21%</div>
      <div style="color:var(--bad); font-size:13px;">▲ 0.04 pp</div>
    </div>
  </div>
</section>

<section>
  <h2>Latency trend</h2>
  <!-- Sparkline: 12 weekly points, hand-drawn polyline -->
  <svg viewBox="0 0 240 60" width="100%" style="max-width:480px;" preserveAspectRatio="none">
    <polyline fill="none" stroke="var(--accent)" stroke-width="2"
      points="0,30 20,28 40,35 60,40 80,38 100,42 120,36 140,32 160,28 180,20 200,18 220,14"/>
  </svg>
  <p class="muted">12 weeks, P95 ms.</p>
</section>

<section>
  <h2>Week at a glance</h2>
  <div style="display:flex; height:36px; border-radius:6px; overflow:hidden; border:1px solid var(--border);">
    <div style="flex:1; background:var(--ok);"      title="Mon — green"></div>
    <div style="flex:1; background:var(--ok);"      title="Tue — cache rollout"></div>
    <div style="flex:1; background:var(--ok);"      title="Wed — green"></div>
    <div style="flex:1; background:var(--bad);"     title="Thu — 4min auth outage"></div>
    <div style="flex:1; background:var(--ok);"      title="Fri — green"></div>
    <div style="flex:1; background:var(--code-bg);" title="Sat — quiet"></div>
    <div style="flex:1; background:var(--code-bg);" title="Sun — quiet"></div>
  </div>
  <p class="muted" style="font-size:12px;">Mon · Tue · Wed · Thu · Fri · Sat · Sun</p>
</section>

<section>
  <h2>Areas</h2>
  <table>
    <thead><tr><th>Area</th><th>Status</th><th>Notes</th></tr></thead>
    <tbody>
      <tr><td>Performance</td><td><span class="tag tag-ok">On track</span></td><td>Cache hit rate 92%.</td></tr>
      <tr><td>Reliability</td><td><span class="tag tag-warn">Watch</span></td><td>One incident; RCA filed.</td></tr>
      <tr><td>Growth</td><td><span class="tag">Flat</span></td><td>Awaiting experiment results.</td></tr>
    </tbody>
  </table>
</section>

<details>
  <summary>Raw numbers</summary>
  <pre><code>p50 = 38ms (-12%)
p95 = 142ms (-18%)
p99 = 410ms (-9%)
err = 0.21% (+0.04pp)</code></pre>
</details>
```

## Pitfalls

- Don't lead with a 20-row table. Lead with 3 big numbers.
- Color deltas need to match domain — for latency, down=green; for revenue, up=green. Don't auto-color by sign.
- Keep sparklines simple. If you need axes and ticks, you want a real chart and should ask the user before pulling a library.
