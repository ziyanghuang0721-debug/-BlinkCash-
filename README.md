# -BlinkCash-
A tool to help users reconnect with their finances
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>看不见的钱</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet" />
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #FAF9F6;
    --text-primary: #2A2A2A;
    --text-secondary: #6B6B6B;
    --accent: #0F4C3D;
    --accent-light: #E8F2EE;
    --card-bg: #FFFFFF;
    --border: #ECEAE4;
    --shadow: 0 4px 12px rgba(0,0,0,0.08);
    --radius: 20px;
  }

  html { scroll-behavior: smooth; }

  body {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'SF Pro Display', sans-serif;
    background: var(--bg);
    color: var(--text-primary);
    min-height: 100vh;
    -webkit-font-smoothing: antialiased;
  }

  .page {
    display: none;
    min-height: 100vh;
    padding: 48px 24px 80px;
    max-width: 640px;
    margin: 0 auto;
    animation: fadeUp 0.4s ease both;
  }
  .page.active { display: flex; flex-direction: column; }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(16px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  /* ── BUTTONS ── */
  .btn-primary {
    display: flex; align-items: center; justify-content: center;
    width: 100%; height: 56px;
    background: var(--accent); color: #fff;
    border: none; border-radius: var(--radius);
    font-size: 16px; font-weight: 600; cursor: pointer;
    transition: opacity .2s, transform .15s;
    user-select: none;
  }
  .btn-primary:hover { opacity: .88; }
  .btn-primary:active { transform: scale(.98); }
  .btn-primary:disabled { opacity: .35; cursor: not-allowed; }

  .btn-secondary {
    display: flex; align-items: center; justify-content: center;
    height: 48px; padding: 0 20px;
    background: transparent; color: var(--text-secondary);
    border: 1.5px solid var(--border); border-radius: var(--radius);
    font-size: 15px; font-weight: 500; cursor: pointer;
    transition: background .15s, color .15s;
  }
  .btn-secondary:hover { background: var(--accent-light); color: var(--accent); }

  /* ── CARDS ── */
  .card {
    background: var(--card-bg);
    border: 1.5px solid var(--border);
    border-radius: var(--radius);
    box-shadow: var(--shadow);
    padding: 20px;
  }

  /* ── HOME PAGE ── */
  #page-home {
    justify-content: center;
    text-align: center;
    gap: 0;
  }
  .home-badge {
    display: inline-block;
    background: var(--accent-light);
    color: var(--accent);
    font-size: 12px; font-weight: 600;
    padding: 6px 14px; border-radius: 99px;
    margin-bottom: 32px;
    letter-spacing: .04em;
  }
  .home-title {
    font-size: clamp(36px, 9vw, 52px);
    font-weight: 700;
    color: var(--accent);
    line-height: 1.15;
    margin-bottom: 20px;
    letter-spacing: -.02em;
  }
  .home-subtitle {
    font-size: 16px;
    color: var(--text-secondary);
    line-height: 1.7;
    margin-bottom: 44px;
    max-width: 400px;
    margin-left: auto; margin-right: auto;
  }
  .home-footer {
    margin-top: 28px;
    font-size: 13px;
    color: #ABABAB;
    letter-spacing: .02em;
  }

  /* ── PROGRESS ── */
  .progress-wrap {
    margin-bottom: 32px;
  }
  .progress-label {
    display: flex; justify-content: space-between;
    font-size: 13px; color: var(--text-secondary);
    margin-bottom: 8px; font-weight: 500;
  }
  .progress-track {
    height: 4px; background: var(--border); border-radius: 99px; overflow: hidden;
  }
  .progress-fill {
    height: 100%; background: var(--accent); border-radius: 99px;
    transition: width .4s ease;
  }

  /* ── QUESTION ── */
  .q-label {
    font-size: 12px; font-weight: 600; color: var(--accent);
    letter-spacing: .08em; text-transform: uppercase;
    margin-bottom: 10px;
  }
  .q-title {
    font-size: 22px; font-weight: 700; line-height: 1.4;
    margin-bottom: 8px; color: var(--text-primary);
  }
  .q-sub {
    font-size: 14px; color: var(--text-secondary);
    margin-bottom: 28px; line-height: 1.6;
  }

  /* ── OPTION CARDS ── */
  .options-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin-bottom: 32px;
  }
  .options-grid.single-col { grid-template-columns: 1fr; }

  .option-card {
    background: var(--card-bg);
    border: 2px solid var(--border);
    border-radius: 16px;
    padding: 16px 14px;
    cursor: pointer;
    transition: border-color .15s, background .15s, transform .1s;
    text-align: center;
    user-select: none;
  }
  .option-card:hover { border-color: #B0D4C8; background: #F6FBF9; }
  .option-card.selected {
    border-color: var(--accent);
    background: var(--accent-light);
  }
  .option-card.selected .opt-label { color: var(--accent); }
  .option-card:active { transform: scale(.97); }

  .opt-icon { font-size: 28px; margin-bottom: 8px; display: block; }
  .opt-label { font-size: 14px; font-weight: 600; color: var(--text-primary); display: block; }
  .opt-sub   { font-size: 12px; color: var(--text-secondary); margin-top: 3px; display: block; }

  .options-grid.single-col .option-card {
    display: flex; align-items: center; gap: 14px;
    text-align: left; padding: 16px 20px;
  }
  .options-grid.single-col .opt-icon { margin-bottom: 0; font-size: 24px; flex-shrink: 0; }

  /* ── NAV FOOTER ── */
  .nav-footer {
    display: flex; gap: 12px; margin-top: auto; padding-top: 24px;
  }
  .nav-footer .btn-secondary { flex-shrink: 0; }
  .nav-footer .btn-primary   { flex: 1; }

  /* ── CUSTOM INPUT ── */
  .custom-input-wrap {
    margin-top: 12px;
    position: relative;
  }
  .custom-chip-list {
    display: flex; flex-wrap: wrap; gap: 10px;
    margin-bottom: 16px;
  }
  .chip {
    padding: 8px 16px; border-radius: 99px;
    border: 1.5px solid var(--border);
    background: var(--card-bg);
    font-size: 14px; font-weight: 500;
    cursor: pointer; color: var(--text-primary);
    transition: all .15s;
  }
  .chip:hover { border-color: var(--accent); color: var(--accent); }
  .chip.selected {
    background: var(--accent); border-color: var(--accent);
    color: #fff;
  }
  .custom-text-input {
    width: 100%; height: 52px;
    border: 1.5px solid var(--border); border-radius: 14px;
    padding: 0 16px; font-size: 15px;
    font-family: inherit; color: var(--text-primary);
    background: var(--card-bg); outline: none;
    transition: border-color .15s;
  }
  .custom-text-input:focus { border-color: var(--accent); }
  .custom-text-input::placeholder { color: #C0BFBA; }

  /* ── RESULT PAGE ── */
  #page-result { gap: 0; }

  .result-section {
    margin-bottom: 40px;
    animation: fadeUp .5s ease both;
  }
  .result-section:nth-child(2) { animation-delay: .1s; }
  .result-section:nth-child(3) { animation-delay: .2s; }
  .result-section:nth-child(4) { animation-delay: .3s; }
  .result-section:nth-child(5) { animation-delay: .4s; }
  .result-section:nth-child(6) { animation-delay: .5s; }

  .section-tag {
    display: inline-block;
    font-size: 11px; font-weight: 700; letter-spacing: .1em;
    text-transform: uppercase; color: var(--accent);
    margin-bottom: 16px;
    background: var(--accent-light); padding: 4px 12px; border-radius: 99px;
  }

  .shock-amount {
    font-size: clamp(56px, 14vw, 80px);
    font-weight: 700; color: var(--accent);
    line-height: 1; letter-spacing: -.03em;
    margin-bottom: 8px;
    font-variant-numeric: tabular-nums;
  }
  .shock-subtitle { font-size: 17px; color: var(--text-secondary); margin-bottom: 24px; }

  .equivalents {
    display: grid; grid-template-columns: repeat(3, 1fr);
    gap: 10px; margin-bottom: 0;
  }
  .equiv-card {
    background: var(--card-bg);
    border: 1.5px solid var(--border);
    border-radius: 16px; padding: 16px 12px; text-align: center;
    box-shadow: var(--shadow);
  }
  .equiv-icon { font-size: 26px; display: block; margin-bottom: 6px; }
  .equiv-num  { font-size: 20px; font-weight: 700; color: var(--accent); display: block; }
  .equiv-desc { font-size: 11px; color: var(--text-secondary); margin-top: 2px; display: block; }

  .leak-card {
    background: var(--card-bg); border: 1.5px solid var(--border);
    border-radius: var(--radius); box-shadow: var(--shadow);
    padding: 20px; margin-bottom: 12px;
  }
  .leak-rank { font-size: 20px; margin-right: 8px; }
  .leak-name { font-size: 16px; font-weight: 700; color: var(--text-primary); }
  .leak-quote {
    font-size: 13px; color: var(--text-secondary); line-height: 1.6;
    margin: 8px 0 10px; font-style: italic;
  }
  .leak-tip {
    font-size: 13px; color: var(--accent); font-weight: 500;
    background: var(--accent-light); padding: 8px 12px;
    border-radius: 10px;
  }
  .leak-bar-wrap { margin-top: 8px; }
  .leak-bar-track {
    height: 6px; background: var(--border); border-radius: 99px; overflow: hidden;
  }
  .leak-bar-fill {
    height: 100%; background: var(--accent); border-radius: 99px;
    width: 0; transition: width 1.2s cubic-bezier(.4,0,.2,1);
  }

  .personality-main {
    background: var(--accent); border-radius: var(--radius);
    padding: 28px; margin-bottom: 16px; color: #fff;
  }
  .personality-main .p-name { font-size: 22px; font-weight: 700; margin-bottom: 4px; }
  .personality-main .p-core { font-size: 14px; opacity: .8; margin-bottom: 20px; }
  .p-section-title { font-size: 12px; font-weight: 600; letter-spacing: .06em; opacity: .7; margin-bottom: 8px; }
  .p-list { list-style: none; }
  .p-list li { font-size: 14px; line-height: 1.7; padding-left: 14px; position: relative; }
  .p-list li::before { content: "—"; position: absolute; left: 0; opacity: .5; }
  .p-quote { font-size: 13px; font-style: italic; opacity: .75; line-height: 1.6; padding-left: 14px; position: relative; }
  .p-quote::before { content: '"'; position: absolute; left: 0; font-style: normal; opacity: .5; }

  .personality-sub {
    background: var(--card-bg); border: 1.5px solid var(--border);
    border-radius: var(--radius); padding: 22px; box-shadow: var(--shadow);
  }
  .personality-sub .p-name { font-size: 18px; font-weight: 700; color: var(--accent); margin-bottom: 4px; }
  .personality-sub .p-core { font-size: 13px; color: var(--text-secondary); margin-bottom: 16px; }

  .awareness-score-wrap { display: flex; align-items: flex-end; gap: 12px; margin-bottom: 20px; }
  .awareness-score { font-size: 56px; font-weight: 700; color: var(--accent); line-height: 1; }
  .awareness-score-label { font-size: 14px; color: var(--text-secondary); padding-bottom: 8px; }
  .awareness-bar-row { margin-bottom: 14px; }
  .abar-label { display: flex; justify-content: space-between; font-size: 13px; color: var(--text-secondary); margin-bottom: 6px; }
  .abar-label span:last-child { font-weight: 600; color: var(--text-primary); }
  .abar-track { height: 8px; background: var(--border); border-radius: 99px; overflow: hidden; }
  .abar-fill {
    height: 100%; border-radius: 99px;
    background: linear-gradient(90deg, #0F4C3D, #2E8B6A);
    width: 0; transition: width 1.4s cubic-bezier(.4,0,.2,1);
  }

  .hope-box {
    background: var(--accent); border-radius: var(--radius);
    padding: 28px; text-align: center; color: #fff;
    margin-bottom: 24px;
  }
  .hope-quote { font-size: 16px; line-height: 1.8; margin-bottom: 20px; opacity: .9; }
  .hope-goal-label { font-size: 12px; font-weight: 600; letter-spacing: .08em; text-transform: uppercase; opacity: .65; margin-bottom: 8px; }
  .hope-pct { font-size: 48px; font-weight: 700; letter-spacing: -.02em; }
  .hope-pct-label { font-size: 13px; opacity: .75; margin-top: 4px; }
  .hope-diff { font-size: 15px; margin-top: 16px; opacity: .85; }

  .future-card {
    background: var(--card-bg); border: 1.5px solid var(--border);
    border-radius: var(--radius); padding: 22px;
    display: flex; align-items: center; gap: 16px;
    margin-bottom: 12px; box-shadow: var(--shadow);
    opacity: .6;
  }
  .future-icon-wrap {
    width: 48px; height: 48px; border-radius: 14px;
    background: var(--accent-light); display: flex; align-items: center; justify-content: center;
    font-size: 22px; flex-shrink: 0;
  }
  .future-name { font-size: 15px; font-weight: 700; color: var(--text-primary); }
  .future-desc { font-size: 13px; color: var(--text-secondary); margin-top: 2px; }
  .future-badge {
    margin-left: auto; font-size: 11px; font-weight: 600;
    background: var(--border); color: var(--text-secondary);
    padding: 4px 10px; border-radius: 99px; white-space: nowrap;
  }

  .share-btn-wrap { margin-top: 8px; }

  .toast {
    position: fixed; bottom: 32px; left: 50%; transform: translateX(-50%) translateY(20px);
    background: #1A1A1A; color: #fff; padding: 12px 24px; border-radius: 99px;
    font-size: 14px; font-weight: 500; opacity: 0;
    transition: opacity .3s, transform .3s; z-index: 999; white-space: nowrap;
  }
  .toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }

  @media (max-width: 400px) {
    .equivalents { grid-template-columns: 1fr 1fr; }
  }
</style>
</head>
<body>

<!-- ═══════════════════════════════════════
     PAGE 0: HOME
═══════════════════════════════════════ -->
<div id="page-home" class="page active">
  <div style="flex:1; display:flex; flex-direction:column; justify-content:center; align-items:center; text-align:center;">
    <span class="home-badge">金钱感知测试</span>
    <h1 class="home-title">你以为自己<br/>一个月只花3000？</h1>
    <p class="home-subtitle">
      实际上很多人一年会漏掉 3 万～5 万元<br/>
      这些钱不是大额消费<br/>
      而是你根本没注意到的"看不见的钱"
    </p>
    <button class="btn-primary" id="btn-start" style="max-width:340px;">
      测测我一年漏掉多少钱
    </button>
    <p class="home-footer">帮助你重新建立和金钱的关系</p>
  </div>
</div>

<!-- ═══════════════════════════════════════
     PAGE 1: GOAL SELECT
═══════════════════════════════════════ -->
<div id="page-goal" class="page">
  <div class="progress-wrap">
    <div class="progress-label">
      <span>第 1 题 / 共 9 题</span><span id="goal-pct">11%</span>
    </div>
    <div class="progress-track"><div class="progress-fill" style="width:11%"></div></div>
  </div>

  <p class="q-label">目标</p>
  <h2 class="q-title">留住这些钱，你最希望用在哪里？</h2>
  <p class="q-sub">选择最能代表你的一个目标</p>

  <div class="options-grid" id="goal-grid">
    <div class="option-card" data-val="travel">
      <span class="opt-icon">&#9992;</span>
      <span class="opt-label">旅行</span>
    </div>
    <div class="option-card" data-val="study">
      <span class="opt-icon">&#128218;</span>
      <span class="opt-label">学习</span>
    </div>
    <div class="option-card" data-val="hobby">
      <span class="opt-icon">&#127775;</span>
      <span class="opt-label">兴趣爱好</span>
    </div>
    <div class="option-card" data-val="home">
      <span class="opt-icon">&#127968;</span>
      <span class="opt-label">搬家 / 租房</span>
    </div>
    <div class="option-card" data-val="save">
      <span class="opt-icon">&#128176;</span>
      <span class="opt-label">储蓄</span>
    </div>
    <div class="option-card" data-val="future">
      <span class="opt-icon">&#10084;</span>
      <span class="opt-label">给未来的自己</span>
    </div>
  </div>

  <div class="nav-footer">
    <button class="btn-primary" id="btn-goal-next" disabled>继续</button>
  </div>
</div>

<!-- ═══════════════════════════════════════
     PAGE 2: GOAL DETAIL
═══════════════════════════════════════ -->
<div id="page-goal-detail" class="page">
  <div class="progress-wrap">
    <div class="progress-label">
      <span>第 2 题 / 共 9 题</span><span>22%</span>
    </div>
    <div class="progress-track"><div class="progress-fill" style="width:22%"></div></div>
  </div>

  <p class="q-label">具象目标</p>
  <h2 class="q-title" id="detail-question">你最想去哪里？</h2>
  <p class="q-sub">选择热门选项，或填写你自己的目标</p>

  <div class="custom-chip-list" id="detail-chips"></div>

  <input type="text" class="custom-text-input" id="detail-custom-input"
         placeholder="自定义填写..." />

  <div class="nav-footer">
    <button class="btn-secondary" id="btn-detail-prev">上一题</button>
    <button class="btn-primary"   id="btn-detail-next" disabled>下一题</button>
  </div>
</div>

<!-- ═══════════════════════════════════════
     PAGE 3: QUESTIONS
═══════════════════════════════════════ -->
<div id="page-questions" class="page">
  <div class="progress-wrap">
    <div class="progress-label">
      <span id="q-label-num">第 3 题 / 共 9 题</span>
      <span id="q-label-pct">33%</span>
    </div>
    <div class="progress-track">
      <div class="progress-fill" id="q-progress-fill" style="width:33%"></div>
    </div>
  </div>

  <p class="q-label" id="q-tag">消费行为</p>
  <h2 class="q-title" id="q-title"></h2>
  <p class="q-sub"  id="q-sub"></p>

  <div class="options-grid single-col" id="q-options"></div>

  <div class="nav-footer">
    <button class="btn-secondary" id="btn-q-prev">上一题</button>
    <button class="btn-primary"   id="btn-q-next" disabled id="btn-q-next">下一题</button>
  </div>
</div>

<!-- ═══════════════════════════════════════
     PAGE 4: RESULT
═══════════════════════════════════════ -->
<div id="page-result" class="page">

  <!-- SECTION 1: SHOCK -->
  <div class="result-section">
    <span class="section-tag">震惊</span>
    <p style="font-size:15px;color:var(--text-secondary);margin-bottom:8px;">你一年有这么多看不见的钱</p>
    <div class="shock-amount" id="shock-num">¥0</div>
    <p class="shock-subtitle" id="shock-range"></p>
    <div class="equivalents" id="equivalents"></div>
  </div>

  <!-- SECTION 2: AHA -->
  <div class="result-section">
    <span class="section-tag">恍然大悟</span>
    <p style="font-size:18px;font-weight:700;margin-bottom:20px;line-height:1.5;" id="aha-headline"></p>
    <div id="leak-cards"></div>
  </div>

  <!-- SECTION 3: PERSONALITY -->
  <div class="result-section">
    <span class="section-tag">消费人格</span>
    <div class="personality-main" id="main-personality"></div>
    <div class="personality-sub"  id="sub-personality"></div>
  </div>

  <!-- SECTION 4: AWARENESS -->
  <div class="result-section">
    <span class="section-tag">金钱感知力</span>
    <div class="awareness-score-wrap">
      <div class="awareness-score" id="awareness-score">0</div>
      <div class="awareness-score-label">/ 100</div>
    </div>
    <p id="awareness-desc" style="font-size:14px;color:var(--text-secondary);margin-bottom:20px;line-height:1.7;"></p>
    <div id="awareness-bars"></div>
  </div>

  <!-- SECTION 5: HOPE -->
  <div class="result-section">
    <span class="section-tag">希望</span>
    <div class="hope-box" id="hope-box">
      <p class="hope-quote">这些钱并没有消失。<br/>它只是流向了你没有注意到的地方。</p>
      <p class="hope-goal-label">如果把这些钱留下来</p>
      <div class="hope-pct" id="hope-pct">0%</div>
      <p class="hope-pct-label" id="hope-pct-label"></p>
      <p class="hope-diff" id="hope-diff"></p>
    </div>
  </div>

  <!-- SECTION 6: FUTURE PRODUCTS -->
  <div class="result-section">
    <span class="section-tag">即将上线</span>
    <div class="future-card">
      <div class="future-icon-wrap">&#128721;</div>
      <div>
        <div class="future-name">我该不该买</div>
        <div class="future-desc">购物前的冷静室</div>
      </div>
      <span class="future-badge">敬请期待</span>
    </div>
    <div class="future-card">
      <div class="future-icon-wrap">&#128274;</div>
      <div>
        <div class="future-name">守住的钱</div>
        <div class="future-desc">记录那些没有买下来的东西</div>
      </div>
      <span class="future-badge">敬请期待</span>
    </div>
  </div>

  <!-- SHARE BUTTON -->
  <div class="share-btn-wrap">
    <button class="btn-primary" id="btn-share">保存结果图片</button>
  </div>
  <div style="height:40px;"></div>
</div>

<!-- TOAST -->
<div class="toast" id="toast"></div>

<!-- ═══════════════════════════════════════
     JAVASCRIPT
═══════════════════════════════════════ -->
<script>
// ─────────────────────────────────────────
// STATE
// ─────────────────────────────────────────
const state = {
  goal: '',          // travel / study / hobby / home / save / future
  goalDetail: '',    // specific sub-goal
  answers: {},       // qIndex → { value, amount, personalityKey }
  currentQ: 0,
};

// ─────────────────────────────────────────
// GOAL DATA
// ─────────────────────────────────────────
const goalDetailMap = {
  travel: {
    q: '你最想去哪里旅行？',
    chips: ['日本', '韩国', '泰国', '欧洲', '北美'],
    placeholder: '其他目标国家 ...',
  },
  study: {
    q: '你最想学习什么？',
    chips: ['语言学习', '职业技能', '自媒体运营', '投资理财'],
    placeholder: '其他学习方向 ...',
  },
  hobby: {
    q: '你的兴趣爱好是什么？',
    chips: ['摄影', '绘画', '音乐', '运动健身', '手工'],
    placeholder: '其他兴趣 ...',
  },
  home: {
    q: '你的租房 / 搬家目标是？',
    chips: ['独居整租', '换大房间', '搬离现在的城市', '装修布置'],
    placeholder: '其他目标 ...',
  },
  save: {
    q: '今年你想存多少钱？',
    chips: ['3 万', '5 万', '10 万', '20 万'],
    placeholder: '自定义金额 ...',
  },
  future: {
    q: '你想为未来的自己做什么？',
    chips: ['建立应急基金', '投资自己', '提前退休准备', '养老储备'],
    placeholder: '其他计划 ...',
  },
};

// ─────────────────────────────────────────
// QUESTIONS  (index 0-6 = q3~q9 in overall flow)
// ─────────────────────────────────────────
const QUESTIONS = [
  {
    tag: '冲动消费',
    title: '最近一次你说"先买了再说"是什么时候？',
    sub: '选择最接近的选项',
    key: 'impulse',
    options: [
      { label: '昨天',       sub: '非常频繁',         amount: 8400,  pKey: 'immediate' },
      { label: '上周',       sub: '比较常见',         amount: 5200,  pKey: 'immediate' },
      { label: '这个月',     sub: '偶尔发生',         amount: 2400,  pKey: 'drift'     },
      { label: '想不起来了', sub: '很少或没有',        amount: 600,   pKey: 'rational'  },
    ],
  },
  {
    tag: '日常小额',
    title: '最近一次你顺手买的小东西大概多少钱？',
    sub: '奶茶、零食、小摆件……',
    key: 'daily',
    options: [
      { label: '¥10 - 50',   sub: '生活小确幸',  amount: 3600,  pKey: 'immediate' },
      { label: '¥50 - 100',  sub: '中等消费',    amount: 7200,  pKey: 'immediate' },
      { label: '¥100 - 300', sub: '比较大方',    amount: 14400, pKey: 'immediate' },
      { label: '¥300+',      sub: '大手笔',      amount: 28800, pKey: 'status'    },
    ],
  },
  {
    tag: '情绪消费',
    title: '最近一次你因为心情好/不好奖励自己买了什么？',
    sub: '美食、衣物、小礼物……',
    key: 'emotion',
    options: [
      { label: '¥10 - 50',   sub: '小奖励',   amount: 2400,  pKey: 'immediate' },
      { label: '¥50 - 100',  sub: '中度奖励', amount: 6000,  pKey: 'immediate' },
      { label: '¥100 - 300', sub: '大犒赏',   amount: 12000, pKey: 'immediate' },
      { label: '¥300+',      sub: '豪华犒赏', amount: 24000, pKey: 'ideal'     },
    ],
  },
  {
    tag: '订阅 / 会员',
    title: '你有多少自动扣费的会员或订阅？',
    sub: '视频、音乐、健身、工具……',
    key: 'subscription',
    options: [
      { label: '很多，每月自动扣',   sub: '5 个以上', amount: 4800,  pKey: 'drift'     },
      { label: '有一些但不常关注',   sub: '2~4 个',   amount: 2400,  pKey: 'drift'     },
      { label: '很少，我知道每一个', sub: '1~2 个',   amount: 800,   pKey: 'rational'  },
      { label: '没有',              sub: '不用订阅',  amount: 0,     pKey: 'rational'  },
    ],
  },
  {
    tag: '种草消费',
    title: '最近一次被广告 / 朋友推荐种草的东西多少钱？',
    sub: '小红书、直播、朋友圈……',
    key: 'influenced',
    options: [
      { label: '¥10 - 50',   sub: '小额种草', amount: 2400,  pKey: 'env'      },
      { label: '¥50 - 100',  sub: '中额种草', amount: 6000,  pKey: 'env'      },
      { label: '¥100 - 300', sub: '大额种草', amount: 12000, pKey: 'env'      },
      { label: '¥300+',      sub: '重度种草', amount: 24000, pKey: 'env'      },
    ],
  },
  {
    tag: '理想自我',
    title: '最近一次你为了"理想的自己"买的东西是什么？',
    sub: '课程、健身卡、工具书、设备……',
    key: 'ideal',
    options: [
      { label: '¥10 - 50',   sub: '小投资',  amount: 1800,  pKey: 'ideal'    },
      { label: '¥50 - 100',  sub: '中等投资', amount: 6000,  pKey: 'ideal'    },
      { label: '¥100 - 300', sub: '大额投资', amount: 14400, pKey: 'ideal'    },
      { label: '¥300+',      sub: '重度投资', amount: 28800, pKey: 'ideal'    },
    ],
  },
  {
    tag: '效率消费',
    title: '最近一次你花钱换时间/便利是什么？',
    sub: '外卖、打车、跑腿、加速……',
    key: 'efficiency',
    options: [
      { label: '¥10 - 50',   sub: '偶尔',   amount: 1200,  pKey: 'efficiency' },
      { label: '¥50 - 100',  sub: '常用',   amount: 4800,  pKey: 'efficiency' },
      { label: '¥100 - 300', sub: '依赖',   amount: 9600,  pKey: 'efficiency' },
      { label: '¥300+',      sub: '高频',   amount: 19200, pKey: 'efficiency' },
    ],
  },
];

// ─────────────────────────────────────────
// PERSONALITIES
// ─────────────────────────────────────────
const PERSONALITIES = {
  ideal: {
    name: '理想自我型',
    core: '为了成为更好的自己而消费',
    pros: ['目标感强，敢于自我投资', '长期规划能力出色', '对成长有真诚的渴望'],
    risks: ['容易自我催眠式购买', '未使用的课程和工具堆积', '理想与行动之间有落差'],
    quotes: ['"先投资自己，小欲望以后再说"', '"这个工具肯定能让我更高效"', '"我要变得更好，现在的不适是值得的"', '"这笔钱花了，我就没有借口不做了"'],
  },
  immediate: {
    name: '及时满足型',
    core: '快乐比计划更重要',
    pros: ['生活体验丰富，情绪感知细腻', '敢于享受当下', '消费决策快速，不内耗'],
    risks: ['储蓄能力偏弱', '情绪低落时容易冲动消费', '小额消费累积成大数字'],
    quotes: ['"买了才开心，不买一直想"', '"等一下可能就卖完了"', '"就这一次，下次不买了"', '"这个不贵，值得的"'],
  },
  security: {
    name: '安全感型',
    core: '消费和储蓄都在追求稳定感',
    pros: ['未雨绸缪，生活保障意识强', '不容易被大额冲动消费', '情绪稳定'],
    risks: ['囤货过多造成浪费', '容易为"以防万一"多花钱', '过度准备降低灵活性'],
    quotes: ['"有备无患，多备一份安心"', '"万一用到了呢"', '"提前准备总比临时抱佛脚好"', '"多买一件打折的，以后肯定用得上"'],
  },
  efficiency: {
    name: '效率优先型',
    core: '时间比金钱更有价值',
    pros: ['决策高效，不在小事上内耗', '能量用在更有价值的地方', '生活节奏感好'],
    risks: ['长期外包小事花费可观', '容易忽视替代方案', '花钱换时间的习惯难以改变'],
    quotes: ['"花钱换时间，这个账算得过来"', '"懒得麻烦，直接买"', '"时间就是钱，这个值"', '"我宁愿多花点，省心最重要"'],
  },
  env: {
    name: '环境影响型',
    core: '容易受到周围环境的消费刺激',
    pros: ['对新事物保持开放', '社交圈子消费体验丰富', '审美品味容易提升'],
    risks: ['种草效率极高，容易跟风', '购买后使用率偏低', '消费决策被外部主导'],
    quotes: ['"大家都在买，我先收藏一下"', '"博主说这个真的好用"', '"朋友推荐的，应该不会踩雷"', '"最近一直刷到，感觉真的需要"'],
  },
  drift: {
    name: '无意识漂流型',
    core: '钱慢慢流走，自己没有明显感知',
    pros: ['生活压力小，对钱不焦虑', '消费不执念，随遇而安', '很少因购物感到后悔'],
    risks: ['订阅自动续费极易被忽视', '月底余额总比预期少', '难以锁定漏钱的具体来源'],
    quotes: ['"顺便就买了，也没怎么想"', '"这个月怎么又没存下来"', '"有吗？我不记得花了"', '"随手加进购物车了，后来就买了"'],
  },
  rational: {
    name: '理智计算型',
    core: '每一笔消费都经过权衡',
    pros: ['预算清晰，消费有规划', '不容易被营销操控', '长期财务状况优于平均'],
    risks: ['过度比价耗费时间和精力', '可能错过性价比高的及时机会', '有时影响生活体验和幸福感'],
    quotes: ['"这个值不值，先算算再说"', '"我先对比一下再决定"', '"能不买就不买，省下来更踏实"', '"这个等活动再买"'],
  },
};

// ─────────────────────────────────────────
// LEAK ARCHETYPES
// ─────────────────────────────────────────
const LEAK_TYPES = {
  ideal:      { name: '为理想中的自己买单',  quote: '你买的从来不是课程，你买的是那个以为会认真上课的自己', tip: '在购买前问一问：我上周花了多少时间在上一次买的课上？', pct: 0 },
  immediate:  { name: '小奖励消费',          quote: '你买的从来不是甜品，你买的是那个以为快乐可以被购买的自己', tip: '试试"24小时法则"：把想买的东西放进购物车，等一天再决定', pct: 0 },
  env:        { name: '被种草消费',           quote: '你买的从来不是彩妆，你买的是那个以为别人用了自己也会变好的自己', tip: '收藏但不立即购买，等热情冷却后再评估是否真的需要', pct: 0 },
  drift:      { name: '自动续费 / 订阅',      quote: '你不是在为会员付钱，你是在为一个以为自己会用的习惯付钱', tip: '今天花 10 分钟检查所有自动扣费，取消三个月没用过的', pct: 0 },
  efficiency: { name: '花钱换时间',           quote: '你买的从来不是外卖，你买的是那个不想面对疲惫的自己', tip: '记录一周的效率消费，看看真正省出的时间用在了哪里', pct: 0 },
};

// ─────────────────────────────────────────
// UTILS
// ─────────────────────────────────────────
function showPage(id) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  const el = document.getElementById(id);
  if (el) {
    el.classList.add('active');
    window.scrollTo(0, 0);
  }
}

function showToast(msg, duration = 2800) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), duration);
}

function animateNumber(el, target, prefix = '¥', duration = 1600) {
  const start = performance.now();
  function tick(now) {
    const p = Math.min((now - start) / duration, 1);
    const ease = 1 - Math.pow(1 - p, 3);
    const val = Math.round(ease * target);
    el.textContent = prefix + val.toLocaleString('zh-CN');
    if (p < 1) requestAnimationFrame(tick);
  }
  requestAnimationFrame(tick);
}

function animateBar(el, pct, delay = 0) {
  setTimeout(() => { el.style.width = pct + '%'; }, delay + 100);
}

// ─────────────────────────────────────────
// PAGE: HOME
// ─────────────────────────────────────────
document.getElementById('btn-start').addEventListener('click', function() {
  showPage('page-goal');
});

// ─────────────────────────────────────────
// PAGE: GOAL SELECT
// ─────────────────────────────────────────
const goalGrid = document.getElementById('goal-grid');
const btnGoalNext = document.getElementById('btn-goal-next');

goalGrid.querySelectorAll('.option-card').forEach(card => {
  card.addEventListener('click', function() {
    goalGrid.querySelectorAll('.option-card').forEach(c => c.classList.remove('selected'));
    this.classList.add('selected');
    state.goal = this.dataset.val;
    btnGoalNext.disabled = false;
  });
});

btnGoalNext.addEventListener('click', function() {
  if (!state.goal) return;
  setupGoalDetail();
  showPage('page-goal-detail');
});

// ─────────────────────────────────────────
// PAGE: GOAL DETAIL
// ─────────────────────────────────────────
function setupGoalDetail() {
  const data = goalDetailMap[state.goal];
  document.getElementById('detail-question').textContent = data.q;
  const chipsEl = document.getElementById('detail-chips');
  chipsEl.innerHTML = '';
  document.getElementById('detail-custom-input').value = '';
  document.getElementById('detail-custom-input').placeholder = data.placeholder;
  state.goalDetail = '';
  document.getElementById('btn-detail-next').disabled = true;

  data.chips.forEach(chip => {
    const el = document.createElement('div');
    el.className = 'chip';
    el.textContent = chip;
    el.addEventListener('click', function() {
      chipsEl.querySelectorAll('.chip').forEach(c => c.classList.remove('selected'));
      this.classList.add('selected');
      document.getElementById('detail-custom-input').value = '';
      state.goalDetail = chip;
      document.getElementById('btn-detail-next').disabled = false;
    });
    chipsEl.appendChild(el);
  });
}

document.getElementById('detail-custom-input').addEventListener('input', function() {
  const val = this.value.trim();
  if (val) {
    document.getElementById('detail-chips').querySelectorAll('.chip').forEach(c => c.classList.remove('selected'));
    state.goalDetail = val;
    document.getElementById('btn-detail-next').disabled = false;
  } else {
    const anyChip = document.querySelector('#detail-chips .chip.selected');
    if (!anyChip) {
      state.goalDetail = '';
      document.getElementById('btn-detail-next').disabled = true;
    }
  }
});

document.getElementById('btn-detail-prev').addEventListener('click', function() {
  showPage('page-goal');
});

document.getElementById('btn-detail-next').addEventListener('click', function() {
  state.currentQ = 0;
  renderQuestion();
  showPage('page-questions');
});

// ─────────────────────────────────────────
// PAGE: QUESTIONS
// ─────────────────────────────────────────
function renderQuestion() {
  const q = QUESTIONS[state.currentQ];
  const totalQ = QUESTIONS.length;
  const overallIndex = state.currentQ + 3; // questions start at step 3
  const overallTotal = totalQ + 2;
  const pct = Math.round((overallIndex / (overallTotal + 1)) * 100);

  document.getElementById('q-label-num').textContent = `第 ${overallIndex} 题 / 共 ${overallTotal} 题`;
  document.getElementById('q-label-pct').textContent = pct + '%';
  document.getElementById('q-progress-fill').style.width = pct + '%';
  document.getElementById('q-tag').textContent   = q.tag;
  document.getElementById('q-title').textContent = q.title;
  document.getElementById('q-sub').textContent   = q.sub;

  const optEl = document.getElementById('q-options');
  optEl.innerHTML = '';
  const btnNext = document.getElementById('btn-q-next');
  btnNext.disabled = !(state.answers[state.currentQ]);
  btnNext.textContent = state.currentQ === totalQ - 1 ? '查看结果' : '下一题';

  q.options.forEach((opt, idx) => {
    const card = document.createElement('div');
    card.className = 'option-card' + (state.answers[state.currentQ]?.idx === idx ? ' selected' : '');
    card.innerHTML = `<span class="opt-label">${opt.label}</span><span class="opt-sub">${opt.sub}</span>`;
    card.addEventListener('click', function() {
      optEl.querySelectorAll('.option-card').forEach(c => c.classList.remove('selected'));
      this.classList.add('selected');
      state.answers[state.currentQ] = { idx, amount: opt.amount, pKey: opt.pKey };
      // fix subscription
      if (q.key === 'subscription' && idx === 3) {
        state.answers[state.currentQ].amount = 0;
      }
      btnNext.disabled = false;
    });
    optEl.appendChild(card);
  });

  document.getElementById('btn-q-prev').style.display = state.currentQ === 0 ? 'none' : '';
}

document.getElementById('btn-q-prev').addEventListener('click', function() {
  if (state.currentQ === 0) {
    showPage('page-goal-detail');
  } else {
    state.currentQ--;
    renderQuestion();
  }
});

document.getElementById('btn-q-next').addEventListener('click', function() {
  if (!state.answers[state.currentQ]) return;
  if (state.currentQ < QUESTIONS.length - 1) {
    state.currentQ++;
    renderQuestion();
  } else {
    buildResult();
    showPage('page-result');
  }
});

// ─────────────────────────────────────────
// BUILD RESULT
// ─────────────────────────────────────────
function buildResult() {
  // ── 1. Calculate total ──
  let total = 0;
  const pScore = {};

  Object.values(state.answers).forEach(a => {
    total += a.amount || 0;
    pScore[a.pKey] = (pScore[a.pKey] || 0) + 1;
  });

  // fix: if subscription = "none" (amount=0), remove drift score from that answer
  const subAns = state.answers[3];
  if (subAns && subAns.amount === 0) {
    pScore['drift'] = Math.max(0, (pScore['drift'] || 0) - 1);
  }

  // Round to midpoint of range
  const rangeLow  = Math.round(total * 0.85 / 1000) * 1000;
  const rangeHigh = Math.round(total * 1.15 / 1000) * 1000;
  const midpoint  = Math.round((rangeLow + rangeHigh) / 2 / 1000) * 1000;

  // ── 2. Shock ──
  const shockEl = document.getElementById('shock-num');
  animateNumber(shockEl, midpoint, '¥', 1800);
  document.getElementById('shock-range').textContent =
    `估算区间 ¥${rangeLow.toLocaleString()} ～ ¥${rangeHigh.toLocaleString()}`;

  const eqData = [
    { icon: '&#9992;', num: Math.max(1, Math.floor(midpoint / 8000)), desc: '次亚洲旅行' },
    { icon: '&#9749;', num: Math.floor(midpoint / 32),                desc: '杯奶茶' },
    { icon: '&#127968;', num: +(midpoint / 3500).toFixed(1),          desc: '个月租金' },
  ];
  const eqEl = document.getElementById('equivalents');
  eqEl.innerHTML = '';
  eqData.forEach(e => {
    eqEl.innerHTML += `<div class="equiv-card">
      <span class="equiv-icon">${e.icon}</span>
      <span class="equiv-num">${e.num}</span>
      <span class="equiv-desc">${e.desc}</span>
    </div>`;
  });

  // ── 3. Aha — Leak cards ──
  // compute pct per leak
  const pKeyAmounts = {};
  Object.values(state.answers).forEach(a => {
    pKeyAmounts[a.pKey] = (pKeyAmounts[a.pKey] || 0) + (a.amount || 0);
  });

  const leakOrder = Object.entries(pKeyAmounts)
    .filter(([k]) => LEAK_TYPES[k])
    .sort((a, b) => b[1] - a[1])
    .slice(0, 3);

  const medals = ['1st', '2nd', '3rd'];
  const medalSymbols = ['&#129351;', '&#129352;', '&#129353;'];

  const ahaHeadlineMap = {
    ideal:      '你买的不是课程，你买的是那个以为以后每天都会认真学的自己',
    immediate:  '你买的不是甜品，你买的是那个以为快乐可以被购买的自己',
    env:        '你买的不是彩妆，你买的是那个以为跟着"必买清单"就不会错的自己',
    drift:      '你付的不是会员费，你付的是那个以为以后会认真用的习惯',
    efficiency: '你买的不是外卖，你买的是那个以为省事就能轻松的自己',
  };

  const topKey = leakOrder[0] ? leakOrder[0][0] : 'ideal';
  document.getElementById('aha-headline').textContent =
    ahaHeadlineMap[topKey] || ahaHeadlineMap['ideal'];

  const leakCardsEl = document.getElementById('leak-cards');
  leakCardsEl.innerHTML = '';
  const totalLeakAmt = Object.values(pKeyAmounts).reduce((a, b) => a + b, 0) || 1;

  leakOrder.forEach(([key, amt], i) => {
    const leak = LEAK_TYPES[key];
    if (!leak) return;
    const pct = Math.round(amt / totalLeakAmt * 100);
    const card = document.createElement('div');
    card.className = 'leak-card';
    card.innerHTML = `
      <div style="display:flex;align-items:center;margin-bottom:8px;">
        <span class="leak-rank">${medalSymbols[i]}</span>
        <span class="leak-name">${leak.name}</span>
        <span style="margin-left:auto;font-size:13px;font-weight:600;color:var(--accent);">${pct}%</span>
      </div>
      <div class="leak-bar-wrap">
        <div class="leak-bar-track"><div class="leak-bar-fill" data-pct="${pct}"></div></div>
      </div>
      <p class="leak-quote">${leak.quote}</p>
      <p class="leak-tip">${leak.tip}</p>`;
    leakCardsEl.appendChild(card);
  });
  // animate bars
  setTimeout(() => {
    document.querySelectorAll('.leak-bar-fill').forEach(b => {
      b.style.width = b.dataset.pct + '%';
    });
  }, 200);

  // ── 4. Personality ──
  const sorted = Object.entries(pScore).sort((a, b) => b[1] - a[1]);
  const mainKey = sorted[0]?.[0] || 'drift';
  const subKey  = sorted[1]?.[0] || 'rational';

  const main = PERSONALITIES[mainKey] || PERSONALITIES['drift'];
  const sub  = PERSONALITIES[subKey]  || PERSONALITIES['rational'];

  document.getElementById('main-personality').innerHTML = `
    <div class="p-name">${main.name}</div>
    <div class="p-core">${main.core}</div>
    <div style="margin-bottom:14px;">
      <div class="p-section-title">PROS</div>
      <ul class="p-list">${main.pros.map(p => `<li>${p}</li>`).join('')}</ul>
    </div>
    <div style="margin-bottom:14px;">
      <div class="p-section-title">RISKS</div>
      <ul class="p-list">${main.risks.map(r => `<li>${r}</li>`).join('')}</ul>
    </div>
    <div>
      <div class="p-section-title">OFTEN SAYS</div>
      ${main.quotes.map(q => `<div class="p-quote" style="margin-bottom:6px;">${q}</div>`).join('')}
    </div>`;

  document.getElementById('sub-personality').innerHTML = `
    <div style="font-size:11px;font-weight:600;color:var(--text-secondary);letter-spacing:.06em;margin-bottom:8px;">副人格</div>
    <div class="p-name">${sub.name}</div>
    <div class="p-core">${sub.core}</div>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-top:14px;">
      <div>
        <div class="p-section-title" style="color:var(--accent);font-size:11px;font-weight:600;letter-spacing:.06em;margin-bottom:6px;">优点</div>
        <ul class="p-list">${sub.pros.slice(0,2).map(p=>`<li style="font-size:13px;color:var(--text-secondary);margin-bottom:4px;">${p}</li>`).join('')}</ul>
      </div>
      <div>
        <div class="p-section-title" style="color:#B05A2F;font-size:11px;font-weight:600;letter-spacing:.06em;margin-bottom:6px;">风险</div>
        <ul class="p-list">${sub.risks.slice(0,2).map(r=>`<li style="font-size:13px;color:var(--text-secondary);margin-bottom:4px;">${r}</li>`).join('')}</ul>
      </div>
    </div>
    <div style="margin-top:14px;">
      ${sub.quotes.slice(0,2).map(q=>`<div class="p-quote" style="font-size:13px;color:var(--text-secondary);margin-bottom:6px;padding-left:14px;position:relative;">${q}</div>`).join('')}
    </div>`;

  // ── 5. Awareness ──
  const hasSubscription = state.answers[3]?.amount > 0;
  const impulsiveAmt = (state.answers[0]?.amount || 0) + (state.answers[1]?.amount || 0);
  const planningAmt  = (state.answers[5]?.amount || 0);

  let awareness = 50;
  if (impulsiveAmt < 5000)  awareness += 15;
  if (impulsiveAmt < 10000) awareness += 8;
  if (hasSubscription)      awareness -= 10;
  if (planningAmt > 10000)  awareness -= 8;
  if (pScore['rational'])   awareness += 12;
  awareness = Math.max(10, Math.min(95, awareness));

  const awarenessDescMap = [
    [80, '你对金钱有很强的感知力。大多数消费都是有意识的决定，偶尔的无意识消费不影响整体的财务清醒度。'],
    [60, '你对金钱有一定感知，但在某些场景下容易失去警觉。主要漏点集中在情绪消费和环境影响。'],
    [40, '你的金钱流向有些模糊。很多消费发生在无意识状态中，月底总觉得钱不知去哪了。'],
    [0,  '你的钱正在悄悄离开。绝大多数消费都缺乏清醒的感知，建议先从记录开始重建和金钱的关系。'],
  ];
  const desc = awarenessDescMap.find(([threshold]) => awareness >= threshold)?.[1] || awarenessDescMap[3][1];

  const awarenessScoreEl = document.getElementById('awareness-score');
  let aw = 0;
  const awTimer = setInterval(() => {
    aw = Math.min(aw + 2, awareness);
    awarenessScoreEl.textContent = aw;
    if (aw >= awareness) clearInterval(awTimer);
  }, 20);
  document.getElementById('awareness-desc').textContent = desc;

  const barDims = [
    { label: '消费前意识', val: Math.min(95, awareness + 5) },
    { label: '情绪控制力', val: Math.max(10, awareness - 10) },
    { label: '订阅管理',   val: hasSubscription ? Math.max(10, awareness - 20) : Math.min(95, awareness + 10) },
  ];
  const barsEl = document.getElementById('awareness-bars');
  barsEl.innerHTML = '';
  barDims.forEach((b, i) => {
    barsEl.innerHTML += `<div class="awareness-bar-row">
      <div class="abar-label"><span>${b.label}</span><span>${b.val}</span></div>
      <div class="abar-track"><div class="abar-fill" data-val="${b.val}" style="transition-delay:${i * 0.15}s"></div></div>
    </div>`;
  });
  setTimeout(() => {
    document.querySelectorAll('.abar-fill').forEach(b => {
      b.style.width = b.dataset.val + '%';
    });
  }, 300);

  // ── 6. Hope ──
  const goalTargets = {
    travel:  { target: 8000,  unit: 'trip',   label: '次亚洲旅行' },
    study:   { target: 3000,  unit: 'course',  label: '门专业课程' },
    hobby:   { target: 2000,  unit: 'item',    label: '个兴趣项目' },
    home:    { target: 15000, unit: 'month',   label: '个月租房基金' },
    save:    { target: 50000, unit: 'yuan',    label: '元储蓄目标' },
    future:  { target: 30000, unit: 'reserve', label: '元应急储备' },
  };

  const goalTarget = goalTargets[state.goal] || goalTargets['save'];
  const pct        = Math.min(99, Math.round(midpoint / goalTarget.target * 100));
  const diff       = Math.max(0, goalTarget.target - midpoint);

  document.getElementById('hope-pct').textContent = pct + '%';
  const goalDetailText = state.goalDetail || '你的目标';
  document.getElementById('hope-pct-label').textContent =
    `看见这些钱，你已经找到了「${goalDetailText}」目标的 ${pct}%`;
  document.getElementById('hope-diff').textContent =
    diff > 0
      ? `距离目标还差 ¥${diff.toLocaleString()}，已经近在咫尺`
      : `这些钱足以完全实现你的目标`;
}

// ─────────────────────────────────────────
// SHARE
// ─────────────────────────────────────────
document.getElementById('btn-share').addEventListener('click', function() {
  showToast('长按页面截图，即可保存分享到小红书 / 朋友圈');
});
</script>
</body>
</html>
```
