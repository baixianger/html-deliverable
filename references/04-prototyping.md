# 04 — Prototyping

**Use when:** demonstrating motion, transitions, micro-interactions, or a multi-screen flow that the user needs to *feel*, not just read about.

## What to put in the file

1. **TL;DR** — what interaction is being prototyped and the design question it's answering.
2. **Live demo** — the actual animated/interactive element, prominently centered.
3. **Controls** — sliders or inputs to tweak duration, easing, distance, etc. so the user can dial it in.
4. **Multi-screen flow** (if applicable) — clickable steps. Tap → advance.
5. **Notes** — what to evaluate, what trade-offs exist (e.g. snappy vs. smooth).

## Snippet — animation sandbox with sliders

```html
<section>
  <h2>Card lift on hover</h2>

  <div id="stage" style="display:flex; justify-content:center; padding:60px 0;">
    <div id="demo-card" class="card" style="width:200px; text-align:center; cursor:pointer;
         transition: transform var(--dur, 200ms) var(--ease, ease-out),
                     box-shadow var(--dur, 200ms) var(--ease, ease-out);">
      Hover me
    </div>
  </div>

  <div class="card">
    <label>Duration: <span id="dur-val">200</span> ms
      <input id="dur" type="range" min="50" max="800" value="200" step="10" style="width:100%;">
    </label>
    <label style="display:block; margin-top:12px;">Easing:
      <select id="ease">
        <option>ease-out</option>
        <option>ease-in-out</option>
        <option>cubic-bezier(.34,1.56,.64,1)</option>
        <option>linear</option>
      </select>
    </label>
    <label style="display:block; margin-top:12px;">Lift (px): <span id="lift-val">6</span>
      <input id="lift" type="range" min="0" max="24" value="6" style="width:100%;">
    </label>
  </div>
</section>

<script>
  const card = document.getElementById('demo-card');
  const dur  = document.getElementById('dur');
  const ease = document.getElementById('ease');
  const lift = document.getElementById('lift');
  const apply = () => {
    card.style.setProperty('--dur',  dur.value + 'ms');
    card.style.setProperty('--ease', ease.value);
    document.getElementById('dur-val').textContent  = dur.value;
    document.getElementById('lift-val').textContent = lift.value;
  };
  card.addEventListener('mouseenter', () => {
    card.style.transform = `translateY(-${lift.value}px)`;
    card.style.boxShadow = '0 12px 24px rgba(0,0,0,.12)';
  });
  card.addEventListener('mouseleave', () => {
    card.style.transform = '';
    card.style.boxShadow = '';
  });
  [dur, ease, lift].forEach(el => el.addEventListener('input', apply));
  apply();
</script>
```

## Snippet — clickable multi-screen flow

```html
<section>
  <h2>Onboarding flow</h2>
  <div id="flow" class="card" style="min-height:280px; cursor:pointer; user-select:none;"></div>
  <p class="muted" style="text-align:center;">Click to advance • <span id="step">1</span> / 3</p>
</section>
<script>
  const screens = [
    `<h3>Welcome</h3><p>Sign up in 30 seconds.</p>`,
    `<h3>Pick a workspace</h3><ul><li>Personal</li><li>Team</li></ul>`,
    `<h3>You're in 🎉</h3><p>Tap to restart.</p>`,
  ];
  let i = 0;
  const flow = document.getElementById('flow');
  const step = document.getElementById('step');
  const render = () => { flow.innerHTML = screens[i]; step.textContent = i + 1; };
  flow.addEventListener('click', () => { i = (i + 1) % screens.length; render(); });
  render();
</script>
```

## Pitfalls

- Don't over-control. 3 sliders max — more becomes a settings panel, not a prototype.
- Default values matter. Land on what *you* recommend; let the user tune.
- Respect `prefers-reduced-motion` if the prototype is meant to be production-realistic.
