---
layout: single
title: Cube Calculator
permalink: /cubecalculator/
---


<style>
.mtg-calc {
  --mc-muted: #666;
  --mc-card: #f5f5f3;
  --mc-border: #e0e0e0;
  --mc-input-border: #ccc;
  --mc-blue: #378ADD;
  --mc-green: #639922;
  --mc-orange: #EF9F27;
  --mc-danger-bg: #fff0f0;
  --mc-danger-border: #f5c1c1;
  --mc-danger-text: #a32d2d;
  --mc-bg: #fff;
  --mc-text: #111;
}

@media (prefers-color-scheme: dark) {
  .mtg-calc {
    --mc-muted: #aaa;
    --mc-card: #27272b;
    --mc-border: #3a3a3a;
    --mc-input-border: #3a3a3a;
    --mc-blue: #4ea1ff;
    --mc-green: #7cc04a;
    --mc-orange: #ffb347;
    --mc-danger-bg: #2a1414;
    --mc-danger-border: #5a2a2a;
    --mc-danger-text: #ff8b8b;
    --mc-bg: transparent;
    --mc-text: #eaeaea;
  }
}

.mtg-calc *,
.mtg-calc *::before,
.mtg-calc *::after {
  box-sizing: border-box;
}

.mtg-calc {
  color: var(--mc-text);
  padding: 0 0 2rem;
}

.mtg-calc .mc-section-label {
  font-size: 12px;
  font-weight: 500;
  color: var(--mc-muted);
  margin-bottom: 0.65rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.mtg-calc .mc-slider-row {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 0.65rem;
}

.mtg-calc .mc-slider-label {
  font-size: 13px;
  color: var(--mc-muted);
  width: 165px;
  flex-shrink: 0;
}

.mtg-calc .mc-slider-row input[type=range] {
  flex: 1;
  accent-color: var(--mc-blue);
  margin: 0;
}

.mtg-calc .mc-slider-row input[type=range].mc-threshold {
  accent-color: var(--mc-green);
}

.mtg-calc .mc-num-input {
  width: 62px;
  font-size: 13px;
  font-weight: 500;
  text-align: center;
  padding: 0 6px;
  height: 32px;
  border-radius: 8px;
  border: 0.5px solid var(--mc-input-border);
  background: var(--mc-bg);
  color: var(--mc-text);
  appearance: textfield;
  -moz-appearance: textfield;
  flex-shrink: 0;
}

.mtg-calc .mc-num-input::-webkit-outer-spin-button,
.mtg-calc .mc-num-input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}

.mtg-calc .mc-num-input:focus {
  outline: none;
  border-color: var(--mc-blue);
  box-shadow: 0 0 0 2px rgba(55,138,221,0.15);
}

.mtg-calc .mc-pct-symbol {
  font-size: 13px;
  color: var(--mc-muted);
  flex-shrink: 0;
}

.mtg-calc .mc-divider {
  border: none;
  border-top: 0.5px solid var(--mc-border);
  margin: 1.25rem 0;
}

.mtg-calc .mc-threshold-box {
  background: var(--mc-card);
  border-radius: 10px;
  padding: 12px 14px;
  margin-bottom: 1.1rem;
}

.mtg-calc .mc-metrics {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 10px;
  margin: 1.1rem 0;
}

.mtg-calc .mc-metric {
  background: var(--mc-card);
  border-radius: 8px;
  padding: 12px 14px;
}

.mtg-calc .mc-metric-label {
  font-size: 12px;
  color: var(--mc-muted);
  margin-bottom: 4px;
}

.mtg-calc .mc-metric-value {
  font-size: 22px;
  font-weight: 500;
  line-height: 1.1;
}

.mtg-calc .mc-metric-sub {
  font-size: 11px;
  color: var(--mc-muted);
  margin-top: 3px;
}

.mtg-calc .mc-bar-track {
  height: 6px;
  background: var(--mc-border);
  border-radius: 3px;
  overflow: hidden;
  margin-top: 6px;
  position: relative;
}

.mtg-calc .mc-bar-fill {
  height: 100%;
  border-radius: 3px;
  transition: width 0.3s;
  background: var(--mc-blue);
}

.mtg-calc .mc-bar-fill.warn { background: var(--mc-orange); }
.mtg-calc .mc-bar-fill.good { background: var(--mc-green); }

.mtg-calc .mc-bar-threshold {
  position: absolute;
  top: -2px;
  width: 2px;
  height: 10px;
  background: #555;
  border-radius: 1px;
}

.mtg-calc .mc-error-box {
  background: var(--mc-danger-bg);
  border: 0.5px solid var(--mc-danger-border);
  border-radius: 8px;
  padding: 12px 14px;
  font-size: 13px;
  color: var(--mc-danger-text);
  margin: 1.1rem 0;
}

.mtg-calc .mc-preset-group-label {
  font-size: 12px;
  color: var(--mc-muted);
  margin: 0.75rem 0 0.4rem;
}

.mtg-calc .mc-presets {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 8px;
}

.mtg-calc .mc-preset-btn {
  font-size: 12px;
  color: var(--mc-muted);
  padding: 7px 4px;
  background: var(--mc-bg);
  border: 0.5px solid var(--mc-border);
  border-radius: 8px;
  cursor: pointer;
}

.mtg-calc .mc-preset-btn:hover {
  background: var(--mc-card);
}

.mtg-calc .mc-loading {
  font-size: 13px;
  color: var(--mc-muted);
  margin: 1rem 0;
}

.mtg-calc .mc-toggle-row {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 0.65rem;
}

.mtg-calc .mc-toggle-label {
  font-size: 13px;
  color: var(--mc-muted);
  cursor: pointer;
  user-select: none;
}

.mtg-calc .mc-toggle {
  position: relative;
  width: 36px;
  height: 20px;
  flex-shrink: 0;
}

.mtg-calc .mc-toggle input {
  opacity: 0;
  width: 0;
  height: 0;
  position: absolute;
}

.mtg-calc .mc-toggle-track {
  position: absolute;
  inset: 0;
  border-radius: 10px;
  background: var(--mc-border);
  border: 0.5px solid var(--mc-input-border);
  transition: background 0.2s, border-color 0.2s;
  cursor: pointer;
}

.mtg-calc .mc-toggle input:checked + .mc-toggle-track {
  background: var(--mc-blue);
  border-color: var(--mc-blue);
}

.mtg-calc .mc-toggle-track::after {
  content: '';
  position: absolute;
  top: 2px;
  left: 2px;
  width: 14px;
  height: 14px;
  border-radius: 50%;
  background: #fff;
  transition: transform 0.2s;
}

.mtg-calc .mc-toggle input:checked + .mc-toggle-track::after {
  transform: translateX(16px);
}
</style>

<div class="mtg-calc">

  <div class="mc-slider-row">
    <span class="mc-slider-label">Players</span>
    <input type="range" id="mc-s-players" min="2" max="12" value="8">
    <input type="number" id="mc-n-players" class="mc-num-input" value="8">
  </div>

  <div class="mc-slider-row">
    <span class="mc-slider-label">Cube size</span>
    <input type="range" id="mc-s-cube" min="60" max="720" value="360">
    <input type="number" id="mc-n-cube" class="mc-num-input" value="360">
  </div>

  <div class="mc-slider-row">
    <span class="mc-slider-label">Desired pool size</span>
    <input type="range" id="mc-s-pool" min="10" max="90" value="45">
    <input type="number" id="mc-n-pool" class="mc-num-input" value="45">
  </div>

  <hr class="mc-divider">

  <div class="mc-slider-row">
    <span class="mc-slider-label">Max cards per pack</span>
    <input type="range" id="mc-s-cpp" min="3" max="30" value="20">
    <input type="number" id="mc-n-cpp" class="mc-num-input" value="20">
  </div>

  <div class="mc-slider-row">
    <span class="mc-slider-label">Max packs per player</span>
    <input type="range" id="mc-s-ppp" min="1" max="20" value="15">
    <input type="number" id="mc-n-ppp" class="mc-num-input" value="15">
  </div>

  <hr class="mc-divider">

  <div class="mc-threshold-box">
    <div class="mc-section-label">Minimum targets</div>

    <div class="mc-slider-row">
      <span class="mc-slider-label">Min % seen</span>
      <input type="range" id="mc-s-min-seen" class="mc-threshold" min="0" max="100" value="0">
      <input type="number" id="mc-n-min-seen" class="mc-num-input" value="0">
      <span class="mc-pct-symbol">%</span>
    </div>

    <div class="mc-slider-row">
      <span class="mc-slider-label">Min % used</span>
      <input type="range" id="mc-s-min-used" class="mc-threshold" min="0" max="100" value="0">
      <input type="number" id="mc-n-min-used" class="mc-num-input" value="0">
      <span class="mc-pct-symbol">%</span>
    </div>

    <div class="mc-toggle-row">
      <label class="mc-toggle" for="mc-no-burn">
        <input type="checkbox" id="mc-no-burn">
        <span class="mc-toggle-track"></span>
      </label>
      <label class="mc-toggle-label" for="mc-no-burn">No burn — every picked card enters the pool</label>
    </div>
  </div>

  <div id="mc-output"><p class="mc-loading">Calculating…</p></div>

  <hr class="mc-divider">

  <p class="mc-preset-group-label">Normal cube · 360 cards · 45-card pool</p>
  <div class="mc-presets" id="mc-presets-360"></div>

  <p class="mc-preset-group-label">Micro cube · 192 cards · 24-card pool</p>
  <div class="mc-presets" id="mc-presets-192"></div>

</div>

<script>
(function () {
  function $id(id) { return document.getElementById(id); }

  function seenPerPack(players, cards) {
    var s = 0;
    for (var i = 0; i < players; i++) s += cards - i;
    return s;
  }

  function bestConfig(players, cube, pool, maxPacks, maxCards, minSeen, minUsed, noBurn) {
    var best = null, score = -Infinity;
    for (var p = 1; p <= maxPacks; p++) {
      for (var c = 1; c <= maxCards; c++) {
        var seen = seenPerPack(players, c) * p;
        var burn = Math.round(c - pool / p);
        var used = p * c * players;
        if (used > cube || burn < 0) continue;
        if (noBurn && burn !== 0) continue;
        var util = used / cube;
        var seenPct = seen / cube * 100;
        if (seenPct < minSeen || util * 100 < minUsed) continue;
        var poolSize = p * (c - burn);
        var s = seen / cube + util - (burn * 0.1 + Math.abs(poolSize - pool) / pool);
        if (s > score) {
          score = s;
          best = { packs: p, cards: c, seen: seen, burn: burn, used: used, utilization: util, pool_size: poolSize };
        }
      }
    }
    return best;
  }

  function val(id) { return +$id(id).value; }

  function calculate() {
    var noBurn = $id('mc-no-burn').checked;
    var r = bestConfig(
      val('mc-s-players'), val('mc-s-cube'), val('mc-s-pool'),
      val('mc-s-ppp'), val('mc-s-cpp'),
      val('mc-s-min-seen'), val('mc-s-min-used'),
      noBurn
    );
    render(r, noBurn);
  }

  function render(c, noBurn) {
    var cube    = val('mc-s-cube');
    var pool    = val('mc-s-pool');
    var minSeen = val('mc-s-min-seen');
    var minUsed = val('mc-s-min-used');
    var out     = $id('mc-output');

    if (!c) {
      out.innerHTML = '<div class="mc-error-box">No configuration found matching your constraints.</div>';
      return;
    }

    var seenPct = Math.round(c.seen / cube * 100);
    var usedPct = Math.round(c.utilization * 100);

    var poolDiff = c.pool_size - pool;
    var poolSub = poolDiff === 0 ? 'exact'
      : (poolDiff > 0 ? '+' + poolDiff : poolDiff) + ' from target';

    var seenClass = seenPct >= 60 ? 'good' : seenPct >= 35 ? '' : 'warn';
    var usedClass = usedPct >= 60 ? 'good' : usedPct >= 35 ? '' : 'warn';

    var seenMarker = minSeen > 0
      ? '<div class="mc-bar-threshold" style="left:' + Math.min(minSeen, 99) + '%"></div>' : '';
    var usedMarker = minUsed > 0
      ? '<div class="mc-bar-threshold" style="left:' + Math.min(minUsed, 99) + '%"></div>' : '';

    out.innerHTML = '<div class="mc-metrics">' +
      metric('Packs per player', c.packs, '') +
      metric('Cards per pack', c.cards, '') +
      (noBurn ? '' : metric('Burn per pack', c.burn, '')) +
      metric('Pool size', c.pool_size, poolSub) +
      metricBar('Cards seen', c.seen, seenPct, seenClass, seenMarker,
        'Seen: ' + seenPct + '% of cube. Target: ' + minSeen + '%') +
      metricBar('Cards used', c.used, usedPct, usedClass, usedMarker,
        'Used: ' + usedPct + '% of cube. Target: ' + minUsed + '%') +
      '</div>';
  }

  function metric(label, value, sub) {
    return '<div class="mc-metric">' +
      '<div class="mc-metric-label">' + label + '</div>' +
      '<div class="mc-metric-value">' + value + '</div>' +
      (sub ? '<div class="mc-metric-sub">' + sub + '</div>' : '') +
      '</div>';
  }

  function metricBar(label, value, pct, cls, marker, sub) {
    return '<div class="mc-metric">' +
      '<div class="mc-metric-label">' + label + '</div>' +
      '<div class="mc-metric-value">' + value + '</div>' +
      '<div class="mc-bar-track">' +
        '<div class="mc-bar-fill ' + cls + '" style="width:' + pct + '%"></div>' +
        marker +
      '</div>' +
      '<div class="mc-metric-sub">' + sub + '</div>' +
      '</div>';
  }

  function link(sliderId, numberId) {
    var s = $id(sliderId), n = $id(numberId);
    if (!s || !n) return;
    s.addEventListener('input', function () { n.value = s.value; calculate(); });
    n.addEventListener('input', function () { s.value = n.value; calculate(); });
  }

  link('mc-s-players', 'mc-n-players');
  link('mc-s-cube',    'mc-n-cube');
  link('mc-s-pool',    'mc-n-pool');
  link('mc-s-cpp',     'mc-n-cpp');
  link('mc-s-ppp',     'mc-n-ppp');
  link('mc-s-min-seen','mc-n-min-seen');
  link('mc-s-min-used','mc-n-min-used');

  $id('mc-no-burn').addEventListener('change', calculate);

  function makePresets(containerId, cube, pool) {
    var el = $id(containerId);
    [8, 6, 4, 2].forEach(function (p) {
      var b = document.createElement('button');
      b.className = 'mc-preset-btn';
      b.textContent = p + ' players';
      b.addEventListener('click', function () {
        $id('mc-s-players').value = p; $id('mc-n-players').value = p;
        $id('mc-s-cube').value = cube;  $id('mc-n-cube').value = cube;
        $id('mc-s-pool').value = pool;  $id('mc-n-pool').value = pool;
        $id('mc-s-cpp').value = 20;     $id('mc-n-cpp').value = 20;
        $id('mc-s-ppp').value = 15;     $id('mc-n-ppp').value = 15;
        $id('mc-s-min-seen').value = 0; $id('mc-n-min-seen').value = 0;
        $id('mc-s-min-used').value = 0; $id('mc-n-min-used').value = 0;
        calculate();
      });
      el.appendChild(b);
    });
  }

  makePresets('mc-presets-360', 360, 45);
  makePresets('mc-presets-192', 192, 24);

  calculate();
})();
</script>