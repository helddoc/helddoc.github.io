---
layout: single
title: "Draft Log"
permalink: /cubelogger/
---

<h2 class="sr-only">Cube draft logging wizard — guided deck entry from a CubeCobra cube</h2>

<style>
  /*
   * All surface/text/border tokens mirror main.scss exactly.
   * If you change a value in main.scss, update the matching token here.
   *
   * $background-color : #1b1b1e  → --dl-bg
   * $border-color     : #2e2e33  → --dl-border
   * $text-color       : #afb0b1  → --dl-text
   * $muted-text-color : #6e6e73  → --dl-muted
   * $link-color       : #6ea8d8  → --dl-accent
   * .notice bg        : #232327  → --dl-surface   (one step up from bg)
   * :not(pre)>code bg : #2a2a2f  → --dl-surface2  (two steps up)
   */
  #draft-log-app {
    --dl-bg:      #1b1b1e;
    --dl-surface: #232327;
    --dl-surface2:#2a2a2f;
    --dl-border:  #2e2e33;
    --dl-border2: #3d3d44;
    --dl-text:    #afb0b1;
    --dl-muted:   #6e6e73;
    --dl-strong:  #d4d5d6;
    --dl-accent:  #6ea8d8;
    --dl-accent2: #3d6a92;
    --dl-ok-bg:   #182618; --dl-ok-bd: #2d5230; --dl-ok-tx: #6fcf97;
    --dl-err-bg:  #2a1818; --dl-err-bd: #622020; --dl-err-tx: #f28b82;
    /* CubeCobra dark card-color tokens (bare RGB, use as rgb(var(--card-*))) */
    --card-white:     95 95 75;
    --card-blue:      52 77 95;
    --card-black:     69 63 82;
    --card-red:       95 56 68;
    --card-green:     73 95 52;
    --card-multi:     92 88 9;
    --card-colorless: 60 58 64;
    --cc-text:        240 240 240;
    font-family: var(--global-font-family, "Inter", -apple-system, sans-serif);
    max-width: 900px;
    margin: 0 auto;
  }
  #draft-log-app * { box-sizing: border-box; }

  /* ── Sections ─────────────────────────────────────────────── */
  .dl-section       { border: 1px solid var(--dl-border); border-radius: 8px; padding: 1.5rem; margin-bottom: 1.5rem; background: var(--dl-surface); }
  .dl-collapsed     { display: none; }
  .dl-label         { font-size: 0.78rem; font-weight: 600; letter-spacing: 0.06em; text-transform: uppercase; color: var(--dl-muted); margin-bottom: 0.5rem; display: block; }
  .dl-title         { font-size: 1.1rem; font-weight: 600; margin: 0 0 1rem; color: var(--dl-strong); }
  .dl-hint          { font-size: 0.82rem; color: var(--dl-muted); margin: 0.4rem 0 0; }
  .dl-error         { background: var(--dl-err-bg); border: 1px solid var(--dl-err-bd); border-radius: 6px; padding: 0.6rem 0.9rem; font-size: 0.85rem; color: var(--dl-err-tx); margin: 0.5rem 0; }
  .dl-success-banner{ background: var(--dl-ok-bg);  border: 1px solid var(--dl-ok-bd);  border-radius: 8px; padding: 1rem 1.25rem; color: var(--dl-ok-tx); margin-bottom: 1rem; }
  .dl-info-badge    { display: inline-block; padding: 0.2rem 0.55rem; background: #1a2d3d; color: var(--dl-accent); border-radius: 12px; font-size: 0.78rem; font-weight: 600; margin-left: 0.5rem; }

  /* ── Inputs & buttons ─────────────────────────────────────── */
  .dl-input-row     { display: flex; gap: 0.5rem; align-items: center; flex-wrap: wrap; }
  .dl-input-row input[type="text"],
  .filter-row input[type="text"],
  .player-name-input,
  input[type="number"] {
    padding: 0.4rem 0.7rem; border: 1px solid var(--dl-border2); border-radius: 6px;
    background: var(--dl-bg); color: var(--dl-text); font-size: 0.9rem;
  }
  .dl-input-row input[type="text"] { flex: 1; min-width: 180px; font-size: 0.95rem; }
  .player-name-input { width: 100%; margin-top: 0.3rem; }

  .dl-btn { padding: 0.45rem 1rem; border: 1px solid var(--dl-border2); border-radius: 6px; background: var(--dl-surface2); color: var(--dl-text); cursor: pointer; font-size: 0.9rem; transition: background 0.12s; white-space: nowrap; }
  .dl-btn:hover  { background: #353540; }
  .dl-btn:active { transform: scale(0.98); }
  .dl-btn.primary { background: var(--dl-accent2); color: #fff; border-color: var(--dl-accent2); }
  .dl-btn.primary:hover { background: var(--dl-accent); border-color: var(--dl-accent); }
  .dl-btn.danger  { background: #4a1a1a; color: #f28b82; border-color: #622020; }
  .dl-btn.danger:hover { background: #5c2020; }

  /* ── Progress bar ─────────────────────────────────────────── */
  .dl-progress      { display: flex; gap: 0.4rem; margin-bottom: 1.25rem; flex-wrap: wrap; }
  .dl-progress-step { padding: 0.25rem 0.6rem; border-radius: 4px; font-size: 0.78rem; font-weight: 600; background: var(--dl-surface2); color: var(--dl-muted); border: 1px solid var(--dl-border); }
  .dl-progress-step.active { background: var(--dl-accent2); color: #fff; border-color: var(--dl-accent2); }
  .dl-progress-step.done   { background: var(--dl-ok-bg);   color: var(--dl-ok-tx); border-color: var(--dl-ok-bd); }

  /* ── Color pips (selection circles) ──────────────────────── */
  .color-selector { display: flex; gap: 0.5rem; flex-wrap: wrap; margin: 0.75rem 0; }
  .color-pip { width: 40px; height: 40px; border-radius: 50%; border: 2px solid var(--dl-border2); cursor: pointer; font-size: 1rem; font-weight: 700; display: flex; align-items: center; justify-content: center; transition: transform 0.1s; user-select: none; color: rgb(var(--cc-text)); }
  .color-pip:hover    { transform: scale(1.1); }
  .color-pip.selected { border-width: 3px; }
  .color-pip.pip-w { background: rgb(var(--card-white));     border-color: rgb(115 115 90);  }
  .color-pip.pip-u { background: rgb(var(--card-blue));      border-color: rgb(72 107 135);  }
  .color-pip.pip-b { background: rgb(var(--card-black));     border-color: rgb(99 93 122);   }
  .color-pip.pip-r { background: rgb(var(--card-red));       border-color: rgb(135 86 98);   }
  .color-pip.pip-g { background: rgb(var(--card-green));     border-color: rgb(103 135 82);  }
  .color-pip.pip-w.selected { box-shadow: 0 0 0 3px rgb(var(--card-white)); }
  .color-pip.pip-u.selected { box-shadow: 0 0 0 3px rgb(var(--card-blue)); }
  .color-pip.pip-b.selected { box-shadow: 0 0 0 3px rgb(var(--card-black)); }
  .color-pip.pip-r.selected { box-shadow: 0 0 0 3px rgb(var(--card-red)); }
  .color-pip.pip-g.selected { box-shadow: 0 0 0 3px rgb(var(--card-green)); }

  /* ── Color identity badges (small circles in titles) ─────── */
  .deck-badge { width: 22px; height: 22px; border-radius: 50%; font-size: 0.7rem; font-weight: 700; display: inline-flex; align-items: center; justify-content: center; border: 1px solid var(--dl-border2); color: rgb(var(--cc-text)); }
  .badge-w { background: rgb(var(--card-white));     border-color: rgb(115 115 90);  }
  .badge-u { background: rgb(var(--card-blue));      border-color: rgb(72 107 135);  }
  .badge-b { background: rgb(var(--card-black));     border-color: rgb(99 93 122);   }
  .badge-r { background: rgb(var(--card-red));       border-color: rgb(135 86 98);   }
  .badge-g { background: rgb(var(--card-green));     border-color: rgb(103 135 82);  }

  /* ── Deck image preview ───────────────────────────────────── */
  .deck-preview       { border: 1px solid var(--dl-border); border-radius: 8px; background: var(--dl-bg); padding: 0.75rem 0.75rem 0.5rem; margin-bottom: 1rem; }
  .deck-preview-title { font-weight: 600; font-size: 0.85rem; margin-bottom: 0.6rem; color: var(--dl-muted); }

  .img-curve     { display: flex; gap: 0.75rem; overflow-x: auto; padding-bottom: 0.5rem; }
  .img-curve-col { flex: 0 0 auto; min-width: 120px; }
  .img-curve-header { text-align: center; font-size: 0.72rem; font-weight: 700; color: var(--dl-muted); letter-spacing: 0.05em; text-transform: uppercase; margin-bottom: 0.4rem; padding: 0.15rem 0.3rem; background: var(--dl-surface2); border-radius: 4px; }
  .img-curve-stack { display: flex; flex-direction: column; gap: 3px; }

  .img-card-wrap { position: relative; width: 120px; height: 36px; cursor: pointer; transition: height 0.15s ease; }
  .img-curve-stack .img-card-wrap:last-child { height: 60px; }
  .img-curve-stack:hover .img-card-wrap { height: 40px; }
  .img-card-wrap:hover { height: 167px !important; z-index: 20; }
  .img-card-wrap img { position: absolute; top: 0; left: 0; width: 120px; border-radius: 6px; display: block; box-shadow: 0 1px 6px rgba(0,0,0,0.7); transition: box-shadow 0.12s; }
  .img-card-wrap:hover img { box-shadow: 0 4px 18px rgba(0,0,0,0.9); z-index: 10; }
  .img-card-remove { position: absolute; top: 3px; right: 3px; width: 18px; height: 18px; background: rgba(0,0,0,0.75); color: #fff; border-radius: 50%; font-size: 11px; line-height: 18px; text-align: center; cursor: pointer; z-index: 30; display: none; border: none; padding: 0; }
  .img-card-wrap:hover .img-card-remove { display: block; }

  /* ── CMC pick columns ─────────────────────────────────────── */
  .pick-area  { margin: 0.5rem 0; }
  .filter-row { display: flex; gap: 0.4rem; align-items: center; flex-wrap: wrap; margin-bottom: 0.5rem; }
  .filter-row input[type="text"] { flex: 1; min-width: 120px; }

  .selection-counter        { font-size: 0.82rem; color: var(--dl-muted); margin: 0.25rem 0 0.5rem; }
  .selection-counter strong { color: var(--dl-accent); }

  .cmc-pick-grid   { display: flex; gap: 0.5rem; overflow-x: auto; padding-bottom: 0.4rem; }
  .cmc-pick-col    { flex: 0 0 auto; min-width: 130px; max-width: 160px; }
  .cmc-pick-header { text-align: center; font-size: 0.72rem; font-weight: 700; color: var(--dl-muted); letter-spacing: 0.05em; text-transform: uppercase; padding: 0.2rem 0.4rem; background: var(--dl-surface2); border-radius: 4px; margin-bottom: 0.35rem; }
  .cmc-pick-list   { display: flex; flex-direction: column; gap: 0.3rem; }

  /* Card chips — base styles; color overridden by inline JS chipStyle() */
  .card-chip { padding: 0.3rem 0.5rem; border-radius: 5px; border: 1px solid var(--dl-border2); font-size: 0.79rem; cursor: pointer; transition: filter 0.1s; line-height: 1.3; display: flex; align-items: center; gap: 5px; user-select: none; }
  .card-chip:hover { filter: brightness(1.2); }

  .chip-colors { display: flex; gap: 2px; flex-shrink: 0; }
  .chip-dot    { width: 7px; height: 7px; border-radius: 50%; flex-shrink: 0; }
  .chip-dot.w { background: rgb(115 115 95); }
  .chip-dot.u { background: rgb(72 107 135); }
  .chip-dot.b { background: rgb(94 88 112);  }
  .chip-dot.r { background: rgb(135 86 98);  }
  .chip-dot.g { background: rgb(103 135 72); }
  .chip-dot.c { background: rgb(85 83 90);   }

  /* ── Lands section ────────────────────────────────────────── */
  .lands-section { margin-top: 1rem; }
  .lands-header {
    font-size: 0.72rem; font-weight: 700; letter-spacing: 0.05em; text-transform: uppercase;
    color: var(--dl-muted); padding: 0.2rem 0.4rem; background: var(--dl-surface2);
    border-radius: 4px; margin-bottom: 0.4rem; display: inline-block;
  }
  .lands-header span { font-weight: 400; opacity: 0.6; }
  .lands-chip-list { display: flex; flex-wrap: wrap; gap: 0.3rem; }

  /* ── Results & history ────────────────────────────────────── */
  .deck-summary-card { border: 1px solid var(--dl-border); border-radius: 8px; padding: 1rem; margin-bottom: 0.75rem; background: var(--dl-bg); }
  .deck-summary-card .deck-header { display: flex; justify-content: space-between; align-items: flex-start; flex-wrap: wrap; gap: 0.5rem; }
  .deck-name-display { font-weight: 600; font-size: 1rem; color: var(--dl-strong); }
  .deck-card-count   { font-size: 0.82rem; color: var(--dl-muted); margin-top: 0.3rem; }
  .deck-card-list    { margin-top: 0.5rem; font-size: 0.8rem; color: var(--dl-text); line-height: 1.6; column-count: 2; column-gap: 1rem; }

  .step-actions  { display: flex; gap: 0.5rem; flex-wrap: wrap; margin-top: 1rem; align-items: center; }
  .step-spacer   { flex: 1; }

  .section-divider          { display: flex; align-items: center; gap: 0.75rem; margin: 1.25rem 0 1rem; }
  .section-divider hr       { flex: 1; border: none; border-top: 1px solid var(--dl-border); margin: 0; }
  .section-divider .divider-label { font-size: 0.78rem; font-weight: 600; letter-spacing: 0.06em; text-transform: uppercase; color: var(--dl-muted); white-space: nowrap; }

  .session-output { background: var(--dl-bg); border: 1px solid var(--dl-border); border-radius: 6px; padding: 1rem; font-family: monospace; font-size: 0.8rem; white-space: pre-wrap; max-height: 400px; overflow-y: auto; color: var(--dl-text); }

  .history-table    { width: 100%; border-collapse: collapse; font-size: 0.85rem; }
  .history-table th { text-align: left; font-size: 0.75rem; font-weight: 600; text-transform: uppercase; letter-spacing: 0.04em; color: var(--dl-muted); padding: 0.4rem 0.5rem; border-bottom: 1px solid var(--dl-border); }
  .history-table td { padding: 0.5rem; border-bottom: 1px solid var(--dl-border); vertical-align: top; color: var(--dl-text); }
  .history-table tr:last-child td { border-bottom: none; }

  @media (max-width: 600px) {
    .deck-card-list  { column-count: 1; }
    .cmc-pick-col    { min-width: 110px; }
    .img-curve-col   { min-width: 90px; }
    .img-card-wrap, .img-card-wrap img { width: 88px; }
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

function scryfallImg(name) {
  return 'https://api.scryfall.com/cards/named?exact=' + encodeURIComponent(name) + '&format=image&version=small';
}

let cubeCards   = [];
let cubeName    = '';
let cubeId      = '';
let session     = { date: '', cubeId: '', cubeName: '', decks: [] };
let wizardState = { step: 'howmany', deckCount: 0, currentDeck: 0, currentColors: [] };

const el   = id => document.getElementById(id);
const show = id => el(id).classList.remove('dl-collapsed');
const hide = id => el(id).classList.add('dl-collapsed');

// ── Load cube ─────────────────────────────────────────────────────
async function loadCube() {
  cubeId = el('cube-id-input').value.trim();
  if (!cubeId) { showStatus('Please enter a CubeCobra ID.', 'error'); return; }
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
  const url  = 'https://cubecobra.com/cube/api/cubeJSON/' + encodeURIComponent(id);
  const resp = await fetch(url);
  if (!resp.ok) throw new Error('HTTP ' + resp.status);
  return resp.json();
}

function normalizeCards(data) {
  let cards = [];
  if (data.cards) {
    if      (Array.isArray(data.cards.mainboard)) cards = data.cards.mainboard;
    else if (Array.isArray(data.cards))           cards = data.cards;
  }
  return cards.map(card => {
    const d = card.details || card.card_details || card;
    let colors = d.colors || card.colors || [];
    if (typeof colors === 'string') colors = colors.split('');
    colors = (colors || []).map(c => String(c).toUpperCase()).filter(c => 'WUBRG'.includes(c));
    // colorIdentity is used for lands to show which mana they produce
    let colorIdentity = d.color_identity || card.color_identity || colors;
    if (typeof colorIdentity === 'string') colorIdentity = colorIdentity.split('');
    colorIdentity = (colorIdentity || []).map(c => String(c).toUpperCase()).filter(c => 'WUBRG'.includes(c));
    return {
      name:          d.name     || card.name || 'Unknown',
      colors,
      colorIdentity,
      typeLine:      d.type_line || d.type  || '',
      cmc:           d.cmc   != null ? d.cmc   : (card.cmc != null ? card.cmc : null),
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

// ── Sorting ───────────────────────────────────────────────────────
// Color order: W U B R G → multicolor → colorless
const COLOR_ORDER = { W: 0, U: 1, B: 2, R: 3, G: 4 };
function colorSortKey(card) {
  const c = card.colors || [];
  if (c.length === 0) return 6;
  if (c.length  > 1) return 5;
  return COLOR_ORDER[c[0]] ?? 4;
}

// Type order: Creature → Planeswalker → Battle → Instant → Sorcery → Artifact → Enchantment → Land → other
const TYPE_ORDER = ['Creature','Planeswalker','Battle','Instant','Sorcery','Artifact','Enchantment','Land'];
function typeSortKey(card) {
  const t   = card.typeLine || '';
  const idx = TYPE_ORDER.findIndex(type => t.includes(type));
  return idx === -1 ? TYPE_ORDER.length : idx;
}

// Primary: color  Secondary: type  Tertiary: name
function sortCards(arr) {
  return [...arr].sort((a, b) => {
    const ck = colorSortKey(a) - colorSortKey(b); if (ck !== 0) return ck;
    const tk = typeSortKey(a)  - typeSortKey(b);  if (tk !== 0) return tk;
    return a.name.localeCompare(b.name);
  });
}

// ── CMC bucketing (spells only — lands excluded) ──────────────────
function isLand(card) { return (card.typeLine || '').includes('Land'); }

function cmcBuckets(cardList) {
  const b = {0:[],1:[],2:[],3:[],4:[],5:[],6:[]};
  cardList.forEach(card => {
    if (isLand(card)) return;           // lands handled separately
    const raw = typeof card.cmc === 'number' && !isNaN(card.cmc) ? card.cmc : 6;
    b[Math.min(Math.floor(raw), 6)].push(card);
  });
  Object.keys(b).forEach(k => { b[k] = sortCards(b[k]); });
  return b;
}

// Land type order: Basic → fetch/shock/dual → other, then alpha within group
const LAND_TYPE_ORDER = ['Basic','Snow','Gate'];
function landTypeSortKey(card) {
  const t = card.typeLine || '';
  if (t.includes('Basic')) return 0;
  // fetches/shocks/duals tend to have 2+ colors in identity
  const ci = card.colorIdentity || card.colors || [];
  if (ci.length >= 2) return 1;
  if (ci.length === 1) return 2;
  return 3; // colorless utility lands
}

function sortLands(arr) {
  return [...arr].sort((a, b) => {
    const lk = landTypeSortKey(a) - landTypeSortKey(b); if (lk !== 0) return lk;
    // within same land type, sort by color identity breadth desc, then alpha
    const ca = (a.colorIdentity || a.colors || []).length;
    const cb = (b.colorIdentity || b.colors || []).length;
    if (ca !== cb) return cb - ca;
    return a.name.localeCompare(b.name);
  });
}

function getLands(cardList) {
  return sortLands(cardList.filter(isLand));
}

// ── Chip color styles — CubeCobra card color tokens ──────────────
// Variables declared on #draft-log-app; values are bare RGB triplets.
// Border is each bg lightened ~20 (for a subtle lifted edge).
const CHIP_MONO = {
  W: 'background:rgb(95 95 75);color:rgb(240 240 240);border-color:rgb(120 120 98);',
  U: 'background:rgb(52 77 95);color:rgb(240 240 240);border-color:rgb(72 107 130);',
  B: 'background:rgb(69 63 82);color:rgb(240 240 240);border-color:rgb(94 88 112);',
  R: 'background:rgb(95 56 68);color:rgb(240 240 240);border-color:rgb(130 76 93);',
  G: 'background:rgb(73 95 52);color:rgb(240 240 240);border-color:rgb(98 130 72);',
};
const CHIP_MULTI     = 'background:rgb(92 88 9);color:rgb(240 240 240);border-color:rgb(122 117 30);';
const CHIP_COLORLESS = 'background:rgb(60 58 64);color:rgb(240 240 240);border-color:rgb(85 83 90);';

function chipStyle(colors) {
  if (!colors || colors.length === 0) return CHIP_COLORLESS;
  if (colors.length > 1)             return CHIP_MULTI;
  return CHIP_MONO[colors[0]] || CHIP_COLORLESS;
}

// Land chips use --card-lands background; dots show mana production via colorIdentity
const CHIP_LAND = 'background:rgb(95 58 36);color:rgb(240 240 240);border-color:rgb(125 83 56);';

function renderLandDots(card) {
  // prefer colorIdentity (mana produced) over colors for lands
  const ci = (card.colorIdentity && card.colorIdentity.length) ? card.colorIdentity : card.colors;
  if (!ci || !ci.length) return '<span class="chip-dot c"></span>';
  return ci.map(c => '<span class="chip-dot ' + c.toLowerCase() + '"></span>').join('');
}

// ── Deck preview (stacked card images) ───────────────────────────
function renderDeckPreview(deck) {
  const selCards = deck.cards || [];
  const cardObjs = cubeCards.filter(c => selCards.includes(c.name));
  const buckets  = cmcBuckets(cardObjs);
  const lands    = getLands(cardObjs);
  const LABELS   = ['0','1','2','3','4','5','6+'];

  const colsHTML = [0,1,2,3,4,5,6].map(cmc => {
    const cards = buckets[cmc];
    if (!cards.length) return '';
    const stackHTML = cards.map(card => `
      <div class="img-card-wrap" title="${escHtml(card.name)}">
        <img src="${scryfallImg(card.name)}" alt="${escHtml(card.name)}" loading="lazy"
             onerror="this.style.display='none';this.nextElementSibling.style.display='block';" />
        <span style="display:none;font-size:0.7rem;padding:2px 4px;background:var(--dl-surface2);color:var(--dl-text);border-radius:3px;line-height:1.3;">${escHtml(card.name)}</span>
        <button class="img-card-remove" onclick="removeCard('${escHtml(card.name).replace(/'/g,"\\'")}',event)" title="Remove">✕</button>
      </div>`).join('');
    return `
      <div class="img-curve-col">
        <div class="img-curve-header">${LABELS[cmc]} <span style="font-weight:400;opacity:0.6;">(${cards.length})</span></div>
        <div class="img-curve-stack">${stackHTML}</div>
      </div>`;
  }).join('');

  const landsHTML = lands.length ? `
    <div class="lands-section">
      <div class="lands-header">Lands <span>(${lands.length})</span></div>
      <div style="display:flex;flex-wrap:wrap;gap:3px;">
        ${lands.map(card => `
          <div class="img-card-wrap" title="${escHtml(card.name)}" style="width:88px;height:30px;">
            <img src="${scryfallImg(card.name)}" alt="${escHtml(card.name)}" loading="lazy"
                 onerror="this.style.display='none';this.nextElementSibling.style.display='block';" style="width:88px;" />
            <span style="display:none;font-size:0.65rem;padding:2px 3px;background:var(--dl-surface2);color:var(--dl-text);border-radius:3px;">${escHtml(card.name)}</span>
            <button class="img-card-remove" onclick="removeCard('${escHtml(card.name).replace(/'/g,"\\'")}',event)" title="Remove">✕</button>
          </div>`).join('')}
      </div>
    </div>` : '';

  return `
    <div class="deck-preview-title">Current deck — <strong style="color:var(--dl-strong)">${selCards.length}</strong> card${selCards.length !== 1 ? 's' : ''}</div>
    <div class="img-curve">${colsHTML || '<span style="font-size:0.82rem;color:var(--dl-muted)">No cards selected yet</span>'}</div>
    ${landsHTML}`;
}

// ── CMC pick grid ─────────────────────────────────────────────────
function renderPickGrid(deck, searchVal) {
  const selCards = deck.cards || [];
  const filtered = filterCardsByColors(cubeCards, deck.colors)
    .filter(c => !selCards.includes(c.name));
  const shown = searchVal
    ? filtered.filter(c => c.name.toLowerCase().includes(searchVal.toLowerCase()))
    : filtered;

  const buckets    = cmcBuckets(shown);
  const shownLands = getLands(shown);
  const LABELS     = ['0','1','2','3','4','5','6+'];

  const colsHTML = [0,1,2,3,4,5,6].map(cmc => {
    const cards = buckets[cmc];
    if (!cards.length) return '';
    const chipsHTML = cards.map(card => `
      <div class="card-chip" style="${chipStyle(card.colors)}"
           onclick="toggleCard('${escHtml(card.name).replace(/'/g,"\\'")}')">
        <span class="chip-colors">${renderDots(card.colors)}</span>
        <span>${escHtml(card.name)}</span>
      </div>`).join('');
    return `
      <div class="cmc-pick-col">
        <div class="cmc-pick-header">${LABELS[cmc]} <span style="font-weight:400;opacity:0.6;">(${cards.length})</span></div>
        <div class="cmc-pick-list">${chipsHTML}</div>
      </div>`;
  }).join('');

  const landsHTML = shownLands.length ? `
    <div class="lands-section" style="width:100%;">
      <div class="lands-header">Lands <span>(${shownLands.length})</span></div>
      <div class="lands-chip-list">
        ${shownLands.map(card => `
          <div class="card-chip" style="${CHIP_LAND}"
               onclick="toggleCard('${escHtml(card.name).replace(/'/g,"\\'")}')">
            <span class="chip-colors">${renderLandDots(card)}</span>
            <span>${escHtml(card.name)}</span>
          </div>`).join('')}
      </div>
    </div>` : '';

  const spellCount  = Object.values(buckets).reduce((s, arr) => s + arr.length, 0);
  const totalShown  = spellCount + shownLands.length;

  const gridHTML = (colsHTML || landsHTML)
    ? `<div class="cmc-pick-grid">${colsHTML}</div>${landsHTML}`
    : '<span style="font-size:0.82rem;color:var(--dl-muted);padding:0.5rem;">No cards match.</span>';

  return { html: gridHTML, total: totalShown };
}

// ── Wizard renderer ───────────────────────────────────────────────
function renderWizard() {
  const container = el('wizard-content');
  const s = wizardState;

  // Progress breadcrumb
  let stepsHTML = '';
  if (s.step !== 'howmany') {
    const steps = ['Count'];
    for (let i = 0; i < s.deckCount; i++) steps.push('Deck ' + (i+1));
    stepsHTML = '<div class="dl-progress">';
    steps.forEach((lbl, i) => {
      let cls = 'dl-progress-step';
      if (i === 0 && s.step === 'howmany')  cls += ' active';
      else if (i > 0 && (s.step === 'colors' || s.step === 'cards') && s.currentDeck === i-1) cls += ' active';
      else if (i === 0 && s.step !== 'howmany') cls += ' done';
      else if (i > 0 && s.currentDeck > i-1)   cls += ' done';
      stepsHTML += '<span class="' + cls + '">' + lbl + '</span>';
    });
    stepsHTML += '</div>';
  }

  // Step: how many decks
  if (s.step === 'howmany') {
    container.innerHTML = stepsHTML + `
      <div class="dl-title">How many decks were drafted?</div>
      <div class="dl-input-row" style="flex-wrap:wrap;gap:0.5rem;">
        ${[4,6,8].map(n => '<button class="dl-btn" onclick="setDeckCount('+n+')">'+n+'</button>').join('')}
        <input type="number" id="deck-count-custom" min="1" max="30" placeholder="other…"
               style="width:90px;" />
        <button class="dl-btn" onclick="setDeckCountCustom()">OK</button>
      </div>`;
    return;
  }

  // Step: player name + colors
  if (s.step === 'colors') {
    const deckNum = s.currentDeck + 1;
    const player  = (session.decks[s.currentDeck] || {}).player || '';
    container.innerHTML = stepsHTML + `
      <div class="dl-title">Deck ${deckNum} of ${s.deckCount} — player &amp; colors</div>
      <label class="dl-label" style="margin-top:0.5rem;">Player name (optional)</label>
      <input class="player-name-input" type="text" id="player-name" placeholder="e.g. Alice" value="${escHtml(player)}" />
      <label class="dl-label" style="margin-top:1rem;">Colors in the deck</label>
      <div class="color-selector">
        ${COLOR_DEFS.map(c => `
          <div class="color-pip ${c.cls}${s.currentColors.includes(c.code)?' selected':''}"
               onclick="toggleColor('${c.code}')" title="${c.label}"
               role="checkbox" aria-label="${c.label}" aria-checked="${s.currentColors.includes(c.code)}">${c.glyph}</div>
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

  // Step: pick cards
  if (s.step === 'cards') {
    const deckNum  = s.currentDeck + 1;
    const deck     = session.decks[s.currentDeck];
    const searchVal = (el('card-search') && el('card-search').value) || '';
    const { html: gridHTML, total } = renderPickGrid(deck, searchVal);

    container.innerHTML = stepsHTML + `
      <div class="dl-title">Deck ${deckNum} of ${s.deckCount} — select cards
        <span class="dl-info-badge">${escHtml(deck.player || 'Deck ' + deckNum)}</span>
        ${renderColorBadges(deck.colors)}
      </div>

      <div class="deck-preview" id="deck-preview-area">${renderDeckPreview(deck)}</div>

      <div class="pick-area">
        <div class="filter-row">
          <input type="text" id="card-search" placeholder="Search cards…" value="${escHtml(searchVal)}" oninput="updatePickGrid()" />
          <button class="dl-btn" onclick="clearCardSearch()">Clear</button>
        </div>
        <div class="selection-counter">
          <strong>${deck.cards.length}</strong> card${deck.cards.length !== 1 ? 's' : ''} selected
          &nbsp;·&nbsp; showing ${total} of ${cubeCards.length} cards
        </div>
        <div id="pick-grid-container">${gridHTML}</div>
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

// ── Incremental updates ───────────────────────────────────────────
window.updatePickGrid = function() {
  if (wizardState.step !== 'cards') return;
  const deck      = session.decks[wizardState.currentDeck];
  const searchVal = (el('card-search') && el('card-search').value) || '';
  const { html, total } = renderPickGrid(deck, searchVal);
  const grid = el('pick-grid-container');
  if (grid) grid.innerHTML = html || '<span style="font-size:0.82rem;color:var(--dl-muted);padding:0.5rem;">No cards match.</span>';
  const ctr = document.querySelector('.selection-counter');
  if (ctr) ctr.innerHTML =
    '<strong>' + deck.cards.length + '</strong> card' + (deck.cards.length !== 1 ? 's' : '') + ' selected' +
    '&nbsp;·&nbsp; showing ' + total + ' of ' + cubeCards.length + ' cards';
};

function updateDeckPreview() {
  const area = el('deck-preview-area');
  if (!area) return;
  const deck = session.decks[wizardState.currentDeck];
  if (deck) area.innerHTML = renderDeckPreview(deck);
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
  if (idx >= 0) deck.cards.splice(idx, 1); else deck.cards.push(name);
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

// ── Navigation ────────────────────────────────────────────────────
window.setDeckCount = function(n) {
  wizardState.deckCount = n; wizardState.step = 'colors';
  wizardState.currentDeck = 0; wizardState.currentColors = [];
  session.decks = [];
  renderWizard();
};

window.setDeckCountCustom = function() {
  const v = parseInt(el('deck-count-custom').value);
  if (v && v >= 1) setDeckCount(v);
};

window.toggleColor = function(code) {
  const idx = wizardState.currentColors.indexOf(code);
  if (idx >= 0) wizardState.currentColors.splice(idx, 1); else wizardState.currentColors.push(code);
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
function filterCardsByColors(cards, sel) {
  if (!sel || !sel.length) return cards;
  return cards.filter(c => !c.colors.length || c.colors.some(x => sel.includes(x)));
}

function renderDots(colors) {
  if (!colors || !colors.length) return '<span class="chip-dot c"></span>';
  return colors.map(c => '<span class="chip-dot ' + c.toLowerCase() + '"></span>').join('');
}

function renderColorBadges(colors) {
  if (!colors || !colors.length) return '';
  return '<span style="display:inline-flex;gap:3px;vertical-align:middle;margin-left:4px;">' +
    colors.map(c => '<span class="deck-badge badge-' + c.toLowerCase() + '">' + c + '</span>').join('') + '</span>';
}

function escHtml(s) {
  return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}

// ── Finish & results ──────────────────────────────────────────────
function finishSession() {
  saveSession(session);
  show('results-section'); renderResults();
  loadHistory();
  hide('wizard-section');
}

function renderResults() {
  const s = session;
  const deckHTML = s.decks.map((deck, i) => {
    const label = deck.player || 'Deck ' + (i+1);
    return `
      <div class="deck-summary-card">
        <div class="deck-header">
          <div>
            <div class="deck-name-display">${escHtml(label)} ${renderColorBadges(deck.colors)}</div>
            <div class="deck-card-count">${deck.cards.length} card${deck.cards.length !== 1 ? 's' : ''} selected</div>
          </div>
          <button class="dl-btn" style="font-size:0.78rem;" onclick="editDeck(${i})">Edit</button>
        </div>
        <div class="deck-card-list">${[...deck.cards].sort().map(c => escHtml(c)).join('<br/>')}</div>
      </div>`;
  }).join('');

  el('results-content').innerHTML = `
    <div class="dl-success-banner">✓ Draft session logged — <strong>${escHtml(s.cubeName)}</strong>, ${escHtml(s.date)}</div>
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
  show('wizard-section'); hide('results-section');
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
    lines.push('### ' + (d.player || 'Deck '+(i+1)) + ' (' + (d.colors.join('') || '—') + ')');
    [...d.cards].sort().forEach(c => lines.push('- ' + c));
    lines.push('');
  });
  navigator.clipboard.writeText(lines.join('\n')).then(() => {
    const btn = document.querySelector('[onclick="copyMarkdown()"]');
    if (btn) { const o = btn.textContent; btn.textContent = '✓ Copied!'; setTimeout(() => btn.textContent = o, 1800); }
  });
};

window.startNewSession = function() {
  session = { date: '', cubeId, cubeName, decks: [] };
  wizardState = { step: 'howmany', deckCount: 0, currentDeck: 0, currentColors: [] };
  hide('results-section'); show('wizard-section'); renderWizard();
};

// ── Local storage ─────────────────────────────────────────────────
function saveSession(sess) {
  try {
    const key = 'dl_sessions_v1';
    const all = JSON.parse(localStorage.getItem(key) || '[]');
    all.unshift({ ...sess, savedAt: new Date().toISOString() });
    if (all.length > 50) all.length = 50;
    localStorage.setItem(key, JSON.stringify(all));
  } catch(e) {}
}

function loadHistory() {
  try {
    const key      = 'dl_sessions_v1';
    const sessions = JSON.parse(localStorage.getItem(key) || '[]').filter(s => s.cubeId === cubeId);
    if (!sessions.length) return;
    show('history-section');
    const rows = sessions.map((s, i) => {
      const deckList = s.decks.map(d =>
        escHtml(d.player || '—') + ' <span style="font-size:0.75rem;color:var(--dl-muted)">(' +
        (d.colors.join('') || '—') + ', ' + d.cards.length + ' cards)</span>'
      ).join('<br/>');
      return '<tr>' +
        '<td>' + escHtml(s.date || (s.savedAt||'').slice(0,10)) + '</td>' +
        '<td>' + deckList + '</td>' +
        '<td style="text-align:right;white-space:nowrap;">' +
          '<button class="dl-btn" style="font-size:0.75rem;" onclick="restoreSession('+i+')">View</button> ' +
          '<button class="dl-btn danger" style="font-size:0.75rem;" onclick="deleteSession('+i+')">Delete</button>' +
        '</td></tr>';
    }).join('');
    el('history-content').innerHTML =
      '<table class="history-table"><thead><tr><th>Date</th><th>Decks</th><th></th></tr></thead><tbody>' + rows + '</tbody></table>';
  } catch(e) {}
}

window.restoreSession = function(i) {
  try {
    const sessions = JSON.parse(localStorage.getItem('dl_sessions_v1') || '[]').filter(s => s.cubeId === cubeId);
    session = sessions[i];
    show('results-section'); renderResults();
    window.scrollTo({ top: el('results-section').offsetTop - 60, behavior: 'smooth' });
  } catch(e) {}
};

window.deleteSession = function(i) {
  try {
    const key  = 'dl_sessions_v1';
    const all  = JSON.parse(localStorage.getItem(key) || '[]');
    const mine = all.filter(s => s.cubeId === cubeId);
    const del  = mine[i];
    localStorage.setItem(key, JSON.stringify(all.filter(s => s !== del)));
    loadHistory();
  } catch(e) {}
};

window.loadCube = loadCube;

})();
</script>