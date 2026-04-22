<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Painel SME · COMLURB</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@300;400;500;600;700&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-annotation@3.0.1/dist/chartjs-plugin-annotation.min.js"></script>
<style>
  :root {
    --orange: #F97316;
    --orange-dim: rgba(249,115,22,0.15);
    --orange-glow: rgba(249,115,22,0.35);

    /* Navy Frio (default) */
    --bg-base: #0A0F1E;
    --bg-card: #0F1729;
    --bg-card2: #131D35;
    --bg-hover: #1A2540;
    --border: rgba(255,255,255,0.07);
    --border-accent: rgba(249,115,22,0.3);
    --text-primary: #E8EDF8;
    --text-secondary: #7A8BAD;
    --text-muted: #4A5A7A;
    --green: #22D3A0;
    --green-dim: rgba(34,211,160,0.15);
    --red: #F43F5E;
    --red-dim: rgba(244,63,94,0.15);
    --yellow: #FACC15;
    --yellow-dim: rgba(250,204,21,0.15);
    --blue-accent: #3B82F6;
  }

  body.navy-quente {
    --bg-base: #100A05;
    --bg-card: #1A0F07;
    --bg-card2: #22140A;
    --bg-hover: #2A1C10;
    --border: rgba(255,255,255,0.07);
    --text-secondary: #9A7A5A;
    --text-muted: #5A3A20;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--bg-base);
    color: var(--text-primary);
    min-height: 100vh;
    font-size: 14px;
    line-height: 1.5;
  }

  /* ── TOPBAR ── */
  .topbar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 28px;
    height: 56px;
    background: var(--bg-card);
    border-bottom: 1px solid var(--border);
    position: sticky;
    top: 0;
    z-index: 100;
  }

  .topbar-left {
    display: flex;
    align-items: center;
    gap: 20px;
  }

  .logo-mark {
    width: 32px; height: 32px;
    background: var(--orange);
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-weight: 700; font-size: 13px; color: #fff;
    letter-spacing: -0.5px;
  }

  .logo-text {
    font-size: 15px;
    font-weight: 600;
    color: var(--text-primary);
    letter-spacing: -0.3px;
  }

  .logo-sub {
    font-size: 11px;
    color: var(--text-secondary);
    font-weight: 400;
  }

  .page-tabs {
    display: flex;
    gap: 4px;
    background: var(--bg-base);
    padding: 4px;
    border-radius: 10px;
  }

  .page-tab {
    padding: 6px 18px;
    border-radius: 7px;
    font-size: 13px;
    font-weight: 500;
    cursor: pointer;
    border: none;
    background: transparent;
    color: var(--text-secondary);
    transition: all 0.2s;
  }

  .page-tab.active {
    background: var(--orange);
    color: #fff;
  }

  .page-tab:hover:not(.active) {
    background: var(--bg-hover);
    color: var(--text-primary);
  }

  .topbar-right {
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .theme-toggle {
    display: flex;
    gap: 2px;
    background: var(--bg-base);
    padding: 3px;
    border-radius: 8px;
  }

  .theme-btn {
    padding: 4px 10px;
    border-radius: 5px;
    font-size: 11px;
    font-weight: 500;
    cursor: pointer;
    border: none;
    background: transparent;
    color: var(--text-secondary);
    transition: all 0.2s;
  }

  .theme-btn.active {
    background: var(--bg-card2);
    color: var(--text-primary);
  }

  .topbar-date {
    font-size: 11px;
    color: var(--text-muted);
    font-family: 'DM Mono', monospace;
  }

  /* ── FILTER BAR ── */
  .filterbar {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 12px 28px;
    background: var(--bg-card);
    border-bottom: 1px solid var(--border);
    flex-wrap: wrap;
  }

  .filter-label {
    font-size: 11px;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.8px;
    font-weight: 600;
    margin-right: 4px;
  }

  .filter-select {
    background: var(--bg-base);
    border: 1px solid var(--border);
    color: var(--text-primary);
    padding: 6px 12px;
    border-radius: 8px;
    font-size: 13px;
    font-family: 'DM Sans', sans-serif;
    cursor: pointer;
    transition: border-color 0.2s;
    outline: none;
  }

  .filter-select:focus { border-color: var(--orange); }

  .filter-divider {
    width: 1px; height: 20px;
    background: var(--border);
  }

  .methodology-toggle {
    display: flex;
    gap: 2px;
    background: var(--bg-base);
    padding: 3px;
    border-radius: 8px;
    margin-left: auto;
  }

  .meth-btn {
    padding: 5px 12px;
    border-radius: 6px;
    font-size: 11px;
    font-weight: 500;
    cursor: pointer;
    border: none;
    background: transparent;
    color: var(--text-muted);
    transition: all 0.2s;
  }

  .meth-btn.active {
    background: var(--orange-dim);
    color: var(--orange);
    border: 1px solid var(--border-accent);
  }

  /* ── PAGES ── */
  .page { display: none; padding: 24px 28px; }
  .page.active { display: block; }

  /* ── KPI GRID ── */
  .kpi-grid {
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    gap: 12px;
    margin-bottom: 20px;
  }

  .kpi-card {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 16px;
    position: relative;
    overflow: hidden;
    transition: border-color 0.2s;
  }

  .kpi-card:hover { border-color: var(--border-accent); }

  .kpi-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: var(--orange);
    opacity: 0.5;
  }

  .kpi-label {
    font-size: 11px;
    color: var(--text-secondary);
    text-transform: uppercase;
    letter-spacing: 0.6px;
    font-weight: 600;
    margin-bottom: 8px;
  }

  .kpi-value {
    font-size: 22px;
    font-weight: 700;
    color: var(--text-primary);
    letter-spacing: -0.5px;
    line-height: 1;
    margin-bottom: 8px;
  }

  .kpi-value span {
    font-size: 13px;
    font-weight: 500;
    color: var(--text-secondary);
  }

  .kpi-deltas {
    display: flex;
    flex-direction: column;
    gap: 3px;
  }

  .kpi-delta {
    display: flex;
    align-items: center;
    gap: 5px;
    font-size: 11px;
    font-family: 'DM Mono', monospace;
  }

  .delta-up { color: var(--red); }
  .delta-down { color: var(--green); }
  .delta-neutral { color: var(--text-muted); }

  .delta-label {
    color: var(--text-muted);
    font-size: 10px;
    font-family: 'DM Sans', sans-serif;
  }

  /* ── MAIN GRID ── */
  .main-grid {
    display: grid;
    grid-template-columns: 1fr 340px;
    gap: 16px;
    margin-bottom: 16px;
  }

  .card {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 20px;
  }

  .card-title {
    font-size: 13px;
    font-weight: 600;
    color: var(--text-secondary);
    text-transform: uppercase;
    letter-spacing: 0.6px;
    margin-bottom: 16px;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .card-title-dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--orange);
  }

  /* ── NARRATIVE CARD ── */
  .narrative-bullets {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  .narrative-bullet {
    display: flex;
    gap: 12px;
    align-items: flex-start;
    padding: 12px;
    background: var(--bg-card2);
    border-radius: 8px;
    border-left: 3px solid var(--orange);
  }

  .narrative-bullet.green { border-left-color: var(--green); }
  .narrative-bullet.red { border-left-color: var(--red); }
  .narrative-bullet.yellow { border-left-color: var(--yellow); }

  .bullet-icon {
    font-size: 16px;
    flex-shrink: 0;
    margin-top: 1px;
  }

  .bullet-text {
    font-size: 13px;
    color: var(--text-primary);
    line-height: 1.5;
  }

  .bullet-text strong { color: var(--orange); font-weight: 600; }
  .narrative-bullet.green .bullet-text strong { color: var(--green); }
  .narrative-bullet.red .bullet-text strong { color: var(--red); }
  .narrative-bullet.yellow .bullet-text strong { color: var(--yellow); }

  /* ── CHART ── */
  .chart-wrap {
    position: relative;
    height: 280px;
  }

  /* ── BOTTOM GRID ── */
  .bottom-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
  }

  /* ── RANKING ── */
  .ranking-list {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .ranking-item {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 10px 12px;
    background: var(--bg-card2);
    border-radius: 8px;
    border: 1px solid var(--border);
    transition: border-color 0.15s;
  }

  .ranking-item:hover { border-color: var(--border-accent); }

  .rank-pos {
    font-size: 12px;
    font-weight: 700;
    color: var(--text-muted);
    font-family: 'DM Mono', monospace;
    width: 20px;
    text-align: center;
  }

  .rank-pos.top1 { color: var(--red); }
  .rank-pos.top2 { color: var(--orange); }
  .rank-pos.top3 { color: var(--yellow); }

  .rank-info { flex: 1; }

  .rank-name {
    font-size: 13px;
    font-weight: 600;
    color: var(--text-primary);
  }

  .rank-sub {
    font-size: 11px;
    color: var(--text-secondary);
    font-family: 'DM Mono', monospace;
  }

  .rank-bar-wrap {
    width: 80px;
  }

  .rank-bar-bg {
    height: 4px;
    background: var(--bg-base);
    border-radius: 2px;
    overflow: hidden;
    margin-bottom: 3px;
  }

  .rank-bar-fill {
    height: 100%;
    border-radius: 2px;
    background: var(--orange);
    transition: width 0.6s ease;
  }

  .rank-val {
    font-size: 11px;
    font-family: 'DM Mono', monospace;
    color: var(--text-secondary);
    text-align: right;
  }

  .badge-deterioration {
    display: inline-flex;
    align-items: center;
    gap: 3px;
    background: var(--red-dim);
    color: var(--red);
    border: 1px solid rgba(244,63,94,0.3);
    padding: 2px 7px;
    border-radius: 20px;
    font-size: 10px;
    font-weight: 600;
    letter-spacing: 0.3px;
  }

  /* ── TREND TABS ── */
  .trend-tabs {
    display: flex;
    gap: 4px;
    margin-bottom: 14px;
  }

  .trend-tab {
    padding: 5px 12px;
    border-radius: 6px;
    font-size: 12px;
    font-weight: 500;
    cursor: pointer;
    border: 1px solid var(--border);
    background: transparent;
    color: var(--text-secondary);
    transition: all 0.2s;
  }

  .trend-tab.active {
    background: var(--orange-dim);
    color: var(--orange);
    border-color: var(--border-accent);
  }

  /* ── ATYPICAL MONTH LEGEND ── */
  .atypical-legend {
    display: flex;
    align-items: center;
    gap: 16px;
    margin-top: 10px;
    padding-top: 10px;
    border-top: 1px solid var(--border);
  }

  .legend-item {
    display: flex;
    align-items: center;
    gap: 5px;
    font-size: 11px;
    color: var(--text-muted);
  }

  .legend-dot {
    width: 8px; height: 8px;
    border-radius: 50%;
  }

  /* ── PAGE ANALÍTICA ── */
  .analytic-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin-bottom: 16px;
  }

  .heatmap-wrap {
    overflow-x: auto;
  }

  .heatmap-table {
    width: 100%;
    border-collapse: separate;
    border-spacing: 3px;
    font-size: 11px;
  }

  .heatmap-table th {
    color: var(--text-muted);
    font-weight: 600;
    padding: 4px 6px;
    text-align: center;
    font-size: 10px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }

  .heatmap-table td {
    padding: 6px 8px;
    border-radius: 5px;
    text-align: center;
    font-family: 'DM Mono', monospace;
    font-size: 10px;
    cursor: default;
    transition: transform 0.1s;
  }

  .heatmap-table td:hover { transform: scale(1.05); }

  .hm-label {
    text-align: left !important;
    font-family: 'DM Sans', sans-serif !important;
    font-weight: 600 !important;
    color: var(--text-secondary) !important;
    background: transparent !important;
    font-size: 11px !important;
    padding-right: 12px !important;
    white-space: nowrap;
  }

  .hm-green { background: rgba(34,211,160,0.2); color: var(--green); }
  .hm-yellow { background: rgba(250,204,21,0.2); color: var(--yellow); }
  .hm-red { background: rgba(244,63,94,0.2); color: var(--red); }
  .hm-empty { background: var(--bg-card2); color: var(--text-muted); }

  /* ── TABLE ── */
  .detail-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 12px;
  }

  .detail-table th {
    padding: 8px 12px;
    text-align: left;
    color: var(--text-muted);
    font-size: 10px;
    text-transform: uppercase;
    letter-spacing: 0.6px;
    font-weight: 600;
    border-bottom: 1px solid var(--border);
  }

  .detail-table td {
    padding: 9px 12px;
    border-bottom: 1px solid var(--border);
    color: var(--text-primary);
  }

  .detail-table tr:hover td { background: var(--bg-hover); }

  .status-badge {
    display: inline-block;
    padding: 2px 8px;
    border-radius: 20px;
    font-size: 10px;
    font-weight: 600;
  }

  .status-green { background: var(--green-dim); color: var(--green); }
  .status-yellow { background: var(--yellow-dim); color: var(--yellow); }
  .status-red { background: var(--red-dim); color: var(--red); }

  /* ── SCOREBOARD ── */
  .scoreboard-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 8px;
  }

  .score-card {
    background: var(--bg-card2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 12px;
    text-align: center;
    transition: border-color 0.2s;
  }

  .score-card:hover { border-color: var(--border-accent); }

  .score-cre { font-size: 12px; font-weight: 700; color: var(--text-primary); margin-bottom: 6px; }
  .score-exec { font-size: 18px; font-weight: 700; margin-bottom: 2px; }
  .score-label { font-size: 10px; color: var(--text-muted); margin-bottom: 8px; }

  .score-status {
    display: inline-block;
    padding: 3px 10px;
    border-radius: 20px;
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 0.3px;
  }

  .score-ok { background: var(--green-dim); color: var(--green); border: 1px solid rgba(34,211,160,0.3); }
  .score-att { background: var(--yellow-dim); color: var(--yellow); border: 1px solid rgba(250,204,21,0.3); }
  .score-crit { background: var(--red-dim); color: var(--red); border: 1px solid rgba(244,63,94,0.3); }

  /* ── KPI ANALÍTICA ── */
  .kpi-mini-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    margin-bottom: 16px;
  }

  .kpi-mini {
    background: var(--bg-card2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 14px;
    text-align: center;
  }

  .kpi-mini-label { font-size: 10px; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.6px; margin-bottom: 6px; }
  .kpi-mini-val { font-size: 20px; font-weight: 700; color: var(--text-primary); }

  /* ── RESPONSIVE ── */
  @media (max-width: 1200px) {
    .kpi-grid { grid-template-columns: repeat(3, 1fr); }
    .main-grid { grid-template-columns: 1fr; }
    .scoreboard-grid { grid-template-columns: repeat(2, 1fr); }
  }

  /* ── SCROLLBAR ── */
  ::-webkit-scrollbar { width: 6px; height: 6px; }
  ::-webkit-scrollbar-track { background: var(--bg-base); }
  ::-webkit-scrollbar-thumb { background: var(--bg-hover); border-radius: 3px; }
</style>
</head>
<body>

<!-- TOPBAR -->
<div class="topbar">
  <div class="topbar-left">
    <div class="logo-mark">CL</div>
    <div>
      <div class="logo-text">Painel SME · COMLURB</div>
      <div class="logo-sub">Contrato de Limpeza Escolar</div>
    </div>
    <div style="width:1px;height:30px;background:var(--border);margin:0 8px;"></div>
    <div class="page-tabs">
      <button class="page-tab active" onclick="switchPage('executiva')">Executiva</button>
      <button class="page-tab" onclick="switchPage('analitica')">Analítica</button>
    </div>
  </div>
  <div class="topbar-right">
    <div class="theme-toggle">
      <button class="theme-btn active" onclick="setTheme('frio', this)">Navy Frio</button>
      <button class="theme-btn" onclick="setTheme('quente', this)">Navy Quente</button>
    </div>
    <div class="topbar-date" id="topbar-date"></div>
  </div>
</div>

<!-- FILTER BAR -->
<div class="filterbar">
  <span class="filter-label">Filtros</span>
  <select class="filter-select" id="f-ano" onchange="updateDash()">
    <option value="2025">2025</option>
    <option value="2024">2024</option>
  </select>
  <select class="filter-select" id="f-periodo" onchange="toggleMesFilter()">
    <option value="ytd">YTD</option>
    <option value="mes">Mês isolado</option>
  </select>
  <select class="filter-select" id="f-mes" style="display:none" onchange="updateDash()">
    <option value="1">Janeiro</option><option value="2">Fevereiro</option>
    <option value="3">Março</option><option value="4">Abril</option>
    <option value="5">Maio</option><option value="6">Junho</option>
    <option value="7">Julho</option><option value="8">Agosto</option>
    <option value="9">Setembro</option><option value="10">Outubro</option>
    <option value="11">Novembro</option><option value="12">Dezembro</option>
  </select>
  <div class="filter-divider"></div>
  <select class="filter-select" id="f-cre" onchange="updateDash()">
    <option value="all">Todas as CREs</option>
    <option value="1">CRE-01 · Campo Grande</option>
    <option value="2">CRE-02 · Santa Cruz</option>
    <option value="3">CRE-03 · Realengo</option>
    <option value="4">CRE-04 · Bangu</option>
    <option value="5">CRE-05 · Jacarepaguá</option>
    <option value="6">CRE-06 · Barra</option>
    <option value="7">CRE-07 · Ilha</option>
    <option value="8">CRE-08 · Méier</option>
    <option value="9">CRE-09 · Engenho Novo</option>
    <option value="10">CRE-10 · Tijuca</option>
    <option value="11">CRE-11 · Vila Isabel</option>
    <option value="12">CRE-12 · Penha</option>
    <option value="13">CRE-13 · Maré</option>
    <option value="14">CRE-14 · Pavuna</option>
    <option value="15">CRE-15 · Leopoldina</option>
    <option value="16">CRE-16 · Centro</option>
  </select>
  <div class="methodology-toggle">
    <button class="meth-btn active" onclick="setMeth('exec', this)" title="Média = Faltas ÷ (meses × 30)">Executivo ÷30</button>
    <button class="meth-btn" onclick="setMeth('tec', this)" title="Média = Faltas ÷ dias reais">Técnico real</button>
  </div>
</div>

<!-- ═══════════════════════════════ PÁGINA EXECUTIVA ═══════════════════════════════ -->
<div class="page active" id="page-executiva">

  <!-- KPI ROW -->
  <div class="kpi-grid">
    <div class="kpi-card">
      <div class="kpi-label">Contratado</div>
      <div class="kpi-value" id="kpi-contratado">R$ 0</div>
      <div class="kpi-deltas" style="color:var(--text-muted);font-size:11px;">Referência do período</div>
    </div>
    <div class="kpi-card">
      <div class="kpi-label">Fat. Operacional</div>
      <div class="kpi-value" id="kpi-fat">R$ 0</div>
      <div class="kpi-deltas" id="delta-fat"></div>
    </div>
    <div class="kpi-card">
      <div class="kpi-label">Glosa</div>
      <div class="kpi-value" id="kpi-glosa">R$ 0</div>
      <div class="kpi-deltas" id="delta-glosa"></div>
    </div>
    <div class="kpi-card">
      <div class="kpi-label">% Execução</div>
      <div class="kpi-value" id="kpi-exec">0%</div>
      <div class="kpi-deltas" id="delta-exec"></div>
    </div>
    <div class="kpi-card">
      <div class="kpi-label">Valor Dia (÷30)</div>
      <div class="kpi-value" id="kpi-valdia">R$ 0</div>
      <div class="kpi-deltas" id="delta-valdia"></div>
    </div>
    <div class="kpi-card">
      <div class="kpi-label">Média Faltas/dia</div>
      <div class="kpi-value" id="kpi-faltas">0</div>
      <div class="kpi-deltas" id="delta-faltas"></div>
    </div>
  </div>

  <!-- MAIN GRID -->
  <div class="main-grid">

    <!-- GRÁFICO PRINCIPAL -->
    <div class="card">
      <div class="card-title">
        <div class="card-title-dot"></div>
        Desempenho do Período
        <div class="trend-tabs" style="margin-left:auto;margin-bottom:0">
          <button class="trend-tab active" onclick="setTrend('fat', this)">Faturamento</button>
          <button class="trend-tab" onclick="setTrend('glosa', this)">Glosa</button>
          <button class="trend-tab" onclick="setTrend('faltas', this)">Faltas/dia</button>
        </div>
      </div>
      <div class="chart-wrap">
        <canvas id="mainChart"></canvas>
      </div>
      <div class="atypical-legend">
        <span style="font-size:11px;color:var(--text-muted);font-weight:600;">Meses atípicos:</span>
        <div class="legend-item"><div class="legend-dot" style="background:#6366F1"></div>Recesso</div>
        <div class="legend-item"><div class="legend-dot" style="background:#EC4899"></div>Carnaval</div>
        <div class="legend-item"><div class="legend-dot" style="background:#8B5CF6"></div>Julho</div>
      </div>
    </div>

    <!-- CARD NARRATIVO -->
    <div class="card">
      <div class="card-title">
        <div class="card-title-dot"></div>
        Situação do Contrato
      </div>
      <div class="narrative-bullets" id="narrative-bullets">
        <!-- preenchido por JS -->
      </div>
    </div>

  </div>

  <!-- BOTTOM GRID -->
  <div class="bottom-grid">

    <!-- RANKING POR GLOSA -->
    <div class="card">
      <div class="card-title">
        <div class="card-title-dot"></div>
        Top CREs · Glosa
      </div>
      <div class="ranking-list" id="ranking-glosa"></div>
    </div>

    <!-- RANKING POR FALTAS/DIA -->
    <div class="card">
      <div class="card-title">
        <div class="card-title-dot"></div>
        Top CREs · Média Faltas/dia
      </div>
      <div class="ranking-list" id="ranking-faltas"></div>
    </div>

  </div>
</div>

<!-- ═══════════════════════════════ PÁGINA ANALÍTICA ═══════════════════════════════ -->
<div class="page" id="page-analitica">

  <!-- KPIs ANALÍTICA -->
  <div class="kpi-mini-grid">
    <div class="kpi-mini">
      <div class="kpi-mini-label">% Unidades sem registro de faltas</div>
      <div class="kpi-mini-val" style="color:var(--green)" id="an-presenca">—</div>
    </div>
    <div class="kpi-mini">
      <div class="kpi-mini-label">Concentração Top-3 CREs na Glosa</div>
      <div class="kpi-mini-val" style="color:var(--orange)" id="an-concentracao">—</div>
    </div>
    <div class="kpi-mini">
      <div class="kpi-mini-label">CREs em situação crítica</div>
      <div class="kpi-mini-val" style="color:var(--red)" id="an-criticas">—</div>
    </div>
  </div>

  <div class="analytic-grid">

    <!-- HEATMAP -->
    <div class="card" style="grid-column: 1 / -1">
      <div class="card-title" style="justify-content:space-between">
        <span style="display:flex;align-items:center;gap:8px">
          <div class="card-title-dot"></div>
          Heatmap · Faltas/dia por CRE × Mês
        </span>
        <div style="display:flex;align-items:center;gap:8px">
          <span style="font-size:11px;color:var(--text-muted)">Threshold (faltas/dia):</span>
          <input type="number" id="threshold-verde" value="3" min="0" max="20" step="0.5"
            style="width:52px;background:var(--bg-base);border:1px solid var(--border);color:var(--text-primary);padding:4px 8px;border-radius:6px;font-size:12px;font-family:'DM Mono',monospace;text-align:center"
            onchange="buildHeatmap()" title="Limite verde/amarelo">
          <span style="font-size:11px;color:var(--text-muted)">/</span>
          <input type="number" id="threshold-amarelo" value="6" min="0" max="30" step="0.5"
            style="width:52px;background:var(--bg-base);border:1px solid var(--border);color:var(--text-primary);padding:4px 8px;border-radius:6px;font-size:12px;font-family:'DM Mono',monospace;text-align:center"
            onchange="buildHeatmap()" title="Limite amarelo/vermelho">
        </div>
      </div>
      <div class="heatmap-wrap">
        <table class="heatmap-table" id="heatmap-table"></table>
      </div>
    </div>

    <!-- GRÁFICO ANALÍTICO EMPILHADO -->
    <div class="card">
      <div class="card-title"><div class="card-title-dot"></div>Faturado + Glosa × Contratado</div>
      <div class="chart-wrap" style="height:240px">
        <canvas id="stackedChart"></canvas>
      </div>
    </div>

    <!-- RANKING COMPLETO -->
    <div class="card">
      <div class="card-title"><div class="card-title-dot"></div>Ranking Completo de CREs</div>
      <div style="overflow-y:auto;max-height:260px">
        <table class="detail-table" id="ranking-table"></table>
      </div>
    </div>

  </div>

  <!-- SCOREBOARD -->
  <div class="card" style="margin-bottom:16px">
    <div class="card-title"><div class="card-title-dot"></div>Scoreboard · Contratado × Executado por CRE</div>
    <div class="scoreboard-grid" id="scoreboard"></div>
  </div>

  <!-- TABELA DETALHADA -->
  <div class="card">
    <div class="card-title"><div class="card-title-dot"></div>Tabela Detalhada</div>
    <div style="overflow-x:auto">
      <table class="detail-table" id="detail-table"></table>
    </div>
  </div>

</div>

<script>
// ════════════════════════════════════════════════════════
// DADOS MOCK — estruturados como viriam do CSV
// ════════════════════════════════════════════════════════
const MESES_LABELS = ['Jan','Fev','Mar','Abr','Mai','Jun','Jul','Ago','Set','Out','Nov','Dez'];
const MESES_ATIPICOS = {1: 'recesso', 2: 'carnaval', 7: 'julho'};

const CRES = [
  'CRE-01','CRE-02','CRE-03','CRE-04','CRE-05','CRE-06',
  'CRE-07','CRE-08','CRE-09','CRE-10','CRE-11','CRE-12',
  'CRE-13','CRE-14','CRE-15','CRE-16'
];

// Postos por CRE (soma = 1854)
const POSTOS_CRE = [148,122,115,108,131,89,45,142,98,112,95,118,127,103,88,113];

// Fat.Operacional mensal por CRE (mock realista ~R$12.5M/mês total)
// [CRE][Mês 1..12]
const FAT_MENSAL = [
  [1280000,1150000,1290000,1275000,1285000,1270000,1100000,1280000,1275000,1290000,1280000,1285000],
  [1055000, 940000,1060000,1050000,1058000,1045000, 900000,1055000,1050000,1062000,1055000,1060000],
  [ 995000, 890000,1000000, 990000, 998000, 985000, 850000, 995000, 992000,1002000, 995000,1000000],
  [ 934000, 835000, 940000, 930000, 937000, 924000, 800000, 934000, 930000, 941000, 934000, 940000],
  [1134000,1015000,1138000,1125000,1133000,1119000, 965000,1132000,1128000,1140000,1134000,1138000],
  [ 771000, 690000, 775000, 767000, 772000, 763000, 658000, 770000, 766000, 776000, 771000, 775000],
  [ 390000, 349000, 391000, 387000, 390000, 386000, 333000, 389000, 387000, 392000, 390000, 391000],
  [1228000,1100000,1233000,1220000,1228000,1215000,1048000,1227000,1222000,1234000,1228000,1233000],
  [ 848000, 759000, 852000, 844000, 849000, 840000, 725000, 847000, 843000, 853000, 848000, 852000],
  [ 968000, 867000, 973000, 964000, 969000, 959000, 827000, 968000, 963000, 974000, 968000, 973000],
  [ 821000, 735000, 825000, 817000, 822000, 813000, 701000, 821000, 817000, 826000, 821000, 825000],
  [1021000, 913000,1026000,1016000,1022000,1011000, 872000,1020000,1016000,1027000,1021000,1026000],
  [1099000, 984000,1104000,1094000,1100000,1089000, 939000,1098000,1094000,1105000,1099000,1104000],
  [ 891000, 797000, 895000, 887000, 892000, 882000, 760000, 890000, 887000, 896000, 891000, 895000],
  [ 761000, 681000, 765000, 758000, 762000, 753000, 649000, 760000, 756000, 766000, 761000, 765000],
  [ 977000, 875000, 982000, 973000, 978000, 968000, 834000, 976000, 972000, 983000, 977000, 982000]
];

// Valor contratado mensal por CRE (ligeiramente acima do faturado)
const CONTRAT_CRE_MES = FAT_MENSAL.map(cre => cre.map(v => Math.round(v * 1.032)));

// Faltas reais por CRE por mês
const FALTAS_MENSAL = [
  [42,55,38,40,41,43,65,39,41,37,42,40],
  [35,46,32,34,33,36,54,33,34,31,35,33],
  [48,62,44,46,47,49,71,45,46,43,47,45],
  [29,38,26,27,28,30,44,27,28,25,29,27],
  [38,50,35,36,37,39,57,35,36,33,38,36],
  [22,29,20,21,21,23,33,20,21,19,22,20],
  [12,16,11,11,12,12,18,11,11,10,12,11],
  [55,72,51,53,54,57,83,52,53,49,55,52],
  [31,40,28,29,30,31,46,28,29,27,31,29],
  [44,57,40,42,43,45,66,41,42,38,44,41],
  [28,36,25,26,27,28,41,25,26,24,28,26],
  [63,82,58,60,61,64,94,59,61,56,63,59],
  [71,93,65,68,69,73,106,67,68,63,71,67],
  [39,51,36,37,38,40,58,36,37,34,39,36],
  [25,33,23,24,24,25,37,22,23,21,25,23],
  [41,53,37,39,40,42,61,38,39,36,41,38]
];

// Valor_Posto por CRE (÷30 = valor_dia)
const VALOR_POSTO = CONTRAT_CRE_MES.map((cre, i) => Math.round(cre[0] / POSTOS_CRE[i]));

// Badge deterioração (mock: CREs com piora 2+ meses consecutivos)
const BADGE_DETERI = [false, false, true, false, false, false, false, true, false, false, false, true, true, false, false, false];

// Dados período anterior (para delta)
const FAT_ANT = FAT_MENSAL.map(cre => cre.map(v => Math.round(v * (0.96 + Math.random()*0.06))));
const FALTAS_ANT = FALTAS_MENSAL.map(cre => cre.map(v => Math.round(v * (0.95 + Math.random()*0.1))));

// ════════════════════════════════════════════════════════
// STATE
// ════════════════════════════════════════════════════════
let currentMeth = 'exec';
let mainChartInst = null;
let stackedChartInst = null;
let currentTrend = 'fat';

// ════════════════════════════════════════════════════════
// HELPERS
// ════════════════════════════════════════════════════════
function fmtBRL(v) {
  if (v >= 1e6) return 'R$ ' + (v/1e6).toFixed(2).replace('.',',') + 'M';
  if (v >= 1e3) return 'R$ ' + (v/1e3).toFixed(0) + 'k';
  return 'R$ ' + v.toFixed(0);
}
function fmtPct(v) { return (v*100).toFixed(1).replace('.',',') + '%'; }
function fmtNum(v) { return v.toFixed(1).replace('.',','); }

function getDias(mes) {
  const d = [31,28,31,30,31,30,31,31,30,31,30,31];
  return d[mes-1];
}

function getFilters() {
  const periodo = document.getElementById('f-periodo').value;
  const mesRef = parseInt(document.getElementById('f-mes').value || 4);
  const creFilter = document.getElementById('f-cre').value;

  let meses = [];
  if (periodo === 'ytd') {
    for (let m = 1; m <= mesRef; m++) meses.push(m);
  } else {
    meses = [mesRef];
  }

  let cres = CRES.map((_, i) => i);
  if (creFilter !== 'all') cres = [parseInt(creFilter) - 1];

  return { meses, cres, periodo };
}

function calcKPIs(meses, cres) {
  let contratado = 0, fat = 0, faltas = 0, postosMeses = 0;
  let contratadoAnt = 0, fatAnt = 0, faltasAnt = 0;

  cres.forEach(ci => {
    meses.forEach(m => {
      contratado += CONTRAT_CRE_MES[ci][m-1];
      fat += FAT_MENSAL[ci][m-1];
      faltas += FALTAS_MENSAL[ci][m-1];
      postosMeses += POSTOS_CRE[ci] * 30;
      contratadoAnt += CONTRAT_CRE_MES[ci][m-1] * 0.98;
      fatAnt += FAT_ANT[ci][m-1];
      faltasAnt += FALTAS_ANT[ci][m-1];
    });
  });

  const glosa = contratado - fat;
  const glosaAnt = contratadoAnt - fatAnt;
  const execPct = fat / contratado;
  const execPctAnt = fatAnt / contratadoAnt;
  const valDia = fat / postosMeses;
  const valDiaAnt = fatAnt / (postosMeses * 0.98);

  let mediaFaltas;
  if (currentMeth === 'exec') {
    mediaFaltas = faltas / (meses.length * 30);
  } else {
    const diasReais = meses.reduce((a, m) => a + getDias(m), 0);
    mediaFaltas = faltas / diasReais;
  }
  let mediaFaltasAnt;
  if (currentMeth === 'exec') {
    mediaFaltasAnt = faltasAnt / (meses.length * 30);
  } else {
    const diasReais = meses.reduce((a, m) => a + getDias(m), 0);
    mediaFaltasAnt = faltasAnt / diasReais;
  }

  return { contratado, fat, glosa, execPct, valDia, mediaFaltas,
           fatAnt, glosaAnt, execPctAnt, valDiaAnt, mediaFaltasAnt, faltas };
}

function deltaHtml(curr, prev, invertido = false) {
  const d = ((curr - prev) / Math.abs(prev)) * 100;
  const bom = invertido ? d < 0 : d > 0;
  const cls = bom ? 'delta-up' : 'delta-down';
  const arrow = d > 0 ? '▲' : '▼';
  // invertido = quando subir é ruim (glosa, faltas)
  const color = invertido ? (d > 0 ? 'var(--red)' : 'var(--green)') : (d > 0 ? 'var(--green)' : 'var(--red)');
  return `<span style="color:${color};font-family:'DM Mono',monospace;font-size:11px">${arrow} ${Math.abs(d).toFixed(1)}%</span> <span class="delta-label">vs mês ant.</span>`;
}

// ════════════════════════════════════════════════════════
// UPDATE DASHBOARD
// ════════════════════════════════════════════════════════
function updateDash() {
  const { meses, cres } = getFilters();
  const kpi = calcKPIs(meses, cres);

  // KPIs
  document.getElementById('kpi-contratado').innerHTML = fmtBRL(kpi.contratado);
  document.getElementById('kpi-fat').innerHTML = fmtBRL(kpi.fat);
  document.getElementById('kpi-glosa').innerHTML = `<span style="color:var(--red)">${fmtBRL(kpi.glosa)}</span>`;
  document.getElementById('kpi-exec').innerHTML = `<span style="color:${kpi.execPct >= 0.97 ? 'var(--green)' : kpi.execPct >= 0.94 ? 'var(--yellow)' : 'var(--red)'}">${fmtPct(kpi.execPct)}</span>`;
  document.getElementById('kpi-valdia').innerHTML = `R$ ${kpi.valDia.toFixed(2).replace('.',',')}`;
  document.getElementById('kpi-faltas').innerHTML = fmtNum(kpi.mediaFaltas) + '<span>/dia</span>';

  // Deltas
  document.getElementById('delta-fat').innerHTML = deltaHtml(kpi.fat, kpi.fatAnt, false);
  document.getElementById('delta-glosa').innerHTML = deltaHtml(kpi.glosa, kpi.glosaAnt, true);
  document.getElementById('delta-exec').innerHTML = deltaHtml(kpi.execPct, kpi.execPctAnt, false);
  document.getElementById('delta-valdia').innerHTML = deltaHtml(kpi.valDia, kpi.valDiaAnt, false);
  document.getElementById('delta-faltas').innerHTML = deltaHtml(kpi.mediaFaltas, kpi.mediaFaltasAnt, true);

  buildNarrative(kpi, meses, cres);
  buildMainChart(meses, cres);
  buildRankings(meses, cres);
  buildAnalytica(meses, cres);
}

// ════════════════════════════════════════════════════════
// NARRATIVE
// ════════════════════════════════════════════════════════
function buildNarrative(kpi, meses, cres) {
  // Concentração de risco
  const glosaPorCre = cres.map(ci => {
    let g = 0;
    meses.forEach(m => { g += (CONTRAT_CRE_MES[ci][m-1] - FAT_MENSAL[ci][m-1]); });
    return { nome: CRES[ci], glosa: g };
  }).sort((a,b) => b.glosa - a.glosa);

  const top3 = glosaPorCre.slice(0,3);
  const top3sum = top3.reduce((a,b) => a + b.glosa, 0);
  const totalGlosa = glosaPorCre.reduce((a,b) => a + b.glosa, 0);
  const concPct = totalGlosa > 0 ? (top3sum / totalGlosa * 100).toFixed(0) : 0;

  const glosaOk = kpi.execPct >= 0.97;
  const faltasOk = kpi.mediaFaltas <= 3;

  const b1Type = parseFloat(concPct) >= 70 ? 'red' : parseFloat(concPct) >= 50 ? 'yellow' : 'green';
  const b2Type = glosaOk ? 'green' : kpi.execPct >= 0.94 ? 'yellow' : 'red';
  const b3Type = faltasOk ? 'green' : kpi.mediaFaltas <= 6 ? 'yellow' : 'red';

  const b1Icon = b1Type === 'red' ? '⚠️' : b1Type === 'yellow' ? '🔶' : '✅';
  const b2Icon = b2Type === 'green' ? '✅' : b2Type === 'yellow' ? '🔶' : '⚠️';
  const b3Icon = b3Type === 'green' ? '✅' : b3Type === 'yellow' ? '🔶' : '⚠️';

  const nomeTop3 = top3.map(c => c.nome).join(', ');

  document.getElementById('narrative-bullets').innerHTML = `
    <div class="narrative-bullet ${b1Type}">
      <div class="bullet-icon">${b1Icon}</div>
      <div class="bullet-text"><strong>${nomeTop3}</strong> concentram <strong>${concPct}%</strong> da glosa do período.</div>
    </div>
    <div class="narrative-bullet ${b2Type}">
      <div class="bullet-icon">${b2Icon}</div>
      <div class="bullet-text">Execução do contrato em <strong>${fmtPct(kpi.execPct)}</strong>. Glosa acumulada: <strong>${fmtBRL(kpi.glosa)}</strong>.</div>
    </div>
    <div class="narrative-bullet ${b3Type}">
      <div class="bullet-icon">${b3Icon}</div>
      <div class="bullet-text">Média de faltas/dia: <strong>${fmtNum(kpi.mediaFaltas)}</strong> ${currentMeth === 'exec' ? '(metodologia ÷30)' : '(dias reais)'}.</div>
    </div>
  `;
}

// ════════════════════════════════════════════════════════
// MAIN CHART
// ════════════════════════════════════════════════════════
function buildMainChart(meses, cres) {
  const labels = meses.map(m => {
    const at = MESES_ATIPICOS[m];
    return MESES_LABELS[m-1] + (at ? ' ◆' : '');
  });

  let contratData = [], fatData = [], glosaData = [], glosaRData = [];

  if (currentTrend === 'fat' || currentTrend === 'glosa') {
    meses.forEach(m => {
      let c = 0, f = 0;
      cres.forEach(ci => { c += CONTRAT_CRE_MES[ci][m-1]; f += FAT_MENSAL[ci][m-1]; });
      contratData.push(c);
      fatData.push(f);
      glosaData.push(c - f);
      glosaRData.push(((c-f)/c*100).toFixed(2));
    });
  } else {
    meses.forEach(m => {
      let faltas = 0;
      cres.forEach(ci => { faltas += FALTAS_MENSAL[ci][m-1]; });
      const divisor = currentMeth === 'exec' ? 30 : getDias(m);
      fatData.push(parseFloat((faltas / divisor).toFixed(2)));
    });
  }

  const ctx = document.getElementById('mainChart').getContext('2d');
  if (mainChartInst) mainChartInst.destroy();

  if (currentTrend === 'fat') {
    mainChartInst = new Chart(ctx, {
      data: {
        labels,
        datasets: [
          {
            type: 'bar', label: 'Contratado',
            data: contratData,
            backgroundColor: 'rgba(59,130,246,0.35)',
            borderColor: 'rgba(59,130,246,0.8)',
            borderWidth: 1, borderRadius: 4, yAxisID: 'y'
          },
          {
            type: 'bar', label: 'Fat. Operacional',
            data: fatData,
            backgroundColor: 'rgba(249,115,22,0.5)',
            borderColor: '#F97316',
            borderWidth: 1, borderRadius: 4, yAxisID: 'y'
          },
          {
            type: 'line', label: 'Glosa R$',
            data: glosaData,
            borderColor: '#F43F5E',
            backgroundColor: 'rgba(244,63,94,0.1)',
            borderWidth: 2, pointRadius: 4,
            fill: false, tension: 0.3, yAxisID: 'y2'
          }
        ]
      },
      options: chartOptions('R$', 'R$ Glosa')
    });
  } else if (currentTrend === 'glosa') {
    mainChartInst = new Chart(ctx, {
      data: {
        labels,
        datasets: [{
          type: 'bar', label: 'Glosa R$',
          data: glosaData,
          backgroundColor: 'rgba(244,63,94,0.4)',
          borderColor: '#F43F5E',
          borderWidth: 1, borderRadius: 4, yAxisID: 'y'
        },{
          type: 'line', label: 'Glosa %',
          data: glosaRData,
          borderColor: '#FACC15',
          borderWidth: 2, pointRadius: 4,
          fill: false, tension: 0.3, yAxisID: 'y2'
        }]
      },
      options: chartOptions('R$', '%')
    });
  } else {
    mainChartInst = new Chart(ctx, {
      data: {
        labels,
        datasets: [{
          type: 'line', label: 'Média Faltas/dia',
          data: fatData,
          borderColor: '#F97316',
          backgroundColor: 'rgba(249,115,22,0.1)',
          borderWidth: 2, pointRadius: 5,
          fill: true, tension: 0.3
        }]
      },
      options: {
        responsive: true, maintainAspectRatio: false,
        plugins: { legend: { labels: { color: '#7A8BAD', font: { family: 'DM Sans', size: 12 } } } },
        scales: {
          x: { ticks: { color: '#7A8BAD', font: { size: 11 } }, grid: { color: 'rgba(255,255,255,0.04)' } },
          y: { ticks: { color: '#7A8BAD', font: { size: 11 } }, grid: { color: 'rgba(255,255,255,0.04)' } }
        }
      }
    });
  }
}

function chartOptions(lY, lY2) {
  return {
    responsive: true, maintainAspectRatio: false,
    plugins: {
      legend: { labels: { color: '#7A8BAD', font: { family: 'DM Sans', size: 12 }, boxWidth: 12 } },
      tooltip: {
        callbacks: {
          label: function(ctx) {
            if (ctx.dataset.yAxisID === 'y2' && lY2 === '%') return ctx.dataset.label + ': ' + ctx.raw + '%';
            return ctx.dataset.label + ': ' + fmtBRL(ctx.raw);
          }
        }
      }
    },
    scales: {
      x: { ticks: { color: '#7A8BAD', font: { size: 11 } }, grid: { color: 'rgba(255,255,255,0.04)' } },
      y: {
        position: 'left',
        ticks: { color: '#7A8BAD', font: { size: 11 }, callback: v => fmtBRL(v) },
        grid: { color: 'rgba(255,255,255,0.04)' }
      },
      y2: {
        position: 'right',
        ticks: {
          color: '#F43F5E', font: { size: 11 },
          callback: v => lY2 === '%' ? v + '%' : fmtBRL(v)
        },
        grid: { drawOnChartArea: false }
      }
    }
  };
}

// ════════════════════════════════════════════════════════
// RANKINGS
// ════════════════════════════════════════════════════════
function buildRankings(meses, cres) {
  const data = cres.map(ci => {
    let glosa = 0, faltas = 0;
    meses.forEach(m => {
      glosa += (CONTRAT_CRE_MES[ci][m-1] - FAT_MENSAL[ci][m-1]);
      faltas += FALTAS_MENSAL[ci][m-1];
    });
    const mediaFaltas = currentMeth === 'exec'
      ? faltas / (meses.length * 30)
      : faltas / meses.reduce((a, m) => a + getDias(m), 0);
    return { i: ci, nome: CRES[ci], glosa, mediaFaltas };
  });

  const byGlosa = [...data].sort((a,b) => b.glosa - a.glosa).slice(0,6);
  const byFaltas = [...data].sort((a,b) => b.mediaFaltas - a.mediaFaltas).slice(0,6);
  const maxGlosa = byGlosa[0]?.glosa || 1;
  const maxFaltas = byFaltas[0]?.mediaFaltas || 1;

  const posCls = ['top1','top2','top3','','',''];

  document.getElementById('ranking-glosa').innerHTML = byGlosa.map((d, idx) => `
    <div class="ranking-item">
      <div class="rank-pos ${posCls[idx]}">${idx+1}</div>
      <div class="rank-info">
        <div class="rank-name">${d.nome} ${BADGE_DETERI[d.i] ? '<span class="badge-deterioration">↑ Deterioração</span>' : ''}</div>
        <div class="rank-sub">${fmtBRL(d.glosa)}</div>
      </div>
      <div class="rank-bar-wrap">
        <div class="rank-bar-bg"><div class="rank-bar-fill" style="width:${(d.glosa/maxGlosa*100).toFixed(0)}%"></div></div>
        <div class="rank-val">${fmtPct(d.glosa / (d.glosa + byGlosa.reduce((a,x)=>a+x.glosa,0) - d.glosa + 0.01))}</div>
      </div>
    </div>
  `).join('');

  document.getElementById('ranking-faltas').innerHTML = byFaltas.map((d, idx) => `
    <div class="ranking-item">
      <div class="rank-pos ${posCls[idx]}">${idx+1}</div>
      <div class="rank-info">
        <div class="rank-name">${d.nome} ${BADGE_DETERI[d.i] ? '<span class="badge-deterioration">↑ Deterioração</span>' : ''}</div>
        <div class="rank-sub">${fmtNum(d.mediaFaltas)} faltas/dia</div>
      </div>
      <div class="rank-bar-wrap">
        <div class="rank-bar-bg"><div class="rank-bar-fill" style="width:${(d.mediaFaltas/maxFaltas*100).toFixed(0)}%"></div></div>
        <div class="rank-val">${fmtNum(d.mediaFaltas)}</div>
      </div>
    </div>
  `).join('');
}

// ════════════════════════════════════════════════════════
// HEATMAP
// ════════════════════════════════════════════════════════
function buildHeatmap() {
  const thV = parseFloat(document.getElementById('threshold-verde').value) || 3;
  const thA = parseFloat(document.getElementById('threshold-amarelo').value) || 6;
  const { meses, cres } = getFilters();

  const thead = `<thead><tr><th class="hm-label">CRE</th>${meses.map(m => `<th>${MESES_LABELS[m-1]}${MESES_ATIPICOS[m]?'◆':''}</th>`).join('')}</tr></thead>`;

  const tbody = '<tbody>' + cres.map(ci => {
    const cells = meses.map(m => {
      const divisor = currentMeth === 'exec' ? 30 : getDias(m);
      const val = FALTAS_MENSAL[ci][m-1] / divisor;
      const cls = val <= thV ? 'hm-green' : val <= thA ? 'hm-yellow' : 'hm-red';
      return `<td class="${cls}" title="${CRES[ci]} · ${MESES_LABELS[m-1]}: ${val.toFixed(1)} f/dia">${val.toFixed(1)}</td>`;
    }).join('');
    return `<tr><td class="hm-label">${CRES[ci]}</td>${cells}</tr>`;
  }).join('') + '</tbody>';

  document.getElementById('heatmap-table').innerHTML = thead + tbody;
}

// ════════════════════════════════════════════════════════
// ANALÍTICA
// ════════════════════════════════════════════════════════
function buildAnalytica(meses, cres) {
  buildHeatmap();

  // KPIs analítica
  const thV = parseFloat(document.getElementById('threshold-verde')?.value) || 3;
  let semFaltas = 0;
  cres.forEach(ci => {
    const totalFaltas = meses.reduce((a, m) => a + FALTAS_MENSAL[ci][m-1], 0);
    if (totalFaltas === 0) semFaltas++;
  });
  document.getElementById('an-presenca').textContent = fmtPct(semFaltas / cres.length);

  const glosasCre = cres.map(ci => {
    return meses.reduce((a, m) => a + (CONTRAT_CRE_MES[ci][m-1] - FAT_MENSAL[ci][m-1]), 0);
  }).sort((a,b) => b-a);
  const totalGlosa = glosasCre.reduce((a,b)=>a+b,0);
  const top3conc = glosasCre.slice(0,3).reduce((a,b)=>a+b,0);
  document.getElementById('an-concentracao').textContent = totalGlosa > 0 ? fmtPct(top3conc/totalGlosa) : '—';

  let criticas = 0;
  cres.forEach(ci => {
    const fat = meses.reduce((a,m)=>a+FAT_MENSAL[ci][m-1],0);
    const cont = meses.reduce((a,m)=>a+CONTRAT_CRE_MES[ci][m-1],0);
    if (fat/cont < 0.94) criticas++;
  });
  document.getElementById('an-criticas').textContent = criticas;

  // Stacked chart
  const stackLabels = meses.map(m => MESES_LABELS[m-1]);
  const stackFat = [], stackGlosa = [], stackCont = [];
  meses.forEach(m => {
    let f=0, c=0;
    cres.forEach(ci => { f += FAT_MENSAL[ci][m-1]; c += CONTRAT_CRE_MES[ci][m-1]; });
    stackFat.push(f); stackGlosa.push(c-f); stackCont.push(c);
  });

  const ctx2 = document.getElementById('stackedChart').getContext('2d');
  if (stackedChartInst) stackedChartInst.destroy();
  stackedChartInst = new Chart(ctx2, {
    data: {
      labels: stackLabels,
      datasets: [
        { type:'bar', label:'Fat. Operacional', data:stackFat, backgroundColor:'rgba(249,115,22,0.6)', borderColor:'#F97316', borderWidth:1, borderRadius:3, stack:'s' },
        { type:'bar', label:'Glosa', data:stackGlosa, backgroundColor:'rgba(244,63,94,0.4)', borderColor:'#F43F5E', borderWidth:1, borderRadius:3, stack:'s' },
        { type:'line', label:'Contratado', data:stackCont, borderColor:'rgba(59,130,246,0.8)', borderWidth:2, pointRadius:3, fill:false, tension:0.3 }
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: false,
      plugins: { legend: { labels: { color:'#7A8BAD', font:{family:'DM Sans',size:11}, boxWidth:12 } } },
      scales: {
        x: { ticks:{color:'#7A8BAD',font:{size:10}}, grid:{color:'rgba(255,255,255,0.04)'}, stacked:true },
        y: { ticks:{color:'#7A8BAD',font:{size:10},callback:v=>fmtBRL(v)}, grid:{color:'rgba(255,255,255,0.04)'}, stacked:true }
      }
    }
  });

  // Ranking table completo
  const rankData = cres.map(ci => {
    const fat = meses.reduce((a,m)=>a+FAT_MENSAL[ci][m-1],0);
    const cont = meses.reduce((a,m)=>a+CONTRAT_CRE_MES[ci][m-1],0);
    const glosa = cont - fat;
    const faltas = meses.reduce((a,m)=>a+FALTAS_MENSAL[ci][m-1],0);
    const mf = currentMeth === 'exec' ? faltas/(meses.length*30) : faltas/meses.reduce((a,m)=>a+getDias(m),0);
    const exec = fat/cont;
    const status = exec >= 0.97 ? 'green' : exec >= 0.94 ? 'yellow' : 'red';
    const statusLabel = exec >= 0.97 ? 'Controlada' : exec >= 0.94 ? 'Atenção' : 'Crítica';
    return { nome: CRES[ci], fat, glosa, exec, mf, status, statusLabel };
  }).sort((a,b) => b.glosa - a.glosa);

  document.getElementById('ranking-table').innerHTML = `
    <thead><tr>
      <th>CRE</th><th>Fat. Op.</th><th>Glosa</th><th>% Exec.</th><th>F/dia</th><th>Status</th>
    </tr></thead>
    <tbody>
      ${rankData.map(d => `<tr>
        <td style="font-weight:600">${d.nome}</td>
        <td>${fmtBRL(d.fat)}</td>
        <td style="color:var(--red)">${fmtBRL(d.glosa)}</td>
        <td style="color:${d.exec>=0.97?'var(--green)':d.exec>=0.94?'var(--yellow)':'var(--red)'}">${fmtPct(d.exec)}</td>
        <td>${fmtNum(d.mf)}</td>
        <td><span class="status-badge status-${d.status}">${d.statusLabel}</span></td>
      </tr>`).join('')}
    </tbody>
  `;

  // Scoreboard
  document.getElementById('scoreboard').innerHTML = cres.map(ci => {
    const fat = meses.reduce((a,m)=>a+FAT_MENSAL[ci][m-1],0);
    const cont = meses.reduce((a,m)=>a+CONTRAT_CRE_MES[ci][m-1],0);
    const exec = fat/cont;
    const cls = exec >= 0.97 ? 'score-ok' : exec >= 0.94 ? 'score-att' : 'score-crit';
    const label = exec >= 0.97 ? 'Controlada' : exec >= 0.94 ? 'Atenção' : 'Crítica';
    const color = exec >= 0.97 ? 'var(--green)' : exec >= 0.94 ? 'var(--yellow)' : 'var(--red)';
    return `<div class="score-card">
      <div class="score-cre">${CRES[ci]}</div>
      <div class="score-exec" style="color:${color}">${fmtPct(exec)}</div>
      <div class="score-label">execução</div>
      <span class="score-status ${cls}">${label}</span>
    </div>`;
  }).join('');

  // Tabela detalhada
  document.getElementById('detail-table').innerHTML = `
    <thead><tr>
      <th>CRE</th><th>Contratado</th><th>Fat. Op.</th><th>Glosa R$</th><th>Glosa %</th><th>Média F/dia</th><th>Postos</th><th>Valor Dia</th><th>Status</th>
    </tr></thead>
    <tbody>
      ${cres.map(ci => {
        const fat = meses.reduce((a,m)=>a+FAT_MENSAL[ci][m-1],0);
        const cont = meses.reduce((a,m)=>a+CONTRAT_CRE_MES[ci][m-1],0);
        const glosa = cont-fat;
        const exec = fat/cont;
        const faltas = meses.reduce((a,m)=>a+FALTAS_MENSAL[ci][m-1],0);
        const mf = currentMeth==='exec' ? faltas/(meses.length*30) : faltas/meses.reduce((a,m)=>a+getDias(m),0);
        const status = exec>=0.97?'green':exec>=0.94?'yellow':'red';
        const statusLabel = exec>=0.97?'Controlada':exec>=0.94?'Atenção':'Crítica';
        const vd = fat/(POSTOS_CRE[ci]*meses.length*30);
        return `<tr>
          <td style="font-weight:700">${CRES[ci]}</td>
          <td>${fmtBRL(cont)}</td>
          <td>${fmtBRL(fat)}</td>
          <td style="color:var(--red)">${fmtBRL(glosa)}</td>
          <td style="color:${exec>=0.97?'var(--green)':exec>=0.94?'var(--yellow)':'var(--red)'}">${fmtPct(glosa/cont)}</td>
          <td>${fmtNum(mf)}</td>
          <td style="color:var(--text-secondary)">${POSTOS_CRE[ci]}</td>
          <td style="font-family:'DM Mono',monospace;font-size:11px">R$ ${vd.toFixed(2).replace('.',',')}</td>
          <td><span class="status-badge status-${status}">${statusLabel}</span></td>
        </tr>`;
      }).join('')}
    </tbody>
  `;
}

// ════════════════════════════════════════════════════════
// UI CONTROLS
// ════════════════════════════════════════════════════════
function switchPage(page) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.page-tab').forEach(t => t.classList.remove('active'));
  document.getElementById('page-' + page).classList.add('active');
  event.target.classList.add('active');
  if (page === 'analitica') {
    setTimeout(() => { const {meses, cres} = getFilters(); buildAnalytica(meses, cres); }, 50);
  }
}

function toggleMesFilter() {
  const v = document.getElementById('f-periodo').value;
  document.getElementById('f-mes').style.display = v === 'mes' ? 'block' : 'none';
  updateDash();
}

function setTheme(t, el) {
  document.querySelectorAll('.theme-btn').forEach(b => b.classList.remove('active'));
  el.classList.add('active');
  document.body.classList.toggle('navy-quente', t === 'quente');
}

function setMeth(m, el) {
  document.querySelectorAll('.meth-btn').forEach(b => b.classList.remove('active'));
  el.classList.add('active');
  currentMeth = m;
  updateDash();
}

function setTrend(t, el) {
  document.querySelectorAll('.trend-tab').forEach(b => b.classList.remove('active'));
  el.classList.add('active');
  currentTrend = t;
  const { meses, cres } = getFilters();
  buildMainChart(meses, cres);
}

// Date
document.getElementById('topbar-date').textContent =
  new Date().toLocaleDateString('pt-BR', { day:'2-digit', month:'short', year:'numeric' });

// Init
updateDash();
</script>
</body>
</html>
