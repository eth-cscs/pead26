[](){#ref-docs-submissions}
# Doc Example Submissions

Submit a link to a piece of documentation you'd like to share with the group — good, bad, or somewhere in between.
Submissions are publicly visible to everyone.

<style>
.bof-section { margin: 1.5rem 0; }
.bof-form label {
  display: block;
  font-weight: 600;
  font-size: 0.85rem;
  margin-bottom: 4px;
}
.bof-form input,
.bof-form select,
.bof-form textarea {
  width: 100%;
  padding: 8px 12px;
  margin-bottom: 14px;
  border: 1px solid var(--md-default-fg-color--lighter);
  border-radius: 4px;
  background: var(--md-default-bg-color);
  color: var(--md-default-fg-color);
  font-family: inherit;
  font-size: 0.9rem;
  box-sizing: border-box;
}
.bof-form input:focus,
.bof-form select:focus,
.bof-form textarea:focus {
  outline: 2px solid var(--md-primary-fg-color);
  border-color: transparent;
}
.bof-submit-btn {
  background: var(--md-primary-fg-color);
  color: var(--md-primary-bg-color);
  border: none;
  padding: 10px 28px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.9rem;
  font-weight: 600;
  font-family: inherit;
}
.bof-submit-btn:hover { opacity: 0.85; }
.bof-submit-btn:disabled { opacity: 0.5; cursor: not-allowed; }
.bof-status { margin-left: 12px; font-size: 0.9rem; vertical-align: middle; }
.bof-refresh-btn {
  background: none;
  border: 1px solid var(--md-default-fg-color--lighter);
  border-radius: 4px;
  padding: 4px 12px;
  cursor: pointer;
  font-size: 0.85rem;
  color: var(--md-default-fg-color);
  font-family: inherit;
}
.bof-refresh-btn:hover { border-color: var(--md-primary-fg-color); }
.sub-card {
  border: 1px solid var(--md-default-fg-color--lighter);
  border-radius: 6px;
  padding: 12px 16px;
  margin: 10px 0;
}
.sub-badge {
  display: inline-block;
  padding: 2px 10px;
  border-radius: 12px;
  font-size: 0.75rem;
  font-weight: 700;
  margin-bottom: 8px;
  text-transform: uppercase;
  letter-spacing: 0.04em;
}
.badge-great       { background: #c3e6cb; color: #155724; }
.badge-interesting { background: #bee5eb; color: #0c5460; }
.badge-dragons     { background: #f5c6cb; color: #721c24; }
[data-md-color-scheme="slate"] .badge-great       { background: #1e4d2b; color: #8fd4a5; }
[data-md-color-scheme="slate"] .badge-interesting { background: #1a3f47; color: #7ecfdb; }
[data-md-color-scheme="slate"] .badge-dragons     { background: #4d1a1e; color: #e8a0a5; }
.sub-url a { word-break: break-all; font-size: 0.95rem; }
.sub-meta { font-size: 0.8rem; color: var(--md-default-fg-color--light); margin-top: 4px; }
.sub-notes { margin-top: 8px; font-size: 0.9rem; }
.sub-empty { font-style: italic; color: var(--md-default-fg-color--light); }
</style>

## Submit an example

<div class="bof-section bof-form">
  <form id="sub-form">
    <label for="sub-url">Documentation URL *</label>
    <input type="url" id="sub-url" placeholder="https://..." required>

    <label for="sub-site">Site / Organization</label>
    <input type="text" id="sub-site" placeholder="e.g. CSCS, ORNL, Bristol…">

    <label for="sub-category">Category *</label>
    <select id="sub-category" required>
      <option value="">— select one —</option>
      <option value="great">Great example</option>
      <option value="interesting">Interesting story</option>
      <option value="dragons">Here be dragons</option>
    </select>

    <label for="sub-notes">Notes</label>
    <textarea id="sub-notes" rows="3" placeholder="What makes this stand out?"></textarea>

    <button type="submit" class="bof-submit-btn" id="sub-btn">Submit</button>
    <span class="bof-status" id="sub-status"></span>
  </form>
</div>

## Submissions

<div class="bof-section" style="display:flex; align-items:center; gap:12px; margin-bottom:0.5rem;">
  <button class="bof-refresh-btn" onclick="loadSubmissions()">↻ Refresh</button>
  <span id="sub-count" style="font-size:0.85rem; color:var(--md-default-fg-color--light);"></span>
</div>

<div id="sub-list" class="bof-section"><em class="sub-empty">Loading…</em></div>

<script>
// ── Supabase config ──────────────────────────────────────────────────────────
// Replace these two values after creating the Supabase project and table.
const SUPABASE_URL      = 'https://uxmurrqkicaqjflzopma.supabase.co';
const SUPABASE_ANON_KEY = 'sb_publishable_0-Smgn30omOoznSTUzYgNA_UJ4SwuXx';
// ────────────────────────────────────────────────────────────────────────────

const API_HEADERS = {
  'apikey':        SUPABASE_ANON_KEY,
  'Authorization': 'Bearer ' + SUPABASE_ANON_KEY,
  'Content-Type':  'application/json',
};

const CATEGORIES = {
  great:       { label: 'Great example',     cls: 'badge-great' },
  interesting: { label: 'Interesting story', cls: 'badge-interesting' },
  dragons:     { label: 'Here be dragons',   cls: 'badge-dragons' },
};

function esc(str) {
  return String(str ?? '')
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;');
}

async function loadSubmissions() {
  const list  = document.getElementById('sub-list');
  const count = document.getElementById('sub-count');
  try {
    const resp = await fetch(
      SUPABASE_URL + '/rest/v1/submissions?select=*&order=created_at.asc',
      { headers: API_HEADERS }
    );
    if (!resp.ok) throw new Error(await resp.text());
    const rows = await resp.json();
    count.textContent = rows.length === 0 ? '' : rows.length + ' submission' + (rows.length === 1 ? '' : 's');
    if (rows.length === 0) {
      list.innerHTML = '<em class="sub-empty">No submissions yet.</em>';
      return;
    }
    list.innerHTML = rows.map(row => {
      const cat = CATEGORIES[row.category] || { label: row.category, cls: '' };
      return [
        '<div class="sub-card">',
        '  <span class="sub-badge ' + cat.cls + '">' + cat.label + '</span>',
        '  <div class="sub-url"><a href="' + esc(row.url) + '" target="_blank" rel="noopener">' + esc(row.url) + '</a></div>',
        row.site  ? '<div class="sub-meta">' + esc(row.site) + '</div>' : '',
        row.notes ? '<div class="sub-notes">' + esc(row.notes) + '</div>' : '',
        '</div>',
      ].join('\n');
    }).join('\n');
  } catch (err) {
    list.innerHTML = '<span style="color:var(--md-typeset-del-color)">Error loading submissions: ' + esc(err.message) + '</span>';
    count.textContent = '';
  }
}

document.getElementById('sub-form').addEventListener('submit', async function(e) {
  e.preventDefault();
  const btn    = document.getElementById('sub-btn');
  const status = document.getElementById('sub-status');
  btn.disabled = true;
  status.style.color = 'inherit';
  status.textContent = 'Submitting…';

  const payload = {
    url:      document.getElementById('sub-url').value.trim(),
    site:     document.getElementById('sub-site').value.trim()  || null,
    category: document.getElementById('sub-category').value,
    notes:    document.getElementById('sub-notes').value.trim() || null,
  };

  try {
    const resp = await fetch(SUPABASE_URL + '/rest/v1/submissions', {
      method:  'POST',
      headers: Object.assign({}, API_HEADERS, { 'Prefer': 'return=minimal' }),
      body:    JSON.stringify(payload),
    });
    if (!resp.ok) throw new Error(await resp.text());
    status.style.color = 'green';
    status.textContent = 'Submitted!';
    e.target.reset();
    loadSubmissions();
    setTimeout(() => { status.textContent = ''; }, 4000);
  } catch (err) {
    status.style.color = 'red';
    status.textContent = 'Error: ' + esc(err.message);
  } finally {
    btn.disabled = false;
  }
});

// Load on page open, then refresh every 30 s
loadSubmissions();
setInterval(loadSubmissions, 30000);
</script>
