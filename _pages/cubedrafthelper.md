---
layout: single
title: "Draft Log"
permalink: /cubelogger/
---

<h2 class="sr-only">Cube draft logging wizard — guided deck entry from a CubeCobra cube</h2>

<style>
  :root {
    --c-w: #f9f3d4;
    --c-u: #c0d9f0;
    --c-b: #d1c9e0;
    --c-r: #f5cec7;
    --c-g: #c5e0c0;
    --c-m: #f0e2b0;
    --c-colorless: #ddd9d0;
    --pip-w: #c8a800;
    --pip-u: #1d6fa4;
    --pip-b: #5c3d8f;
    --pip-r: #c0392b;
    --pip-g: #1a7a40;
  }

  #draft-log-app { font-family: var(--global-font-family, -apple-system, sans-serif); max-width: 900px; margin: 0 auto; }
  #draft-log-app * { box-sizing: border-box; }

  .dl-section { border: 1px solid #ddd; border-radius: 8px; padding: 1.5rem; margin-bottom: 1.5rem; background: #fff; }
  .dl-label { font-size: 0.78rem; font-weight: 600; letter-spacing: 0.06em; text-transform: uppercase; color: #888; margin-bottom: 0.5rem; }
  .dl-title { font-size: 1.1rem; font-weight: 600; margin: 0 0 1rem; color: #222; }
  .dl-collapsed { display: none; }

  .dl-input-row { display: flex; gap: 0.5rem; align-items: center; flex-wrap: wrap; }
  .dl-input-row input[type="text"] { flex: 1; min-width: 180px; padding: 0.45rem 0.75rem; border: 1px solid #ccc; border-radius: 6px; font-size: 0.95rem; }

  .dl-btn { padding: 0.45rem 1rem; border: 1px solid #888; border-radius: 6px; background: #f5f5f5; cursor: pointer; font-size: 0.9rem; transition: background 0.15s; white-space: nowrap; }
  .dl-btn:hover { background: #e8e8e8; }
  .dl-btn:active { transform: scale(0.98); }
  .dl-btn.primary { background: #2a5caa; color: #fff; border-color: #2a5caa; }
  .dl-btn.primary:hover { background: #1f4a8c; }
  .dl-btn.danger { background: #c0392b; color: #fff; border-color: #c0392b; }
  .dl-btn.danger:hover { background: #a93226; }

  .dl-progress { display: flex; gap: 0.4rem; margin-bottom: 1.25rem; flex-wrap: wrap; }
  .dl-progress-step { padding: 0.25rem 0.6rem; border-radius: 4px; font-size: 0.78rem; font-weight: 600; background: #f0f0f0; color: #aaa; border: 1px solid #ddd; }
  .dl-progress-step.active { background: #2a5caa; color: #fff; border-color: #2a5caa; }
  .dl-progress-step.done { background: #e6f3ec; color: #1a7a40; border-color: #b3d9c4; }

  .color-selector { display: flex; gap: 0.5rem; flex-wrap: wrap; margin: 0.75rem 0; }
  .color-pip { width: 40px; height: 40px; border-radius: 50%; border: 2px solid #ccc; cursor: pointer; font-size: 1rem; font-weight: 700; display: flex; align-items: center; justify-content: center; transition: border-color 0.15s, transform 0.1s; user-select: none; }
  .color-pip:hover { transform: scale(1.08); }
  .color-pip.selected { border-width: 3px; }
  .color-pip.pip-w { background: var(--c-w); color: var(--pip-w); border-color: #c8a800; }
  .color-pip.pip-u { background: var(--c-u); color: var(--pip-u); border-color: #1d6fa4; }
  .color-pip.pip-b { background: var(--c-b); color: var(--pip-b); border-color: #5c3d8f; }
  .color-pip.pip-r { background: var(--c-r); color: var(--pip-r); border-color: #c0392b; }
  .color-pip.pip-g { background: var(--c-g); color: var(--pip-g); border-color: #1a7a40; }
  .color-pip.pip-w.selected { box-shadow: 0 0 0 3px var(--pip-w); }
  .color-pip.pip-u.selected { box-shadow: 0 0 0 3px var(--pip-u); }
  .color-pip.pip-b.selected { box-shadow: 0 0 0 3px var(--pip-b); }
  .color-pip.pip-r.selected { box-shadow: 0 0 0 3px var(--pip-r); }
  .color-pip.pip-g.selected { box-shadow: 0 0 0 3px var(--pip-g); }

  /* ── Deck preview (images) ─────────────────────────────────── */
  .deck-preview { border: 1px solid #ddd; border-radius: 8px; background: #f8f8f8; padding: 0.75rem 0.75rem 0.5rem; margin-bottom: 1rem; }
  .deck-preview-title { font-weight: 600; font-size: 0.85rem; margin-bottom: 0.6rem; color: #444; }

  .img-curve { display: flex; gap: 0.75rem; overflow-x: auto; padding-bottom: 0.5rem; }
  .img-curve-col { flex: 0 0 auto; min-width: 120px; }
  .img-curve-header { text-align: center; font-size: 0.72rem; font-weight: 700; color: #666; letter-spacing: 0.05em; text-transform: uppercase; margin-bottom: 0.4rem; padding: 0.15rem 0.3rem; background: #eee; border-radius: 4px; }
  .img-curve-stack { display: flex; flex-direction: column; gap: 3px; }

  /* Stacked card images — collapsed to show top strip, expand on hover */
  .img-card-wrap {
    position: relative;
    width: 120px;
    height: 36px;
    cursor: pointer;
    transition: height 0.15s ease;
  }
  .img-curve-stack .img-card-wrap:last-child { height: 60px; }
  .img-curve-stack:hover .img-card-wrap { height: 40px; }
  .img-card-wrap:hover { height: 167px !important; z-index: 20; } /* 120 * 1.392 = MTG aspect ratio */

  .img-card-wrap img {
    position: absolute;
    top: 0; left: 0;
    width: 120px;
    border-radius: 6px;
    display: block;
    box-shadow: 0 1px 5px rgba(0,0,0,0.28);
    transition: box-shadow 0.12s;
  }
  .img-card-wrap:hover img { box-shadow: 0 4px 14px rgba(0,0,0,0.42); z-index: 10; }

  .img-card-remove {
    position: absolute;
    top: 3px; right: 3px;
    width: 18px; height: 18px;
    background: rgba(0,0,0,0.65);
    color: #fff;
    border-radius: 50%;
    font-size: 11px;
    line-height: 18px;
    text-align: center;
    cursor: pointer;
    z-index: 30;
    display: none;
    border: none;
    padding: 0;
  }
  .img-card-wrap:hover .img-card-remove { display: block; }

  /* ── CMC pick columns ─────────────────────────────────────── */
  .pick-area { margin: 0.5rem 0; }
  .pick-area-header { display: flex; align-items: center; gap: 0.75rem; margin-bottom: 0.5rem; flex-wrap: wrap; }

  .filter-row { display: flex; gap: 0.4rem; align-items: center; flex-wrap: wrap; margin-bottom: 0.5rem; }
  .filter-row input[type="text"] { padding: 0.35rem 0.65rem; border: 1px solid #ccc; border-radius: 6px; font-size: 0.85rem; flex: 1; min-width: 120px; }

  .selection-counter { font-size: 0.82rem; color: #555; margin: 0.25rem 0 0.5rem; }
  .selection-counter strong { color: #2a5caa; }

  /* CMC columns for the pick list */
  .cmc-pick-grid { display: flex; gap: 0.5rem; overflow-x: auto; padding-bottom: 0.4rem; }
  .cmc-pick-col { flex: 0 0 auto; min-width: 130px; max-width: 160px; }
  .cmc-pick-header {
    text-align: center;
    font-size: 0.72rem;
    font-weight: 700;
    color: #666;
    letter-spacing: 0.05em;
    text-transform: uppercase;
    padding: 0.2rem 0.4rem;
    background: #eee;
    border-radius: 4px;
    margin-bottom: 0.35rem;
    position: sticky;
    top: 0;
  }
  .cmc-pick-list { display: flex; flex-direction: column; gap: 0.3rem; }

  .card-chip {
    padding: 0.3rem 0.5rem;
    border-radius: 5px;
    border: 1px solid #ccc;
    font-size: 0.79rem;
    cursor: pointer;
    background: #fafafa;
    transition: filter 0.1s;
    line-height: 1.3;
    display: flex;
    align-items: center;
    gap: 5px;
    user-select: none;
  }
  .card-chip:hover { filter: brightness(0.93); }

  .chip-colors { display: flex; gap: 2px; flex-shrink: 0; }
  .chip-dot { width: 7px; height: 7px; border-radius: 50%; flex-shrink: 0; }
  .chip-dot.w { background: var(--pip-w); }
  .chip-dot.u { background: var(--pip-u); }
  .chip-dot.b { background: var(--pip-b); }
  .chip-dot.r { background: var(--pip-r); }
  .chip-dot.g { background: var(--pip-g); }
  .chip-dot.c { background: #999; }

  /* ── Results / history ──────────────────────────────────────── */
  .deck-summary-card { border: 1px solid #ccc; border-radius: 8px; padding: 1rem; margin-bottom: 0.75rem; background: #fafafa; }
  .deck-summary-card .deck-header { display: flex; justify-content: space-between; align-items: flex-start; flex-wrap: wrap; gap: 0.5rem; }
  .deck-name-display { font-weight: 600; font-size: 1rem; color: #222; }
  .deck-card-count { font-size: 0.82rem; color: #666; margin-top: 0.3rem; }
  .deck-card-list { margin-top: 0.5rem; font-size: 0.8rem; color: #555; line-height: 1.6; column-count: 2; column-gap: 1rem; }

  .deck-badge { width: 22px; height: 22px; border-radius: 50%; font-size: 0.7rem; font-weight: 700; display: inline-flex; align-items: center; justify-content: center; border: 1px solid #ccc; }
  .badge-w { background: var(--c-w); color: var(--pip-w); border-color: #c8a800; }
  .badge-u { background: var(--c-u); color: var(--pip-u); border-color: #1d6fa4; }
  .badge-b { background: var(--c-b); color: var(--pip-b); border-color: #5c3d8f; }
  .badge-r { background: var(--c-r); color: var(--pip-r); border-color: #c0392b; }
  .badge-g { background: var(--c-g); color: var(--pip-g); border-color: #1a7a40; }

  .dl-hint { font-size: 0.82rem; color: #888; margin: 0.4rem 0 0; }
  .dl-info-badge { display: inline-block; padding: 0.2rem 0.55rem; background: #e8f0fb; color: #1a3d7c; border-radius: 12px; font-size: 0.78rem; font-weight: 600; margin-left: 0.5rem; }
  .dl-error { background: #fff0f0; border: 1px solid #f5a0a0; border-radius: 6px; padding: 0.6rem 0.9rem; font-size: 0.85rem; color: #880000; margin: 0.5rem 0; }
  .dl-success-banner { background: #e6f3ec; border: 1px solid #b3d9c4; border-radius: 8px; padding: 1rem 1.25rem; color: #1a4d2c; margin-bottom: 1rem; }
  .player-name-input { padding: 0.35rem 0.65rem; border: 1px solid #ccc; border-radius: 6px; font-size: 0.9rem; width: 100%; margin-top: 0.3rem; }

  .step-actions { display: flex; gap: 0.5rem; flex-wrap: wrap; margin-top: 1rem; align-items: center; }
  .step-spacer { flex: 1; }

  .section-divider { display: flex; align-items: center; gap: 0.75rem; margin: 1.25rem 0 1rem; }
  .section-divider .divider-label { font-size: 0.78rem; font-weight: 600; letter-spacing: 0.06em; text-transform: uppercase; color: #888; white-space: nowrap; }
  .section-divider hr { flex: 1; border: none; border-top: 1px solid #ddd; margin: 0; }

  .session-output { background: #f6f6f6; border: 1px solid #ddd; border-radius: 6px; padding: 1rem; font-family: monospace; font-size: 0.8rem; white-space: pre-wrap; max-height: 400px; overflow-y: auto; color: #333; }

  .history-table { width: 100%; border-collapse: collapse; font-size: 0.85rem; }
  .history-table th { text-align: left; font-size: 0.75rem; font-weight: 600; text-transform: uppercase; letter-spacing: 0.04em; color: #888; padding: 0.4rem 0.5rem; border-bottom: 1px solid #ddd; }
  .history-table td { padding: 0.5rem; border-bottom: 1px solid #f0f0f0; vertical-align: top; }
  .history-table tr:last-child td { border-bottom: none; }

  @media (max-width: 600px) {
    .deck-card-list { column-count: 1; }
    .cmc-pick-col { min-width: 110px; }
    .img-curve-col { min-width: 90px; }
    .img-card-wrap { width: 88px; }
    .img-card-wrap img { width: 88px; }
    .img-card-wrap:hover { height: 122px !important; }
  }
</style>

<div id="draft-log-app">

  <div class="dl-section" id="setup-section">
    <div class="dl-label">Cube setup</div>
    <div class="dl-input-row">
      <input type="text" id="cube-id-input" placeholder="CubeCobra ID (e.g. c0b20dfd-ac3b-41bd-8fa0-c971fb6bd301)" />
      <button class="dl-btn primary" onclick="loadCube()">Load cube</button>
    </div>
    <div class="dl-hint">Paste your CubeCobra cube ID or full URL. The card list is fetched once and cached for the session.</div>
    <div id="cube-status"></div>
  </div>

  <div class="dl-section dl-collapsed" id="wizard-section">
    <div id="wizard-content"></div>
  </div>

  <div class="dl-section dl-collapsed" id="results-section">
    <div class="dl-label">Draft session</div>
    <div id="results-content"></div>
  </div>

  <div class="dl-section dl-collapsed" id="history-section">
    <div class="dl-label">Past sessions</div>
    <div id="history-content"></div>
  </div>

</div>

<script>
(function() {

const COLOR_DEFS = [
  { code: 'W', label: 'White', cls: 'pip-w', glyph: 'W' },
  { code: 'U', label: 'Blue',  cls: 'pip-u', glyph: 'U' },
  { code: 'B', label: 'Black', cls: 'pip-b', glyph: 'B' },
  { code: 'R', label: 'Red',   cls: 'pip-r', glyph: 'R' },
  { code: 'G', label: 'Green', cls: 'pip-g', glyph: 'G' },
];

// Scryfall small image URL from a card name — browser follows the redirect fine
function scryfallImg(name) {
  return 'https://api.scryfall.com/cards/named?exact=' + encodeURIComponent(name) + '&format=image&version=small';
}

let cubeCards = [];
let cubeName  = '';
let cubeId    = '';
let session   = { date: '', cubeId: '', cubeName: '', decks: [] };
let wizardState = { step: 'howmany', deckCount: 0, currentDeck: 0, currentColors: [] };

function el(id) { return document.getElementById(id); }
function show(id) { el(id).classList.remove('dl-collapsed'); }
function hide(id) { el(id).classList.add('dl-collapsed'); }

async function loadCube() {
  cubeId = el('cube-id-input').value.trim();
  if (!cubeId) { showStatus('Please enter a CubeCobra ID.', 'error'); return; }
  // strip full URLs down to just the ID
  const m = cubeId.match(/cubecobra\.com\/cube\/[^/]+\/([^/?#\s]+)/);
  if (m) cubeId = m[1];

  showStatus('Loading cube…', 'info');
  try {
    const data = await fetchCubeJSON(cubeId);
    cubeCards = normalizeCards(data);
    cubeName  = data.name || cubeId;
    if (!cubeCards.length) throw new Error('No cards found in cube response.');
    showStatus('✓ Loaded <strong>' + cubeName + '</strong> — ' + cubeCards.length + ' cards', 'ok');
    startWizard();
    loadHistory();
  } catch(e) {
    showStatus('Could not load cube: ' + e.message + '. Check the ID and try again.', 'error');
  }
}

async function fetchCubeJSON(id) {
  const url = 'https://cubecobra.com/cube/api/cubeJSON/' + encodeURIComponent(id);
  const resp = await fetch(url);
  if (!resp.ok) throw new Error('HTTP ' + resp.status);
  return resp.json();
}

function normalizeCards(data) {
  let cards = [];
  if (data.cards) {
    if (Array.isArray(data.cards.mainboard)) cards = data.cards.mainboard;
    else if (Array.isArray(data.cards))      cards = data.cards;
  }
  return cards.map(card => {
    const details = card.details || card.card_details || card;
    let colors = details.colors || card.colors || [];
    if (typeof colors === 'string') colors = colors.split('');
    colors = (colors || []).map(c => String(c).toUpperCase()).filter(c => 'WUBRG'.includes(c));
    return {
      name:     details.name     || card.name     || 'Unknown',
      colors,
      typeLine: details.type_line || details.type || '',
      cmc:      details.cmc != null ? details.cmc : (card.cmc != null ? card.cmc : null),
    };
  }).filter(c => c.name !== 'Unknown');
}

function showStatus(msg, type) {
  const div = el('cube-status');
  div.className = type === 'error' ? 'dl-error' : type === 'ok' ? 'dl-success-banner' : 'dl-hint';
  div.innerHTML = msg;
  div.style.marginTop = '0.75rem';
}

function startWizard() {
  session = {
    date: new Date().toLocaleDateString('de-DE', { year: 'numeric', month: '2-digit', day: '2-digit' }),
    cubeId, cubeName, decks: []
  };
  wizardState = { step: 'howmany', deckCount: 0, currentDeck: 0, currentColors: [] };
  show('wizard-section');
  renderWizard();
}

// ── Color ordering & sorting ──────────────────────────────────────
// WUBRG order index; multicolor = 5, colorless = 6
const COLOR_ORDER = { W: 0, U: 1, B: 2, R: 3, G: 4 };

function colorSortKey(card) {
  const c = card.colors || [];
  if (c.length === 0) return 6;          // colorless last
  if (c.length > 1)  return 5;          // multicolor second-to-last
  return COLOR_ORDER[c[0]] ?? 4;        // mono by WUBRG order
}

function sortByColorThenName(arr) {
  return [...arr].sort((a, b) => {
    const ck = colorSortKey(a) - colorSortKey(b);
    if (ck !== 0) return ck;
    return a.name.localeCompare(b.name);
  });
}

// ── Mana curve bucketing ──────────────────────────────────────────
function cmcBuckets(cardList, sortFn) {
  const b = {0:[],1:[],2:[],3:[],4:[],5:[],6:[]};
  cardList.forEach(card => {
    const raw = typeof card.cmc === 'number' && !isNaN(card.cmc) ? card.cmc : 6;
    const key = Math.min(Math.floor(raw), 6);
    b[key].push(card);
  });
  const fn = sortFn || (arr => arr.sort((a,b) => a.name.localeCompare(b.name)));
  Object.keys(b).forEach(k => { b[k] = fn(b[k]); });
  return b;
}

// ── Chip name coloring ────────────────────────────────────────────
// Mono colors: gentle tinted background + dark matching text
const CHIP_MONO_STYLE = {
  W: 'background:#fdf7d4;color:#7a6000;border-color:#d4b800;',
  U: 'background:#daedf9;color:#0d4d7a;border-color:#1d6fa4;',
  B: 'background:#e0d8ee;color:#3a1f6e;border-color:#5c3d8f;',
  R: 'background:#fde0da;color:#8c2010;border-color:#c0392b;',
  G: 'background:#d4edcc;color:#0f4d20;border-color:#1a7a40;',
};
// Multicolor: gold gradient
const CHIP_MULTI_STYLE = 'background:linear-gradient(135deg,#fdf7d4 0%,#fce8a0 50%,#f5d060 100%);color:#5c3d00;border-color:#c8a000;';
// Colorless: neutral gray
const CHIP_COLORLESS_STYLE = 'background:#f0ede8;color:#4a4640;border-color:#aaa;';

function chipStyle(colors) {
  if (!colors || colors.length === 0) return CHIP_COLORLESS_STYLE;
  if (colors.length > 1)             return CHIP_MULTI_STYLE;
  return CHIP_MONO_STYLE[colors[0]] || CHIP_COLORLESS_STYLE;
}

// ── Deck preview with card images ─────────────────────────────────
function renderDeckPreview(deck) {
  const selCards = deck.cards || [];
  const cardObjs = cubeCards.filter(c => selCards.includes(c.name));
  const buckets  = cmcBuckets(cardObjs, sortByColorThenName);
  const CMC_LABELS = ['0','1','2','3','4','5','6+'];

  const colsHTML = [0,1,2,3,4,5,6].map(cmc => {
    const cards = buckets[cmc];
    if (!cards.length) return '';
    const stackHTML = cards.map(card => `
      <div class="img-card-wrap" title="${escHtml(card.name)}">
        <img src="${scryfallImg(card.name)}"
             alt="${escHtml(card.name)}"
             loading="lazy"
             onerror="this.style.display='none';this.nextElementSibling.style.display='block';" />
        <span style="display:none;font-size:0.7rem;padding:2px 4px;background:#eee;border-radius:3px;line-height:1.3;">${escHtml(card.name)}</span>
        <button class="img-card-remove" onclick="removeCard('${escHtml(card.name).replace(/'/g,"\\'")}',event)" title="Remove">✕</button>
      </div>
    `).join('');
    return `
      <div class="img-curve-col">
        <div class="img-curve-header">${CMC_LABELS[cmc]} <span style="font-weight:400;opacity:0.7;">(${cards.length})</span></div>
        <div class="img-curve-stack">${stackHTML}</div>
      </div>`;
  }).join('');

  const total = selCards.length;
  return `
    <div class="deck-preview-title">Current deck — <strong>${total}</strong> card${total !== 1 ? 's' : ''}</div>
    <div class="img-curve">
      ${colsHTML || '<span style="font-size:0.82rem;color:#aaa;">No cards selected yet</span>'}
    </div>`;
}

// ── Pick list by CMC ──────────────────────────────────────────────
function renderPickGrid(deck, searchVal) {
  const selCards = deck.cards || [];
  const colors   = deck.colors;

  // color-filter, then exclude already-selected cards
  const filtered = filterCardsByColors(cubeCards, colors)
    .filter(c => !selCards.includes(c.name));

  const shown = searchVal
    ? filtered.filter(c => c.name.toLowerCase().includes(searchVal.toLowerCase()))
    : filtered;

  const buckets = cmcBuckets(shown, sortByColorThenName);
  const CMC_LABELS = ['0','1','2','3','4','5','6+'];
  const total = shown.length;

  const colsHTML = [0,1,2,3,4,5,6].map(cmc => {
    const cards = buckets[cmc];
    if (!cards.length) return '';
    const chipsHTML = cards.map(card => {
      const style = chipStyle(card.colors);
      return `<div class="card-chip" style="${style}"
                   onclick="toggleCard('${escHtml(card.name).replace(/'/g,"\\'")}')">
        <span class="chip-colors">${renderDots(card.colors)}</span>
        <span>${escHtml(card.name)}</span>
      </div>`;
    }).join('');
    return `
      <div class="cmc-pick-col">
        <div class="cmc-pick-header">${CMC_LABELS[cmc]} <span style="font-weight:400;opacity:0.7;">(${cards.length})</span></div>
        <div class="cmc-pick-list">${chipsHTML}</div>
      </div>`;
  }).join('');

  return { html: colsHTML, total };
}

// ── Main wizard renderer ──────────────────────────────────────────
function renderWizard() {
  const container = el('wizard-content');
  const s = wizardState;

  let stepsHTML = '';
  if (s.step !== 'howmany') {
    const steps = ['Count'];
    for (let i = 0; i < s.deckCount; i++) steps.push('Deck ' + (i+1));
    stepsHTML = '<div class="dl-progress">';
    steps.forEach((lbl, i) => {
      let cls = 'dl-progress-step';
      if      (i === 0 && s.step === 'howmany')                             cls += ' active';
      else if (i > 0   && (s.step === 'colors' || s.step === 'cards') && s.currentDeck === i-1) cls += ' active';
      else if (i === 0 && s.step !== 'howmany')                             cls += ' done';
      else if (i > 0   && s.currentDeck > i-1)                             cls += ' done';
      stepsHTML += '<span class="' + cls + '">' + lbl + '</span>';
    });
    stepsHTML += '</div>';
  }

  // ── Step: how many ──
  if (s.step === 'howmany') {
    container.innerHTML = stepsHTML + `
      <div class="dl-title">How many decks were drafted?</div>
      <div class="dl-input-row" style="flex-wrap:wrap;gap:0.5rem;">
        ${[4,6,8].map(n => '<button class="dl-btn" onclick="setDeckCount(' + n + ')">' + n + '</button>').join('')}
        <input type="number" id="deck-count-custom" min="1" max="30" placeholder="other…"
               style="width:90px;padding:0.45rem 0.5rem;border:1px solid #ccc;border-radius:6px;font-size:0.9rem;" />
        <button class="dl-btn" onclick="setDeckCountCustom()">OK</button>
      </div>`;
    return;
  }

  // ── Step: colors ──
  if (s.step === 'colors') {
    const deckNum    = s.currentDeck + 1;
    const existing   = session.decks[s.currentDeck] || {};
    const playerName = existing.player || '';
    const selColors  = s.currentColors;

    container.innerHTML = stepsHTML + `
      <div class="dl-title">Deck ${deckNum} of ${s.deckCount} — player &amp; colors</div>
      <label class="dl-label" style="display:block;margin-top:0.5rem;">Player name (optional)</label>
      <input class="player-name-input" type="text" id="player-name" placeholder="e.g. Alice" value="${escHtml(playerName)}" />
      <label class="dl-label" style="display:block;margin-top:1rem;">Colors in the deck</label>
      <div class="color-selector">
        ${COLOR_DEFS.map(c => `
          <div class="color-pip ${c.cls}${selColors.includes(c.code) ? ' selected' : ''}"
               onclick="toggleColor('${c.code}')" title="${c.label}"
               role="checkbox" aria-label="${c.label}" aria-checked="${selColors.includes(c.code)}">${c.glyph}</div>
        `).join('')}
      </div>
      <div class="dl-hint">Select the colors this deck primarily uses — used to pre-filter the card list.</div>
      <div class="step-actions">
        ${s.currentDeck > 0 ? '<button class="dl-btn" onclick="prevDeck()">← Back</button>' : ''}
        <span class="step-spacer"></span>
        <button class="dl-btn primary" onclick="proceedToCards()">Next — pick cards →</button>
      </div>`;
    return;
  }

  // ── Step: cards ──
  if (s.step === 'cards') {
    const deckNum  = s.currentDeck + 1;
    const deck     = session.decks[s.currentDeck];
    const selCards = deck.cards || [];

    const searchVal = (el('card-search') && el('card-search').value) || '';
    const { html: colsHTML, total: shownTotal } = renderPickGrid(deck, searchVal);

    container.innerHTML = stepsHTML + `
      <div class="dl-title">Deck ${deckNum} of ${s.deckCount} — select cards
        <span class="dl-info-badge">${deck.player || 'Deck ' + deckNum}</span>
        ${renderColorBadges(deck.colors)}
      </div>

      <div class="deck-preview" id="deck-preview-area">
        ${renderDeckPreview(deck)}
      </div>

      <div class="pick-area">
        <div class="filter-row">
          <input type="text" id="card-search" placeholder="Search cards…" value="${escHtml(searchVal)}"
                 oninput="updatePickGrid()" />
          <button class="dl-btn" onclick="clearCardSearch()">Clear</button>
        </div>
        <div class="selection-counter">
          <strong>${selCards.length}</strong> card${selCards.length !== 1 ? 's' : ''} selected
          &nbsp;·&nbsp; showing ${shownTotal} of ${cubeCards.length} cards
        </div>
        <div class="cmc-pick-grid" id="pick-grid-container">
          ${colsHTML || '<span style="font-size:0.82rem;color:#aaa;padding:0.5rem;">No cards match.</span>'}
        </div>
      </div>

      <div class="step-actions">
        <button class="dl-btn" onclick="backToColors()">← Colors</button>
        <span class="step-spacer"></span>
        <button class="dl-btn primary" onclick="finishDeck()">
          ${s.currentDeck < s.deckCount - 1 ? 'Save & next deck →' : 'Save & finish →'}
        </button>
      </div>`;
  }
}

// ── Incremental UI updates (no full re-render) ────────────────────

// Called when search input changes
window.updatePickGrid = function() {
  const s = wizardState;
  if (s.step !== 'cards') return;
  const deck     = session.decks[s.currentDeck];
  const selCards = deck.cards || [];
  const searchVal = (el('card-search') && el('card-search').value) || '';

  const { html: colsHTML, total: shownTotal } = renderPickGrid(deck, searchVal);

  const grid = el('pick-grid-container');
  if (grid) grid.innerHTML = colsHTML || '<span style="font-size:0.82rem;color:#aaa;padding:0.5rem;">No cards match.</span>';

  const counter = document.querySelector('.selection-counter');
  if (counter) counter.innerHTML =
    '<strong>' + selCards.length + '</strong> card' + (selCards.length !== 1 ? 's' : '') + ' selected' +
    '&nbsp;·&nbsp; showing ' + shownTotal + ' of ' + cubeCards.length + ' cards';
};

function updateDeckPreview() {
  const previewArea = el('deck-preview-area');
  if (!previewArea) return;
  const deck = session.decks[wizardState.currentDeck];
  if (!deck) return;
  previewArea.innerHTML = renderDeckPreview(deck);
}

window.clearCardSearch = function() {
  const inp = el('card-search');
  if (inp) { inp.value = ''; updatePickGrid(); }
};

// ── Card toggling ─────────────────────────────────────────────────
window.toggleCard = function(name) {
  const deck = session.decks[wizardState.currentDeck];
  if (!deck) return;
  const idx = deck.cards.indexOf(name);
  if (idx >= 0) deck.cards.splice(idx, 1);
  else deck.cards.push(name);
  // re-render pick grid (card disappears on pick, reappears on remove)
  updatePickGrid();
  updateDeckPreview();
};

window.removeCard = function(name, e) {
  e.stopPropagation();
  const deck = session.decks[wizardState.currentDeck];
  if (!deck) return;
  const idx = deck.cards.indexOf(name);
  if (idx >= 0) deck.cards.splice(idx, 1);
  updatePickGrid();
  updateDeckPreview();
};

// ── Wizard navigation ─────────────────────────────────────────────
window.setDeckCount = function(n) {
  wizardState.deckCount = n;
  wizardState.step = 'colors';
  wizardState.currentDeck = 0;
  wizardState.currentColors = [];
  session.decks = [];
  renderWizard();
};

window.setDeckCountCustom = function() {
  const v = parseInt(el('deck-count-custom').value);
  if (!v || v < 1) return;
  setDeckCount(v);
};

window.toggleColor = function(code) {
  const idx = wizardState.currentColors.indexOf(code);
  if (idx >= 0) wizardState.currentColors.splice(idx, 1);
  else wizardState.currentColors.push(code);
  renderWizard();
};

window.proceedToCards = function() {
  const i      = wizardState.currentDeck;
  const player = (el('player-name') && el('player-name').value.trim()) || '';
  const colors = [...wizardState.currentColors];
  if (!session.decks[i]) session.decks[i] = { player, colors, cards: [] };
  else { session.decks[i].player = player; session.decks[i].colors = colors; }
  wizardState.step = 'cards';
  renderWizard();
};

window.backToColors = function() {
  wizardState.step = 'colors';
  wizardState.currentColors = [...(session.decks[wizardState.currentDeck]?.colors || [])];
  renderWizard();
};

window.prevDeck = function() {
  if (wizardState.currentDeck > 0) {
    wizardState.currentDeck--;
    wizardState.currentColors = [...(session.decks[wizardState.currentDeck]?.colors || [])];
    wizardState.step = 'colors';
    renderWizard();
  }
};

window.finishDeck = function() {
  if (wizardState.currentDeck < wizardState.deckCount - 1) {
    wizardState.currentDeck++;
    wizardState.currentColors = [...(session.decks[wizardState.currentDeck]?.colors || [])];
    wizardState.step = 'colors';
    renderWizard();
  } else {
    finishSession();
  }
};

// ── Helpers ───────────────────────────────────────────────────────
function filterCardsByColors(cards, selectedColors) {
  if (!selectedColors || !selectedColors.length) return cards;
  return cards.filter(card => !card.colors.length || card.colors.some(c => selectedColors.includes(c)));
}

function renderDots(colors) {
  if (!colors || !colors.length) return '<span class="chip-dot c"></span>';
  return colors.map(c => '<span class="chip-dot ' + c.toLowerCase() + '"></span>').join('');
}

function renderColorBadges(colors) {
  if (!colors || !colors.length) return '';
  return '<span style="display:inline-flex;gap:3px;vertical-align:middle;margin-left:4px;">' +
    colors.map(c => '<span class="deck-badge badge-' + c.toLowerCase() + '">' + c + '</span>').join('') +
    '</span>';
}

function escHtml(s) {
  return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}

// ── Session finish & results ──────────────────────────────────────
function finishSession() {
  saveSession(session);
  show('results-section');
  renderResults();
  loadHistory();
  hide('wizard-section');
}

function renderResults() {
  const container = el('results-content');
  const s = session;
  const deckHTML = s.decks.map((deck, i) => {
    const label = deck.player || 'Deck ' + (i+1);
    const cardsSorted = [...deck.cards].sort();
    return `
      <div class="deck-summary-card">
        <div class="deck-header">
          <div>
            <div class="deck-name-display">${escHtml(label)} ${renderColorBadges(deck.colors)}</div>
            <div class="deck-card-count">${deck.cards.length} card${deck.cards.length !== 1 ? 's' : ''} selected</div>
          </div>
          <button class="dl-btn" style="font-size:0.78rem;" onclick="editDeck(${i})">Edit</button>
        </div>
        <div class="deck-card-list">${cardsSorted.map(c => escHtml(c)).join('<br/>')}</div>
      </div>`;
  }).join('');

  container.innerHTML = `
    <div class="dl-success-banner">✓ Draft session logged — <strong>${s.cubeName}</strong>, ${s.date}</div>
    ${deckHTML}
    <div style="display:flex;gap:0.5rem;flex-wrap:wrap;margin-top:1rem;">
      <button class="dl-btn" onclick="showExport()">Export JSON</button>
      <button class="dl-btn" onclick="copyMarkdown()">Copy as text</button>
      <button class="dl-btn primary" onclick="startNewSession()">Log another draft</button>
    </div>
    <div id="export-area"></div>`;
}

window.editDeck = function(i) {
  wizardState.currentDeck = i;
  wizardState.currentColors = [...(session.decks[i]?.colors || [])];
  wizardState.step = 'colors';
  show('wizard-section');
  hide('results-section');
  renderWizard();
};

window.showExport = function() {
  el('export-area').innerHTML = `
    <div class="section-divider"><hr/><span class="divider-label">JSON export</span><hr/></div>
    <pre class="session-output">${escHtml(JSON.stringify(session, null, 2))}</pre>`;
};

window.copyMarkdown = function() {
  const lines = ['## Draft — ' + session.cubeName + ' (' + session.date + ')\n'];
  session.decks.forEach((d, i) => {
    lines.push('### ' + (d.player || 'Deck ' + (i+1)) + ' (' + (d.colors.join('') || '—') + ')');
    [...d.cards].sort().forEach(c => lines.push('- ' + c));
    lines.push('');
  });
  navigator.clipboard.writeText(lines.join('\n')).then(() => {
    const btn = document.querySelector('[onclick="copyMarkdown()"]');
    if (btn) { const orig = btn.textContent; btn.textContent = '✓ Copied!'; setTimeout(() => btn.textContent = orig, 1800); }
  });
};

window.startNewSession = function() {
  session = { date: '', cubeId, cubeName, decks: [] };
  wizardState = { step: 'howmany', deckCount: 0, currentDeck: 0, currentColors: [] };
  hide('results-section');
  show('wizard-section');
  renderWizard();
};

// ── Local storage ─────────────────────────────────────────────────
function saveSession(sess) {
  try {
    const key = 'dl_sessions_v1';
    const existing = JSON.parse(localStorage.getItem(key) || '[]');
    existing.unshift({ ...sess, savedAt: new Date().toISOString() });
    if (existing.length > 50) existing.length = 50;
    localStorage.setItem(key, JSON.stringify(existing));
  } catch(e) {}
}

function loadHistory() {
  try {
    const key = 'dl_sessions_v1';
    const sessions = JSON.parse(localStorage.getItem(key) || '[]').filter(s => s.cubeId === cubeId);
    if (!sessions.length) return;
    show('history-section');
    const rows = sessions.map((s, i) => {
      const deckList = s.decks.map(d =>
        escHtml(d.player || '—') + ' <span style="font-size:0.75rem;color:#888;">(' + (d.colors.join('') || '—') + ', ' + d.cards.length + ' cards)</span>'
      ).join('<br/>');
      return '<tr>' +
        '<td>' + escHtml(s.date || (s.savedAt||'').slice(0,10)) + '</td>' +
        '<td>' + deckList + '</td>' +
        '<td style="text-align:right;">' +
          '<button class="dl-btn" style="font-size:0.75rem;" onclick="restoreSession(' + i + ')">View</button> ' +
          '<button class="dl-btn danger" style="font-size:0.75rem;" onclick="deleteSession(' + i + ')">Delete</button>' +
        '</td></tr>';
    }).join('');
    el('history-content').innerHTML = '<table class="history-table"><thead><tr><th>Date</th><th>Decks</th><th></th></tr></thead><tbody>' + rows + '</tbody></table>';
  } catch(e) {}
}

window.restoreSession = function(i) {
  try {
    const key = 'dl_sessions_v1';
    const sessions = JSON.parse(localStorage.getItem(key) || '[]').filter(s => s.cubeId === cubeId);
    session = sessions[i];
    show('results-section');
    renderResults();
    window.scrollTo({ top: el('results-section').offsetTop - 60, behavior: 'smooth' });
  } catch(e) {}
};

window.deleteSession = function(i) {
  try {
    const key = 'dl_sessions_v1';
    const all = JSON.parse(localStorage.getItem(key) || '[]');
    const cubeFiltered = all.filter(s => s.cubeId === cubeId);
    const toDelete = cubeFiltered[i];
    localStorage.setItem(key, JSON.stringify(all.filter(s => s !== toDelete)));
    loadHistory();
  } catch(e) {}
};

window.loadCube = loadCube;

})();
</script>