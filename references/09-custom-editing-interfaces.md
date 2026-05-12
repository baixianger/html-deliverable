# 09 — Custom Editing Interfaces

**Use when:** the user needs to *manipulate* structured data (re-order, toggle, edit values, drag between buckets) and then export the result. The deliverable isn't a doc — it's a tiny tool.

## What to put in the file

1. **TL;DR** — what data the user is editing and what they get out.
2. **The editor** — the interactive surface (board, list, form, template editor) in the main column.
3. **Live preview / state panel** — what the current edits look like (rendered output, JSON, etc.).
4. **Validation / warnings** — inline messages when an edit creates a conflict or breaks a dependency.
5. **Export button** — download as JSON / CSV / Markdown / clipboard copy. The user must be able to leave with their work.

## Snippet — drag-and-drop ticket board with export

```html
<style>
  .board { display:grid; grid-template-columns: repeat(3, 1fr); gap:12px; }
  .col { background:var(--card); border:1px solid var(--border); border-radius:10px;
         padding:12px; min-height:280px; }
  .col h3 { margin:0 0 10px; font-size:13px; text-transform:uppercase;
            letter-spacing:.08em; color:var(--muted); }
  .ticket { background:var(--bg); border:1px solid var(--border); border-radius:6px;
            padding:8px 10px; margin-bottom:8px; cursor:grab; }
  .ticket.dragging { opacity:.4; }
  .col.over { outline: 2px dashed var(--accent); }
  .toolbar { display:flex; justify-content:flex-end; gap:8px; margin:16px 0; }
  .btn { background:var(--accent); color:#fff; border:0; padding:8px 14px;
         border-radius:6px; cursor:pointer; }
</style>

<section>
  <h2>Sprint planning board</h2>
  <p class="muted">Drag tickets between columns, then export.</p>

  <div class="board" id="board">
    <div class="col" data-col="todo"><h3>To do</h3>
      <div class="ticket" draggable="true" data-id="A-1">A-1 · Login flow</div>
      <div class="ticket" draggable="true" data-id="A-2">A-2 · Empty state</div>
    </div>
    <div class="col" data-col="doing"><h3>In progress</h3>
      <div class="ticket" draggable="true" data-id="A-3">A-3 · Session rotation</div>
    </div>
    <div class="col" data-col="done"><h3>Done</h3></div>
  </div>

  <div class="toolbar">
    <button class="btn" id="export-json">Copy JSON</button>
    <button class="btn" id="download">Download .json</button>
  </div>

  <details><summary>Current state</summary>
    <pre id="state"></pre>
  </details>
</section>

<script>
  const board = document.getElementById('board');
  let dragged = null;

  board.addEventListener('dragstart', e => {
    if (!e.target.classList.contains('ticket')) return;
    dragged = e.target;
    dragged.classList.add('dragging');
  });
  board.addEventListener('dragend', () => {
    dragged?.classList.remove('dragging');
    dragged = null;
    document.querySelectorAll('.col').forEach(c => c.classList.remove('over'));
    render();
  });
  document.querySelectorAll('.col').forEach(col => {
    col.addEventListener('dragover', e => { e.preventDefault(); col.classList.add('over'); });
    col.addEventListener('dragleave', () => col.classList.remove('over'));
    col.addEventListener('drop', e => {
      e.preventDefault();
      if (dragged) col.appendChild(dragged);
    });
  });

  const snapshot = () => {
    const out = {};
    document.querySelectorAll('.col').forEach(col => {
      out[col.dataset.col] = [...col.querySelectorAll('.ticket')].map(t => t.dataset.id);
    });
    return out;
  };
  const render = () => {
    document.getElementById('state').textContent = JSON.stringify(snapshot(), null, 2);
  };
  render();

  document.getElementById('export-json').addEventListener('click', async () => {
    await navigator.clipboard.writeText(JSON.stringify(snapshot(), null, 2));
    alert('Copied');
  });
  document.getElementById('download').addEventListener('click', () => {
    const blob = new Blob([JSON.stringify(snapshot(), null, 2)], { type: 'application/json' });
    const a = document.createElement('a');
    a.href = URL.createObjectURL(blob);
    a.download = 'board.json';
    a.click();
  });
</script>
```

## Other useful editor shapes

- **Toggle matrix** — checkbox grid with dependency warnings ("enabling X requires Y").
- **Template editor** — `<textarea>` with `{{variable}}` highlighting and a live-rendered preview pane beside it.
- **CSV-style inline editor** — `contenteditable` cells, validate on blur, export back to CSV.

## Pitfalls

- Always provide export. An editor that can't get the user's work out is worse than a piece of paper.
- Persist to `localStorage` if the user might reload — losing 20 minutes of edits is a betrayal.
- Don't reimplement a full app. If the editor needs auth, multi-user, or a real backend, this skill is the wrong tool — say so and propose a real project.
