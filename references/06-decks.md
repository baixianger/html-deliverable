# 06 — Decks

**Use when:** the user asks for slides, a deck, a presentation, or something to walk a meeting through. Keep it single-file, keyboard-navigable, no external tooling.

## What to put in the file

1. **Title slide** — title, subtitle, presenter, date.
2. **Slides** — one `<section class="slide">` per slide. Keep each slide to one idea.
3. **Navigation** — arrow keys (← →), spacebar advances, slide counter in corner.
4. **Print-friendly** (optional) — `@media print` styles so the user can export to PDF via the browser.

## Snippet — minimal single-file deck

```html
<style>
  body { margin:0; padding:0; max-width:none; overflow:hidden; height:100vh;
         display:flex; align-items:center; justify-content:center;
         background:var(--bg); color:var(--fg); }
  .slide { display:none; width:100vw; height:100vh; padding:6vh 8vw;
           box-sizing:border-box; flex-direction:column; justify-content:center; }
  .slide.active { display:flex; }
  .slide h1 { font-size:64px; margin:0 0 16px; letter-spacing:-0.02em; }
  .slide h2 { font-size:44px; border:0; padding:0; margin:0 0 24px; }
  .slide p, .slide li { font-size:24px; line-height:1.5; }
  .counter { position:fixed; bottom:16px; right:20px; color:var(--muted); font-size:12px; }
  .hint    { position:fixed; bottom:16px; left:20px;  color:var(--muted); font-size:12px; }
  @media print {
    body { display:block; height:auto; }
    .slide { display:flex !important; page-break-after:always; height:100vh; }
    .counter, .hint { display:none; }
  }
</style>

<section class="slide active">
  <h1>Deck title</h1>
  <p class="muted">Subtitle · Presenter · 2026-05-12</p>
</section>

<section class="slide">
  <h2>One idea per slide</h2>
  <ul>
    <li>Big type, short lines</li>
    <li>Numbers and visuals over prose</li>
    <li>If it needs a paragraph, it's not a slide</li>
  </ul>
</section>

<section class="slide">
  <h2>Q&amp;A</h2>
  <p>Thanks — questions?</p>
</section>

<div class="counter"><span id="cur">1</span> / <span id="total">3</span></div>
<div class="hint">← → arrows · space · F to fullscreen</div>

<script>
  const slides = document.querySelectorAll('.slide');
  let i = 0;
  const total = document.getElementById('total');
  const cur   = document.getElementById('cur');
  total.textContent = slides.length;
  const show = n => {
    i = Math.max(0, Math.min(slides.length - 1, n));
    slides.forEach((s, idx) => s.classList.toggle('active', idx === i));
    cur.textContent = i + 1;
  };
  document.addEventListener('keydown', e => {
    if (e.key === 'ArrowRight' || e.key === ' ') show(i + 1);
    else if (e.key === 'ArrowLeft') show(i - 1);
    else if (e.key === 'f' || e.key === 'F') document.documentElement.requestFullscreen?.();
  });
  document.addEventListener('click', e => {
    if (e.target.closest('a, button, input, select')) return;
    show(i + 1);
  });
</script>
```

## Pitfalls

- Don't cram. If a slide has more than ~30 words, split it.
- Don't use the `max-width: 860px` from `base.html` for decks — slides want the full viewport. Override.
- Test arrow keys *and* spacebar — meeting clickers usually send Right Arrow.
- Tell the user how to fullscreen (`F`) and how to print to PDF (Cmd+P, "Save as PDF") in the chat reply.
