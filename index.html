<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>512 Song Bracket</title>
<style>
:root{
  --bg:#f7f5f2; --fg:#201a24; --muted:#7a7080; --card:#ffffff; --border:#d9d3dc;
  --line:#b8afbd; --accent:#c2255c; --accentbg:#fde7ef; --hl:#ffe08a; --lose:#a79eac;
}
@media (prefers-color-scheme:dark){
  :root{--bg:#15111a; --fg:#f2eef5; --muted:#9d93a4; --card:#221c29; --border:#3a3243;
        --line:#4c4457; --accent:#ff6b9d; --accentbg:#3a1a2a; --hl:#7a5a00; --lose:#6f667a;}
}
*{box-sizing:border-box}
html,body{height:100%;margin:0}
body{background:var(--bg);color:var(--fg);font:14px/1.4 system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;display:flex;flex-direction:column}
header{display:flex;flex-wrap:wrap;gap:8px;align-items:center;padding:10px 14px;border-bottom:1px solid var(--border);background:var(--card)}
header h1{font-size:17px;margin:0 8px 0 0;font-weight:700}
button,input,textarea{font:inherit;color:inherit}
button{background:var(--card);border:1px solid var(--border);border-radius:6px;padding:5px 11px;cursor:pointer}
button:hover{border-color:var(--accent)}
button:focus-visible,input:focus-visible,textarea:focus-visible,.s:focus-visible{outline:2px solid var(--accent);outline-offset:1px}
button.primary{background:var(--accent);border-color:var(--accent);color:#fff}
input[type=text],input[type=password]{background:var(--bg);border:1px solid var(--border);border-radius:6px;padding:5px 9px}
#status{margin-left:auto;color:var(--muted);font-size:12px}
#admin{display:none;padding:12px 14px;border-bottom:1px solid var(--border);background:var(--card)}
#admin textarea{width:100%;height:150px;background:var(--bg);border:1px solid var(--border);border-radius:6px;padding:8px;resize:vertical}
#admin .row{display:flex;flex-wrap:wrap;gap:8px;align-items:center;margin-top:8px}
#msg{color:var(--accent);font-size:13px}
#view{flex:1;overflow:auto;position:relative;background:var(--bg)}
#empty{padding:40px;text-align:center;color:var(--muted)}
#canvas{position:absolute;left:0;top:0;transform-origin:0 0}
.m{position:absolute;width:170px;height:40px;border:1px solid var(--border);border-radius:5px;background:var(--card);overflow:hidden}
.s{height:19px;padding:0 6px;line-height:19px;font-size:11px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;cursor:default}
.s + .s{border-top:1px solid var(--border)}
.s.can{cursor:pointer}
.s.can:hover{background:var(--accentbg)}
.s.empty{color:var(--muted)}
.s.win{background:var(--accentbg);color:var(--accent);font-weight:700}
.s.lose{color:var(--lose);text-decoration:line-through}
.s.hl{background:var(--hl);color:var(--fg)}
.lbl{position:absolute;width:170px;text-align:center;font-size:12px;font-weight:600;color:var(--muted)}
.champ{position:absolute;width:170px;text-align:center;font-weight:700;padding:8px 6px;border:2px solid var(--accent);border-radius:8px;background:var(--card);color:var(--accent);font-size:13px}
svg{position:absolute;left:0;top:0;pointer-events:none}
svg path{fill:none;stroke:var(--line);stroke-width:1.5}
svg path.done{stroke:var(--accent);stroke-width:2}
</style>
</head>
<body>

<header>
  <h1>512 Song Bracket</h1>
  <button id="zout" title="Zoom out">−</button>
  <button id="zin" title="Zoom in">+</button>
  <button id="gofinal">Go to final</button>
  <input type="text" id="search" placeholder="Find a song (Enter)">
  <button id="adminBtn">Admin</button>
  <span id="status"></span>
</header>

<section id="admin">
  <div id="lock">
    <input type="password" id="pw" placeholder="Admin password">
    <button id="unlock" class="primary">Unlock</button>
    <span id="pwmsg" style="color:var(--accent)"></span>
  </div>
  <div id="tools" style="display:none">
    <div style="margin-bottom:6px;color:var(--muted)">Paste exactly 512 songs, one per line. Pairings are randomized when you create the bracket.</div>
    <textarea id="ta" placeholder="Song 1&#10;Song 2&#10;Song 3&#10;..."></textarea>
    <div class="row">
      <span id="count" style="color:var(--muted)">0 / 512</span>
      <button id="create" class="primary">Create bracket</button>
      <button id="reshuffle">Re-shuffle pairings</button>
      <button id="resetPicks">Clear all picks</button>
      <span id="msg"></span>
    </div>
  </div>
</section>

<div id="view">
  <div id="empty">The bracket hasn't been created yet.</div>
  <div id="sizer"><div id="canvas"></div></div>
</div>

<script>
/* ================= SETTINGS ================= */
const CONFIG = {
  // Your Firebase Realtime Database URL, no trailing slash. Example:
  // "https://my-bracket-default-rtdb.firebaseio.com"
  DB_URL: "",
  PATH: "bracket",             // where the data lives inside the database
  ADMIN_PASSWORD: "changeme",  // change this
  ONLY_ADMIN_CAN_PICK: true,   // false = any visitor can click to advance songs
  POLL_MS: 8000                // how often visitors check for updates
};
/* ============================================ */

const N = 512, ROUNDS = 9, W = 170, G = 30, H = 40, TOP = 34;
const LABELS = ["Round of 512","Round of 256","Round of 128","Round of 64","Round of 32",
                "Round of 16","Quarterfinals","Semifinals","Final"];
const TOTAL_W = 10 * (W + G) - G;   // 9 rounds + champion column
const TOTAL_H = TOP + 256 * H + 20;

let state = null;      // {seeds:[512], picks:[[...] per round], updated:number}
let admin = false;
let dirty = false;
let zoom = 0.8;
let hlName = null;

const $ = id => document.getElementById(id);
const esc = s => String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
const dbUrl = () => CONFIG.DB_URL.replace(/\/$/, "") + "/" + CONFIG.PATH + ".json";
const setStatus = t => { $('status').textContent = t; };

/* ---------- data helpers ---------- */
function emptyPicks(){
  const p = [];
  for (let r = 0; r < ROUNDS; r++) p.push(new Array(256 >> r).fill(null));
  return p;
}
const encodePicks = () => state.picks.map(a => a.map(v => v === null ? '-' : v).join('')).join(',');
function decodePicks(s){
  const p = emptyPicks();
  const parts = String(s || "").split(',');
  for (let r = 0; r < ROUNDS; r++){
    const str = parts[r] || "";
    for (let i = 0; i < p[r].length; i++){
      const c = str[i];
      p[r][i] = (c === '0' || c === '1') ? Number(c) : null;
    }
  }
  return p;
}
function shuffle(a){
  a = a.slice();
  for (let i = a.length - 1; i > 0; i--){
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}
function entry(r, i, j){
  if (r === 0) return state.seeds[2 * i + j];
  const feeder = 2 * i + j;
  const p = state.picks[r - 1][feeder];
  return p === null ? null : entry(r - 1, feeder, p);
}

/* ---------- layout ---------- */
function pos(r, i){
  return { x: r * (W + G), y: TOP + (i + 0.5) * H * (1 << r) };
}

function render(){
  const has = !!state;
  $('empty').style.display = has ? 'none' : 'block';
  $('sizer').style.display = has ? 'block' : 'none';
  if (!has) return;

  const canPick = !CONFIG.ONLY_ADMIN_CAN_PICK || admin;
  let html = '';
  let svg = '';

  // round labels
  for (let r = 0; r < ROUNDS; r++){
    html += `<div class="lbl" style="left:${r * (W + G)}px;top:6px">${LABELS[r]}</div>`;
  }

  for (let r = 0; r < ROUNDS; r++){
    const n = 256 >> r;
    for (let i = 0; i < n; i++){
      const p = pos(r, i);
      const picked = state.picks[r][i];
      let slots = '';
      for (let j = 0; j < 2; j++){
        const name = entry(r, i, j);
        let cls = 's';
        if (name === null) cls += ' empty';
        else if (canPick) cls += ' can';
        if (picked === j) cls += ' win';
        else if (picked !== null) cls += ' lose';
        if (hlName && name === hlName) cls += ' hl';
        slots += `<div class="${cls}" data-r="${r}" data-i="${i}" data-j="${j}" title="${name === null ? '' : esc(name)}">${name === null ? '—' : esc(name)}</div>`;
      }
      html += `<div class="m" style="left:${p.x}px;top:${p.y - H / 2}px">${slots}</div>`;

      // connector to next round
      if (r < 8){
        const q = pos(r + 1, i >> 1);
        const x1 = p.x + W, x2 = q.x;
        const xm = (x1 + x2) / 2;
        svg += `<path class="${picked !== null ? 'done' : ''}" d="M${x1} ${p.y}H${xm}V${q.y}H${x2}"/>`;
      }
    }
  }

  // champion
  const f = pos(8, 0), fp = state.picks[8][0];
  const champ = fp === null ? null : entry(8, 0, fp);
  const cx = 9 * (W + G);
  html += `<div class="lbl" style="left:${cx}px;top:6px">Champion</div>`;
  html += `<div class="champ" style="left:${cx}px;top:${f.y - 20}px;height:40px;line-height:22px;overflow:hidden;white-space:nowrap;text-overflow:ellipsis" title="${champ === null ? '' : esc(champ)}">${champ === null ? '—' : esc(champ)}</div>`;
  svg += `<path class="${fp !== null ? 'done' : ''}" d="M${f.x + W} ${f.y}H${cx}"/>`;

  $('canvas').innerHTML = `<svg width="${TOTAL_W}" height="${TOTAL_H}">${svg}</svg>` + html;
  applyZoom();
}

function applyZoom(){
  $('canvas').style.transform = `scale(${zoom})`;
  $('sizer').style.width = TOTAL_W * zoom + 'px';
  $('sizer').style.height = TOTAL_H * zoom + 'px';
}

/* ---------- picking ---------- */
function pick(r, i, j){
  if (!state || entry(r, i, j) === null) return;
  if (state.picks[r][i] === j) return;
  state.picks[r][i] = j;
  let ci = i;
  for (let rr = r + 1; rr < ROUNDS; rr++){
    const ni = ci >> 1;
    if (state.picks[rr][ni] === (ci & 1)){ state.picks[rr][ni] = null; ci = ni; }
    else break;
  }
  save();
  render();
}

$('canvas').addEventListener('click', e => {
  const el = e.target.closest('.s');
  if (!el) return;
  if (CONFIG.ONLY_ADMIN_CAN_PICK && !admin){
    setStatus('Only the admin can advance songs.');
    return;
  }
  pick(+el.dataset.r, +el.dataset.i, +el.dataset.j);
});

/* ---------- saving & loading ---------- */
let saveTimer = null;
function save(){
  state.updated = Date.now();
  try { localStorage.setItem('bracket512', JSON.stringify(state)); } catch (e) {}
  if (!CONFIG.DB_URL){ setStatus('Saved in this browser only (no database set)'); return; }
  dirty = true;
  setStatus('Saving…');
  clearTimeout(saveTimer);
  saveTimer = setTimeout(push, 400);
}
async function push(){
  const body = { seeds: state.seeds.join('\n'), picks: encodePicks(), updated: state.updated };
  try {
    const res = await fetch(dbUrl(), { method: 'PUT', body: JSON.stringify(body) });
    if (!res.ok) throw new Error(res.status);
    dirty = false;
    setStatus('Saved ✓ ' + new Date().toLocaleTimeString());
  } catch (e){
    setStatus('Save failed, retrying…');
    saveTimer = setTimeout(push, 4000);
  }
}
function apply(d){
  state = {
    seeds: String(d.seeds || '').split('\n'),
    picks: decodePicks(d.picks),
    updated: d.updated || 0
  };
}
async function pull(){
  if (!CONFIG.DB_URL) return;
  if (dirty) return;
  try {
    const res = await fetch(dbUrl(), { cache: 'no-store' });
    if (!res.ok) throw new Error(res.status);
    const d = await res.json();
    if (d && d.seeds && (!state || d.updated !== state.updated)){
      apply(d);
      render();
    }
    setStatus('Up to date ' + new Date().toLocaleTimeString());
  } catch (e){
    setStatus('Could not reach the database');
  }
}
async function init(){
  if (!CONFIG.DB_URL){
    try {
      const s = localStorage.getItem('bracket512');
      if (s) state = JSON.parse(s);
    } catch (e) {}
    setStatus('No database set: saving in this browser only');
    render();
    return;
  }
  setStatus('Loading…');
  await pull();
  render();
  setInterval(pull, CONFIG.POLL_MS);
}

/* ---------- admin ---------- */
function parseLines(){
  return $('ta').value.split(/\r?\n/).map(s => s.trim()).filter(Boolean);
}
$('adminBtn').onclick = () => {
  const a = $('admin');
  a.style.display = a.style.display === 'block' ? 'none' : 'block';
};
function tryUnlock(){
  if ($('pw').value === CONFIG.ADMIN_PASSWORD){
    admin = true;
    $('lock').style.display = 'none';
    $('tools').style.display = 'block';
    render();
  } else {
    $('pwmsg').textContent = 'Wrong password.';
  }
}
$('unlock').onclick = tryUnlock;
$('pw').addEventListener('keydown', e => { if (e.key === 'Enter') tryUnlock(); });
$('ta').addEventListener('input', () => {
  const n = parseLines().length;
  $('count').textContent = n + ' / 512';
  $('count').style.color = n === 512 ? 'var(--accent)' : 'var(--muted)';
});

let confirmCreate = false;
$('create').onclick = () => {
  const lines = parseLines();
  if (lines.length !== N){
    $('msg').textContent = `You pasted ${lines.length} songs. The bracket needs exactly 512.`;
    return;
  }
  if (state && !confirmCreate){
    confirmCreate = true;
    $('create').textContent = 'Click again to replace the current bracket';
    $('msg').textContent = '';
    setTimeout(() => { confirmCreate = false; $('create').textContent = 'Create bracket'; }, 5000);
    return;
  }
  confirmCreate = false;
  $('create').textContent = 'Create bracket';
  state = { seeds: shuffle(lines), picks: emptyPicks(), updated: 0 };
  hlName = null;
  $('msg').textContent = 'Bracket created.';
  save();
  render();
};
$('reshuffle').onclick = () => {
  if (!state){ $('msg').textContent = 'Create a bracket first.'; return; }
  state.seeds = shuffle(state.seeds);
  state.picks = emptyPicks();
  $('msg').textContent = 'Pairings re-shuffled and picks cleared.';
  save();
  render();
};
$('resetPicks').onclick = () => {
  if (!state) return;
  state.picks = emptyPicks();
  $('msg').textContent = 'Picks cleared.';
  save();
  render();
};

/* ---------- toolbar ---------- */
$('zin').onclick = () => { zoom = Math.min(1.6, zoom + 0.1); applyZoom(); };
$('zout').onclick = () => { zoom = Math.max(0.2, zoom - 0.1); applyZoom(); };
$('gofinal').onclick = () => {
  const v = $('view'), p = pos(8, 0);
  v.scrollTo({ left: (p.x + W / 2) * zoom - v.clientWidth / 2, top: (p.y) * zoom - v.clientHeight / 2, behavior: 'smooth' });
};
$('search').addEventListener('keydown', e => {
  if (e.key !== 'Enter' || !state) return;
  const q = e.target.value.trim().toLowerCase();
  if (!q){ hlName = null; render(); return; }
  const k = state.seeds.findIndex(s => s.toLowerCase().includes(q));
  if (k < 0){ setStatus('No song matches that search.'); return; }
  hlName = state.seeds[k];
  render();
  const v = $('view'), p = pos(0, k >> 1);
  v.scrollTo({ left: p.x * zoom - 40, top: p.y * zoom - v.clientHeight / 2, behavior: 'smooth' });
});

init();
</script>
</body>
</html>
