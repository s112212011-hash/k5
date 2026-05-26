# k5
<html lang="zh-Hant" class="scroll-smooth">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kura Sushi - TP0 Analysis</title>
  
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            kuraRed: { DEFAULT: '#E60012', hover: '#C00010' },
            kuraDark: { 900: '#0F0F12', 800: '#18181F', 700: '#252530' }
          }
        }
      }
    }
  </script>
  
  <!-- Icons & Fonts -->
  <script src="https://unpkg.com/lucide@latest"></script>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;700;900&display=swap" rel="stylesheet">
  
  <style>
    body { font-family: 'Noto Sans TC', sans-serif; }
    #progress-bar { position: fixed; top: 0; left: 0; height: 4px; background: #E60012; z-index: 100; width: 0%; transition: width 0.1s; }
    .audio-highlight { background-color: rgba(250, 204, 21, 0.4); border-radius: 4px; padding: 2px; }
    .wave-bg { background-image: radial-gradient(circle at 10px 10px, rgba(230,0,18,0.05) 2px, transparent 0); background-size: 30px 30px; }
  </style>
</head>
<body class="bg-gray-50 text-gray-900 dark:bg-kuraDark-900 dark:text-gray-100 transition-colors duration-300 relative overflow-x-hidden">

  <!-- 頂部進度條 -->
  <div id="progress-bar"></div>

  <!-- 語音導覽控制面板 -->
  <div class="fixed bottom-6 right-6 z-50 flex flex-col items-end gap-2 max-w-[320px] w-full px-4">
    <button id="backToTop" class="hidden p-3 bg-kuraRed text-white rounded-full shadow-lg hover:scale-110 transition-transform"><i data-lucide="arrow-up" class="w-5 h-5"></i></button>
    <div class="bg-white/95 dark:bg-kuraDark-800/95 backdrop-blur-md p-4 rounded-3xl shadow-2xl border border-gray-200 dark:border-gray-700 w-full">
      <div class="flex justify-between items-center border-b border-gray-200 dark:border-gray-700 pb-2 mb-2">
        <div class="flex items-center gap-2">
          <span class="relative flex h-3 w-3"><span id="audioPulse" class="animate-ping absolute h-full w-full rounded-full bg-red-400 opacity-75 hidden"></span><span class="relative rounded-full h-3 w-3 bg-red-600"></span></span>
          <span class="font-bold text-xs" data-en="AI Narrator" data-zh="AI 智能語音導覽">AI 智能語音導覽</span>
        </div>
        <span id="audioProgress" class="text-xs font-bold text-kuraRed">0 / 0</span>
      </div>
      <div id="audioSubtitle" class="text-[11px] text-gray-500 dark:text-gray-400 h-12 overflow-hidden mb-3 italic" data-en="Click play to start reading the analysis..." data-zh="點擊播放鈕開始為您朗讀分析重點...">點擊播放鈕開始為您朗讀分析重點...</div>
      <div class="flex justify-between items-center">
        <span id="voiceLabel" class="text-[10px] font-black uppercase px-2 py-1 bg-gray-200 dark:bg-kuraDark-700 rounded">TW YATING</span>
        <div class="flex gap-2">
          <button id="btnPrev" class="p-2 bg-gray-100 dark:bg-kuraDark-700 rounded-full hover:bg-gray-200"><i data-lucide="skip-back" class="w-4 h-4"></i></button>
          <button id="btnPlay" class="p-3 bg-kuraRed text-white rounded-full shadow-md hover:bg-red-700"><i id="iconPlay" data-lucide="play" class="w-4 h-4"></i></button>
          <button id="btnNext" class="p-2 bg-gray-100 dark:bg-kuraDark-700 rounded-full hover:bg-gray-200"><i data-lucide="skip-forward" class="w-4 h-4"></i></button>
          <button id="btnStop" class="p-2 bg-red-100 dark:bg-red-900 text-red-600 rounded-full"><i data-lucide="square" class="w-4 h-4"></i></button>
        </div>
      </div>
    </div>
  </div>

  <!-- 頂部導覽列 (Sticky Navbar) -->
  <nav class="sticky top-0 z-40 bg-white/80 dark:bg-kuraDark-900/80 backdrop-blur-md border-b border-gray-200 dark:border-gray-800">
    <div class="max-w-7xl mx-auto px-4 flex items-center justify-between h-16">
      <a href="#hero" class="flex items-center gap-2">
        <div class="w-8 h-8 bg-kuraRed text-white flex items-center justify-center font-bold rounded">くら</div>
        <span class="font-black text-lg hidden sm:block">KURA <span class="text-kuraRed">STRATEGY</span></span>
      </a>
      <div class="flex space-x-4 text-sm font-bold">
        <a href="#hero" class="hover:text-kuraRed hidden sm:block" data-en="Home" data-zh="首頁">首頁</a>
        <a href="#tp0" class="hover:text-kuraRed" data-en="TP0 Analysis" data-zh="TP0 分析">TP0 分析</a>
      </div>
      <div class="flex items-center gap-2">
        <button id="langToggle" class="px-3 py-1 text-xs font-bold border border-gray-300 dark:border-gray-600 rounded-full flex gap-1 items-center hover:bg-gray-100 dark:hover:bg-gray-800"><i data-lucide="globe" class="w-3 h-3 text-kuraRed"></i> <span id="langLabel">EN</span></button>
        <button id="themeToggle" class="p-1.5 rounded-full hover:bg-gray-200 dark:hover:bg-gray-800"><i id="themeIcon" data-lucide="moon" class="w-4 h-4"></i></button>
      </div>
    </div>
  </nav>

  <!-- Hero Section -->
  <section id="hero" class="relative min-h-[60vh] flex flex-col items-center justify-center text-center px-4 wave-bg">
    <span class="px-4 py-1.5 bg-kuraRed/10 text-kuraRed font-bold rounded-full text-xs mb-6" data-en="SMMC Final Project - TP0 Segment" data-zh="SMMC 期末專案 - TP0 區塊">SMMC 期末專案 - TP0 區塊</span>
    <h1 class="text-5xl md:text-7xl font-black mb-6">KURA SUSHI <br><span class="text-kuraRed">TP0 ANALYSIS</span></h1>
    <p class="max-w-2xl text-lg text-gray-600 dark:text-gray-300 read-target" data-en="Exploring the external environment and industry dynamics shaping Kura Sushi's future." data-zh="深入探討塑造藏壽司未來的外部環境與產業動態。">深入探討塑造藏壽司未來的外部環境與產業動態。</p>
    <a href="#tp0" class="mt-8 px-8 py-3 bg-kuraRed text-white font-bold rounded-full shadow-lg hover:bg-red-700 transition-colors" data-en="Read Analysis" data-zh="開始閱讀分析">開始閱讀分析</a>
  </section>

  <!-- ========================= -->
  <!-- TP0 SECTION START -->
  <!-- ========================= -->
  <section id="tp0" class="py-24 bg-gray-50 dark:bg-kuraDark-900 border-t border-gray-200 dark:border-gray-800">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">

      <!-- SECTION TITLE -->
      <div class="text-center mb-20">
        <h2 class="text-5xl font-black tracking-tight dark:text-white read-target" data-en="TP0: External & Industry Environment" data-zh="TP0: 外部與產業環境分析">
          TP0: 外部與產業環境分析
        </h2>
        <p class="mt-6 text-lg text-gray-600 dark:text-gray-300 max-w-4xl mx-auto leading-relaxed read-target" data-en="Kura Sushi is evolving from a traditional conveyor sushi chain into a technology-enabled entertainment dining platform." data-zh="藏壽司正從傳統的迴轉壽司連鎖店，演變為科技賦能的娛樂餐飲平台。">
          藏壽司正從傳統的迴轉壽司連鎖店，演變為科技賦能的娛樂餐飲平台。
        </p>
      </div>

      <!-- MACRO ENVIRONMENT -->
      <div class="mb-28">
        <div class="flex items-center gap-4 mb-10">
          <div class="w-2 h-12 bg-red-500 rounded-full"></div>
          <h3 class="text-4xl font-black dark:text-white read-target" data-en="Macro Environment Analysis" data-zh="總體環境分析">
            總體環境分析
          </h3>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-4 gap-8">
          <!-- Political -->
          <div class="bg-white dark:bg-kuraDark-800 rounded-3xl p-8 shadow-sm hover:shadow-xl transition-all">
            <div class="text-red-500 text-sm font-black uppercase mb-4" data-en="Political & Legal" data-zh="政治與法律">政治與法律</div>
            <h4 class="text-2xl font-black mb-5 dark:text-white read-target" data-en="Regulation & Restructuring" data-zh="法規與重組">法規與重組</h4>
            <ul class="space-y-3 text-sm leading-relaxed text-gray-600 dark:text-gray-300">
              <li class="read-target" data-en="Food safety regulations strengthen consumer trust." data-zh="食品安全法規強化了消費者的信任。">• 食品安全法規強化了消費者的信任。</li>
              <li class="read-target" data-en="IFRS 16 increases reported liabilities." data-zh="IFRS 16 增加了帳面負債。">• <span class="glossary-term text-red-500 cursor-pointer font-bold border-b-2 border-dashed border-red-500 hover:bg-red-50 dark:hover:bg-red-950 transition-colors" data-term="IFRS 16">IFRS 16</span> 增加了帳面負債。</li>
              <li class="read-target" data-en="Hong Kong withdrawal reflects resource reallocation." data-zh="撤出香港反映了資源的重新分配。">• 撤出香港反映了資源的重新分配。</li>
            </ul>
          </div>

          <!-- Economic -->
          <div class="bg-white dark:bg-kuraDark-800 rounded-3xl p-8 shadow-sm hover:shadow-xl transition-all">
            <div class="text-red-500 text-sm font-black uppercase mb-4" data-en="Economic" data-zh="經濟">經濟</div>
            <h4 class="text-2xl font-black mb-5 dark:text-white read-target" data-en="Growth vs Profit Pressure" data-zh="成長與獲利壓力">成長與獲利壓力</h4>
            <ul class="space-y-3 text-sm leading-relaxed text-gray-600 dark:text-gray-300">
              <li class="read-target" data-en="Revenue rapidly increased from expansion." data-zh="營收隨展店迅速增加。">• 營收隨展店迅速增加。</li>
              <li class="read-target" data-en="Labor and rental costs continue rising." data-zh="勞動力與租金成本持續上升。">• 勞動力與租金成本持續上升。</li>
              <li class="read-target" data-en="Investors still expect future Economies of Scale." data-zh="投資者仍期待未來的規模經濟。">• 投資者仍期待未來的 <span class="glossary-term text-red-500 cursor-pointer font-bold border-b-2 border-dashed border-red-500 hover:bg-red-50 dark:hover:bg-red-950 transition-colors" data-term="Economies of Scale">規模經濟</span>。</li>
            </ul>
          </div>

          <!-- Social -->
          <div class="bg-white dark:bg-kuraDark-800 rounded-3xl p-8 shadow-sm hover:shadow-xl transition-all">
            <div class="text-red-500 text-sm font-black uppercase mb-4" data-en="Social" data-zh="社會">社會</div>
            <h4 class="text-2xl font-black mb-5 dark:text-white read-target" data-en="Entertainment Dining" data-zh="娛樂餐飲">娛樂餐飲</h4>
            <ul class="space-y-3 text-sm leading-relaxed text-gray-600 dark:text-gray-300">
              <li class="read-target" data-en="Dining shifted from food-focused to experience-focused." data-zh="餐飲需求從「以食物為中心」轉向「以體驗為中心」。">• 餐飲需求從「以食物為中心」轉向「以體驗為中心」。</li>
              <li class="read-target" data-en="Bikkura Pon strengthens gamification and repeat visits." data-zh="Bikkura Pon 強化了遊戲化體驗與回訪率。">• <span class="glossary-term text-red-500 cursor-pointer font-bold border-b-2 border-dashed border-red-500 hover:bg-red-50 dark:hover:bg-red-950 transition-colors" data-term="Bikkura Pon">Bikkura Pon</span> 強化了遊戲化體驗與回訪率。</li>
              <li class="read-target" data-en="Strong attraction among young consumers and families." data-zh="對年輕消費者與家庭客群具備強大吸引力。">• 對年輕消費者與家庭客群具備強大吸引力。</li>
            </ul>
          </div>

          <!-- Technology -->
          <div class="bg-white dark:bg-kuraDark-800 rounded-3xl p-8 shadow-sm hover:shadow-xl transition-all">
            <div class="text-red-500 text-sm font-black uppercase mb-4" data-en="Technology" data-zh="科技">科技</div>
            <h4 class="text-2xl font-black mb-5 dark:text-white read-target" data-en="Smart Restaurant Systems" data-zh="智慧餐飲系統">智慧餐飲系統</h4>
            <ul class="space-y-3 text-sm leading-relaxed text-gray-600 dark:text-gray-300">
              <li class="read-target" data-en="AI-assisted Forecasting improves preparation efficiency." data-zh="AI輔助預測提升了備餐效率。">• <span class="glossary-term text-red-500 cursor-pointer font-bold border-b-2 border-dashed border-red-500 hover:bg-red-50 dark:hover:bg-red-950 transition-colors" data-term="AI-assisted Forecasting">AI輔助預測</span>提升了備餐效率。</li>
              <li class="read-target" data-en="Automation reduces labor dependency." data-zh="自動化降低了對勞動力的依賴。">• 自動化降低了對勞動力的依賴。</li>
              <li class="read-target" data-en="E-Plate freshness tracking improves food safety." data-zh="E-Plate鮮度追蹤提升了食品安全。">• E-Plate鮮度追蹤提升了食品安全。</li>
            </ul>
          </div>
        </div>
      </div>

      <!-- PORTER FIVE FORCES -->
      <div class="mb-28">
        <div class="flex items-center gap-4 mb-10">
          <div class="w-2 h-12 bg-red-500 rounded-full"></div>
          <h3 class="text-4xl font-black dark:text-white read-target" data-en="Industry Analysis" data-zh="產業分析">
            產業分析
          </h3>
        </div>

        <div class="bg-white dark:bg-kuraDark-800 rounded-[2rem] p-10 shadow-sm border border-gray-100 dark:border-gray-700">
          <h4 class="text-3xl font-black mb-12 dark:text-white read-target" data-en="Porter Five Forces" data-zh="波特五力分析">
            波特五力分析
          </h4>

          <div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-5 gap-6">
            <!-- Rivalry -->
            <div class="bg-gray-50 dark:bg-kuraDark-700 rounded-2xl p-6">
              <h5 class="font-black text-xl mb-4 dark:text-white read-target" data-en="Rivalry" data-zh="現有競爭者">現有競爭者</h5>
              <p class="text-sm text-gray-600 dark:text-gray-300 read-target" data-en="Strong competition from Sushiro, Hama Sushi and Genki Sushi." data-zh="面臨來自壽司郎、Hama壽司與元氣壽司的強烈競爭。">
                面臨來自壽司郎、Hama壽司與元氣壽司的強烈競爭。
              </p>
            </div>
            <!-- Entrants -->
            <div class="bg-gray-50 dark:bg-kuraDark-700 rounded-2xl p-6">
              <h5 class="font-black text-xl mb-4 dark:text-white read-target" data-en="New Entrants" data-zh="潛在進入者">潛在進入者</h5>
              <p class="text-sm text-gray-600 dark:text-gray-300 read-target" data-en="High technological and capital barriers reduce new competitors." data-zh="高科技與資本壁壘減少了新競爭者的加入。">
                高科技與資本壁壘減少了新競爭者的加入。
              </p>
            </div>
            <!-- Substitutes -->
            <div class="bg-gray-50 dark:bg-kuraDark-700 rounded-2xl p-6">
              <h5 class="font-black text-xl mb-4 dark:text-white read-target" data-en="Substitutes" data-zh="替代品威脅">替代品威脅</h5>
              <p class="text-sm text-gray-600 dark:text-gray-300 read-target" data-en="Consumers can easily switch to hotpot, ramen or food delivery." data-zh="消費者可輕易轉換至火鍋、拉面或外送平台。">
                消費者可輕易轉換至火鍋、拉面或外送平台。
              </p>
            </div>
            <!-- Suppliers -->
            <div class="bg-gray-50 dark:bg-kuraDark-700 rounded-2xl p-6">
              <h5 class="font-black text-xl mb-4 dark:text-white read-target" data-en="Suppliers" data-zh="供應商議價能力">供應商議價能力</h5>
              <p class="text-sm text-gray-600 dark:text-gray-300 read-target" data-en="Seafood prices and exchange rates directly affect costs." data-zh="海鮮價格與匯率直接影響成本。">
                海鮮價格與匯率直接影響成本。
              </p>
            </div>
            <!-- Buyers -->
            <div class="bg-gray-50 dark:bg-kuraDark-700 rounded-2xl p-6">
              <h5 class="font-black text-xl mb-4 dark:text-white read-target" data-en="Buyers" data-zh="買方議價能力">買方議價能力</h5>
              <p class="text-sm text-gray-600 dark:text-gray-300 read-target" data-en="Social media reviews strongly influence customer traffic." data-zh="社群媒體評價強烈影響客流量。">
                社群媒體評價強烈影響客流量。
              </p>
            </div>
          </div>
        </div>
      </div>

      <!-- STRATEGIC IMPLICATIONS -->
      <div class="mb-28">
        <div class="flex items-center gap-4 mb-10">
          <div class="w-2 h-12 bg-red-500 rounded-full"></div>
          <h3 class="text-4xl font-black dark:text-white read-target" data-en="Strategic Implications" data-zh="策略意涵">
            策略意涵
          </h3>
        </div>

        <div class="bg-gradient-to-r from-red-500 to-red-600 text-white rounded-[2rem] p-12 shadow-xl">
          <div class="text-center mb-14">
            <p class="text-lg uppercase tracking-[0.3em] font-bold opacity-80 mb-4 read-target" data-en="Strategic Shift" data-zh="策略轉型">策略轉型</p>
            <h4 class="text-4xl md:text-5xl font-black leading-tight read-target" data-en="Sushi Restaurant → Technology-driven Entertainment Platform" data-zh="壽司餐廳 → 科技驅動的娛樂平台">
              壽司餐廳
              <span class="block text-3xl mt-6 opacity-70">→</span>
              科技驅動的娛樂平台
            </h4>
          </div>

          <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
            <div>
              <h5 class="text-2xl font-black mb-4 read-target" data-en="Taiwan Focus" data-zh="聚焦台灣">聚焦台灣</h5>
              <p class="text-sm leading-relaxed text-red-100 read-target" data-en="Taiwan becomes the primary strategic market after Hong Kong restructuring." data-zh="在香港重組後，台灣成為首要的戰略市場。">
                在香港重組後，台灣成為首要的戰略市場。
              </p>
            </div>
            <div>
              <h5 class="text-2xl font-black mb-4 read-target" data-en="Automation Advantage" data-zh="自動化優勢">自動化優勢</h5>
              <p class="text-sm leading-relaxed text-red-100 read-target" data-en="Smart operational systems reduce labor dependency and improve efficiency." data-zh="智慧營運系統減少了勞動力依賴並提升了效率。">
                智慧營運系統減少了勞動力依賴並提升了效率。
              </p>
            </div>
            <div>
              <h5 class="text-2xl font-black mb-4 read-target" data-en="Scale Risk" data-zh="擴張風險">擴張風險</h5>
              <p class="text-sm leading-relaxed text-red-100 read-target" data-en="Expansion does not automatically guarantee long-term profitability." data-zh="單純的擴張無法自動保證長期的獲利能力。">
                單純的擴張無法自動保證長期的獲利能力。
              </p>
            </div>
          </div>
        </div>
      </div>

      <!-- FUTURE RISKS -->
      <div class="mb-28">
        <div class="flex items-center gap-4 mb-10">
          <div class="w-2 h-12 bg-red-500 rounded-full"></div>
          <h3 class="text-4xl font-black dark:text-white read-target" data-en="Future Risks" data-zh="未來風險">未來風險</h3>
        </div>

        <div class="grid grid-cols-2 xl:grid-cols-5 gap-6">
          <div class="bg-white dark:bg-kuraDark-800 rounded-2xl p-6 shadow-sm border border-gray-100 dark:border-gray-700 text-center"><p class="font-bold dark:text-white read-target" data-en="Rising Labor Costs" data-zh="勞動成本上升">勞動成本上升</p></div>
          <div class="bg-white dark:bg-kuraDark-800 rounded-2xl p-6 shadow-sm border border-gray-100 dark:border-gray-700 text-center"><p class="font-bold dark:text-white read-target" data-en="Consumer Fatigue" data-zh="消費者疲勞">消費者疲勞</p></div>
          <div class="bg-white dark:bg-kuraDark-800 rounded-2xl p-6 shadow-sm border border-gray-100 dark:border-gray-700 text-center"><p class="font-bold dark:text-white read-target" data-en="Expansion Pressure" data-zh="展店壓力">展店壓力</p></div>
          <div class="bg-white dark:bg-kuraDark-800 rounded-2xl p-6 shadow-sm border border-gray-100 dark:border-gray-700 text-center"><p class="font-bold dark:text-white read-target" data-en="High Valuation Risk" data-zh="高估值風險">高估值風險</p></div>
          <div class="bg-white dark:bg-kuraDark-800 rounded-2xl p-6 shadow-sm border border-gray-100 dark:border-gray-700 text-center col-span-2 xl:col-span-1"><p class="font-bold dark:text-white read-target" data-en="Automation Investment Risk" data-zh="自動化投資風險">自動化投資風險</p></div>
        </div>
      </div>

      <!-- CONCLUSION -->
      <div class="bg-white dark:bg-kuraDark-800 rounded-[2rem] p-12 shadow-sm border border-gray-100 dark:border-gray-700">
        <h3 class="text-3xl font-black mb-6 dark:text-white read-target" data-en="TP0 Conclusion" data-zh="TP0 總結論">TP0 總結論</h3>
        <p class="text-lg leading-relaxed text-gray-700 dark:text-gray-300 read-target" data-en="Kura Sushi demonstrates how the global restaurant industry is shifting from product-oriented competition toward experience-driven and technology-driven operations. Through automation, gamification and smart operational systems, the company has established a differentiated competitive advantage. However, future success will depend on whether Kura Sushi can successfully achieve economies of scale while maintaining operational efficiency and customer engagement." data-zh="藏壽司展示了全球餐飲業如何從「產品導向」的競爭，轉向「體驗導向」與「科技驅動」的營運模式。透過自動化、遊戲化與智慧營運系統，該公司建立了差異化的競爭優勢。然而，未來的成功將取決於藏壽司能否在維持營運效率與顧客參與度的同時，成功實現規模經濟。">
          藏壽司展示了全球餐飲業如何從「產品導向」的競爭，轉向「體驗導向」與「科技驅動」的營運模式。透過自動化、遊戲化與智慧營運系統，該公司建立了差異化的競爭優勢。然而，未來的成功將取決於藏壽司能否在維持營運效率與顧客參與度的同時，成功實現規模經濟。
        </p>
      </div>

    </div>
  </section>

  <!-- 頁尾 (Footer) -->
  <footer class="bg-gray-900 text-white text-center py-10">
    <p class="font-bold">SMMC 期末學術專案報告 - Kura Sushi TP0</p>
    <p class="text-xs text-gray-500 mt-2">Instructor: Prof. Shihmin Lo</p>
  </footer>

  <!-- 專有名詞解釋 (Glossary Modal) -->
  <div id="modal" class="fixed inset-0 bg-black/70 z-[100] hidden flex items-center justify-center p-4 transition-opacity">
    <div class="bg-white dark:bg-kuraDark-800 p-8 rounded-3xl max-w-md w-full relative shadow-2xl">
      <button id="btnCloseModal" class="absolute top-4 right-4 text-gray-400 hover:text-kuraRed transition-colors"><i data-lucide="x"></i></button>
      <span class="text-xs font-bold text-kuraRed uppercase tracking-widest block mb-2" data-en="Glossary Term" data-zh="專有名詞解釋">專有名詞解釋</span>
      <h3 id="modalTitle" class="text-2xl font-black mb-3 dark:text-white"></h3>
      <p id="modalDesc" class="text-gray-700 dark:text-gray-300 leading-relaxed text-sm"></p>
    </div>
  </div>

  <!-- JavaScript 邏輯引擎 -->
  <script>
    // --- 1. 雙語切換與明暗模式 ---
    let isEn = false;
    let isDark = false;
    
    function renderLang() {
      document.getElementById('langToggle').innerHTML = `<i data-lucide="globe" class="w-3 h-3 text-kuraRed"></i> <span id="langLabel">${isEn ? '繁中' : 'EN'}</span>`;
      document.querySelectorAll('[data-en]').forEach(el => {
        if (!el.dataset.zh) el.dataset.zh = el.innerHTML;
        el.innerHTML = isEn ? el.dataset.en : el.dataset.zh;
      });
      document.documentElement.lang = isEn ? 'en' : 'zh-Hant';
      stopAudio();
      lucide.createIcons();
    }
    
    function renderTheme() {
      document.documentElement.classList.toggle('dark', isDark);
      document.getElementById('themeIcon').setAttribute('data-lucide', isDark ? 'sun' : 'moon');
      lucide.createIcons();
    }

    document.getElementById('langToggle')?.addEventListener('click', () => { isEn = !isEn; renderLang(); });
    document.getElementById('themeToggle')?.addEventListener('click', () => { isDark = !isDark; renderTheme(); });

    // --- 2. 專有名詞解釋字典 ---
    const glossaryDb = {
      "IFRS 16": { 
        zh: "國際財務報導準則第 16 號「租賃」，要求將租賃合約認列於資產負債表，這會增加實體展店時的帳面負債。", 
        en: "An International Financial Reporting Standard that requires leases to be reported on balance sheets, affecting reported liabilities during physical expansion." 
      },
      "Economies of Scale": { 
        zh: "規模經濟：企業因擴大營運規模而使單位成本下降的優勢。", 
        en: "Cost advantages reaped by companies when production or operations become efficient through expansion." 
      },
      "Bikkura Pon": { 
        zh: "藏壽司獨創的扭蛋遊戲系統，每投入五盤即可觸發一次抽獎動畫，有效提升客單價與回訪率。", 
        en: "Kura Sushi's unique gamification system triggered by inserting 5 empty plates, boosting customer engagement and spending." 
      },
      "AI-assisted Forecasting": { 
        zh: "使用人工智慧技術預測客流量與食材需求，進而指導廚房備料，減少浪費。", 
        en: "Utilizing AI to predict customer traffic and food demand, optimizing kitchen preparation and minimizing waste." 
      }
    };
    
    document.addEventListener('click', e => {
      const termEl = e.target.closest('.glossary-term');
      if (termEl) {
        const key = termEl.getAttribute('data-term');
        if(glossaryDb[key]) {
          document.getElementById('modalTitle').textContent = key;
          document.getElementById('modalDesc').textContent = isEn ? glossaryDb[key].en : glossaryDb[key].zh;
          document.getElementById('modal').classList.remove('hidden');
        }
      }
    });
    document.getElementById('btnCloseModal')?.addEventListener('click', () => document.getElementById('modal').classList.add('hidden'));

    // --- 3. 整頁智能語音導覽 ---
    let synth = window.speechSynthesis;
    let proseQueue = [];
    let currentIdx = 0;
    let isPlay = false;

    function buildQueue() {
      proseQueue = Array.from(document.querySelectorAll('.read-target')).filter(el => el.innerText.trim().length > 0);
    }

    function highlight(el) {
      document.querySelectorAll('.audio-highlight').forEach(i => i.classList.remove('audio-highlight'));
      if(el) {
        el.classList.add('audio-highlight');
        if(el.getBoundingClientRect().top < 100 || el.getBoundingClientRect().bottom > window.innerHeight) {
          el.scrollIntoView({ behavior: 'smooth', block: 'center' });
        }
      }
    }

    function playChunk() {
      if(!proseQueue.length) buildQueue();
      if(currentIdx >= proseQueue.length) { stopAudio(); return; }
      
      const el = proseQueue[currentIdx];
      highlight(el);
      
      document.getElementById('audioProgress').textContent = `${currentIdx + 1} / ${proseQueue.length}`;
      document.getElementById('audioSubtitle').textContent = el.innerText;
      document.getElementById('voiceLabel').textContent = isEn ? 'UK MALE' : 'TW YATING';
      
      let u = new SpeechSynthesisUtterance(el.innerText);
      let voices = synth.getVoices() || [];
      
      if(isEn) {
        u.lang = 'en-GB';
        u.voice = voices.find(v => v.lang === 'en-GB' && v.name.includes('Male')) || voices.find(v => v.lang === 'en-GB') || voices.find(v => v.lang.startsWith('en'));
      } else {
        u.lang = 'zh-TW';
        u.voice = voices.find(v => v.lang === 'zh-TW' && v.name.includes('Yating')) || voices.find(v => v.lang === 'zh-TW') || voices.find(v => v.lang.startsWith('zh'));
      }
      
      u.onend = () => { if(isPlay) { currentIdx++; playChunk(); } };
      u.onerror = () => { if(isPlay) { currentIdx++; playChunk(); } };
      
      synth.speak(u);
    }

    function stopAudio() {
      synth.cancel(); 
      isPlay = false;
      document.getElementById('iconPlay').setAttribute('data-lucide', 'play');
      document.getElementById('audioPulse').classList.add('hidden');
      highlight(null);
      lucide.createIcons();
    }

    document.getElementById('btnPlay')?.addEventListener('click', () => {
      if(isPlay) { 
        stopAudio(); 
      } else { 
        isPlay = true; 
        document.getElementById('iconPlay').setAttribute('data-lucide', 'pause'); 
        document.getElementById('audioPulse').classList.remove('hidden'); 
        lucide.createIcons(); 
        playChunk(); 
      }
    });
    
    document.getElementById('btnStop')?.addEventListener('click', () => { 
      stopAudio(); 
      currentIdx = 0; 
      document.getElementById('audioProgress').textContent = '0 / 0'; 
      document.getElementById('audioSubtitle').textContent = isEn ? 'Click play to start reading the analysis...' : '點擊播放鈕開始為您朗讀分析重點...';
    });
    
    document.getElementById('btnNext')?.addEventListener('click', () => { 
      if(!isPlay) return; 
      synth.cancel(); 
      currentIdx++; 
      playChunk(); 
    });
    
    document.getElementById('btnPrev')?.addEventListener('click', () => { 
      if(!isPlay || currentIdx === 0) return; 
      synth.cancel(); 
      currentIdx--; 
      playChunk(); 
    });

    // --- 4. 滾動進度條與回到頂部 ---
    window.addEventListener('scroll', () => {
      const scrollTotal = document.documentElement.scrollHeight - window.innerHeight;
      if (scrollTotal > 0) {
        document.getElementById('progress-bar').style.width = (window.scrollY / scrollTotal * 100) + '%';
      }
      document.getElementById('backToTop').classList.toggle('hidden', window.scrollY < 300);
    });
    
    document.getElementById('backToTop')?.addEventListener('click', () => window.scrollTo({top:0, behavior:'smooth'}));

    // --- 5. 初始載入 ---
    window.onload = () => {
      lucide.createIcons();
      window.speechSynthesis.getVoices();
      if (window.speechSynthesis.onvoiceschanged !== undefined) {
        window.speechSynthesis.onvoiceschanged = () => window.speechSynthesis.getVoices();
      }
    };
  </script>
</body>
</html>
