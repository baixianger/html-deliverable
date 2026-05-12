# 03 — Design

**Use when:** documenting a design system, proposing a palette, showing typography scale, or cataloging component states & variants.

## What to put in the file

1. **TL;DR** — what the system is and what it's optimized for (mobile-first? dense data? marketing site?).
2. **Color swatches** — actual rendered chips with hex/RGB values and usage labels.
3. **Type scale** — each size rendered at its real size, with name + px/rem.
4. **Spacing tokens** — visible rectangles sized by the actual token.
5. **Component variant grid** — each component × each state × each size, rendered live.

## Snippet — palette + type scale + variants

```html
<section>
  <h2>Palette</h2>
  <div class="grid-3">
    <div class="card" style="background:#2563eb; color:#fff;">
      <strong>Primary</strong><br><code style="background:rgba(255,255,255,.15); color:#fff;">#2563eb</code>
      <p style="margin:8px 0 0; font-size:12px; opacity:.85;">CTAs, links, focus rings</p>
    </div>
    <div class="card" style="background:#16a34a; color:#fff;">
      <strong>Success</strong><br><code style="background:rgba(255,255,255,.15); color:#fff;">#16a34a</code>
      <p style="margin:8px 0 0; font-size:12px; opacity:.85;">Confirmations, positive deltas</p>
    </div>
    <div class="card" style="background:#dc2626; color:#fff;">
      <strong>Danger</strong><br><code style="background:rgba(255,255,255,.15); color:#fff;">#dc2626</code>
      <p style="margin:8px 0 0; font-size:12px; opacity:.85;">Destructive actions, errors</p>
    </div>
  </div>
</section>

<section>
  <h2>Type scale</h2>
  <table>
    <thead><tr><th>Token</th><th>Size</th><th>Preview</th></tr></thead>
    <tbody>
      <tr><td><code>display</code></td><td>40px / 1.1</td><td style="font:600 40px/1.1 system-ui;">The quick brown fox</td></tr>
      <tr><td><code>h1</code></td><td>28px / 1.2</td><td style="font:600 28px/1.2 system-ui;">The quick brown fox</td></tr>
      <tr><td><code>body</code></td><td>15px / 1.6</td><td style="font:400 15px/1.6 system-ui;">The quick brown fox</td></tr>
      <tr><td><code>caption</code></td><td>12px / 1.4</td><td style="font:400 12px/1.4 system-ui;">The quick brown fox</td></tr>
    </tbody>
  </table>
</section>

<section>
  <h2>Spacing</h2>
  <div style="display:flex; align-items:flex-end; gap:24px;">
    <div><div style="background:var(--accent); width:4px;  height:24px;"></div><code>1</code> 4px</div>
    <div><div style="background:var(--accent); width:8px;  height:24px;"></div><code>2</code> 8px</div>
    <div><div style="background:var(--accent); width:16px; height:24px;"></div><code>4</code> 16px</div>
    <div><div style="background:var(--accent); width:24px; height:24px;"></div><code>6</code> 24px</div>
    <div><div style="background:var(--accent); width:40px; height:24px;"></div><code>10</code> 40px</div>
  </div>
</section>

<section>
  <h2>Button — variants × states</h2>
  <table>
    <thead><tr><th></th><th>Default</th><th>Hover</th><th>Disabled</th></tr></thead>
    <tbody>
      <tr>
        <td>Primary</td>
        <td><button style="background:#2563eb; color:#fff; border:0; padding:8px 14px; border-radius:6px;">Save</button></td>
        <td><button style="background:#1d4ed8; color:#fff; border:0; padding:8px 14px; border-radius:6px;">Save</button></td>
        <td><button disabled style="background:#93c5fd; color:#fff; border:0; padding:8px 14px; border-radius:6px;">Save</button></td>
      </tr>
      <tr>
        <td>Ghost</td>
        <td><button style="background:transparent; color:#2563eb; border:1px solid #2563eb; padding:8px 14px; border-radius:6px;">Save</button></td>
        <td><button style="background:#dbeafe; color:#2563eb; border:1px solid #2563eb; padding:8px 14px; border-radius:6px;">Save</button></td>
        <td><button disabled style="background:transparent; color:#93c5fd; border:1px solid #93c5fd; padding:8px 14px; border-radius:6px;">Save</button></td>
      </tr>
    </tbody>
  </table>
</section>
```

## Pitfalls

- Don't describe colors in prose — render them.
- Don't show a single component variant. The whole point is the *grid*: variants × sizes × states.
- Keep dark-mode preview in mind — test the swatches in both schemes if dark mode matters.
