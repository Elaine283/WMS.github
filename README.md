<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>倉儲管理系統 (WMS) 與庫存最佳化 · 專題研究報告</title>
  <link rel="stylesheet" href="./styles.css">
  
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  
  <script>
    window.MathJax = {
      tex: {
        inlineMath: [['$', '$'], ['\\(', '\\)']],
        displayMath: [['$$', '$$'], ['\\[', '\\]']]
      },
      svg: { fontCache: 'global' }
    };
  </script>
  <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
</head>
<body>

  <div id="sidebarOverlay" class="sidebar-overlay"></div>

  <div class="app">
    <aside id="sidebar" class="sidebar" aria-label="章節導航">
      <div class="sidebar__brand">
        <div class="sidebar__logo">WMS · Inventory</div>
        <div id="sidebarTitle" class="sidebar__title">倉儲管理系統 (WMS) 與庫存最佳化</div>
        <div id="sidebarMeta" class="sidebar__meta">陳玉鳳、洪依辰</div>
      </div>
      <nav id="sidebarNav" class="sidebar__nav" aria-label="目錄"></nav>
      <div class="sidebar__footer"><span id="footerVersion">v1.3.0</span></div>
    </aside>

    <div class="main">
      <header class="topbar">
        <button id="menuBtn" class="topbar__menu" type="button" aria-label="開啟選單">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" aria-hidden="true">
            <path d="M4 6h16M4 12h16M4 18h16" />
          </svg>
        </button>
        <p id="topbarSection" class="topbar__section">WMS 專題報告</p>
        <div class="topbar__actions">
          <button id="printBtn" class="btn" type="button">列印</button>
          <a href="#overview" class="btn btn--primary">首頁</a>
        </div>
      </header>

      <div class="content">
        <section id="overview" class="section hero">
          <span class="hero__badge">專題報告 · WMS &amp; 庫存最佳化</span>
          <h1 id="heroTitle" class="hero__title">倉儲管理系統 (WMS) 與庫存最佳化</h1>
          <p id="heroSubtitle" class="hero__subtitle">
            以 WMS 串聯收貨、入庫、儲位、盤點、揀貨、出貨全流程，結合條碼與庫存最佳化模型，協助零售電商提升透明度、正確性與出貨效率。
          </p>
       <dl class="hero__meta">
            <div>
              <dt>作者</dt>
              <dd id="heroAuthor">陳玉鳳、洪依辰</dd>
            </div>
            <div>
              <dt>指導教授</dt>
              <dd id="heroAdvisor">褚文明</dd>
            </div>
            <div>
              <dt>系所</dt>
              <dd id="heroDept">勤益科大 行銷與流通管理系所</dd>
            </div>
            <div>
              <dt>班級</dt>
              <dd id="heroClass">院二流三甲</dd>
            </div>
            <div>
              <dt>科目</dt>
              <dd id="heroSubject">流通科技管理</dd>
            </div>
          </dl>
          
          <div id="kpiGrid" class="grid-kpi" style="margin-top: 2.5rem"></div>
          
          <div class="card" style="margin-top: 2rem">
            <h3 class="card__title">簡報架構（對應 WMS 實務七大主題）</h3>
            <nav class="toc-grid" aria-label="章節捷徑">
              <a class="toc-item" href="#background"><span class="toc-item__num">01</span> 研究背景</a>
              <a class="toc-item" href="#comparison"><span class="toc-item__num">02</span> vs 傳統作業</a>
              <a class="toc-item" href="#wms-modules"><span class="toc-item__num">03</span> 功能與流程</a>
              <a class="toc-item" href="#inventory-optimization"><span class="toc-item__num">04</span> 庫存最佳化</a>
              <a class="toc-item" href="#ecommerce"><span class="toc-item__num">05</span> 電商應用</a>
              <a class="toc-item" href="#analytics"><span class="toc-item__num">06</span> 數據與 KPI</a>
              <a class="toc-item" href="#future"><span class="toc-item__num">07</span> 選型與展望</a>
            </nav>
          </div>
        </section>

        <section id="background" class="section">
          <p class="section__eyebrow">02 · Background</p>
          <h2 class="section__title">研究背景</h2>
          <p class="section__lead">
            零售與電商訂單量成長、SKU 多元化，倉儲與物流管理成為競爭關鍵；庫存堆滿卻仍缺貨、出錯，正是導入 WMS 的契機。
          </p>
          <blockquote class="pain-quote">
            「庫存堆滿倉庫，卻還是天天缺貨、出錯？訂單爆量時，揀貨、包裝亂成一團？」——
            此為電商倉儲常見痛點，亦為本專題導入 <strong>WMS 與庫存最佳化</strong> 之實務動機。
          </blockquote>
          <div class="prose card">
            <p>
              在零售、電商產業中，隨著訂單量快速增加、商品種類多樣化，倉儲與物流管理的重要性日益提升。多通路（官網、蝦皮、MOMO 等）訂單若靠人工同步庫存，極易發生<strong>帳實不符、寄錯漏寄、促銷高峰出貨混亂</strong>等問題。
            </p>
            <p>
              <strong>WMS（Warehouse Management System）</strong>是專門管理倉庫作業流程的資訊系統，目的在提升進出貨透明度與正確性，支援收貨、入庫、儲位管理、盤點、揀貨、出貨等核心作業。傳統 Excel 在小量時尚可應付，一旦品項與訂單增加，便難以維持效率與準確度。
            </p>
            <p><strong>WMS 能協助企業：</strong></p>
            <ul>
              <li>看得清楚庫存數量與儲位</li>
              <li>整理好進貨、出貨流程</li>
              <li>降低人工作業錯誤</li>
              <li>加快倉儲整體運作速度</li>
            </ul>
            <div class="callout">
              <strong>全方位掌握貨況的關鍵：</strong>條碼（Barcode／QR Code）掃描即時更新雲端資料，並串接 ERP、電商 API，從叫貨到出貨整條供應鏈透明化——<strong>WMS 不只是管倉庫，更是連動供應鏈的智慧中樞。</strong>
            </div>
            <p>
              本專題由<strong>陳玉鳳、洪依辰</strong>（勤益科大 行銷與流通管理系所）在<strong>褚文明</strong>教授指導下，整合 WMS 實務架構與 ABC、安全庫存、EOQ 等<strong>庫存最佳化</strong>模型，建構可視化決策原型。
            </p>
          </div>
          <p class="source-note">
            實務參考：<a href="https://www.jenjan.com.tw/ecommerce/articles/45" target="_blank" rel="noopener">JENJAN — WMS 倉儲管理系統功能、流程與應用解析</a>
          </p>
        </section>

        <section id="comparison" class="section">
          <p class="section__eyebrow">03 · Comparison</p>
          <h2 class="section__title">WMS vs. 傳統人工作業</h2>
          <p class="section__lead">
            傳統靠經驗與人工登錄，WMS 靠系統與條碼校驗——效率與正確性的結構性差異。
          </p>
          <div class="table-wrap table-wrap--compare">
            <table>
              <thead>
                <tr>
                  <th>比較項目</th>
                  <th>傳統人工作業</th>
                  <th>WMS 倉儲管理系統</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>庫存更新</td>
                  <td>人工登記，易遺漏</td>
                  <td>自動記錄，系統即時同步</td>
                </tr>
                <tr>
                  <td>揀貨出貨</td>
                  <td>靠經驗找貨、效率低</td>
                  <td>系統派單、最佳動線揀貨</td>
                </tr>
                <tr>
                  <td>錯誤率</td>
                  <td>高（易寄錯、少寄）</td>
                  <td>低（條碼掃描校驗）</td>
                </tr>
                <tr>
                  <td>訂單處理</td>
                  <td>難以同步多平台</td>
                  <td>可整合多通路電商訂單</td>
                </tr>
                <tr>
                  <td>成本控制</td>
                  <td>需大量人力</td>
                  <td>減少錯誤與重工成本</td>
                </tr>
                <tr>
                  <td>決策支援</td>
                  <td>缺乏即時指標</td>
                  <td>出貨率、週轉率等可視化</td>
                </tr>
              </tbody>
            </table>
          </div>
          <p class="quote-inline">傳統靠經驗，WMS 靠系統；前者效率易受人力與訂單波動影響，後者有助穩定成長。（參考 JENJAN, 2025）</p>
        </section>

        <section id="objectives" class="section">
          <p class="section__eyebrow">04 · Objectives</p>
          <h2 class="section__title">研究目的</h2>
          <p class="section__lead">對應零售電商導入 WMS 之五大效益，並以庫存模型強化補貨決策。</p>
          <div class="grid-2">
            <div class="card">
              <h3 class="card__title">主要目的</h3>
              <ol class="prose">
                <li>梳理 WMS 五大模組與標準作業流程（SOP）</li>
                <li>建構原型：條碼入庫、儲位、揀貨、出貨與 Dashboard</li>
                <li>整合 ABC、安全庫存、EOQ 補貨建議</li>
                <li>以模擬數據驗證 KPI 改善（準確率、週轉率、準時率）</li>
              </ol>
            </div>
            <div class="card">
              <h3 class="card__title">對應電商 WMS 效益</h3>
              <ul class="benefit-list">
                <li>提升庫存準確性，減少缺貨、錯貨、積壓</li>
                <li>加速出貨，應對雙 11、黑色星期五等訂單高峰</li>
                <li>降低人力依賴與作業錯誤率</li>
                <li>支援多平台、多倉庫集中管理</li>
                <li>出貨率、週轉率即時可視，輔助決策</li>
              </ul>
            </div>
          </div>
        </section>

        <section id="wms-modules" class="section">
          <p class="section__eyebrow">05 · WMS Core</p>
          <h2 class="section__title">WMS 主要功能與運作流程</h2>
          <p class="section__lead">
            完整 WMS 涵蓋進貨、庫存、儲位、揀貨、出貨五大模組，並依標準流程串聯作業。
          </p>
          <div class="card" style="margin-bottom: 1rem">
            <h3 class="card__title">五大核心模組</h3>
            <div class="module-grid">
              <div class="module-card">
                <div class="module-card__en">Inbound</div>
                <div class="module-card__title">進貨管理</div>
                <div class="module-card__desc">收貨登錄數量、批次、效期，快速入庫</div>
              </div>
              <div class="module-card">
                <div class="module-card__en">Inventory</div>
                <div class="module-card__title">庫存管理</div>
                <div class="module-card__desc">即時 SKU 庫存，多倉多儲位，防缺貨積貨</div>
              </div>
              <div class="module-card">
                <div class="module-card__en">Location</div>
                <div class="module-card__title">儲位管理</div>
                <div class="module-card__desc">空間利用率、動線與熱區最佳化</div>
              </div>
              <div class="module-card">
                <div class="module-card__en">Picking</div>
                <div class="module-card__title">揀貨管理</div>
                <div class="module-card__desc">自動揀貨單、路徑優化、分揀作業</div>
              </div>
              <div class="module-card">
                <div class="module-card__en">Outbound</div>
                <div class="module-card__title">出貨管理</div>
                <div class="module-card__desc">打包、標籤、物流串接、狀態回傳平台</div>
              </div>
            </div>
            
            <h3 class="card__title" style="margin-top: 1.25rem">擴充模組（進階 WMS）</h3>
            <div class="module-grid">
              <div class="module-card module-card--ext">
                <div class="module-card__en">Returns</div>
                <div class="module-card__title">退貨管理</div>
                <div class="module-card__desc">退件入庫、檢驗、重新上架或報廢</div>
              </div>
              <div class="module-card module-card--ext">
                <div class="module-card__en">Multi-DC</div>
                <div class="module-card__title">跨倉調撥</div>
                <div class="module-card__desc">多倉庫庫存視圖與調撥作業</div>
              </div>
              <div class="module-card module-card--ext">
                <div class="module-card__en">Analytics</div>
                <div class="module-card__title">報表分析</div>
                <div class="module-card__desc">週轉率、出貨率、呆滯品分析</div>
              </div>
            </div>
          </div>
          
          <div class="card" style="margin-top: 1rem">
            <h3 class="card__title">條碼掃描後，系統可掌握</h3>
            <ul class="benefit-list">
              <li>貨物流向：從哪來、現在在哪、何時出貨</li>
              <li>效期控管：哪些批次快過期，提早處理</li>
              <li>系統串接：訂單進來 → 自動備貨 → 安排出貨（ERP／電商 API）</li>
            </ul>
          </div>
          
          <div class="card" style="margin-top: 1rem">
            <h3 class="card__title">典型 WMS 運作流程（六步驟）</h3>
            <div class="flow-steps">
              <div class="flow-step">
                <span class="flow-step__num">1</span>
                <div>
                  <div class="flow-step__title">資料建檔</div>
                  <div class="flow-step__desc">建立商品、供應商、使用者與訂單主檔，設定系統參數。</div>
                </div>
              </div>
              <div class="flow-step">
                <span class="flow-step__num">2</span>
                <div>
                  <div class="flow-step__title">進貨收貨</div>
                  <div class="flow-step__desc">供應商送貨 → 掃描入庫 → 自動分配儲位，必要時品檢與貼標。</div>
                </div>
              </div>
              <div class="flow-step">
                <span class="flow-step__num">3</span>
                <div>
                  <div class="flow-step__title">儲位管理</div>
                  <div class="flow-step__desc">依商品特性與出貨頻率安排儲位，縮短揀貨動線。</div>
                </div>
              </div>
              <div class="flow-step">
                <span class="flow-step__num">4</span>
                <div>
                  <div class="flow-step__title">庫存管理與盤點</div>
                  <div class="flow-step__desc">記錄每次異動，定期或即時盤點，確保帳實一致。</div>
                </div>
              </div>
              <div class="flow-step">
                <span class="flow-step__num">5</span>
                <div>
                  <div class="flow-step__title">出貨揀貨</div>
                  <div class="flow-step__desc">依訂單產生揀貨單，指引最短路徑作業。</div>
                </div>
              </div>
              <div class="flow-step">
                <span class="flow-step__num">6</span>
                <div>
                  <div class="flow-step__title">包裝與出貨</div>
                  <div class="flow-step__desc">完成打包、產生物流單，出貨狀態回傳至銷售平台。</div>
                </div>
              </div>
            </div>
          </div>
        </section>

        <section id="inventory-optimization" class="section">
          <p class="section__eyebrow">06 · Inventory Optimization</p>
          <h2 class="section__title">庫存最佳化與 WMS 整合</h2>
          <p class="section__lead">
            WMS 負責「作業正確與效率」，庫存模型負責「訂多少、放哪、何時補」——兩者結合方能同時改善服務水準與資金效率。
          </p>
          <div class="grid-2" style="margin-bottom: 1rem">
            <div class="card prose">
              <h3 class="card__title">ABC 分類法</h3>
              <p>依帕累托原則：約 20% 品項貢獻 80% 金額（A 類），資源集中於高價值 SKU。</p>
              <p><strong>WMS 應用：</strong> A 類靠近出貨熱區、高頻盤點；C 類可放較遠儲位。</p>
            </div>
            <div class="card prose">
              <h3 class="card__title">安全庫存</h3>
              <p>因應需求與供應不確定之緩衝庫存，平衡缺貨風險與持有成本。</p>
              <p>$$\text{安全庫存} = Z \times \sigma_{LT} \times \sqrt{LT}$$</p>
            </div>
            <div class="card prose">
              <h3 class="card__title">EOQ 經濟訂購量</h3>
              <p>使訂購成本與持有成本總和最小化之每次訂購量。</p>
              <p>$$EOQ = \sqrt{\frac{2DS}{H}}$$</p>
            </div>
            <div class="card prose">
              <h3 class="card__title">批次／效期管理</h3>
              <p>食品、藥品等品類需於 WMS 記錄批次與效期，避免出貨即期品或呆滯。與進貨管理模組、報表模組緊密連動。</p>
            </div>
          </div>
          <div class="table-wrap">
            <table class="integration-matrix">
              <thead>
                <tr>
                  <th>庫存策略</th>
                  <th>解決問題</th>
                  <th>對應 WMS 模組</th>
                  <th>本專題 KPI</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>ABC</td>
                  <td>資源配置、儲位優先</td>
                  <td>儲位管理、報表</td>
                  <td>倉儲利用率 87%</td>
                </tr>
                <tr>
                  <td>安全庫存</td>
                  <td>缺貨、斷貨</td>
                  <td>庫存管理、警示</td>
                  <td>缺貨率 2.1%</td>
                </tr>
                <tr>
                  <td>EOQ</td>
                  <td>訂購次數與持有成本</td>
                  <td>進貨管理、補貨建議</td>
                  <td>持有成本 -28%</td>
                </tr>
                <tr>
                  <td>即時帳務</td>
                  <td>帳實不符</td>
                  <td>盤點、條碼異動</td>
                  <td>準確率 98.6%</td>
                </tr>
              </tbody>
            </table>
          </div>
        </section>

        <section id="ecommerce" class="section">
          <p class="section__eyebrow">07 · E-Commerce</p>
          <h2 class="section__title">零售電商為何特別需要 WMS？</h2>
          <p class="section__lead">
            除官網外，還需同步蝦皮、MOMO、PChome 等多平台訂單；若靠人工更新庫存，極難在促銷檔期維持正確出貨。
          </p>
          <div class="channel-tags">
            <span class="channel-tag">品牌官網</span>
            <span class="channel-tag">蝦皮</span>
            <span class="channel-tag">MOMO</span>
            <span class="channel-tag">PChome</span>
            <span class="channel-tag">直播／社群電商</span>
          </div>
          <div class="grid-2">
            <div class="card prose">
              <h3 class="card__title">常見痛點 → WMS 對策</h3>
              <ul>
                <li><strong>多平台庫存不同步</strong> → API 整合、單一庫存池</li>
                <li><strong>大促爆單揀貨混亂</strong> → 波次揀貨、路徑優化</li>
                <li><strong>寄錯漏寄</strong> → 條碼掃描校驗</li>
                <li><strong>效期商品風險</strong> → 批次／效期欄位與警示</li>
              </ul>
            </div>
            <div class="card">
              <h3 class="card__title">電商導入 WMS 效益（文獻整理）</h3>
              <ul class="benefit-list">
                <li>提升庫存準確性，減少缺貨、錯貨、積壓</li>
                <li>自動化流程加速出貨，支撐檔期高峰</li>
                <li>降低對經驗型倉管依賴與錯誤率</li>
                <li>多平台、多倉庫集中管理</li>
                <li>出貨率、週轉率即時可視，輔助經營決策</li>
              </ul>
            </div>
          </div>
          <div class="callout" style="margin-top: 1rem">
            <strong>本專題定位：</strong>以行銷與流通管理視角，將電商倉儲實務（WMS）與庫存科學（ABC／安全庫存／EOQ）整合於同一簡報與原型，補足「只談系統不談模型」或「只談模型不談作業」之缺口。
          </div>
        </section>

        <section id="literature" class="section">
          <p class="section__eyebrow">08 · Literature</p>
          <h2 class="section__title">文獻探討</h2>
          <p class="section__lead">WMS 作業流程與庫存管理理論之整理。</p>
          <div class="table-wrap">
            <table>
              <thead>
                <tr>
                  <th>主題</th>
                  <th>理論／實務</th>
                  <th>本專題應用</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>WMS 實務</td>
                  <td>進貨／庫存／儲位／揀貨／出貨五大模組；條碼＋API 串接</td>
                  <td>模組地圖、六步流程、架構分層</td>
                </tr>
                <tr>
                  <td>零售電商倉儲</td>
                  <td>多通路訂單、促銷高峰、帳實一致需求</td>
                  <td>KPI 儀表板、多平台整合規劃</td>
                </tr>
                <tr>
                  <td>產業實務（JENJAN, 2025）</td>
                  <td>WMS 定義、比較表、選型要項、電商效益</td>
                  <td>簡報架構、電商章、選型對照</td>
                </tr>
                <tr>
                  <td>ABC 分析</td>
                  <td>帕累托 80/20，A 類 20% 品項占 80% 金額</td>
                  <td>儲位優先與盤點頻率差異化</td>
                </tr>
                <tr>
                  <td>安全庫存</td>
                  <td>Z × σ × √LT，服務水準 95%</td>
                  <td>補貨警示與效期／批次管理</td>
                </tr>
                <tr>
                  <td>EOQ</td>
                  <td>√(2DS/H)，平衡訂購與持有成本</td>
                  <td>建議訂購量、持有成本降 28%</td>
                </tr>
              </tbody>
            </table>
          </div>
        </section>

        <section id="methodology" class="section">
          <p class="section__eyebrow">09 · Methodology</p>
          <h2 class="section__title">研究方法</h2>
          <p class="section__lead">文獻分析、系統開發與模擬數據驗證。</p>
          <div class="card" style="margin-bottom: 1rem">
            <h3 class="card__title">研究時程</h3>
            <div id="timeline"></div>
          </div>
          <div class="grid-2">
            <div class="card prose">
              <h3 class="card__title">資料來源</h3>
              <ul>
                <li>6 個月模擬入出庫紀錄（單位：千件）</li>
                <li>五區倉庫利用率與 ABC 金額分布</li>
                <li>倉管人員訪談 3 場（需求與痛點）</li>
              </ul>
            </div>
            <div class="card prose">
              <h3 class="card__title">驗證指標</h3>
              <ul>
                <li>庫存週轉率、準確率、缺貨率</li>
                <li>訂單準時率、揀貨效率</li>
                <li>倉儲利用率（A–E 倉）</li>
              </ul>
            </div>
          </div>
        </section>

        <section id="architecture" class="section">
          <p class="section__eyebrow">10 · Architecture</p>
          <h2 class="section__title">WMS 系統架構</h2>
          <p class="section__lead">串接電商平台、ERP 與物流 API，條碼驅動庫存異動，支援庫存最佳化演算。</p>
          <div class="card">
            <div class="arch__layer">
              <span class="arch__label">串接</span>
              <div class="arch__blocks">
                <span class="arch__block">電商平台 API</span>
                <span class="arch__block">ERP / OMS</span>
                <span class="arch__block">物流商出貨</span>
              </div>
            </div>
            <div class="arch__layer">
              <span class="arch__label">展示</span>
              <div class="arch__blocks">
                <span class="arch__block">庫存 Dashboard</span>
                <span class="arch__block">進貨／揀貨／出貨介面</span>
                <span class="arch__block">ABC / 效期報表</span>
              </div>
            </div>
            <div class="arch__layer">
              <span class="arch__label">運算中心</span>
              <div class="arch__blocks">
                <span class="arch__block">EOQ 最佳化引擎</span>
                <span class="arch__block">動態安全庫存配置</span>
                <span class="arch__block">波次路徑演算法</span>
              </div>
            </div>
          </div>
        </section>

        <section id="analytics" class="section">
          <p class="section__eyebrow">11 · Data Analytics</p>
          <h2 class="section__title">大數據趨勢與實驗效益驗證</h2>
          <p class="section__lead">以下數據與圖表經由專案專屬演算法演算得出，由 `project.json` 數據檔案驅動，前端即時解構動態圖形 [cite: 4]。</p>
          
          <div class="chart-grid">
            <div class="card chart-card--wide">
              <div class="card__title">庫存週轉率（次/月）動態增長趨勢對比</div>
              <div class="chart-wrap">
                <canvas id="chartTurnover"></canvas>
              </div>
            </div>
            
            <div class="card">
              <div class="card__title">每月入庫 vs 出庫總件數 (千件)</div>
              <div class="chart-wrap chart-wrap--sm">
                <canvas id="chartInboundOutbound"></canvas>
              </div>
            </div>

            <div class="card">
              <div class="card__title">實體倉庫空間平均利用率 (%)</div>
              <div class="chart-wrap chart-wrap--sm">
                <canvas id="chartWarehouse"></canvas>
              </div>
            </div>

            <div class="card">
              <div class="card__title">當月作業類別件數分佈比較</div>
              <div class="chart-wrap chart-wrap--sm">
                <canvas id="chartInventory"></canvas>
              </div>
            </div>

            <div class="card">
              <div class="card__title">庫存商品金額 ABC 分級佔比</div>
              <div class="chart-wrap chart-wrap--sm">
                <canvas id="chartAbc"></canvas>
              </div>
            </div>
            
            <div class="card chart-card--wide">
              <div class="card__title">導入 WMS 前後綜合核心效益對比</div>
              <div class="chart-wrap" style="height: 250px;">
                <canvas id="chartWmsImpact"></canvas>
              </div>
            </div>
          </div>
        </section>

        <section id="future" class="section">
          <p class="section__eyebrow">12 · Future &amp; CI/CD</p>
          <h2 class="section__title">系統維運、選型與未來展望</h2>
          <p class="section__lead">本研究專題不僅建立了完善的供應鏈數學模型，在系統發布、自動部署（CI/CD）也採用現代化最高規格 [cite: 10]。</p>
          
          <div class="grid-2">
            <div class="card prose">
              <h3 class="card__title">⚙️ GitHub Actions 運維升級配置</h3>
              <p>為了徹底解決 2026 年中旬全面淘汰舊版環境的維運問題，本專案已在自動部署管線（Workflow）中，全域強制注入以下防禦變數：</p>
              <pre style="background: var(--bg-base); padding: 0.75rem; border-radius: var(--radius); font-family: var(--font-mono); font-size: 0.75rem; border: 1px solid var(--border); overflow-x: auto; color: var(--accent);">
env:
  FORCE_JAVASCRIPT_ACTIONS_TO_NODE24: true</pre>
              <p>此配置能令整個靜態簡報系統即便在 Node.js 20 完全移除執行器後，依然能夠藉由最新的 Node.js 24 引擎高速、順暢且無中斷地編譯上線 [cite: 5, 9]。 </p>
            </div>
            
            <div class="card prose">
              <h3 class="card__title">🚀 未來展望與進階升級方向</h3>
              <ul>
                <li><strong>AI 智能路徑配置：</strong> 未來可融合大數據與遺傳演算法，讓手持 PDA 揀貨路徑達成自適應動態調度。</li>
                <li><strong>IoT 自動化無縫結合：</strong> 架構具備高擴充性，未來可向電子標籤、AMR 移動機器人或自動化立體倉儲 (AS/RS) 進行串接 [cite: 8]。</li>
              </ul>
            </div>
          </div>
        </section>

      </div> </div> </div> <script type="module" src="./app.js"></script>
</body>
</html>
