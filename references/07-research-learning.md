# 07 — Research & Learning

**Use when:** explaining a complex topic, summarizing research, writing a technical deep-dive, or producing reference material the user will re-read and search.

## What to put in the file

1. **TL;DR** — what the reader will know after reading.
2. **Table of contents** — anchor links to each section. Sticky in the sidebar if long.
3. **Progressive sections** — start simple, add depth. Use `<details>` for deeper dives.
4. **Tabs for code samples** — same example in multiple languages or approaches.
5. **Interactive demos** — small live widgets that let the reader poke the concept (e.g. a slider that shows how a parameter changes behavior).
6. **Glossary / definitions** — hoverable terms that show a tooltip with the definition.

## Snippet — TOC + collapsibles + tabbed code + glossary tooltip

```html
<style>
  .glossary {
    text-decoration: underline dotted;
    cursor: help;
    position: relative;
  }
  .glossary:hover::after {
    content: attr(data-def);
    position: absolute; left: 0; top: 100%;
    background: var(--card); border: 1px solid var(--border); border-radius: 6px;
    padding: 8px 10px; font-size: 12px; width: 240px; z-index: 10;
    box-shadow: 0 4px 12px rgba(0,0,0,.12);
  }
  .tabs { display: flex; gap: 4px; border-bottom: 1px solid var(--border); }
  .tabs button {
    background: transparent; border: 0; padding: 8px 12px; cursor: pointer;
    color: var(--muted); border-bottom: 2px solid transparent; font: inherit;
  }
  .tabs button.active { color: var(--fg); border-bottom-color: var(--accent); }
  .tab-panel { display: none; }
  .tab-panel.active { display: block; }
</style>

<nav class="card" style="margin-bottom:24px;">
  <strong>Contents</strong>
  <ol>
    <li><a href="#intro">Intuition</a></li>
    <li><a href="#deeper">How it actually works</a></li>
    <li><a href="#code">Code samples</a></li>
  </ol>
</nav>

<section id="intro">
  <h2>Intuition</h2>
  <p>
    A <span class="glossary" data-def="A data structure that distributes keys across nodes such that adding/removing a node only re-maps K/n keys.">consistent hash ring</span>
    keeps cache hit rates high even as the cluster size changes.
  </p>
</section>

<section id="deeper">
  <h2>How it actually works</h2>
  <details>
    <summary>Why not modulo hashing?</summary>
    <p>Because <code>hash(key) % n</code> reshuffles ~all keys when <code>n</code> changes.</p>
  </details>
  <details>
    <summary>What about virtual nodes?</summary>
    <p>Each physical node gets ~100-200 positions on the ring to smooth out load imbalance.</p>
  </details>
</section>

<section id="code">
  <h2>Code samples</h2>
  <div class="tabs">
    <button class="active" data-tab="py">Python</button>
    <button data-tab="rs">Rust</button>
    <button data-tab="ts">TypeScript</button>
  </div>
  <div class="tab-panel active" data-panel="py"><pre><code>def node_for(key, ring):
    h = hash(key)
    return ring[bisect.bisect(positions, h) % len(ring)]</code></pre></div>
  <div class="tab-panel" data-panel="rs"><pre><code>fn node_for(key: &amp;str, ring: &amp;Ring) -&gt; NodeId {
    let h = hash(key);
    ring.successor(h)
}</code></pre></div>
  <div class="tab-panel" data-panel="ts"><pre><code>function nodeFor(key: string, ring: Ring): NodeId {
  return ring.successor(hash(key));
}</code></pre></div>
</section>

<script>
  document.querySelectorAll('.tabs button').forEach(btn => {
    btn.addEventListener('click', () => {
      const tab = btn.dataset.tab;
      document.querySelectorAll('.tabs button').forEach(b => b.classList.toggle('active', b === btn));
      document.querySelectorAll('.tab-panel').forEach(p => p.classList.toggle('active', p.dataset.panel === tab));
    });
  });
</script>
```

## Pitfalls

- Don't hide the meat behind `<details>`. The top-level reading path must still make sense if every collapsible stays closed.
- Glossary tooltips are great for jargon but don't tooltip *every* technical term — it makes the doc feel hostile.
- For very long docs, add a sticky sidebar TOC instead of inline.
