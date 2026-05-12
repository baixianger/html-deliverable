# 02 — Code Review & Understanding

**Use when:** summarizing a PR, walking through a non-trivial diff, explaining a module's architecture, or producing a review writeup the user will share with teammates.

## What to put in the file

1. **TL;DR** — what changed, why, and what the reader should look at first.
2. **Hot path callout** — 1-3 files/functions that matter most, with line refs.
3. **Annotated diff** — code blocks with margin notes (numbered tags pointing to commentary).
4. **Module diagram** — boxes-and-arrows (inline SVG) of what calls what after the change.
5. **Severity-tagged findings** — `tag-bad` (must fix), `tag-warn` (consider), `tag-ok` (nit / praise).
6. **Test coverage note** — what's tested, what isn't, what should be.

## Snippet — annotated diff + severity findings

```html
<section>
  <h2>Hot path</h2>
  <p>Read these two first:</p>
  <ul>
    <li><code>src/auth/session.rs:142</code> — new token rotation</li>
    <li><code>src/auth/middleware.rs:38</code> — call site, error handling changed</li>
  </ul>
</section>

<section>
  <h2>Annotated diff — <code>session.rs</code></h2>
  <pre><code>  pub fn rotate(&amp;mut self) -&gt; Result&lt;Token&gt; {
-     let now = SystemTime::now();
+     let now = self.clock.now();           <span class="tag tag-ok">①</span>
      if now &lt; self.expires_at - GRACE {
-         return Ok(self.token.clone());
+         return Ok(self.token.clone());    <span class="tag tag-warn">②</span>
      }
      ...
  }</code></pre>
  <ol>
    <li><strong>①</strong> Good: clock is now injected — makes the unit test in <code>session_test.rs:88</code> deterministic.</li>
    <li><strong>②</strong> Same return as before, but the early-exit branch is no longer covered by any test. Add one for the in-grace-window case.</li>
  </ol>
</section>

<section>
  <h2>Findings</h2>
  <table>
    <thead><tr><th>Severity</th><th>Where</th><th>Note</th></tr></thead>
    <tbody>
      <tr><td><span class="tag tag-bad">Must</span></td><td><code>middleware.rs:38</code></td><td><code>?</code> on rotate swallows the underlying error — wrap with <code>.context()</code>.</td></tr>
      <tr><td><span class="tag tag-warn">Consider</span></td><td><code>session.rs:142</code></td><td>Grace window is a magic constant; lift to config.</td></tr>
      <tr><td><span class="tag tag-ok">Nit</span></td><td><code>session.rs:201</code></td><td>Nice naming on <code>RotationOutcome</code>.</td></tr>
    </tbody>
  </table>
</section>

<section>
  <h2>Call graph after change</h2>
  <figure>
    <svg viewBox="0 0 480 160" width="100%" style="background: var(--card); border: 1px solid var(--border); border-radius: 8px;">
      <g font-family="ui-sans-serif" font-size="12" fill="currentColor">
        <rect x="20"  y="60" width="120" height="40" rx="6" fill="none" stroke="currentColor"/>
        <text x="80"  y="84" text-anchor="middle">middleware</text>
        <rect x="180" y="60" width="120" height="40" rx="6" fill="none" stroke="currentColor"/>
        <text x="240" y="84" text-anchor="middle">session.rotate</text>
        <rect x="340" y="60" width="120" height="40" rx="6" fill="none" stroke="currentColor"/>
        <text x="400" y="84" text-anchor="middle">token store</text>
        <line x1="140" y1="80" x2="180" y2="80" stroke="currentColor" marker-end="url(#a)"/>
        <line x1="300" y1="80" x2="340" y2="80" stroke="currentColor" marker-end="url(#a)"/>
      </g>
      <defs><marker id="a" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="currentColor"/></marker></defs>
    </svg>
  </figure>
</section>
```

## Pitfalls

- Don't paste the full diff. Excerpt only the lines that need commentary.
- Don't reorder severity by file. Group `must` together at the top of the findings table.
- Filenames must include line numbers (`path:line`) — the user expects to cmd-click.
