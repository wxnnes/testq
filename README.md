# testqcat > /mnt/user-data/outputs/trading-journal/index.html << 'HTMLEOF'
<!DOCTYPE html>
<html lang="nl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Trading Journal</title>
<link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;600;700&family=Rajdhani:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --gold: #C9A84C;
    --gold-light: #E8C97A;
    --gold-dark: #8B6914;
    --gold-muted: #6B5020;
    --black: #0A0A0A;
    --black-2: #111111;
    --black-3: #1A1A1A;
    --black-4: #222222;
    --black-5: #2A2A2A;
    --border: rgba(201,168,76,0.2);
    --border-hover: rgba(201,168,76,0.5);
    --text: #E8E0CC;
    --text-muted: #8A8070;
    --green: #4CAF7A;
    --red: #C94C4C;
    --neutral: #C9A84C;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--black);
    color: var(--text);
    font-family: 'Rajdhani', sans-serif;
    font-size: 15px;
    min-height: 100vh;
  }

  /* HEADER */
  header {
    background: var(--black-2);
    border-bottom: 1px solid var(--border);
    padding: 0 2rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 64px;
    position: sticky;
    top: 0;
    z-index: 100;
  }

  .logo {
    font-family: 'Cinzel', serif;
    font-size: 20px;
    font-weight: 700;
    color: var(--gold);
    letter-spacing: 3px;
    text-transform: uppercase;
  }

  .logo span {
    color: var(--text-muted);
    font-size: 11px;
    display: block;
    letter-spacing: 5px;
    font-weight: 400;
    margin-top: -2px;
  }

  .header-actions {
    display: flex;
    gap: 12px;
    align-items: center;
  }

  .btn {
    background: transparent;
    border: 1px solid var(--border);
    color: var(--gold);
    font-family: 'Rajdhani', sans-serif;
    font-size: 13px;
    font-weight: 600;
    letter-spacing: 2px;
    padding: 8px 20px;
    cursor: pointer;
    transition: all 0.2s;
    text-transform: uppercase;
  }

  .btn:hover {
    background: rgba(201,168,76,0.08);
    border-color: var(--gold);
  }

  .btn-gold {
    background: var(--gold);
    border-color: var(--gold);
    color: var(--black);
  }

  .btn-gold:hover {
    background: var(--gold-light);
    border-color: var(--gold-light);
  }

  /* MAIN LAYOUT */
  main {
    max-width: 1400px;
    margin: 0 auto;
    padding: 2rem;
  }

  /* DATE HEADER */
  .date-section {
    margin-bottom: 2.5rem;
  }

  .date-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 1.5rem;
    padding-bottom: 1rem;
    border-bottom: 1px solid var(--border);
  }

  .date-title {
    font-family: 'Cinzel', serif;
    font-size: 22px;
    color: var(--gold);
    letter-spacing: 2px;
  }

  .date-subtitle {
    font-size: 12px;
    color: var(--text-muted);
    letter-spacing: 3px;
    text-transform: uppercase;
    margin-top: 2px;
  }

  .date-actions {
    display: flex;
    gap: 8px;
  }

  /* TRADE CARD */
  .trade-card {
    background: var(--black-2);
    border: 1px solid var(--border);
    margin-bottom: 1.5rem;
    position: relative;
    transition: border-color 0.2s;
  }

  .trade-card:hover {
    border-color: var(--border-hover);
  }

  .trade-card::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 3px;
    height: 100%;
    background: var(--gold-dark);
    transition: background 0.2s;
  }

  .trade-card.profit::before { background: var(--green); }
  .trade-card.loss::before { background: var(--red); }
  .trade-card.breakeven::before { background: var(--gold); }

  .trade-card-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 12px 20px;
    border-bottom: 1px solid var(--border);
    background: var(--black-3);
    cursor: pointer;
    user-select: none;
  }

  .trade-card-title {
    font-family: 'Cinzel', serif;
    font-size: 13px;
    color: var(--gold);
    letter-spacing: 2px;
  }

  .trade-card-badges {
    display: flex;
    gap: 8px;
    align-items: center;
  }

  .badge {
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 1.5px;
    padding: 3px 10px;
    text-transform: uppercase;
    border: 1px solid;
  }

  .badge-profit { color: var(--green); border-color: rgba(76,175,122,0.4); background: rgba(76,175,122,0.08); }
  .badge-loss { color: var(--red); border-color: rgba(201,76,76,0.4); background: rgba(201,76,76,0.08); }
  .badge-be { color: var(--gold); border-color: rgba(201,168,76,0.4); background: rgba(201,168,76,0.08); }
  .badge-rating { color: var(--gold-light); border-color: rgba(232,201,122,0.4); background: rgba(232,201,122,0.05); }

  .collapse-icon {
    color: var(--text-muted);
    font-size: 18px;
    transition: transform 0.2s;
    line-height: 1;
  }

  .trade-card.collapsed .collapse-icon { transform: rotate(-90deg); }

  /* TRADE BODY */
  .trade-body {
    padding: 20px;
  }

  .trade-card.collapsed .trade-body { display: none; }

  .fields-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 16px;
  }

  .field-group {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .field-label {
    font-size: 10px;
    font-weight: 600;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: var(--text-muted);
  }

  /* RADIO / CHECKBOX GROUPS */
  .option-group {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
  }

  .opt-btn {
    background: transparent;
    border: 1px solid var(--border);
    color: var(--text-muted);
    font-family: 'Rajdhani', sans-serif;
    font-size: 12px;
    font-weight: 600;
    letter-spacing: 1px;
    padding: 5px 12px;
    cursor: pointer;
    transition: all 0.15s;
    text-transform: uppercase;
    white-space: nowrap;
  }

  .opt-btn:hover {
    border-color: var(--gold-muted);
    color: var(--text);
  }

  .opt-btn.selected {
    background: rgba(201,168,76,0.15);
    border-color: var(--gold);
    color: var(--gold);
  }

  .opt-btn.selected.green-opt {
    background: rgba(76,175,122,0.15);
    border-color: var(--green);
    color: var(--green);
  }

  .opt-btn.selected.red-opt {
    background: rgba(201,76,76,0.15);
    border-color: var(--red);
    color: var(--red);
  }

  /* TOGGLE (ja/nee) */
  .toggle-group {
    display: flex;
    gap: 6px;
  }

  /* PHOTO UPLOAD */
  .photo-zone {
    border: 1px dashed var(--border);
    padding: 16px;
    text-align: center;
    cursor: pointer;
    transition: all 0.2s;
    position: relative;
    min-height: 80px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 6px;
  }

  .photo-zone:hover {
    border-color: var(--gold);
    background: rgba(201,168,76,0.04);
  }

  .photo-zone input[type=file] {
    position: absolute;
    inset: 0;
    opacity: 0;
    cursor: pointer;
    width: 100%;
    height: 100%;
  }

  .photo-zone .upload-icon {
    font-size: 22px;
    color: var(--gold-dark);
  }

  .photo-zone .upload-label {
    font-size: 11px;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: var(--text-muted);
  }

  .photo-previews {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-top: 10px;
  }

  .photo-thumb {
    position: relative;
    width: 80px;
    height: 60px;
  }

  .photo-thumb img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    border: 1px solid var(--border);
  }

  .photo-thumb button {
    position: absolute;
    top: -4px;
    right: -4px;
    width: 16px;
    height: 16px;
    background: var(--red);
    border: none;
    color: #fff;
    font-size: 10px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    line-height: 1;
  }

  /* TEXTAREA */
  .notes-field {
    background: var(--black-3);
    border: 1px solid var(--border);
    color: var(--text);
    font-family: 'Rajdhani', sans-serif;
    font-size: 14px;
    padding: 10px 14px;
    width: 100%;
    resize: vertical;
    min-height: 80px;
    transition: border-color 0.2s;
    outline: none;
  }

  .notes-field:focus {
    border-color: var(--gold);
  }

  .notes-field::placeholder {
    color: var(--text-muted);
    font-style: italic;
  }

  /* DOL — multiselect */
  .dol-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 5px;
  }

  /* SEPARATOR */
  .trade-separator {
    border: none;
    border-top: 1px solid var(--border);
    margin: 16px 0;
  }

  /* STATS BAR */
  .stats-bar {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
    gap: 12px;
    margin-bottom: 2rem;
    padding: 1.25rem;
    background: var(--black-2);
    border: 1px solid var(--border);
  }

  .stat-item {
    text-align: center;
  }

  .stat-value {
    font-family: 'Cinzel', serif;
    font-size: 22px;
    font-weight: 600;
    color: var(--gold);
    display: block;
  }

  .stat-value.green { color: var(--green); }
  .stat-value.red { color: var(--red); }

  .stat-label {
    font-size: 10px;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: var(--text-muted);
    margin-top: 2px;
  }

  /* ORNAMENT */
  .ornament {
    text-align: center;
    color: var(--gold-dark);
    font-size: 18px;
    letter-spacing: 8px;
    margin: 0.5rem 0 1.5rem;
    opacity: 0.6;
  }

  /* ADD TRADE BUTTON */
  .add-trade-row {
    text-align: center;
    padding: 1rem 0;
  }

  /* SECTION HEADER */
  .section-row {
    display: flex;
    align-items: center;
    gap: 16px;
    margin-bottom: 1.5rem;
  }

  .section-row::before,
  .section-row::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--border);
  }

  .section-row-text {
    font-family: 'Cinzel', serif;
    font-size: 11px;
    letter-spacing: 4px;
    color: var(--text-muted);
    text-transform: uppercase;
    white-space: nowrap;
  }

  /* MODAL */
  .modal-overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.85);
    z-index: 500;
    align-items: center;
    justify-content: center;
  }

  .modal-overlay.open { display: flex; }

  .modal {
    background: var(--black-2);
    border: 1px solid var(--border);
    padding: 2rem;
    width: 380px;
    max-width: 95vw;
  }

  .modal-title {
    font-family: 'Cinzel', serif;
    font-size: 16px;
    color: var(--gold);
    letter-spacing: 2px;
    margin-bottom: 1.5rem;
  }

  .modal-field {
    margin-bottom: 1rem;
  }

  .modal-field label {
    display: block;
    font-size: 10px;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: var(--text-muted);
    margin-bottom: 6px;
  }

  .modal-field input[type=date] {
    background: var(--black-3);
    border: 1px solid var(--border);
    color: var(--text);
    font-family: 'Rajdhani', sans-serif;
    font-size: 14px;
    padding: 8px 12px;
    width: 100%;
    outline: none;
    color-scheme: dark;
  }

  .modal-field input[type=date]:focus {
    border-color: var(--gold);
  }

  .modal-actions {
    display: flex;
    gap: 10px;
    margin-top: 1.5rem;
    justify-content: flex-end;
  }

  /* PHOTO FULLSCREEN */
  .lightbox {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.95);
    z-index: 900;
    align-items: center;
    justify-content: center;
  }

  .lightbox.open { display: flex; }

  .lightbox img {
    max-width: 90vw;
    max-height: 90vh;
    border: 1px solid var(--border);
  }

  .lightbox-close {
    position: fixed;
    top: 20px;
    right: 24px;
    background: transparent;
    border: 1px solid var(--border);
    color: var(--gold);
    font-size: 20px;
    width: 36px;
    height: 36px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  /* TOAST */
  .toast {
    position: fixed;
    bottom: 24px;
    right: 24px;
    background: var(--black-3);
    border: 1px solid var(--gold);
    color: var(--gold);
    font-family: 'Rajdhani', sans-serif;
    font-size: 13px;
    font-weight: 600;
    letter-spacing: 2px;
    padding: 10px 20px;
    text-transform: uppercase;
    z-index: 999;
    transform: translateY(80px);
    opacity: 0;
    transition: all 0.3s;
  }

  .toast.show {
    transform: translateY(0);
    opacity: 1;
  }

  @media (max-width: 700px) {
    header { padding: 0 1rem; }
    main { padding: 1rem; }
    .fields-grid { grid-template-columns: 1fr; }
    .stats-bar { grid-template-columns: repeat(2, 1fr); }
  }
</style>
</head>
<body>

<header>
  <div class="logo">
    Trading Journal
    <span>Options &amp; Futures</span>
  </div>
  <div class="header-actions">
    <button class="btn" onclick="exportData()">&#8659; Export</button>
    <button class="btn" onclick="importClick()">&#8657; Import</button>
    <input type="file" id="importFile" accept=".json" style="display:none" onchange="importData(event)">
    <button class="btn btn-gold" onclick="openNewDateModal()">+ New Day</button>
  </div>
</header>

<main id="main">
  <div id="statsBar" class="stats-bar">
    <div class="stat-item">
      <span class="stat-value" id="statTotal">0</span>
      <div class="stat-label">Total Trades</div>
    </div>
    <div class="stat-item">
      <span class="stat-value green" id="statWins">0</span>
      <div class="stat-label">Wins</div>
    </div>
    <div class="stat-item">
      <span class="stat-value red" id="statLosses">0</span>
      <div class="stat-label">Losses</div>
    </div>
    <div class="stat-item">
      <span class="stat-value" id="statBE">0</span>
      <div class="stat-label">Break Even</div>
    </div>
    <div class="stat-item">
      <span class="stat-value" id="statWinRate">0%</span>
      <div class="stat-label">Win Rate</div>
    </div>
    <div class="stat-item">
      <span class="stat-value" id="statTopRating">—</span>
      <div class="stat-label">A+ Trades</div>
    </div>
  </div>

  <div id="journalContainer"></div>

  <div id="emptyState" style="text-align:center;padding:4rem 0;">
    <div style="font-family:'Cinzel',serif;font-size:32px;color:var(--gold-dark);letter-spacing:4px;">◆</div>
    <div style="font-family:'Cinzel',serif;font-size:16px;color:var(--text-muted);letter-spacing:3px;margin-top:12px;">NO ENTRIES YET</div>
    <div style="font-size:13px;color:var(--text-muted);margin-top:8px;letter-spacing:1px;">Click <strong style="color:var(--gold)">+ New Day</strong> to start your journal</div>
  </div>
</main>

<!-- NEW DATE MODAL -->
<div class="modal-overlay" id="modalOverlay">
  <div class="modal">
    <div class="modal-title">◆ Add New Day</div>
    <div class="modal-field">
      <label>Date</label>
      <input type="date" id="modalDate">
    </div>
    <div class="modal-actions">
      <button class="btn" onclick="closeModal()">Cancel</button>
      <button class="btn btn-gold" onclick="createDay()">Create</button>
    </div>
  </div>
</div>

<!-- LIGHTBOX -->
<div class="lightbox" id="lightbox" onclick="closeLightbox()">
  <button class="lightbox-close" onclick="closeLightbox()">✕</button>
  <img id="lightboxImg" src="">
</div>

<!-- TOAST -->
<div class="toast" id="toast"></div>

<script>
// ─── DATA ───────────────────────────────────────────────
let journal = JSON.parse(localStorage.getItem('tj_data') || '{}');

function save() {
  const copy = JSON.parse(JSON.stringify(journal));
  // Remove image data from localStorage (too large) — keep refs
  localStorage.setItem('tj_data', JSON.stringify(copy));
  updateStats();
}

// ─── STATS ──────────────────────────────────────────────
function updateStats() {
  let total=0, wins=0, losses=0, be=0, aplus=0;
  Object.values(journal).forEach(day => {
    day.trades.forEach(t => {
      total++;
      if(t.pnl==='profit') wins++;
      else if(t.pnl==='loss') losses++;
      else if(t.pnl==='be') be++;
      if(t.rating==='A+') aplus++;
    });
  });
  document.getElementById('statTotal').textContent = total;
  document.getElementById('statWins').textContent = wins;
  document.getElementById('statLosses').textContent = losses;
  document.getElementById('statBE').textContent = be;
  const wr = total > 0 ? Math.round((wins/total)*100) : 0;
  document.getElementById('statWinRate').textContent = wr + '%';
  document.getElementById('statTopRating').textContent = aplus;
}

// ─── RENDER ─────────────────────────────────────────────
function render() {
  const container = document.getElementById('journalContainer');
  const empty = document.getElementById('emptyState');
  const dates = Object.keys(journal).sort((a,b) => b.localeCompare(a));

  if(dates.length === 0) {
    container.innerHTML = '';
    empty.style.display = 'block';
    updateStats();
    return;
  }
  empty.style.display = 'none';

  container.innerHTML = dates.map(date => renderDay(date)).join('');
  updateStats();
}

function fmtDate(d) {
  const dt = new Date(d + 'T00:00:00');
  return dt.toLocaleDateString('nl-BE', { weekday:'long', year:'numeric', month:'long', day:'numeric' });
}

function renderDay(date) {
  const day = journal[date];
  const trades = day.trades || [];
  return `
  <div class="date-section" id="day-${date}">
    <div class="date-header">
      <div>
        <div class="date-title">${fmtDate(date)}</div>
        <div class="date-subtitle">${trades.length} trade${trades.length!==1?'s':''}</div>
      </div>
      <div class="date-actions">
        <button class="btn" onclick="addTrade('${date}')">+ Trade</button>
        <button class="btn" style="color:var(--red);border-color:rgba(201,76,76,0.3)" onclick="deleteDay('${date}')">✕ Day</button>
      </div>
    </div>
    ${trades.map((t,i) => renderTrade(date, i, t)).join('')}
    ${trades.length===0 ? '<div style="padding:1rem;color:var(--text-muted);font-size:12px;letter-spacing:2px;text-align:center">NO TRADES — CLICK + TRADE TO ADD</div>' : ''}
  </div>`;
}

function renderTrade(date, idx, t) {
  const pnlClass = t.pnl==='profit'?'profit':t.pnl==='loss'?'loss':t.pnl==='be'?'breakeven':'';
  const pnlBadge = t.pnl ? `<span class="badge badge-${t.pnl==='profit'?'profit':t.pnl==='loss'?'loss':'be'}">${t.pnl==='be'?'B/E':t.pnl||'—'}</span>` : '';
  const ratingBadge = t.rating ? `<span class="badge badge-rating">${t.rating}</span>` : '';
  const collapsed = t.collapsed ? 'collapsed' : '';

  const dolOpts = ['NWOG','NDOG','15M ITL','15M ITH','1H ITL','1H ITH','4H ITL','4H ITH','15M FVG','1H FVG','4H FVG','Daily ITL','Daily ITH','Daily FVG','ATH','Swing Low','Swing High','PDL','PDH'];
  const tfOpts = ['IFVG','30sec','1M','2M','3M','4M','5M'];
  const timeOpts = ['Macro','10AM–10:30','10:30–11AM'];
  const stratOpts = ['Continuation','Mech','Other'];
  const biasOpts = ['Bullish','Bearish','Neutraal'];
  const ratingOpts = ['B','B+','A','A+'];
  const smtLocal = ['Lokaal','Groot'];

  const photos = t.photos || [];

  return `
  <div class="trade-card ${pnlClass} ${collapsed}" id="trade-${date}-${idx}">
    <div class="trade-card-header" onclick="toggleCollapse('${date}',${idx})">
      <div class="trade-card-title">Trade #${idx+1}</div>
      <div class="trade-card-badges">
        ${pnlBadge}${ratingBadge}
        ${t.strategy ? `<span class="badge" style="color:var(--text-muted);border-color:var(--border)">${t.strategy}</span>` : ''}
        ${t.bias ? `<span class="badge" style="color:var(--text-muted);border-color:var(--border)">${t.bias}</span>` : ''}
        <span class="collapse-icon">▾</span>
      </div>
    </div>
    <div class="trade-body">

      <div class="fields-grid">

        <!-- PNL -->
        <div class="field-group">
          <div class="field-label">P&L Result</div>
          <div class="option-group">
            ${[['profit','Profit','green-opt'],['loss','Loss','red-opt'],['be','Break Even','']].map(([v,l,c])=>
              `<button class="opt-btn ${c} ${t.pnl===v?'selected':''}" onclick="setField('${date}',${idx},'pnl','${v}',this,'${c}')">${l}</button>`
            ).join('')}
          </div>
        </div>

        <!-- TIJD -->
        <div class="field-group">
          <div class="field-label">Tijdvenster</div>
          <div class="option-group">
            ${timeOpts.map(v=>
              `<button class="opt-btn ${t.time===v?'selected':''}" onclick="setField('${date}',${idx},'time','${v}',this,'')">${v}</button>`
            ).join('')}
          </div>
        </div>

        <!-- STRATEGIE -->
        <div class="field-group">
          <div class="field-label">Strategie</div>
          <div class="option-group">
            ${stratOpts.map(v=>
              `<button class="opt-btn ${t.strategy===v?'selected':''}" onclick="setField('${date}',${idx},'strategy','${v}',this,'')">${v}</button>`
            ).join('')}
          </div>
        </div>

        <!-- TIMEFRAME -->
        <div class="field-group">
          <div class="field-label">Timeframe</div>
          <div class="option-group">
            ${tfOpts.map(v=>
              `<button class="opt-btn ${t.timeframe===v?'selected':''}" onclick="setField('${date}',${idx},'timeframe','${v}',this,'')">${v}</button>`
            ).join('')}
          </div>
        </div>

        <!-- BIAS -->
        <div class="field-group">
          <div class="field-label">Bias</div>
          <div class="option-group">
            ${biasOpts.map(v=>
              `<button class="opt-btn ${t.bias===v?'selected':''}" onclick="setField('${date}',${idx},'bias','${v}',this,'')">${v}</button>`
            ).join('')}
          </div>
        </div>

        <!-- RATING -->
        <div class="field-group">
          <div class="field-label">Rating</div>
          <div class="option-group">
            ${ratingOpts.map(v=>
              `<button class="opt-btn ${t.rating===v?'selected':''}" onclick="setField('${date}',${idx},'rating','${v}',this,'')">${v}</button>`
            ).join('')}
          </div>
        </div>

        <!-- SMT -->
        <div class="field-group">
          <div class="field-label">SMT?</div>
          <div class="option-group" style="align-items:center">
            <button class="opt-btn ${t.smt===true?'selected':''}" onclick="setField('${date}',${idx},'smt',true,this,'')">Ja</button>
            <button class="opt-btn ${t.smt===false?'selected red-opt':''}" onclick="setField('${date}',${idx},'smt',false,this,'red-opt')">Nee</button>
            ${t.smt===true ? smtLocal.map(v=>
              `<button class="opt-btn ${t.smtType===v?'selected':''}" onclick="setField('${date}',${idx},'smtType','${v}',this,'')">${v}</button>`
            ).join('') : ''}
          </div>
        </div>

        <!-- V SHAPE -->
        <div class="field-group">
          <div class="field-label">V-Shape?</div>
          <div class="option-group">
            <button class="opt-btn ${t.vshape===true?'selected':''}" onclick="setField('${date}',${idx},'vshape',true,this,'')">Ja</button>
            <button class="opt-btn ${t.vshape===false?'selected red-opt':''}" onclick="setField('${date}',${idx},'vshape',false,this,'red-opt')">Nee</button>
          </div>
        </div>

      </div>

      <hr class="trade-separator">

      <!-- DOL -->
      <div class="field-group" style="margin-bottom:16px">
        <div class="field-label">Draw on Liquidity (DOL)</div>
        <div class="dol-grid">
          ${dolOpts.map(v=>
            `<button class="opt-btn ${(t.dol||[]).includes(v)?'selected':''}" onclick="toggleDol('${date}',${idx},'${v}',this)">${v}</button>`
          ).join('')}
        </div>
      </div>

      <hr class="trade-separator">

      <!-- FOTO'S -->
      <div class="field-group" style="margin-bottom:16px">
        <div class="field-label">Screenshots / Grafieken</div>
        <div class="photo-zone">
          <input type="file" accept="image/*" multiple onchange="addPhotos('${date}',${idx},this)">
          <div class="upload-icon">⬆</div>
          <div class="upload-label">Klik om foto's toe te voegen</div>
        </div>
        <div class="photo-previews" id="photos-${date}-${idx}">
          ${photos.map((src,pi)=>`
            <div class="photo-thumb">
              <img src="${src}" onclick="openLightbox('${src}')" style="cursor:zoom-in">
              <button onclick="removePhoto('${date}',${idx},${pi})">✕</button>
            </div>
          `).join('')}
        </div>
      </div>

      <hr class="trade-separator">

      <!-- NOTITIES -->
      <div class="field-group">
        <div class="field-label">Extra Notities</div>
        <textarea class="notes-field" placeholder="Beschrijf je trade, gedachten, fouten, verbeterpunten…" onblur="setNotes('${date}',${idx},this.value)">${t.notes||''}</textarea>
      </div>

      <!-- DELETE -->
      <div style="text-align:right;margin-top:12px">
        <button class="btn" style="color:var(--red);border-color:rgba(201,76,76,0.2);font-size:11px;padding:5px 14px" onclick="deleteTrade('${date}',${idx})">✕ Delete Trade</button>
      </div>

    </div>
  </div>`;
}

// ─── ACTIONS ────────────────────────────────────────────
function toggleCollapse(date, idx) {
  journal[date].trades[idx].collapsed = !journal[date].trades[idx].collapsed;
  save();
  render();
}

function setField(date, idx, field, value, btn, colorClass) {
  const t = journal[date].trades[idx];
  if(t[field] === value) {
    t[field] = null;
    btn.classList.remove('selected', colorClass);
  } else {
    t[field] = value;
    const group = btn.closest('.option-group');
    group.querySelectorAll('.opt-btn').forEach(b => b.classList.remove('selected','green-opt','red-opt'));
    btn.classList.add('selected');
    if(colorClass) btn.classList.add(colorClass);
  }
  save();
  // re-render card header badges only
  const card = document.getElementById(`trade-${date}-${idx}`);
  if(card) {
    card.className = `trade-card ${t.pnl==='profit'?'profit':t.pnl==='loss'?'loss':t.pnl==='be'?'breakeven':''} ${t.collapsed?'collapsed':''}`;
    const titleBar = card.querySelector('.trade-card-badges');
    const pnlBadge = t.pnl ? `<span class="badge badge-${t.pnl==='profit'?'profit':t.pnl==='loss'?'loss':'be'}">${t.pnl==='be'?'B/E':t.pnl}</span>` : '';
    const ratingBadge = t.rating ? `<span class="badge badge-rating">${t.rating}</span>` : '';
    titleBar.innerHTML = `${pnlBadge}${ratingBadge}${t.strategy?`<span class="badge" style="color:var(--text-muted);border-color:var(--border)">${t.strategy}</span>`:''}${t.bias?`<span class="badge" style="color:var(--text-muted);border-color:var(--border)">${t.bias}</span>`:''}<span class="collapse-icon">▾</span>`;
  }
  // SMT re-render just that group
  if(field==='smt') { render(); }
  updateStats();
}

function toggleDol(date, idx, value, btn) {
  const t = journal[date].trades[idx];
  if(!t.dol) t.dol = [];
  const i = t.dol.indexOf(value);
  if(i>-1) { t.dol.splice(i,1); btn.classList.remove('selected'); }
  else { t.dol.push(value); btn.classList.add('selected'); }
  save();
}

function setNotes(date, idx, value) {
  journal[date].trades[idx].notes = value;
  save();
}

function addTrade(date) {
  journal[date].trades.push({ pnl:null, time:null, strategy:null, timeframe:null, bias:null, rating:null, smt:null, smtType:null, vshape:null, dol:[], notes:'', photos:[], collapsed:false });
  save();
  render();
  setTimeout(()=>{
    const cards = document.querySelectorAll(`#day-${date} .trade-card`);
    if(cards.length) cards[cards.length-1].scrollIntoView({behavior:'smooth',block:'start'});
  },50);
}

function deleteTrade(date, idx) {
  if(!confirm('Trade verwijderen?')) return;
  journal[date].trades.splice(idx,1);
  save();
  render();
}

function deleteDay(date) {
  if(!confirm(`Dag ${date} en alle trades verwijderen?`)) return;
  delete journal[date];
  save();
  render();
}

function addPhotos(date, idx, input) {
  const t = journal[date].trades[idx];
  if(!t.photos) t.photos = [];
  const files = Array.from(input.files);
  let loaded = 0;
  files.forEach(f => {
    const reader = new FileReader();
    reader.onload = e => {
      t.photos.push(e.target.result);
      loaded++;
      if(loaded===files.length) { save(); renderPhotoArea(date, idx, t); }
    };
    reader.readAsDataURL(f);
  });
}

function renderPhotoArea(date, idx, t) {
  const area = document.getElementById(`photos-${date}-${idx}`);
  if(!area) return;
  area.innerHTML = (t.photos||[]).map((src,pi)=>`
    <div class="photo-thumb">
      <img src="${src}" onclick="openLightbox('${src}')" style="cursor:zoom-in">
      <button onclick="removePhoto('${date}',${idx},${pi})">✕</button>
    </div>
  `).join('');
}

function removePhoto(date, idx, pi) {
  journal[date].trades[idx].photos.splice(pi,1);
  save();
  renderPhotoArea(date, idx, journal[date].trades[idx]);
}

// ─── MODAL ──────────────────────────────────────────────
function openNewDateModal() {
  const today = new Date().toISOString().split('T')[0];
  document.getElementById('modalDate').value = today;
  document.getElementById('modalOverlay').classList.add('open');
}

function closeModal() {
  document.getElementById('modalOverlay').classList.remove('open');
}

function createDay() {
  const date = document.getElementById('modalDate').value;
  if(!date) return;
  if(!journal[date]) journal[date] = { trades: [] };
  save();
  render();
  closeModal();
  setTimeout(()=>{
    const el = document.getElementById(`day-${date}`);
    if(el) el.scrollIntoView({behavior:'smooth'});
  },100);
  toast('Dag aangemaakt');
}

// ─── EXPORT / IMPORT ─────────────────────────────────────
function exportData() {
  const blob = new Blob([JSON.stringify(journal, null, 2)], {type:'application/json'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = `trading-journal-${new Date().toISOString().split('T')[0]}.json`;
  a.click();
  URL.revokeObjectURL(url);
  toast('Journal geëxporteerd');
}

function importClick() { document.getElementById('importFile').click(); }

function importData(e) {
  const f = e.target.files[0];
  if(!f) return;
  const r = new FileReader();
  r.onload = ev => {
    try {
      const data = JSON.parse(ev.target.result);
      Object.assign(journal, data);
      save();
      render();
      toast('Journal geïmporteerd');
    } catch(err) { alert('Ongeldig bestand'); }
  };
  r.readAsText(f);
  e.target.value = '';
}

// ─── LIGHTBOX ────────────────────────────────────────────
function openLightbox(src) {
  document.getElementById('lightboxImg').src = src;
  document.getElementById('lightbox').classList.add('open');
}

function closeLightbox() {
  document.getElementById('lightbox').classList.remove('open');
}

// ─── TOAST ───────────────────────────────────────────────
function toast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'), 2500);
}

// ─── KEYBOARD ────────────────────────────────────────────
document.addEventListener('keydown', e => {
  if(e.key==='Escape') { closeModal(); closeLightbox(); }
});
document.getElementById('modalOverlay').addEventListener('click', e => {
  if(e.target===e.currentTarget) closeModal();
});

// ─── INIT ────────────────────────────────────────────────
render();
</script>
</body>
</html>
HTMLEOF
mkdir -p /mnt/user-data/outputs/trading-journal
echo "done"
