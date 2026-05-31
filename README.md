<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>WMS 與庫存最佳化｜日系零售選物蜜桃櫻花焦糖色系</title>
  <script>
    window.MathJax = {
      tex: { inlineMath: [['\\(', '\\)']], displayMath: [['$$','$$']] },
      chtml: { matchFontHeight: false },
      startup: { typeset: false }
    };
  </script>
  <script async id="MathJax-script" src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
  <style>
    :root {
      --bg: #fffaf4;
      --bg-soft: #fff5ec;
      --paper: rgba(255, 255, 255, .76);
      --paper-strong: rgba(255, 255, 255, .92);
      --peach-50: #fff0e7;
      --peach-100: #ffe0cf;
      --peach-300: #ffc7ad;
      --peach-500: #ff9f7d;
      --sakura-50: #fff1f6;
      --sakura-100: #ffdce9;
      --sakura-300: #ffb7cf;
      --caramel-50: #fbf1df;
      --caramel-100: #f1dec0;
      --caramel-300: #d7ad79;
      --caramel-500: #a9784d;
      --tea: #7f6453;
      --ink: #3d332d;
      --muted: #8e7d72;
      --line: rgba(201, 149, 111, .24);
      --shadow: 0 18px 48px rgba(156, 103, 69, .12);
      --shadow-soft: 0 10px 28px rgba(255, 159, 125, .12);
      --radius-xl: 28px;
      --radius-lg: 20px;
      --radius-md: 14px;
      --chart-1: #ff9f7d;
      --chart-2: #ffb7cf;
      --chart-3: #d7ad79;
      --chart-4: #f1dec0;
      --chart-5: #b98b6d;
      --chart-6: #f8c7a7;
    }

    * { box-sizing: border-box; }
    html { scroll-behavior: smooth; }
    body {
      margin: 0;
      min-height: 100vh;
      color: var(--ink);
      font-family: "Noto Sans TC", "Noto Sans JP", "Hiragino Sans", "Yu Gothic", "Microsoft JhengHei", system-ui, sans-serif;
      background:
        radial-gradient(circle at 8% 8%, rgba(255, 183, 207, .30), transparent 30%),
        radial-gradient(circle at 92% 10%, rgba(255, 199, 173, .34), transparent 33%),
        radial-gradient(circle at 50% 98%, rgba(215, 173, 121, .18), transparent 42%),
        linear-gradient(135deg, #fffaf4 0%, #fff3eb 47%, #fff7f8 100%);
      overflow-x: hidden;
    }

    body::before {
      content: "";
      position: fixed;
      inset: 0;
      pointer-events: none;
      background-image: linear-gradient(rgba(169, 120, 77, .05) 1px, transparent 1px), linear-gradient(90deg, rgba(169, 120, 77, .04) 1px, transparent 1px);
      background-size: 34px 34px;
      mask-image: linear-gradient(to bottom, black, transparent 82%);
      z-index: -1;
    }

    .topbar {
      position: sticky;
      top: 0;
      z-index: 50;
      border-bottom: 1px solid var(--line);
      background: rgba(255, 250, 244, .78);
      backdrop-filter: blur(22px) saturate(130%);
    }

    .topbar-inner {
      max-width: 1440px;
      margin: 0 auto;
      padding: 16px 26px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 18px;
    }

    .brand {
      display: flex;
      align-items: center;
      gap: 12px;
      min-width: 250px;
    }

    .brand-mark {
      width: 42px;
      height: 42px;
      border-radius: 15px;
      background: linear-gradient(135deg, var(--peach-500), var(--sakura-300) 55%, var(--caramel-300));
      box-shadow: 0 10px 24px rgba(255, 159, 125, .28);
      position: relative;
    }

    .brand-mark::after {
      content: "倉";
      position: absolute;
      inset: 0;
      display: grid;
      place-items: center;
      color: white;
      font-weight: 800;
      letter-spacing: .03em;
    }

    .brand-title { font-weight: 900; letter-spacing: .02em; line-height: 1.1; }
    .brand-title small { display: block; margin-top: 4px; color: var(--muted); font-weight: 600; font-size: 12px; }
    .breadcrumb { color: var(--muted); font-size: 14px; text-align: center; }
    .breadcrumb b { color: var(--peach-500); }
    .status { display: flex; align-items: center; gap: 10px; justify-content: flex-end; min-width: 220px; }
    .pill {
      border: 1px solid var(--line);
      border-radius: 999px;
      padding: 8px 12px;
      background: rgba(255, 255, 255, .58);
      color: var(--tea);
      font-size: 12px;
      font-weight: 800;
      white-space: nowrap;
    }
    .dot { width: 8px; height: 8px; border-radius: 99px; display: inline-block; margin-right: 6px; background: var(--peach-500); box-shadow: 0 0 0 7px rgba(255, 159, 125, .14); animation: pulse 1.8s ease-in-out infinite; }

    .shell {
      max-width: 1440px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: 292px minmax(0, 1fr);
      gap: 26px;
      padding: 28px 26px 54px;
    }

    .sidebar { position: sticky; top: 92px; align-self: start; }
    .nav-card {
      border: 1px solid var(--line);
      border-radius: var(--radius-xl);
      background: rgba(255, 255, 255, .58);
      backdrop-filter: blur(20px);
      box-shadow: var(--shadow-soft);
      padding: 18px;
      overflow: hidden;
    }
    .nav-eyebrow { color: var(--caramel-500); font-size: 12px; font-weight: 900; letter-spacing: .12em; text-transform: uppercase; margin: 8px 10px 12px; }
    .nav-list { list-style: none; padding: 0; margin: 0; display: grid; gap: 8px; }
    .nav-link {
      width: 100%;
      display: grid;
      grid-template-columns: 28px 1fr auto;
      align-items: center;
      gap: 10px;
      border: 1px solid transparent;
      border-radius: 16px;
      padding: 12px 12px;
      color: var(--tea);
      background: transparent;
      font: inherit;
      text-align: left;
      cursor: pointer;
      transition: transform .25s ease, background .25s ease, border-color .25s ease, box-shadow .25s ease;
    }
    .nav-link:hover { transform: translateX(4px); background: rgba(255, 240, 231, .72); border-color: var(--line); }
    .nav-link.active {
      color: var(--ink);
      background: linear-gradient(135deg, rgba(255, 224, 207, .88), rgba(255, 220, 233, .72));
      border-color: rgba(255, 159, 125, .30);
      box-shadow: 0 10px 22px rgba(255, 159, 125, .16);
    }
    .nav-num {
      width: 28px;
      height: 28px;
      display: grid;
      place-items: center;
      border-radius: 10px;
      background: rgba(255, 255, 255, .62);
      color: var(--peach-500);
      font-size: 12px;
      font-weight: 900;
    }
    .nav-title { font-weight: 850; font-size: 14px; }
    .nav-note { font-size: 11px; color: var(--muted); }
    .progress { margin: 18px 10px 4px; padding-top: 16px; border-top: 1px dashed var(--line); }
    .progress-meta { display: flex; justify-content: space-between; color: var(--muted); font-size: 12px; margin-bottom: 8px; }
    .progress-track { height: 8px; border-radius: 99px; background: var(--peach-50); overflow: hidden; }
    .progress-fill { height: 100%; width: 11.11%; border-radius: inherit; background: linear-gradient(90deg, var(--peach-500), var(--sakura-300), var(--caramel-300)); transition: width .45s cubic-bezier(.2,.8,.2,1); }

    main { min-width: 0; }
    .page { display: none; animation: pageIn .55s cubic-bezier(.2,.8,.2,1) both; }
    .page.active { display: block; }

    .hero {
      position: relative;
      overflow: hidden;
      min-height: 390px;
      border: 1px solid rgba(255, 159, 125, .26);
      border-radius: 34px;
      padding: clamp(28px, 5vw, 58px);
      background:
        linear-gradient(135deg, rgba(255,255,255,.78), rgba(255,245,236,.66)),
        radial-gradient(circle at 86% 22%, rgba(255,183,207,.42), transparent 32%),
        radial-gradient(circle at 72% 80%, rgba(215,173,121,.20), transparent 36%);
      box-shadow: var(--shadow);
    }
    .hero::before {
      content: "";
      position: absolute;
      width: 520px;
      height: 520px;
      right: -180px;
      top: -190px;
      border-radius: 999px;
      background: conic-gradient(from 120deg, rgba(255,159,125,.14), rgba(255,183,207,.18), rgba(241,222,192,.18), rgba(255,159,125,.14));
      filter: blur(2px);
      animation: slowSpin 18s linear infinite;
    }
    .hero-content { position: relative; z-index: 2; max-width: 860px; }
    .label {
      display: inline-flex;
      align-items: center;
      gap: 10px;
      color: var(--peach-500);
      font-size: 12px;
      font-weight: 950;
      letter-spacing: .13em;
      text-transform: uppercase;
      margin-bottom: 14px;
    }
    .label::before { content: ""; width: 30px; height: 2px; border-radius: 999px; background: currentColor; }
    h1, h2, h3 { margin: 0; letter-spacing: -.03em; }
    h1 { font-size: clamp(42px, 7vw, 82px); line-height: .98; font-weight: 950; color: var(--ink); }
    .gradient-text { background: linear-gradient(135deg, var(--peach-500), var(--sakura-300) 45%, var(--caramel-300)); -webkit-background-clip: text; background-clip: text; color: transparent; }
    .lead { color: var(--muted); font-size: 17px; line-height: 1.95; max-width: 760px; margin: 22px 0 0; }

    .meta-grid, .kpi-grid, .feature-grid, .case-grid, .formula-grid {
      display: grid;
      grid-template-columns: repeat(4, minmax(0, 1fr));
      gap: 16px;
    }
    .meta-grid { margin-top: 30px; }
    .mini-card, .kpi-card, .card, .chart-card, .formula-card, .case-card, .timeline-card {
      border: 1px solid var(--line);
      border-radius: var(--radius-lg);
      background: var(--paper);
      backdrop-filter: blur(16px);
      box-shadow: var(--shadow-soft);
    }
    .mini-card { padding: 16px; }
    .mini-label, .kpi-label { color: var(--muted); font-size: 12px; font-weight: 800; margin-bottom: 7px; }
    .mini-value { color: var(--ink); font-size: 15px; font-weight: 900; }
    .kpi-grid { margin: 22px 0; }
    .kpi-card { padding: 22px; position: relative; overflow: hidden; transition: transform .25s ease, box-shadow .25s ease; }
    .kpi-card:hover { transform: translateY(-5px); box-shadow: var(--shadow); }
    .kpi-value { font-size: 34px; font-weight: 950; color: var(--ink); }
    .kpi-delta { color: var(--peach-500); font-size: 13px; font-weight: 900; margin-top: 8px; }

    .card, .chart-card { padding: clamp(22px, 3vw, 34px); margin-top: 22px; }
    .card-head { display: flex; justify-content: space-between; gap: 18px; align-items: flex-start; margin-bottom: 22px; }
    .card-title { font-size: clamp(26px, 3.2vw, 42px); font-weight: 950; }
    .card-subtitle { color: var(--muted); line-height: 1.8; margin-top: 10px; max-width: 850px; }
    .tag { display: inline-flex; border: 1px solid var(--line); border-radius: 999px; padding: 8px 12px; color: var(--tea); background: rgba(255,255,255,.54); font-size: 12px; font-weight: 850; white-space: nowrap; }
    .chart-wrap { position: relative; width: 100%; height: 356px; }
    canvas { max-width: 100%; }

    .feature-grid { grid-template-columns: repeat(3, minmax(0, 1fr)); margin-top: 18px; }
    .feature { padding: 20px; border-radius: 18px; background: rgba(255, 255, 255, .56); border: 1px solid var(--line); }
    .feature b { display: block; margin-bottom: 8px; color: var(--ink); }
    .feature p { margin: 0; color: var(--muted); line-height: 1.75; }

    .formula-grid { grid-template-columns: repeat(2, minmax(0, 1fr)); }
    .formula-card { padding: 24px; background: linear-gradient(135deg, rgba(255,255,255,.78), rgba(255,240,231,.54)); }
    .formula-card h3 { color: var(--tea); font-size: 22px; margin-bottom: 14px; }
    .formula-box {
      margin: 16px 0;
      padding: 18px;
      border-left: 5px solid var(--peach-500);
      border-radius: 16px;
      background: rgba(255, 245, 236, .72);
      overflow-x: auto;
    }
    .formula-meaning { color: var(--muted); line-height: 1.85; }
    .inputs { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 12px; margin-top: 16px; }
    .inputs label { display: block; color: var(--tea); font-size: 12px; font-weight: 900; margin-bottom: 7px; }
    input {
      width: 100%;
      border: 1px solid var(--line);
      border-radius: 13px;
      padding: 12px 13px;
      background: rgba(255,255,255,.78);
      color: var(--ink);
      font: inherit;
      outline: none;
    }
    input:focus { border-color: rgba(255, 159, 125, .65); box-shadow: 0 0 0 4px rgba(255,159,125,.12); }
    .button-row { display: flex; gap: 12px; flex-wrap: wrap; margin-top: 14px; }
    .btn {
      border: 0;
      border-radius: 999px;
      padding: 12px 18px;
      cursor: pointer;
      color: white;
      background: linear-gradient(135deg, var(--peach-500), var(--sakura-300));
      font-weight: 900;
      box-shadow: 0 12px 24px rgba(255,159,125,.24);
      transition: transform .22s ease, box-shadow .22s ease;
    }
    .btn:hover { transform: translateY(-2px); box-shadow: 0 16px 30px rgba(255,159,125,.32); }
    .result {
      margin-top: 14px;
      padding: 14px 16px;
      border-radius: 16px;
      color: var(--ink);
      background: linear-gradient(135deg, rgba(255,224,207,.76), rgba(255,220,233,.60));
      border: 1px solid rgba(255,159,125,.24);
      font-weight: 900;
      display: none;
    }

    table { width: 100%; border-collapse: collapse; overflow: hidden; border-radius: 18px; background: rgba(255,255,255,.54); }
    th, td { padding: 14px 16px; text-align: left; border-bottom: 1px solid var(--line); vertical-align: top; }
    th { background: linear-gradient(135deg, var(--peach-50), var(--sakura-50)); color: var(--tea); font-weight: 950; }
    td { color: var(--muted); line-height: 1.7; }
    tr:last-child td { border-bottom: 0; }

    .case-grid { grid-template-columns: repeat(3, minmax(0, 1fr)); margin-top: 18px; }
    .case-card { padding: 22px; background: rgba(255,255,255,.62); }
    .case-card h3 { font-size: 20px; margin-bottom: 10px; }
    .case-card p { color: var(--muted); line-height: 1.8; margin: 0; }

    .timeline { display: grid; gap: 14px; margin-top: 18px; }
    .timeline-card { display: grid; grid-template-columns: 120px 1fr auto; gap: 18px; padding: 18px; align-items: center; }
    .phase { color: var(--peach-500); font-weight: 950; }
    .timeline-card p { margin: 6px 0 0; color: var(--muted); line-height: 1.65; }

    .footer-note { margin-top: 28px; color: var(--muted); font-size: 13px; line-height: 1.8; text-align: center; }

    @keyframes pulse { 0%,100%{ transform: scale(1); opacity: 1; } 50%{ transform: scale(.75); opacity: .68; } }
    @keyframes pageIn { from { opacity: 0; transform: translateY(16px); } to { opacity: 1; transform: translateY(0); } }
    @keyframes slowSpin { to { transform: rotate(360deg); } }

    @media (max-width: 1120px) {
      .shell { grid-template-columns: 1fr; }
      .sidebar { position: static; }
      .nav-list { grid-template-columns: repeat(3, minmax(0, 1fr)); }
      .meta-grid, .kpi-grid { grid-template-columns: repeat(2, minmax(0, 1fr)); }
    }
    @media (max-width: 760px) {
      .topbar-inner { flex-direction: column; align-items: stretch; }
      .brand, .status { min-width: 0; justify-content: center; }
      .shell { padding: 18px 14px 40px; }
      .nav-list, .meta-grid, .kpi-grid, .feature-grid, .formula-grid, .case-grid, .inputs { grid-template-columns: 1fr; }
      .card-head, .timeline-card { display: block; }
      .chart-wrap { height: 300px; }
    }
  </style>
</head>
<body>
  <header class="topbar">
    <div class="topbar-inner">
      <div class="brand">
        <div class="brand-mark"></div>
        <div class="brand-title">WMS Selection Retail Lab<small>流通科技管理｜日系零售選物專題</small></div>
      </div>
      <div class="breadcrumb">九大頁面控制點 / <b id="currentPage">執行摘要</b></div>
      <div class="status"><span class="pill"><span class="dot"></span>MathJax + Chart.js</span><span class="pill">Single HTML</span></div>
    </div>
  </header>

  <div class="shell">
    <aside class="sidebar">
      <nav class="nav-card" aria-label="WMS 九大頁面切換控制點">
        <div class="nav-eyebrow">Page Control</div>
        <ul class="nav-list" id="navList"></ul>
        <div class="progress">
          <div class="progress-meta"><span>閱讀進度</span><span id="progressText">1 / 9</span></div>
          <div class="progress-track"><div class="progress-fill" id="progressFill"></div></div>
        </div>
      </nav>
    </aside>

    <main>
      <section id="overview" class="page active" data-title="執行摘要">
        <div class="hero">
          <div class="hero-content">
            <div class="label">Executive Summary</div>
            <h1>智慧倉儲管理系統<br><span class="gradient-text" id="typewriter">庫存最佳化</span></h1>
            <p class="lead">本檔案將日系零售選物的柔和質感、蜜桃櫻花焦糖色系、庫存科學公式與 WMS 專題骨架整合為單一獨立網頁。專題以「流通科技管理」課程脈絡為核心，由褚文明教授指導，陳玉鳳、洪依辰同學發表，聚焦倉儲準確度、補貨決策與多通路履約效率。</p>
            <div class="meta-grid">
              <div class="mini-card"><div class="mini-label">指導教授</div><div class="mini-value">褚文明 教授</div></div>
              <div class="mini-card"><div class="mini-label">專題成員</div><div class="mini-value">陳玉鳳、洪依辰</div></div>
              <div class="mini-card"><div class="mini-label">課程定位</div><div class="mini-value">流通科技管理</div></div>
              <div class="mini-card"><div class="mini-label">主題風格</div><div class="mini-value">蜜桃櫻花焦糖</div></div>
            </div>
          </div>
        </div>
        <div class="kpi-grid">
          <div class="kpi-card"><div class="kpi-label">庫存準確率目標</div><div class="kpi-value">99.2%</div><div class="kpi-delta">以條碼、批號與即時盤點支援</div></div>
          <div class="kpi-card"><div class="kpi-label">揀貨效率提升</div><div class="kpi-value">38%</div><div class="kpi-delta">依波次與儲位熱區重排</div></div>
          <div class="kpi-card"><div class="kpi-label">缺貨率下降</div><div class="kpi-value">0.8%</div><div class="kpi-delta">導入安全庫存預警</div></div>
          <div class="kpi-card"><div class="kpi-label">庫存週轉提升</div><div class="kpi-value">62%</div><div class="kpi-delta">搭配 ABC 分類與補貨節奏</div></div>
        </div>
        <div class="chart-card">
          <div class="card-head"><div><div class="label">Chart 01</div><h2 class="card-title">年度營收與目標趨勢</h2><p class="card-subtitle">以柔和亮色背景搭配漸層線條與延遲動畫，呈現 WMS 導入後的營收改善軌跡。</p></div><span class="tag">Line Animation</span></div>
          <div class="chart-wrap"><canvas id="revenueChart"></canvas></div>
        </div>
        <div class="chart-card">
          <div class="card-head"><div><div class="label">Chart 02</div><h2 class="card-title">市場佔有率分布</h2><p class="card-subtitle">使用環形圖呈現零售、製造、第三方物流與電商倉配的導入比例。</p></div><span class="tag">Doughnut Rotation</span></div>
          <div class="chart-wrap"><canvas id="marketChart"></canvas></div>
        </div>
      </section>

      <section id="problem" class="page" data-title="問題定義">
        <div class="card"><div class="card-head"><div><div class="label">Problem Definition</div><h2 class="card-title">傳統倉儲的四個斷點</h2><p class="card-subtitle">WMS 專題的問題意識，來自傳統倉儲在資料即時性、揀貨效率、庫存風險與跨通路協同上的落差。</p></div><span class="tag">痛點梳理</span></div>
          <div class="feature-grid">
            <div class="feature"><b>庫存資料延遲</b><p>紙本或批次匯入造成帳實不符，難以及時反映入庫、出庫與退貨狀態。</p></div>
            <div class="feature"><b>人工揀貨路徑冗長</b><p>缺乏波次與熱區儲位設計，使人員在倉內移動成本偏高。</p></div>
            <div class="feature"><b>缺貨與滯銷並存</b><p>未建立 EOQ 與安全庫存模型，容易同時出現高庫存成本與服務水準不足。</p></div>
            <div class="feature"><b>批號追溯困難</b><p>食品、零售與電商退貨需要批號、效期與來源追蹤，傳統管理難以快速回溯。</p></div>
            <div class="feature"><b>多通路訂單衝突</b><p>門市、網購、平台訂單競爭同一庫存，若無共享庫存邏輯易造成超賣。</p></div>
            <div class="feature"><b>管理決策不可視</b><p>缺少圖表化儀表板，主管不易判斷庫存週轉、缺貨風險與作業瓶頸。</p></div>
          </div>
        </div>
      </section>

      <section id="solution" class="page" data-title="解決方案">
        <div class="card"><div class="card-head"><div><div class="label">Solution Architecture</div><h2 class="card-title">WMS 核心解決方案骨架</h2><p class="card-subtitle">系統以「收貨、上架、儲位、補貨、揀貨、包裝、出貨、盤點、報表」作為主流程，並將庫存科學模型嵌入決策層。</p></div><span class="tag">核心骨架</span></div>
          <table>
            <thead><tr><th>模組</th><th>管理重點</th><th>科技應用</th></tr></thead>
            <tbody>
              <tr><td>入庫與驗收</td><td>採購到貨、條碼驗收、批號與效期登錄。</td><td>行動掃描、ASN 到貨通知、例外回報。</td></tr>
              <tr><td>儲位與補貨</td><td>以商品週轉率、體積與溫層條件配置儲位。</td><td>ABC 分類、熱區儲位、補貨門檻。</td></tr>
              <tr><td>揀貨與出貨</td><td>降低行走距離，提升訂單準時履約。</td><td>波次揀貨、路徑最佳化、出貨檢核。</td></tr>
              <tr><td>庫存決策</td><td>控制持有成本、缺貨風險與訂購批量。</td><td>EOQ、安全庫存、需求波動監測。</td></tr>
            </tbody>
          </table>
        </div>
        <div class="formula-grid">
          <div class="formula-card"><h3>經濟訂購量 EOQ</h3><div class="formula-box">$$EOQ = Q^* = \sqrt{\frac{2DS}{H}}$$</div><p class="formula-meaning">其中 \(D\) 為年度需求量，\(S\) 為單次訂購成本，\(H\) 為單位年度持有成本。此模型用來平衡訂購成本與持有成本。</p></div>
          <div class="formula-card"><h3>安全庫存模型</h3><div class="formula-box">$$SS = Z \times \sigma_L$$</div><p class="formula-meaning">其中 \(Z\) 為服務水準係數，\(\sigma_L\) 為前置時間內需求標準差。此模型用來吸收需求與交期波動，降低缺貨機率。</p></div>
        </div>
      </section>

      <section id="market" class="page" data-title="市場分析">
        <div class="chart-card"><div class="card-head"><div><div class="label">Chart 03</div><h2 class="card-title">季度成長分析</h2><p class="card-subtitle">比較本年度與上年度季度表現，並以柔和柱狀動畫呈現導入 WMS 後的成長差距。</p></div><span class="tag">Bar Delay</span></div><div class="chart-wrap"><canvas id="quarterlyChart"></canvas></div></div>
        <div class="card"><div class="card-head"><div><div class="label">Retail Insight</div><h2 class="card-title">零售選物場景中的 WMS 價值</h2><p class="card-subtitle">日系選物店、生活百貨與小型連鎖店常同時面對季節檔期、短生命週期商品與社群銷售波峰，因此需要更細緻的庫存視覺化與補貨節奏。</p></div><span class="tag">OMO</span></div></div>
      </section>

      <section id="product" class="page" data-title="產品功能">
        <div class="chart-card"><div class="card-head"><div><div class="label">Chart 04</div><h2 class="card-title">功能成熟度與使用者成長</h2><p class="card-subtitle">以雙軸感的視覺語彙呈現功能模組逐步成熟後，使用者採納率的提升。</p></div><span class="tag">Area Fill</span></div><div class="chart-wrap"><canvas id="usersChart"></canvas></div></div>
        <div class="feature-grid">
          <div class="feature"><b>即時庫存儀表板</b><p>以 SKU、倉別、批號、可售量與保留量呈現完整庫存狀態。</p></div>
          <div class="feature"><b>補貨預警</b><p>自動比較再訂購點與安全庫存，產生補貨建議清單。</p></div>
          <div class="feature"><b>行動作業</b><p>收貨、上架、移倉、揀貨與盤點均能透過行動裝置完成。</p></div>
        </div>
      </section>

      <section id="finance" class="page" data-title="財務規劃">
        <div class="chart-card"><div class="card-head"><div><div class="label">Chart 05</div><h2 class="card-title">預算配置與成本分析</h2><p class="card-subtitle">將系統開發、硬體設備、教育訓練、維運與資料整合費用視覺化，支援投資決策。</p></div><span class="tag">Polar Area</span></div><div class="chart-wrap"><canvas id="budgetChart"></canvas></div></div>
        <div class="card"><table><thead><tr><th>成本項目</th><th>投資目的</th><th>預期效益</th></tr></thead><tbody><tr><td>系統開發</td><td>建置 WMS 核心流程與報表。</td><td>降低人工紀錄與跨表整併成本。</td></tr><tr><td>掃描設備</td><td>支援條碼、批號與儲位掃描。</td><td>提升帳實一致性與追溯速度。</td></tr><tr><td>教育訓練</td><td>讓倉儲與門市人員掌握標準作業。</td><td>降低導入初期阻力與錯誤率。</td></tr></tbody></table></div>
      </section>

      <section id="risk" class="page" data-title="風險評估">
        <div class="chart-card"><div class="card-head"><div><div class="label">Chart 06</div><h2 class="card-title">綜合風險雷達圖</h2><p class="card-subtitle">以雷達圖評估資料品質、導入成本、流程改造、使用者接受度、系統穩定與供應商依賴。</p></div><span class="tag">Radar Sweep</span></div><div class="chart-wrap"><canvas id="riskChart"></canvas></div></div>
        <div class="feature-grid"><div class="feature"><b>資料風險</b><p>若品號、儲位或批號資料不完整，系統效益將明顯下降。</p></div><div class="feature"><b>流程風險</b><p>導入 WMS 前需重新定義收貨、上架與揀貨責任邊界。</p></div><div class="feature"><b>人員風險</b><p>需透過訓練與績效指標，降低對新流程的抗拒。</p></div></div>
      </section>

      <section id="roadmap" class="page" data-title="執行藍圖">
        <div class="card"><div class="card-head"><div><div class="label">Roadmap</div><h2 class="card-title">18 個月產品迭代藍圖</h2><p class="card-subtitle">執行藍圖以「先可視、再自動、後最佳化」為原則，逐步建立能被現場採用的 WMS 系統。</p></div><span class="tag">三階段導入</span></div>
          <div class="timeline">
            <div class="timeline-card"><div class="phase">0–3 個月</div><div><b>資料盤點與流程建模</b><p>建立商品、儲位、供應商與訂單資料字典，完成現行流程訪談。</p></div><span class="tag">Foundation</span></div>
            <div class="timeline-card"><div class="phase">4–9 個月</div><div><b>核心 WMS 模組上線</b><p>完成入庫、上架、移倉、揀貨、出貨與盤點模組，導入行動掃描作業。</p></div><span class="tag">Go-Live</span></div>
            <div class="timeline-card"><div class="phase">10–18 個月</div><div><b>庫存最佳化與決策儀表板</b><p>導入 EOQ、安全庫存、ABC 分類與管理視覺化，支援補貨策略調整。</p></div><span class="tag">Optimize</span></div>
          </div>
        </div>
      </section>

      <section id="inventory" class="page" data-title="存貨管理">
        <div class="card"><div class="card-head"><div><div class="label">Inventory Science</div><h2 class="card-title">庫存科學公式與動態引擎</h2><p class="card-subtitle">本頁將 EOQ 與安全庫存模型做成可調式計算器，輸入數值後會同步更新結果並重新觸發 MathJax 排版。</p></div><span class="tag">Dynamic MathJax</span></div>
          <div class="formula-grid">
            <div class="formula-card"><h3>EOQ 計算器</h3><div class="formula-box" id="eoqFormula">$$EOQ = \sqrt{\frac{2 \times 10000 \times 50}{5}} = 447.21$$</div><div class="inputs"><div><label>年度需求 D</label><input id="eoqD" type="number" value="10000"></div><div><label>訂購成本 S</label><input id="eoqS" type="number" value="50"></div><div><label>持有成本 H</label><input id="eoqH" type="number" value="5"></div></div><div class="button-row"><button class="btn" onclick="calculateEOQ()">重新計算 EOQ</button></div><div class="result" id="eoqResult"></div></div>
            <div class="formula-card"><h3>安全庫存計算器</h3><div class="formula-box" id="ssFormula">$$SS = 1.65 \times 1131 = 1866.15$$</div><div class="inputs"><div><label>服務係數 Z</label><input id="ssZ" type="number" value="1.65" step="0.01"></div><div><label>需求標準差 σ</label><input id="ssSigma" type="number" value="800"></div><div><label>前置時間 L</label><input id="ssLead" type="number" value="2"></div></div><div class="button-row"><button class="btn" onclick="calculateSS()">重新計算安全庫存</button></div><div class="result" id="ssResult"></div></div>
          </div>
        </div>
        <div class="case-grid">
          <div class="case-card"><h3>電子製造業</h3><p>年度採購 120,000 件、訂購成本 500 元、持有成本 8 元時，EOQ 約為 3,873 件，可降低訂購與持有的總成本。</p></div>
          <div class="case-card"><h3>零售連鎖店</h3><p>在服務水準 95% 下導入安全庫存，可將缺貨率由 5% 降至 0.8%，提升顧客滿意度與銷售穩定性。</p></div>
          <div class="case-card"><h3>食品飲料業</h3><p>以 ABC 分類集中管理高價值 SKU，使庫存投資降低 28%，並釋放更多營運資金。</p></div>
        </div>
      </section>

      <p class="footer-note">© WMS 與庫存最佳化專題。單一檔案整合：CSS 主題、九大頁面切換、六大 Chart.js 圖表、LaTeX 公式與 MathJax 動態排版。</p>
    </main>
  </div>

  <script>
    const pages = [
      ['overview', '執行摘要', '專題總覽'],
      ['problem', '問題定義', '市場缺口'],
      ['solution', '解決方案', 'WMS 骨架'],
      ['market', '市場分析', '成長洞察'],
      ['product', '產品功能', '模組能力'],
      ['finance', '財務規劃', '預算配置'],
      ['risk', '風險評估', '導入治理'],
      ['roadmap', '執行藍圖', '18 個月'],
      ['inventory', '存貨管理', 'EOQ / SS']
    ];

    const chartInstances = {};
    Chart.defaults.font.family = 'Noto Sans TC, Noto Sans JP, Microsoft JhengHei, sans-serif';
    Chart.defaults.color = '#8e7d72';
    Chart.defaults.borderColor = 'rgba(201,149,111,.22)';

    function buildNav() {
      const nav = document.getElementById('navList');
      nav.innerHTML = pages.map((p, i) => `<li><button class="nav-link ${i === 0 ? 'active' : ''}" data-page="${p[0]}" onclick="switchPage('${p[0]}')"><span class="nav-num">${String(i + 1).padStart(2, '0')}</span><span><span class="nav-title">${p[1]}</span><br><span class="nav-note">${p[2]}</span></span><span>›</span></button></li>`).join('');
    }

    function switchPage(pageId) {
      document.querySelectorAll('.page').forEach(page => page.classList.remove('active'));
      document.getElementById(pageId).classList.add('active');
      document.querySelectorAll('.nav-link').forEach(link => link.classList.toggle('active', link.dataset.page === pageId));
      const index = pages.findIndex(p => p[0] === pageId);
      document.getElementById('currentPage').textContent = pages[index][1];
      document.getElementById('progressText').textContent = `${index + 1} / ${pages.length}`;
      document.getElementById('progressFill').style.width = `${((index + 1) / pages.length) * 100}%`;
      window.scrollTo({ top: 0, behavior: 'smooth' });
      setTimeout(() => { renderChartsForPage(pageId); typesetMath(); }, 120);
    }

    function gradient(ctx, area, colors) {
      const g = ctx.createLinearGradient(0, area.bottom, 0, area.top);
      colors.forEach(stop => g.addColorStop(stop[0], stop[1]));
      return g;
    }

    function chartOptions(extra = {}) {
      return {
        responsive: true,
        maintainAspectRatio: false,
        animation: { duration: 1450, easing: 'easeOutQuart', delay: ctx => ctx.dataIndex ? ctx.dataIndex * 90 : 0 },
        plugins: { legend: { labels: { usePointStyle: true, padding: 18, font: { weight: '700' } } }, tooltip: { backgroundColor: 'rgba(61,51,45,.92)', padding: 14, cornerRadius: 12, titleColor: '#fff5ec', bodyColor: '#fffaf4' } },
        scales: { x: { grid: { display: false } }, y: { beginAtZero: true, grid: { color: 'rgba(201,149,111,.16)' } } },
        ...extra
      };
    }

    function renderRevenueChart() {
      if (chartInstances.revenueChart) return;
      const canvas = document.getElementById('revenueChart'); if (!canvas) return;
      const ctx = canvas.getContext('2d');
      chartInstances.revenueChart = new Chart(ctx, { type: 'line', data: { labels: ['Q1','Q2','Q3','Q4','Q5','Q6'], datasets: [{ label: '實際營收', data: [1.2,1.7,2.25,2.9,3.25,3.8], borderColor: '#ff9f7d', pointBackgroundColor: '#ff9f7d', tension: .42, fill: true, backgroundColor: c => { const a = c.chart.chartArea; return a ? gradient(ctx, a, [[0,'rgba(255,159,125,.02)'],[1,'rgba(255,159,125,.30)']]) : 'rgba(255,159,125,.18)'; } }, { label: '目標營收', data: [1.4,1.9,2.4,3.1,3.6,4.1], borderColor: '#ffb7cf', pointBackgroundColor: '#ffb7cf', tension: .42, borderDash: [6,6], fill: false }] }, options: chartOptions() });
    }

    function renderMarketChart() {
      if (chartInstances.marketChart) return;
      const ctx = document.getElementById('marketChart')?.getContext('2d'); if (!ctx) return;
      chartInstances.marketChart = new Chart(ctx, { type: 'doughnut', data: { labels: ['零售連鎖','製造業','第三方物流','電商倉配'], datasets: [{ data: [34, 27, 22, 17], backgroundColor: ['#ff9f7d','#ffb7cf','#d7ad79','#f1dec0'], borderColor: '#fffaf4', borderWidth: 5, hoverOffset: 14 }] }, options: chartOptions({ cutout: '62%', rotation: -90, animation: { animateRotate: true, animateScale: true, duration: 1500, easing: 'easeOutQuart' }, scales: {} }) });
    }

    function renderQuarterlyChart() {
      if (chartInstances.quarterlyChart) return;
      const ctx = document.getElementById('quarterlyChart')?.getContext('2d'); if (!ctx) return;
      chartInstances.quarterlyChart = new Chart(ctx, { type: 'bar', data: { labels: ['Q1','Q2','Q3','Q4'], datasets: [{ label: '去年', data: [55,62,68,73], backgroundColor: 'rgba(241,222,192,.82)', borderRadius: 14 }, { label: '今年', data: [70,84,96,112], backgroundColor: 'rgba(255,159,125,.80)', borderRadius: 14 }] }, options: chartOptions({ animation: { duration: 1200, easing: 'easeOutBack', delay: ctx => ctx.datasetIndex * 180 + ctx.dataIndex * 90 } }) });
    }

    function renderUsersChart() {
      if (chartInstances.usersChart) return;
      const ctx = document.getElementById('usersChart')?.getContext('2d'); if (!ctx) return;
      chartInstances.usersChart = new Chart(ctx, { type: 'line', data: { labels: ['收貨','上架','儲位','揀貨','出貨','盤點','報表'], datasets: [{ label: '功能成熟度', data: [48,56,66,72,81,88,94], borderColor: '#d7ad79', pointBackgroundColor: '#d7ad79', tension: .4, fill: true, backgroundColor: 'rgba(215,173,121,.18)' }, { label: '使用者採納率', data: [30,42,55,63,74,82,90], borderColor: '#ffb7cf', pointBackgroundColor: '#ffb7cf', tension: .4, fill: true, backgroundColor: 'rgba(255,183,207,.16)' }] }, options: chartOptions() });
    }

    function renderBudgetChart() {
      if (chartInstances.budgetChart) return;
      const ctx = document.getElementById('budgetChart')?.getContext('2d'); if (!ctx) return;
      chartInstances.budgetChart = new Chart(ctx, { type: 'polarArea', data: { labels: ['系統開發','硬體設備','教育訓練','維運支援','資料整合'], datasets: [{ data: [38, 22, 14, 16, 10], backgroundColor: ['rgba(255,159,125,.78)','rgba(255,183,207,.72)','rgba(215,173,121,.70)','rgba(241,222,192,.82)','rgba(185,139,109,.62)'], borderColor: '#fffaf4', borderWidth: 4 }] }, options: chartOptions({ scales: { r: { grid: { color: 'rgba(201,149,111,.16)' }, ticks: { display: false } } }, animation: { animateRotate: true, animateScale: true, duration: 1450 } }) });
    }

    function renderRiskChart() {
      if (chartInstances.riskChart) return;
      const ctx = document.getElementById('riskChart')?.getContext('2d'); if (!ctx) return;
      chartInstances.riskChart = new Chart(ctx, { type: 'radar', data: { labels: ['資料品質','導入成本','流程改造','接受度','系統穩定','供應商依賴'], datasets: [{ label: '導入前風險', data: [82,74,86,70,63,58], borderColor: '#ff9f7d', backgroundColor: 'rgba(255,159,125,.18)', pointBackgroundColor: '#ff9f7d' }, { label: '治理後風險', data: [38,44,41,35,28,32], borderColor: '#d7ad79', backgroundColor: 'rgba(215,173,121,.18)', pointBackgroundColor: '#d7ad79' }] }, options: chartOptions({ scales: { r: { beginAtZero: true, max: 100, grid: { color: 'rgba(201,149,111,.18)' }, angleLines: { color: 'rgba(201,149,111,.18)' }, pointLabels: { color: '#7f6453', font: { weight: '800' } } } } }) });
    }

    function renderChartsForPage(pageId) {
      if (pageId === 'overview') { renderRevenueChart(); renderMarketChart(); }
      if (pageId === 'market') renderQuarterlyChart();
      if (pageId === 'product') renderUsersChart();
      if (pageId === 'finance') renderBudgetChart();
      if (pageId === 'risk') renderRiskChart();
    }

    function typesetMath() {
      if (window.MathJax?.typesetPromise) MathJax.typesetPromise().catch(() => {});
    }

    function n(id, fallback = 0) { return Number(document.getElementById(id)?.value || fallback); }
    function fmt(x) { return Number.isFinite(x) ? x.toLocaleString('zh-TW', { maximumFractionDigits: 2 }) : '-'; }

    function calculateEOQ() {
      const D = n('eoqD'), S = n('eoqS'), H = n('eoqH');
      const value = H > 0 ? Math.sqrt((2 * D * S) / H) : NaN;
      document.getElementById('eoqFormula').innerHTML = `$$EOQ = \\sqrt{\\frac{2 \\times ${fmt(D)} \\times ${fmt(S)}}{${fmt(H)}}} = ${fmt(value)}$$`;
      const box = document.getElementById('eoqResult'); box.style.display = 'block'; box.textContent = `建議最適訂購批量：${fmt(value)} 件`;
      typesetMath();
    }

    function calculateSS() {
      const Z = n('ssZ'), sigma = n('ssSigma'), L = n('ssLead');
      const sigmaL = sigma * Math.sqrt(Math.max(L, 0));
      const value = Z * sigmaL;
      document.getElementById('ssFormula').innerHTML = `$$SS = ${fmt(Z)} \\times (${fmt(sigma)} \\times \\sqrt{${fmt(L)}}) = ${fmt(value)}$$`;
      const box = document.getElementById('ssResult'); box.style.display = 'block'; box.textContent = `建議安全庫存：${fmt(value)} 件`;
      typesetMath();
    }

    function typewriterEffect() {
      const words = ['庫存最佳化', '日系零售選物', '蜜桃櫻花焦糖', '流通科技管理'];
      const el = document.getElementById('typewriter');
      let wi = 0, ci = 0, del = false;
      const tick = () => {
        const word = words[wi];
        el.textContent = del ? word.slice(0, ci--) : word.slice(0, ci++);
        if (!del && ci > word.length + 8) del = true;
        if (del && ci < 0) { del = false; wi = (wi + 1) % words.length; ci = 0; }
        setTimeout(tick, del ? 52 : 88);
      };
      tick();
    }

    window.addEventListener('load', () => {
      buildNav();
      renderChartsForPage('overview');
      calculateEOQ();
      calculateSS();
      typewriterEffect();
      setTimeout(typesetMath, 350);
    });
  </script>
</body>
</html>
