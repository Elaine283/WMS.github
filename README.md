<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1.0"/>
<title>專題報告 — 智慧倉儲管理系統 WMS | Elaine</title>
<meta name="description" content="智慧倉儲管理系統專題報告 — 完整產品簡報"/>

<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

<style>
/* ═══════════════════════════════════════════
      ELAINE PROJECT REPORT — STYLE SYSTEM (Dark Minimal Pro)
   ═══════════════════════════════════════════ */
:root {
  --bg-base:      #0A0A0C;
  --bg-1:         #111114;
  --bg-2:         #18181C;
  --bg-3:         #212126;
  --bg-4:         #2C2C33;
  --border:       rgba(255,255,255,0.07);
  --border-soft:  rgba(255,255,255,0.04);
  --border-hover: rgba(255,255,255,0.14);

  --text-1: #F2F2F5;
  --text-2: #A8A8B8;
  --text-3: #6A6A7A;
  --text-4: #3A3A4A;

  --accent:       #7C6AF5;
  --accent-dim:   rgba(124,106,245,0.15);
  --accent-glow:  rgba(124,106,245,0.35);
  --gold:         #C9A84C;
  --gold-dim:     rgba(201,168,76,0.12);
  --teal:         #3ECFCF;
  --teal-dim:     rgba(62,207,207,0.12);
  --danger:       #F05A7A;
  --danger-dim:   rgba(240,90,122,0.12);
  --success:      #4ECDC4;
  --warning:      #FFD166;

  --sidebar-w:    260px;
  --topbar-h:     60px;

  --r-sm: 8px;
  --r-md: 12px;
  --r-lg: 18px;
  --r-xl: 24px;

  --shadow-sm:  0 2px 8px rgba(0,0,0,0.4);
  --shadow-md:  0 6px 24px rgba(0,0,0,0.5);
  --shadow-lg:  0 16px 48px rgba(0,0,0,0.6);
  --shadow-accent: 0 0 40px rgba(124,106,245,0.2);
}

*, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }
html { font-size:16px; scroll-behavior:smooth; }
body {
  font-family: 'Instrument Sans', "PingFang TC", "Microsoft JhengHei", sans-serif;
  background: var(--bg-base);
  color: var(--text-1);
  overflow-x: hidden;
  -webkit-font-smoothing: antialiased;
  line-height: 1.6;
}
::-webkit-scrollbar { width: 4px; height: 4px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--bg-4); border-radius: 2px; }
::-webkit-scrollbar-thumb:hover { background: var(--accent); }
::selection { background: var(--accent-dim); color: var(--text-1); }

.app-shell {
  display: grid;
  grid-template-columns: var(--sidebar-w) 1fr;
  grid-template-rows: auto;
  min-height: 100vh;
}

/* ─── SIDEBAR ────────────────────────────── */
.sidebar {
  position: fixed;
  left: 0; top: 0; bottom: 0;
  width: var(--sidebar-w);
  background: var(--bg-1);
  border-right: 1px solid var(--border);
  display: flex;
  flex-direction: column;
  z-index: 100;
  transition: transform 0.3s cubic-bezier(.4,0,.2,1);
  overflow: hidden;
}
.sidebar::before {
  content: '';
  position: absolute;
  top: -80px; left: -80px;
  width: 220px; height: 220px;
  background: radial-gradient(circle, rgba(124,106,245,0.08), transparent 70%);
  pointer-events: none;
}
.sidebar-logo {
  padding: 24px 24px 20px;
  display: flex;
  align-items: center;
  gap: 12px;
  border-bottom: 1px solid var(--border-soft);
}
.logo-mark {
  width: 32px; height: 32px;
  background: linear-gradient(135deg, var(--accent), #5B8FF9);
  border-radius: var(--r-sm);
  display: flex; align-items: center; justify-content: center;
  font-family: 'Syne', sans-serif;
  font-weight: 800; font-size: .85rem;
  color: white;
  flex-shrink: 0;
  box-shadow: 0 4px 16px rgba(124,106,245,0.4);
}
.logo-text {
  font-family: 'Syne', sans-serif;
  font-weight: 700; font-size: .95rem;
  color: var(--text-1);
  letter-spacing: -.01em;
  line-height: 1.2;
}
.logo-sub {
  font-size: .65rem;
  color: var(--text-3);
  letter-spacing: .06em;
  text-transform: uppercase;
}
.sidebar-section-label {
  padding: 20px 20px 6px;
  font-size: .65rem;
  font-family: 'JetBrains Mono', monospace;
  color: var(--text-4);
  letter-spacing: .1em;
  text-transform: uppercase;
}
.nav-item {
  display: flex;
  align-items: center;
  gap: 11px;
  padding: 10px 20px;
  margin: 1px 10px;
  border-radius: var(--r-md);
  cursor: pointer;
  transition: all .2s cubic-bezier(.4,0,.2,1);
  text-decoration: none;
  color: var(--text-2);
  font-size: .875rem;
  font-weight: 500;
  position: relative;
  white-space: nowrap;
}
.nav-item:hover { background: var(--bg-2); color: var(--text-1); }
.nav-item.active {
  background: var(--accent-dim);
  color: var(--accent);
  border: 1px solid rgba(124,106,245,0.2);
}
.nav-item.active::before {
  content: ''; position: absolute; left: -10px; top: 50%;
  transform: translateY(-50%); width: 3px; height: 18px;
  background: var(--accent); border-radius: 2px;
}
.nav-icon { width: 16px; height: 16px; opacity: .7; flex-shrink: 0; }
.nav-item.active .nav-icon { opacity: 1; }
.nav-badge {
  margin-left: auto; background: var(--accent-dim); color: var(--accent);
  font-size: .62rem; font-family: 'JetBrains Mono', monospace;
  padding: 2px 7px; border-radius: 50px; border: 1px solid rgba(124,106,245,0.2);
}
.sidebar-progress { margin: auto 0 0; padding: 16px 20px 24px; border-top: 1px solid var(--border-soft); }
.sidebar-progress-label { display: flex; justify-content: space-between; font-size: .72rem; color: var(--text-3); margin-bottom: 8px; }
.progress-track { height: 3px; background: var(--bg-3); border-radius: 2px; overflow: hidden; }
.progress-fill { height: 100%; background: linear-gradient(90deg, var(--accent), #5B8FF9); border-radius: 2px; transition: width 1.5s cubic-bezier(.4,0,.2,1); width: 0; }

/* ─── MAIN AREA ──────────────────────────── */
.main { margin-left: var(--sidebar-w); min-height: 100vh; display: flex; flex-direction: column; }
.topbar {
  height: var(--topbar-h); background: rgba(10,10,12,0.85); backdrop-filter: blur(20px);
  border-bottom: 1px solid var(--border); display: flex; align-items: center; padding: 0 32px; gap: 16px; position: sticky; top: 0; z-index: 90;
}
.topbar-breadcrumb { display: flex; align-items: center; gap: 6px; font-size: .78rem; color: var(--text-3); }
.topbar-breadcrumb span { color: var(--text-2); }
.topbar-divider { color: var(--text-4); }
#breadcrumb-current { color: var(--accent); font-weight: 600; }
.topbar-right { margin-left: auto; display: flex; align-items: center; gap: 12px; }
.topbar-tag { padding: 4px 12px; background: var(--bg-2); border: 1px solid var(--border); border-radius: 50px; font-size: .72rem; color: var(--text-3); font-family: 'JetBrains Mono', monospace; }
.topbar-tag.live { background: rgba(62,207,207,0.08); border-color: rgba(62,207,207,0.2); color: var(--teal); display: flex; align-items: center; gap: 5px; }
.live-dot { width: 5px; height: 5px; border-radius: 50%; background: var(--teal); position: relative; }
.live-dot::after { content: ''; position: absolute; inset: -3px; border-radius: 50%; border: 1px solid var(--teal); animation: pulse 2s ease-out infinite; }
@keyframes pulse { 0% { transform: scale(1); opacity: .6; } 100% { transform: scale(2); opacity: 0; } }
.topbar-hamburger { display: none; background: none; border: none; cursor: pointer; color: var(--text-2); padding: 4px; }

.page-content { padding: 40px 32px; flex: 1; }
.section { display: none; }
.section.active { display: block; }

/* ─── COMPONENTS ─────────────────────────── */
.page-header { margin-bottom: 36px; }
.page-header-eyebrow { font-family: 'JetBrains Mono', monospace; font-size: .68rem; color: var(--accent); letter-spacing: .14em; text-transform: uppercase; margin-bottom: 8px; display: flex; align-items: center; gap: 8px; }
.page-header-eyebrow::before { content: ''; display: block; width: 20px; height: 1px; background: var(--accent); }
.page-title { font-family: 'Syne', sans-serif; font-size: clamp(1.8rem, 3vw, 2.6rem); font-weight: 800; color: var(--text-1); letter-spacing: -.03em; line-height: 1.1; margin-bottom: 12px; }
.page-desc { font-size: .92rem; color: var(--text-2); max-width: 600px; line-height: 1.75; }

.grid-3 { display: grid; grid-template-columns: repeat(3,1fr); gap: 16px; }
.grid-2 { display: grid; grid-template-columns: repeat(2,1fr); gap: 20px; }
.grid-4 { display: grid; grid-template-columns: repeat(4,1fr); gap: 14px; }

.card { background: var(--bg-1); border: 1px solid var(--border); border-radius: var(--r-lg); padding: 24px; transition: border-color .25s, transform .25s, box-shadow .25s; }
.card:hover { border-color: var(--border-hover); box-shadow: var(--shadow-md); }
.card-glass { background: rgba(24,24,28,0.7); backdrop-filter: blur(20px); }
.card-accent { border-color: rgba(124,106,245,0.25); background: linear-gradient(145deg, rgba(124,106,245,0.06), var(--bg-1)); }

.hero-card { background: linear-gradient(135deg, rgba(124,106,245,.12), rgba(91,143,249,.06)); border: 1px solid rgba(124,106,245,.2); border-radius: var(--r-xl); padding: 36px 40px; position: relative; overflow: hidden; margin-bottom: 28px; }
.hero-card::before { content: ''; position: absolute; top: -60px; right: -60px; width: 280px; height: 280px; background: radial-gradient(circle, rgba(124,106,245,.12), transparent 70%); pointer-events: none; }
.hero-card::after { content: ''; position: absolute; bottom: -40px; left: -40px; width: 200px; height: 200px; background: radial-gradient(circle, rgba(91,143,249,.08), transparent 70%); pointer-events: none; }
.hero-title { font-family: 'Syne', sans-serif; font-size: clamp(1.5rem, 3vw, 2.4rem); font-weight: 800; color: var(--text-1); letter-spacing: -.03em; line-height: 1.1; margin-bottom: 12px; }
.hero-title em { font-style: normal; background: linear-gradient(135deg, var(--accent), #5B8FF9); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
.hero-desc { font-size: .92rem; color: var(--text-2); line-height: 1.75; max-width: 540px; margin-bottom: 24px; }
.hero-meta { display: flex; align-items: center; gap: 16px; flex-wrap: wrap; }
.hero-stat { display: flex; align-items: center; gap: 6px; font-size: .78rem; color: var(--text-3); }
.hero-stat strong { color: var(--text-1); font-weight: 600; }

/* 💡 核心對接面板：院二流三甲 + 流通科技管理資訊欄 */
.hero-info-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
  gap: 12px;
  background: var(--bg-base);
  padding: 16px;
  border-radius: var(--r-md);
  border: 1px solid var(--border);
  margin-bottom: 20px;
}
.hero-info-item { display: flex; flex-direction: column; }
.hero-info-item .label { font-size: .68rem; font-family: 'JetBrains Mono', monospace; color: var(--text-3); text-transform: uppercase; margin-bottom: 4px; font-weight: bold; }
.hero-info-item .value { font-size: .88rem; color: var(--text-1); font-weight: 600; }

.kpi-card { padding: 20px 22px; display: flex; flex-direction: column; gap: 4px; position: relative; overflow: hidden; }
.kpi-card::after { content: ''; position: absolute; bottom: 0; left: 0; right: 0; height: 2px; }
.kpi-card.k-accent::after { background: linear-gradient(90deg, var(--accent), transparent); }
.kpi-card.k-gold::after   { background: linear-gradient(90deg, var(--gold), transparent); }
.kpi-card.k-teal::after   { background: linear-gradient(90deg, var(--teal), transparent); }
.kpi-card.k-danger::after { background: linear-gradient(90deg, var(--danger), transparent); }

.kpi-value { font-family: 'Syne', sans-serif; font-size: 2.2rem; font-weight: 800; letter-spacing: -.04em; color: var(--text-1); line-height: 1; }
.kpi-value.v-accent { color: var(--accent); }
.kpi-value.v-gold   { color: var(--gold); }
.kpi-value.v-teal   { color: var(--teal); }
.kpi-value.v-danger { color: var(--danger); }

.data-table { width: 100%; border-collapse: collapse; margin-top: 10px; }
.data-table th { padding: 8px 12px; text-align: left; font-size: .65rem; font-family: 'JetBrains Mono', monospace; color: var(--text-3); letter-spacing: .1em; text-transform: uppercase; border-bottom: 1px solid var(--border); }
.data-table td { padding: 12px 12px; font-size: .82rem; color: var(--text-2); border-bottom: 1px solid var(--border-soft); vertical-align: middle; }
.data-table tr:last-child td { border-bottom: none; }
.data-table tbody tr:hover td { background: rgba(255,255,255,.02); }

.timeline-item { display: flex; gap: 16px; padding: 16px 0; border-bottom: 1px solid var(--border-soft); position: relative; }
.timeline-dot { width: 10px; height: 10px; border-radius: 50%; flex-shrink: 0; margin-top: 5px; position: relative; }
.timeline-dot::before { content: ''; position: absolute; top: 10px; left: 50%; transform: translateX(-50%); width: 1px; height: calc(100% + 16px); background: var(--border); }
.timeline-item:last-child .timeline-dot::before { display: none; }

.formula-box { background: var(--bg-base); padding: 18px; border-radius: var(--r-md); border: 1px dashed var(--border-hover); margin: 15px 0; text-align: center; }
.chart-wrap { position: relative; height: 230px; width: 100%; }
.icon-box { width: 40px; height: 40px; border-radius: var(--r-md); display: flex; align-items: center; justify-content: center; font-size: 1rem; flex-shrink: 0; }
.chip { display: inline-flex; align-items: center; gap: 5px; padding: 5px 12px; background: var(--bg-2); border: 1px solid var(--border); border-radius: 50px; font-size: .75rem; color: var(--text-2); margin-right: 4px; margin-bottom: 6px; }

.risk-bar { display: flex; flex-direction: column; gap: 10px; }
.risk-row { display: flex; align-items: center; gap: 10px; }
.risk-label { font-size: .8rem; color: var(--text-2); min-width: 120px; }
.risk-track { flex: 1; height: 5px; background: var(--bg-3); border-radius: 3px; overflow: hidden; }
.risk-fill { height: 100%; border-radius: 3px; transition: width 1.2s cubic-bezier(.4,0,.2,1); width: 0; }
.risk-pct { font-size: .72rem; font-family: 'JetBrains Mono', monospace; color: var(--text-3); min-width: 32px; text-align: right; }

.gantt { display: flex; flex-direction: column; gap: 10px; }
.gantt-row { display: flex; align-items: center; gap: 12px; }
.gantt-label { font-size: .78rem; color: var(--text-2); width: 120px; flex-shrink: 0; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.gantt-track { flex: 1; height: 20px; background: var(--bg-3); border-radius: 4px; position: relative; overflow: hidden; }
.gantt-fill { position: absolute; top: 0; height: 100%; border-radius: 4px; transition: width 1.4s cubic-bezier(.4,0,.2,1), left .8s; width: 0; }
.gantt-fill-done { background: linear-gradient(90deg, var(--accent), rgba(124,106,245,.6)); }
.gantt-fill-wip  { background: linear-gradient(90deg, var(--gold), rgba(201,168,76,.6)); }
.gantt-fill-todo { background: linear-gradient(90deg, var(--bg-4), var(--bg-3)); }

/* ─── RESPONSIVE ─────────────────────────── */
@media (max-width: 1100px) { .grid-4 { grid-template-columns: repeat(2,1fr); } }
@media (max-width: 900px) {
  :root { --sidebar-w: 0px; }
  .sidebar { transform: translateX(-260px); --sidebar-w: 260px; }
  .sidebar.open { transform: translateX(0); }
  .main { margin-left: 0; }
  .topbar-hamburger { display: flex; align-items: center; }
  .grid-3 { grid-template-columns: repeat(2,1fr); }
  .grid-2 { grid-template-columns: 1fr; }
}
@media (max-width: 600px) { .page-content { padding: 24px 16px; } .grid-3, .grid-4 { grid-template-columns: 1fr; } }
.sidebar-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,.6); backdrop-filter: blur(4px); z-index: 99; }
.sidebar-overlay.active { display: block; }
</style>
</head>
<body>

<div class="sidebar-overlay" id="overlay"></div>

<aside class="sidebar" id="sidebar">
  <div class="sidebar-logo">
    <div class="logo-mark">EL</div>
    <div>
      <div class="logo-text">WMS Report</div>
      <div class="logo-sub">專題報告 · 2026</div>
    </div>
  </div>

  <div class="sidebar-section-label">簡報內容</div>
  <a class="nav-item active" data-section="s-overview"><span class="nav-text">執行摘要</span></a>
  <a class="nav-item" data-section="s-problem"><span class="nav-text">問題定義</span></a>
  <a class="nav-item" data-section="s-solution"><span class="nav-text">解決方案</span></a>
  <a class="nav-item" data-section="s-market"><span class="nav-text">市場分析</span><span class="nav-badge">NEW</span></a>
  <a class="nav-item" data-section="s-product"><span class="nav-text">產品功能</span></a>
  <a class="nav-item" data-section="s-finance"><span class="nav-text">財務規劃</span></a>
  <a class="nav-item" data-section="s-risk"><span class="nav-text">風險評估</span></a>
  <a class="nav-item" data-section="s-roadmap"><span class="nav-text">執行藍圖</span></a>
  <a class="nav-item" data-section="s-team"><span class="nav-text">團隊介紹</span></a>

  <div class="sidebar-progress">
    <div class="sidebar-progress-label">
      <span>專案完成度</span>
      <span style="font-family:'JetBrains Mono',monospace;color:#7C6AF5">100%</span>
    </div>
    <div class="progress-track">
      <div class="progress-fill" data-w="100"></div>
    </div>
    <div style="margin-top:10px;font-size:.65rem;color:var(--text-3);font-family:'JetBrains Mono',monospace">
      更新時間: 2026-05-31
    </div>
  </div>
</aside>

<div class="main" id="mainArea">

  <header class="topbar">
    <button class="topbar-hamburger" id="hamburger" aria-label="選單">
      <svg width="18" height="18" viewBox="0 0 18 18" fill="none">
        <line x1="2" y1="4.5" x2="16" y2="4.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        <line x1="2" y1="9"   x2="16" y2="9"   stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        <line x1="2" y1="13.5" x2="16" y2="13.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
    </button>
    <div class="topbar-breadcrumb">
      <span>WMS Report</span>
      <span class="topbar-divider">/</span>
      <span id="breadcrumb-current">執行摘要</span>
    </div>
    <div class="topbar-right">
      <div class="topbar-tag">v2.4.1</div>
      <div class="topbar-tag live"><span class="live-dot"></span>LIVE</div>
    </div>
  </header>

  <div class="page-content">

    <div class="section active" id="s-overview">
      <div class="hero-card">
        <div class="hero-title">
          智慧倉儲管理系統<br/>
          <em id="typing-text">WMS Platform</em>
        </div>
        <div class="hero-desc">
          本次專題報告針對倉儲管理系統（WMS）進行完整的產品呈現。融合現代多通路（OMO）物流體系、自動化儲位分流演算法，打造出高精確度的科學庫存決策大屏。
        </div>

        <div class="hero-info-grid">
          <div class="hero-info-item"><span class="label">作者</span><span class="value" id="heroAuthor">陳玉鳳、洪依辰</span></div>
          <div class="hero-info-item"><span class="label">指導教授</span><span class="value" id="heroProfessor">褚文明 教授</span></div>
          <div class="hero-info-item"><span class="label">系所</span><span class="value" id="heroDepartment">行銷與流通管理系所</span></div>
          <div class="hero-info-item"><span class="label">班級</span><span class="value" id="heroClass">院二流三甲</span></div>
          <div class="hero-info-item"><span class="label">科目</span><span class="value" id="heroSubject">流通科技管理</span></div>
        </div>

        <div class="hero-meta">
          <div class="hero-stat"><span>報告日期</span>&nbsp;<strong>2026-05-31</strong></div>
          <div class="hero-stat"><span>•</span></div>
          <div class="hero-stat"><span>版本規格</span>&nbsp;<strong>Final v2.4</strong></div>
          <div style="margin-left:auto;display:flex;gap:8px;">
            <span class="badge badge-accent">🚀 完整 Present</span>
            <span class="badge badge-teal">📊 含趨勢圖表</span>
          </div>
        </div>
      </div>

      <div class="grid-4">
        <div class="card kpi-card k-accent">
          <div class="kpi-label">目標市場規模</div>
          <div class="kpi-value v-accent counter" data-target="28.6" data-dec="1" data-suffix="B">0</div>
          <div class="kpi-delta up">▲ 18.4% YoY</div>
        </div>
        <div class="card kpi-card k-gold">
          <div class="kpi-label">預計年營收</div>
          <div class="kpi-value v-gold counter" data-target="3.6" data-dec="1" data-prefix="$" data-suffix="M">0</div>
          <div class="kpi-delta up">▲ 34% 成長</div>
        </div>
        <div class="card kpi-card k-teal">
          <div class="kpi-label">使用者規模</div>
          <div class="kpi-value v-teal counter" data-target="7200" data-dec="0">0</div>
          <div class="kpi-delta up">▲ 200% 成長</div>
        </div>
        <div class="card kpi-card k-danger">
          <div class="kpi-label">市場佔有率</div>
          <div class="kpi-value v-danger counter" data-target="38" data-dec="0" data-suffix="%">0</div>
          <div class="kpi-delta flat">→ 目標 45%</div>
        </div>
      </div>

      <div class="divider"></div>

      <div class="grid-2">
        <div class="card">
          <div class="card-header">
            <div>
              <div class="card-title">年度營收趨勢</div>
              <div class="card-sub">實際 vs 目標（單位：百萬）</div>
            </div>
            <span class="badge badge-accent">2026</span>
          </div>
          <div class="chart-wrap"><canvas id="chartRevenue"></canvas></div>
        </div>
        <div class="card">
          <div class="card-header">
            <div>
              <div class="card-title">市場佔有率分布</div>
              <div class="card-sub">WMS 市場競爭態勢</div>
            </div>
            <span class="badge badge-gold">競爭分析</span>
          </div>
          <div class="chart-wrap"><canvas id="chartMarket"></canvas></div>
        </div>
      </div>
    </div>

    <div class="section" id="s-problem">
      <div class="page-header">
        <div class="page-header-eyebrow">Problem Definition</div>
        <h1 class="page-title">問題定義與現況痛點</h1>
        <p class="page-desc">傳統倉儲與粗放的物流行為正產生龐大的隱性運營成本黑洞。</p>
      </div>

      <div class="grid-2">
        <div class="card card-accent">
          <div class="card-header"><div class="card-title">核心痛點軸線 Pain Points</div></div>
          <div class="timeline">
            <div class="timeline-item">
              <div class="timeline-dot" style="background:var(--danger)"></div>
              <div>
                <div class="timeline-title">庫存數據嚴重滯後不準</div>
                <div class="timeline-date">影響程度：High</div>
                <div class="timeline-desc">人工 Excel 登帳造成資訊孤島，平均月帳物差異率達 8-12%，常態性導致多通路缺貨爆單。</div>
              </div>
            </div>
            <div class="timeline-item">
              <div class="timeline-dot" style="background:var(--warning)"></div>
              <div>
                <div class="timeline-title">揀貨動線重疊低效</div>
                <div class="timeline-date">影響程度：High</div>
                <div class="timeline-desc">未配置智慧路徑演算法，重複走動造成每單揀貨耗時 8.5 分鐘，大促期積壓率衝高。</div>
              </div>
            </div>
          </div>
        </div>

        <div class="card">
          <div class="card-header"><div class="card-title">市場缺口與損失量化分析</div></div>
          <div class="risk-bar">
            <div class="risk-row">
              <div class="risk-label">人力無效耗損</div>
              <div class="risk-track"><div class="risk-fill" data-w="85" style="background:var(--danger)"></div></div>
              <div class="risk-pct">85%</div>
            </div>
            <div class="risk-row">
              <div class="risk-label">錯單衍生損失</div>
              <div class="risk-track"><div class="risk-fill" data-w="48" style="background:var(--warning)"></div></div>
              <div class="risk-pct">48%</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="section" id="s-solution">
      <div class="page-header">
        <div class="page-header-eyebrow">SaaS Architecture</div>
        <h1 class="page-title">解決方案與科學控制模型</h1>
      </div>

      <div class="card" style="margin-bottom:20px;">
        <div class="card-header"><div class="card-title">三大倉儲系統管理模式矩陣表</div></div>
        <table class="data-table">
          <thead>
            <tr><th>評估指標</th><th>傳統紙本 / Excel</th><th>一般 ERP 庫存模組</th><th>本專案智慧雲端 WMS</th></tr>
          </thead>
          <tbody>
            <tr><td class="td-main">多通路庫存同步</td><td>✗ 完全人工串接</td><td>△ 定期非即時同步</td><td><span class="badge badge-teal">✓ 秒級即時同步</span></td></tr>
            <tr><td class="td-main">揀貨路徑規劃</td><td>✗ 無（憑員工經驗走動）</td><td>✗ 僅按儲位碼排序</td><td><span class="badge badge-teal">✓ AI 最佳路徑演算法</span></td></tr>
            <tr><td class="td-main">盤點流暢度</td><td>✗ 必須停工大盤點</td><td>△ 批次非即時帳面盤</td><td><span class="badge badge-teal">✓ 動態即時不停工盤點</span></td></tr>
          </tbody>
        </table>
      </div>

      <div class="grid-2">
        <div class="card">
          <div class="card-title">1. 經濟訂購量模型 (EOQ)</div>
          <div class="card-sub">精確抓出平衡「持有成本」與「訂購成本」的最優採購批量：</div>
          <div class="formula-box">$$EOQ = \sqrt{\frac{2DS}{H}}$$</div>
          <div style="font-size: .75rem; color: var(--text-3);">其中 $D$ 為年需求量、$S$ 為單次訂購成本、$H$ 為單位持有成本。</div>
        </div>
        <div class="card">
          <div class="card-title">2. 安全庫存量公式 (Safety Stock)</div>
          <div class="card-sub">抵禦供應鏈前置時間突發延遲與斷貨衝擊的量化防線：</div>
          <div class="formula-box">$$SS = Z \times \sigma_L$$</div>
          <div style="font-size: .75rem; color: var(--text-3);">其中 $Z$ 為服務水準係數、$\sigma_L$ 為前置時間內需求標準差。</div>
        </div>
      </div>
    </div>

    <div class="section" id="s-market">
      <div class="page-header">
        <div class="page-header-eyebrow">Market YoY Compare</div>
        <h1 class="page-title">市場分析與競爭格局</h1>
      </div>
      <div class="grid-2">
        <div class="card">
          <div class="card-header"><div><div class="card-title">季度成長 YoY 比較</div><div class="card-sub">本年度 vs 上年度營收趨勢</div></div></div>
          <div class="chart-wrap"><canvas id="chartQuarterly"></canvas></div>
        </div>
        <div class="card">
          <div class="card-header"><div class="card-title">競品競爭矩陣</div></div>
          <table class="data-table">
            <thead>
              <tr><th>維度</th><th>我方系統</th><th>競品 A</th><th>競品 B</th></tr>
            </thead>
            <tbody>
              <tr><td class="td-main">AI 模型預測</td><td><span class="badge badge-teal">✓ 強大</span></td><td>△ 基本中等</td><td>✗ 無此功能</td></tr>
              <tr><td class="td-main">API 多端對接</td><td><span class="badge badge-teal">✓ 全面開放</span></td><td>△ 有限整合</td><td>✗ 封閉不支援</td></tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <div class="section" id="s-product">
      <div class="page-header">
        <div class="page-header-eyebrow">Product Capabilities</div>
        <h1 class="page-title">產品功能與模組完成度</h1>
      </div>
      <div class="grid-2">
        <div class="card">
          <div class="card-header"><div><div class="card-title">用戶規模與 MAU 成長曲線</div></div></div>
          <div class="chart-wrap"><canvas id="chartUsers"></canvas></div>
        </div>
        <div class="card">
          <div class="card-header"><div class="card-title">系統功能開發完成度</div></div>
          <div class="risk-bar">
            <div class="risk-row">
              <div class="risk-label">入出庫核心模組</div>
              <div class="risk-track"><div class="risk-fill" data-w="100" style="background:var(--success)"></div></div>
              <div class="risk-pct">100%</div>
            </div>
            <div class="risk-row">
              <div class="risk-label">AI 補貨預測模組</div>
              <div class="risk-track"><div class="risk-fill" data-w="85" style="background:var(--accent)"></div></div>
              <div class="risk-pct">85%</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="section" id="s-finance">
      <div class="page-header">
        <div class="page-header-eyebrow">Financial Roadmap</div>
        <h1 class="page-title">財務規劃與預算控管</h1>
      </div>
      <div class="grid-2">
        <div class="card">
          <div class="card-header"><div><div class="card-title">預算 waterfall 瀑布流</div></div></div>
          <div class="chart-wrap"><canvas id="chartBudget"></canvas></div>
        </div>
        <div class="card">
          <div class="card-header"><div class="card-title">運營成本配置比例</div></div>
          <div class="risk-bar">
            <div class="risk-row">
              <div class="risk-label">研發投入成本</div>
              <div class="risk-track"><div class="risk-fill" data-w="35" style="background:var(--accent)"></div></div>
              <div class="risk-pct">35%</div>
            </div>
            <div class="risk-row">
              <div class="risk-label">雲端運維設施</div>
              <div class="risk-track"><div class="risk-fill" data-w="28" style="background:var(--gold)"></div></div>
              <div class="risk-pct">28%</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="section" id="s-risk">
      <div class="page-header">
        <div class="page-header-eyebrow">Risk Control Matrix</div>
        <h1 class="page-title">六維度系統風險評估</h1>
      </div>
      <div class="grid-2">
        <div class="card">
          <div class="card-header"><div><div class="card-title">綜合維度風險指標雷達圖</div></div></div>
          <div class="chart-wrap" style="height:280px"><canvas id="chartRisk"></canvas></div>
        </div>
        <div class="card">
          <div class="card-header"><div class="card-title">風險緩解與防範措施</div></div>
          <table class="data-table">
            <thead>
              <tr><th>風險威脅項目</th><th>嚴重級別</th><th>應對防範戰略</th></tr>
            </thead>
            <tbody>
              <tr><td class="td-main">大促期高併發爆倉卡死</td><td><span class="badge badge-danger">High</span></td><td>布建 Redis 緩衝佇列進行多級高防並發機制</td></tr>
              <tr><td class="td-main">電商多通路敏感資安漏洞</td><td><span class="badge badge-danger">High</span></td><td>全站傳輸鏈路落實 AES-256 去識別化加密</td></tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <div class="section" id="s-roadmap">
      <div class="page-header">
        <div class="page-header-eyebrow">Milestones</div>
        <h1 class="page-title">18 個月產品迭代執行藍圖</h1>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">專案甘特里程進度條</div></div>
        <div class="gantt">
          <div class="gantt-row"><div class="gantt-label">核心底層架構</div><div class="gantt-track"><div class="gantt-fill gantt-fill-done" data-left="0" data-w="40"></div></div></div>
          <div class="gantt-row"><div class="gantt-label">AI 模型調校優化</div><div class="gantt-track"><div class="gantt-fill gantt-fill-wip" data-left="40" data-w="30"></div></div></div>
          <div class="gantt-row"><div class="gantt-label">全面系統整合發布</div><div class="gantt-track"><div class="gantt-fill gantt-fill-todo" data-left="70" data-w="30"></div></div></div>
        </div>
      </div>
    </div>

    <div class="section" id="s-team">
      <div class="page-header">
        <div class="page-header-eyebrow">Team &amp; Acknowledgements</div>
        <h1 class="page-title">專題成員與答辯</h1>
      </div>
      <div class="grid-3">
        <div class="card" style="text-align:center;">
          <div style="font-size:2.2rem; margin-bottom:12px;">👩‍💼</div>
          <div class="card-title" style="font-size:1.05rem;">Elaine Chen</div>
          <p class="card-sub">國立勤益科技大學<br/>行銷與流通管理專題 PM</p>
        </div>
        <div class="card" style="text-align:center;">
          <div style="font-size:2.2rem; margin-bottom:12px;">👩‍🎓</div>
          <div class="card-title" style="font-size:1.05rem;">陳玉鳳 / 洪依辰</div>
          <p class="card-sub">專題核心開發與簡報主講人</p>
        </div>
        <div class="card" style="text-align:center;">
          <div style="font-size:2.2rem; margin-bottom:12px;">👨‍🏫</div>
          <div class="card-title" style="font-size:1.05rem;">褚文明 教授</div>
          <p class="card-sub">指導教授 / 流通科技學術總顧問</p>
        </div>
      </div>

      <div class="card card-accent" style="text-align:center; padding:45px; margin-top:28px;">
        <h2 style="color:var(--text-1); font-family: 'Syne', sans-serif; font-size:1.8rem; margin-bottom:12px;">Q &amp; A 答辯與交流</h2>
        <p style="color:var(--text-2); font-size:0.95rem; margin-bottom:0;">感謝各位評審委員蒞臨指導，歡迎提問與指教。</p>
      </div>
    </div>

  </div></div><script>
'use strict';

// 1. 全域圖表外觀配置：完全適配 Dark Minimal Pro 奢華深色調
const CHART_DEFAULTS = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: { display: true, labels: { color: '#A8A8B8', font: { size: 10 } } },
    tooltip: {
      backgroundColor: '#18181C',
      borderColor: 'rgba(255,255,255,0.08)',
      borderWidth: 1,
      titleColor: '#F2F2F5',
      bodyColor: '#A8A8B8',
      padding: 12,
      cornerRadius: 10,
    }
  },
  scales: {
    x: { grid: { color: 'rgba(255,255,255,0.04)' }, ticks: { color: '#6A6A7A', font: { size: 10 } } },
    y: { grid: { color: 'rgba(255,255,255,0.04)' }, ticks: { color: '#6A6A7A', font: { size: 10 } } }
  }
};

const charts = {};

// 2. 構建渲染 6 大核心圖表
function buildCharts() {
  // [1] 年度營收趨勢 (Line Chart)
  const ctxRevenue = document.getElementById('chartRevenue');
  if (ctxRevenue) {
    charts.revenue = new Chart(ctxRevenue, {
      type: 'line',
      data: {
        labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'],
        datasets: [
          {
            label: '實際營收',
            data: [1.2, 1.5, 1.3, 1.8, 2.1, 2.4, 2.2, 2.7, 3.0, 2.8, 3.2, 3.6],
            borderColor: '#7C6AF5',
            backgroundColor: 'rgba(124,106,245,0.08)',
            borderWidth: 2, pointRadius: 3, fill: true, tension: 0.45
          },
          {
            label: '目標營收',
            data: [1.0, 1.3, 1.5, 1.7, 2.0, 2.2, 2.5, 2.8, 3.0, 3.2, 3.5, 4.0],
            borderColor: 'rgba(255,255,255,0.15)',
            borderWidth: 1.5, borderDash: [4, 4], fill: false, tension: 0.45
          }
        ]
      },
      options: CHART_DEFAULTS
    });
  }

  // [2] 市場佔有率 (Doughnut Chart)
  const ctxMarket = document.getElementById('chartMarket');
  if (ctxMarket) {
    charts.market = new Chart(ctxMarket, {
      type: 'doughnut',
      data: {
        labels: ['我方產品', '競品 A', '競品 B', '其他通路'],
        datasets: [{
          data: [38, 27, 21, 14],
          backgroundColor: ['#7C6AF5', '#C9A84C', '#3ECFCF', '#2C2C33'],
          borderWidth: 0
        }]
      },
      options: { responsive: true, maintainAspectRatio: false, plugins: CHART_DEFAULTS.plugins }
    });
  }

  // [3] 季度成長比較 (Bar Chart)
  const ctxQ = document.getElementById('chartQuarterly');
  if (ctxQ) {
    charts.quarterly = new Chart(ctxQ, {
      type: 'bar',
      data: {
        labels: ['Q1', 'Q2', 'Q3', 'Q4'],
        datasets: [
          { label: '本年度', data: [4.0, 6.3, 7.9, 9.6], backgroundColor: 'rgba(124,106,245,0.75)', borderRadius: 6 },
          { label: '上年度', data: [3.2, 4.8, 6.1, 7.2], backgroundColor: 'rgba(255,255,255,0.07)', borderRadius: 6 }
        ]
      },
      options: CHART_DEFAULTS
    });
  }

  // [4] 用戶成長曲線 (Area Chart)
  const ctxUsers = document.getElementById('chartUsers');
  if (ctxUsers) {
    charts.users = new Chart(ctxUsers, {
      type: 'line',
      data: {
        labels: ['1月','2月','3月','4月','5月','6月'],
        datasets: [{
          label: '活躍用戶數', data: [2400, 3100, 3800, 4600, 5900, 7200],
          borderColor: '#3ECFCF', backgroundColor: 'rgba(62,207,207,0.1)', borderWidth: 2, fill: true, tension: 0.45
        }]
      },
      options: CHART_DEFAULTS
    });
  }

  // [5] 風險雷達圖 (Radar Chart)
  const ctxRisk = document.getElementById('chartRisk');
  if (ctxRisk) {
    charts.risk = new Chart(ctxRisk, {
      type: 'radar',
      data: {
        labels: ['技術風險','市場風險','財務風險','人力資源','法規合規','競爭壓力'],
        datasets: [{
          label: '風險指數', data: [55, 72, 38, 45, 28, 80],
          borderColor: '#C9A84C', backgroundColor: 'rgba(201,168,76,0.12)', borderWidth: 2
        }]
      },
      options: {
        responsive: true, maintainAspectRatio: false, plugins: CHART_DEFAULTS.plugins,
        scales: { r: { grid: { color: 'rgba(255,255,255,0.06)' }, angleLines: { color: 'rgba(255,255,255,0.06)' }, pointLabels: { color: '#6A6A7A' } } }
      }
    });
  }

  // [6] 預算瀑布圖 (Waterfall Bar Chart)
  const ctxBudget = document.getElementById('chartBudget');
  if (ctxBudget) {
    charts.budget = new Chart(ctxBudget, {
      type: 'bar',
      data: {
        labels: ['預算', '研發成本', '行銷費用', '人力成本', '營運費用', '淨利潤'],
        datasets: [{
          label: '金額異動 (M)', data: [10, -2.5, -1.8, -2.2, -0.8, 2.7],
          backgroundColor: [ 'rgba(124,106,245,0.75)', 'rgba(240,90,122,0.65)', 'rgba(240,90,122,0.65)', 'rgba(240,90,122,0.65)', 'rgba(240,90,122,0.65)', 'rgba(62,207,207,0.75)' ],
          borderRadius: 5
        }]
      },
      options: CHART_DEFAULTS
    });
  }
}

// 3. 側邊欄單頁路由與導覽
function initNav() {
  const navItems = document.querySelectorAll('.nav-item[data-section]');
  const sections = document.querySelectorAll('.section');

  function activateSection(id) {
    navItems.forEach(n => n.classList.toggle('active', n.dataset.section === id));
    sections.forEach(s => s.classList.toggle('active', s.id === id));
    
    const active = document.querySelector(`.nav-item[data-section="${id}"]`);
    const bc = document.getElementById('breadcrumb-current');
    if (bc && active) bc.textContent = active.querySelector('.nav-text')?.textContent || active.textContent;
    
    setTimeout(() => animateBars(), 120);
    document.querySelector('.sidebar')?.classList.remove('open');
    document.querySelector('.sidebar-overlay')?.classList.remove('active');
    document.getElementById('mainArea')?.scrollTo({ top: 0, behavior: 'smooth' });
  }

  navItems.forEach(item => {
    item.addEventListener('click', () => activateSection(item.dataset.section));
  });

  // 漢堡選單綁定
  const ham = document.getElementById('hamburger');
  const sidebar = document.querySelector('.sidebar');
  const overlay = document.querySelector('.sidebar-overlay');
  if (ham && sidebar && overlay) {
    ham.addEventListener('click', () => { sidebar.classList.toggle('open'); overlay.classList.toggle('active'); });
    overlay.addEventListener('click', () => { sidebar.classList.remove('open'); overlay.classList.remove('active'); });
  }
}

// 4. 進度條與甘特圖動畫控制
function animateBars() {
  document.querySelectorAll('.progress-fill[data-w]').forEach(el => { el.style.width = el.dataset.w + '%'; });
  document.querySelectorAll('.risk-fill[data-w]').forEach(el => { el.style.width = el.dataset.w + '%'; });
  document.querySelectorAll('.gantt-fill[data-left][data-w]').forEach(el => {
    el.style.left = el.dataset.left + '%'; el.style.width = el.dataset.w + '%';
  });
}

// 5. KPI 滾動跳數動畫效果
function animateCounters() {
  document.querySelectorAll('.counter[data-target]').forEach(el => {
    const target = parseFloat(el.dataset.target);
    const dec = parseInt(el.dataset.dec || 0);
    const prefix = el.dataset.prefix || '';
    const suffix = el.dataset.suffix || '';
    const dur = 1500;
    const start = performance.now();
    const step = now => {
      const p = Math.min((now - start) / dur, 1);
      const ease = 1 - Math.pow(1 - p, 3);
      el.textContent = prefix + (target * ease).toFixed(dec) + suffix;
      if (p < 1) requestAnimationFrame(step);
    };
    requestAnimationFrame(step);
  });
}

// 6. 打字機特效
function initTyping() {
  const el = document.getElementById('typing-text');
  if (!el) return;
  const texts = ['倉儲管理系統', '庫存最佳化', '院二流三甲', '流通科技管理'];
  let i = 0, j = 0, deleting = false;
  function type() {
    const current = texts[i];
    if (!deleting) {
      el.textContent = current.slice(0, j + 1); j++;
      if (j === current.length) { deleting = true; setTimeout(type, 1600); return; }
    } else {
      el.textContent = current.slice(0, j - 1); j--;
      if (j === 0) { deleting = false; i = (i + 1) % texts.length; }
    }
    setTimeout(type, deleting ? 60 : 100);
  }
  type();
}

// 核心初始化
document.addEventListener('DOMContentLoaded', () => {
  initNav();
  initTyping();
  buildCharts();
  animateCounters();
  setTimeout(animateBars, 300);
});
</script>
</body>
</html>
