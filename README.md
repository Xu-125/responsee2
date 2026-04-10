
  <title>嘴替 · Translator</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    :root {
      --ease-expo:  cubic-bezier(0.19, 1, 0.22, 1);
      --ease-quart: cubic-bezier(0.76, 0, 0.24, 1);
      --ease-back:  cubic-bezier(0.34, 1.56, 0.64, 1);
    }
    body {
      font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display",
        "Helvetica Neue", Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      background: #fff; color: #000; overflow-x: hidden;
    }

    /* ── entrance ── */
    @keyframes fadeUp {
      from { opacity:0; transform:translateY(12px); }
      to   { opacity:1; transform:translateY(0); }
    }
    .fu  { animation: fadeUp .6s var(--ease-expo) both; }
    .d1  { animation-delay:.04s } .d2 { animation-delay:.1s }
    .d3  { animation-delay:.17s } .d4 { animation-delay:.24s }
    .d5  { animation-delay:.32s }

    /* ── setup modal overlay ── */
    #setupOverlay {
      position: fixed; inset: 0; background: rgba(255,255,255,.96);
      z-index: 50; display: flex; align-items: center; justify-content: center;
      backdrop-filter: blur(8px);
      transition: opacity .4s ease;
    }
    #setupOverlay.hidden { opacity: 0; pointer-events: none; }

    /* ── settings drawer ── */
    #settingsDrawer {
      position: fixed; top: 0; right: 0; bottom: 0; width: 360px;
      background: #fff; border-left: 1px solid #f0f0f0;
      transform: translateX(100%);
      transition: transform .45s var(--ease-quart);
      z-index: 40; overflow-y: auto;
      padding: 40px 32px;
    }
    #settingsDrawer.open { transform: translateX(0); }
    #drawerBackdrop {
      position: fixed; inset: 0; z-index: 39;
      display: none;
    }
    #drawerBackdrop.open { display: block; }

    /* ── inputs ── */
    .field-input {
      width: 100%; border: none; border-bottom: 1px solid #e8e8e8;
      outline: none; background: transparent;
      font-size: 13px; color: #000; padding: 9px 0;
      font-family: inherit; letter-spacing: .01em;
      transition: border-color .25s ease;
    }
    .field-input::placeholder { color: #d0d0d0; }
    .field-input:focus { border-bottom-color: #000; }
    .field-label {
      font-size: 9px; letter-spacing: .2em; text-transform: uppercase;
      color: #c0c0c0; display: block; margin-bottom: 6px;
    }

    /* ── provider pills ── */
    .provider-pill {
      padding: 6px 12px; border: 1px solid #ebebeb;
      font-size: 11px; letter-spacing: .06em; color: #aaa;
      cursor: pointer; background: transparent;
      font-family: inherit; white-space: nowrap;
      transition: all .22s var(--ease-quart);
    }
    .provider-pill:hover { border-color: #000; color: #000; }
    .provider-pill.active { background: #000; border-color: #000; color: #fff; }

    /* ── status indicator ── */
    .s-dot {
      width: 7px; height: 7px; border-radius: 50%;
      display: inline-block; flex-shrink: 0;
      transition: background .3s ease;
    }
    .s-dot-ok    { background: #22c55e; animation: glow 2s infinite; }
    .s-dot-off   { background: #e5e5e5; }
    .s-dot-err   { background: #ef4444; }
    .s-dot-load  { background: #f59e0b; animation: glow 1s infinite; }
    @keyframes glow {
      0%,100% { box-shadow: 0 0 0 0 rgba(34,197,94,.35); }
      50%      { box-shadow: 0 0 0 4px rgba(34,197,94,0); }
    }

    /* ── role tabs ── */
    .role-wrap {
      position: relative; display: flex;
      border-bottom: 1px solid #f0f0f0;
      overflow-x: auto; scrollbar-width: none;
    }
    .role-wrap::-webkit-scrollbar { display: none; }
    .role-tab {
      position: relative; z-index: 1;
      padding: 8px 14px; font-size: 12px; letter-spacing: .07em;
      color: #c0c0c0; background: none; border: none;
      cursor: pointer; font-family: inherit; white-space: nowrap;
      transition: color .22s; flex-shrink: 0;
    }
    .role-tab:hover { color: #666; }
    .role-tab.active { color: #000; font-weight: 500; }
    .role-bar {
      position: absolute; bottom: -1px; height: 2px; background: #000;
      transition: left .35s var(--ease-quart), width .35s var(--ease-quart);
      pointer-events: none;
    }

    /* ── textarea ── */
    textarea {
      resize: none; border: none; outline: none;
      background: transparent; caret-color: #000;
    }
    textarea::placeholder { color: #e0e0e0; }
    .input-line {
      height: 1px; background: #efefef;
      transition: background .3s ease;
    }
    .input-wrap:focus-within .input-line { background: #000; }

    /* ── buttons ── */
    .btn {
      cursor: pointer; font-family: inherit;
      letter-spacing: .1em; text-transform: uppercase;
      transition: transform .35s var(--ease-expo),
                  background .28s var(--ease-quart),
                  color .28s var(--ease-quart),
                  box-shadow .3s ease, opacity .2s;
      position: relative; overflow: hidden;
    }
    .btn:hover  { transform: translateY(-2px); }
    .btn:active { transform: scale(.98); opacity: .8; }
    .btn-solid  { background: #000; color: #fff; }
    .btn-solid:hover  { box-shadow: 0 8px 28px rgba(0,0,0,.16); }
    .btn-outline { background: transparent; color: #000; border: 1px solid #ccc; }
    .btn-outline:hover { background:#000; color:#fff; border-color:#000;
                         box-shadow: 0 8px 28px rgba(0,0,0,.13); }
    .btn-ghost {
      background: transparent; border: none;
      font-family: inherit; cursor: pointer;
      font-size: 10px; letter-spacing: .12em; text-transform: uppercase;
      color: #c8c8c8; padding: 0;
      transition: color .22s ease, transform .25s var(--ease-back);
      display: inline-flex; align-items: center; gap: 4px;
    }
    .btn-ghost:hover { color: #000; transform: translateY(-1px); }

    /* ── divider ── */
    .vdiv { width:1px; background:#f0f0f0; align-self:stretch; margin:0 50px; flex-shrink:0; }

    /* ── loading ── */
    .bar-wrap { display:flex; align-items:flex-end; gap:3px; height:20px; }
    @keyframes bb {
      0%,100%{transform:scaleY(.2);opacity:.2} 50%{transform:scaleY(1);opacity:1}
    }
    .bar { width:3px; height:100%; background:#ccc; transform-origin:bottom;
           animation:bb .9s ease-in-out infinite; }
    .bar:nth-child(2){animation-delay:.13s}
    .bar:nth-child(3){animation-delay:.26s}
    .bar:nth-child(4){animation-delay:.39s}
    .bar:nth-child(5){animation-delay:.52s}

    /* ── result ── */
    @keyframes resultIn {
      from{opacity:0;transform:translateY(10px)}
      to{opacity:1;transform:translateY(0)}
    }
    .result-in { animation: resultIn .5s var(--ease-expo) both; }
    .quote-wrap { position:relative; padding-left:4px; }
    .quote-wrap::before {
      content:'\201C'; position:absolute; top:-8px; left:-2px;
      font-size:52px; line-height:1; color:#f0f0f0;
      font-family:Georgia,serif; pointer-events:none; user-select:none;
    }

    /* ── badge ── */
    .badge {
      font-size:9px; letter-spacing:.2em; text-transform:uppercase;
      padding:3px 9px; border:1px solid;
      display:inline-flex; align-items:center; gap:5px;
    }
    .badge::before { content:''; width:5px; height:5px; border-radius:50%; flex-shrink:0; }
    .badge-eq    { border-color:#000; color:#000; }
    .badge-eq::before { background:#000; }
    .badge-crazy { border-color:#dc2626; color:#dc2626; }
    .badge-crazy::before { background:#dc2626; animation:bb .7s infinite; }
    .badge-ai    { border-color:#7c3aed; color:#7c3aed; }
    .badge-ai::before { background:#7c3aed; }

    /* ── error msg ── */
    #errorMsg {
      font-size:11px; color:#ef4444; letter-spacing:.02em;
      margin-top:12px; min-height:16px;
      transition: opacity .3s ease;
    }

    /* ── toast ── */
    #toast {
      position:fixed; bottom:28px; left:50%;
      transform:translateX(-50%) translateY(50px);
      background:#000; color:#fff;
      font-size:10px; letter-spacing:.15em; text-transform:uppercase;
      padding:11px 22px;
      transition:transform .45s var(--ease-expo), opacity .4s ease;
      opacity:0; pointer-events:none; white-space:nowrap;
      display:flex; align-items:center; gap:8px;
    }
    #toast::before { content:'✓'; }
    #toast.show { transform:translateX(-50%) translateY(0); opacity:1; }

    /* ── shake ── */
    @keyframes shake {
      0%,100%{transform:translateX(0)}
      20%{transform:translateX(-5px)} 40%{transform:translateX(5px)}
      60%{transform:translateX(-3px)} 80%{transform:translateX(3px)}
    }
    .shake { animation:shake .38s ease; }

    #charCount { transition:color .25s; }

    @media(max-width:768px){
      .main-grid { flex-direction:column!important; }
      .vdiv { display:none; }
      .right-col { border-top:1px solid #f0f0f0; padding-top:36px; margin-top:36px; }
      #settingsDrawer { width:100%; }
    }
  </style>
</head>
<body>

<!-- ══════════════════════════════════════════
     SETUP OVERLAY（首次使用 / 未配置 API）
══════════════════════════════════════════ -->
<div id="setupOverlay">
  <div style="max-width:440px;width:100%;padding:0 32px;text-align:center;">
    <p style="font-size:10px;letter-spacing:.28em;color:#c8c8c8;text-transform:uppercase;margin:0 0 14px;">
      Universal Translator
    </p>
    <h2 style="font-size:26px;font-weight:600;letter-spacing:-.03em;margin:0 0 8px;">嘴替</h2>
    <p style="font-size:12px;color:#c0c0c0;margin:0 0 40px;letter-spacing:.04em;">
      想说的话，让我来说。
    </p>

    <!-- provider presets -->
    <div style="text-align:left;margin-bottom:24px;">
      <span class="field-label" style="margin-bottom:12px;display:block;">选择 AI 服务商</span>
      <div style="display:flex;flex-wrap:wrap;gap:8px;">
        <button class="provider-pill active" data-base="https://api.openai.com/v1" data-model="gpt-4o-mini" data-name="OpenAI">OpenAI</button>
        <button class="provider-pill" data-base="https://api.deepseek.com/v1" data-model="deepseek-chat" data-name="DeepSeek">DeepSeek</button>
        <button class="provider-pill" data-base="https://api.moonshot.cn/v1" data-model="moonshot-v1-8k" data-name="Moonshot">Moonshot</button>
        <button class="provider-pill" data-base="https://dashscope.aliyuncs.com/compatible-mode/v1" data-model="qwen-plus" data-name="通义千问">通义千问</button>
        <button class="provider-pill" data-base="https://api.lingyiwanwu.com/v1" data-model="yi-lightning" data-name="零一万物">零一万物</button>
        <button class="provider-pill" data-base="" data-model="" data-name="自定义">自定义</button>
      </div>
    </div>

    <div style="display:grid;gap:20px;text-align:left;margin-bottom:8px;">
      <div>
        <label class="field-label">API KEY</label>
        <input class="field-input" id="setupKey" type="password" placeholder="粘贴你的 API Key..." autocomplete="off" />
      </div>
      <div id="customFields" style="display:none;gap:16px;grid-template-columns:1fr 1fr;">
        <div>
          <label class="field-label">BASE URL</label>
          <input class="field-input" id="setupBase" type="text" placeholder="https://..." />
        </div>
        <div>
          <label class="field-label">MODEL</label>
          <input class="field-input" id="setupModel" type="text" placeholder="model-name" />
        </div>
      </div>
    </div>

    <p id="setupError" style="font-size:11px;color:#ef4444;margin:0 0 20px;min-height:16px;text-align:left;"></p>

    <div style="display:flex;gap:10px;align-items:center;">
      <button id="setupSaveBtn" onclick="setupSave()"
        style="flex:1;padding:15px;background:#000;color:#fff;border:none;
               font-family:inherit;font-size:11px;letter-spacing:.12em;
               text-transform:uppercase;cursor:pointer;
               transition:opacity .2s;"
        onmouseover="this.style.opacity='.8'" onmouseout="this.style.opacity='1'">
        开始使用
      </button>
      <button onclick="skipSetup()"
        style="padding:15px 20px;background:none;color:#c8c8c8;border:none;
               font-family:inherit;font-size:11px;letter-spacing:.12em;
               text-transform:uppercase;cursor:pointer;
               transition:color .2s;"
        onmouseover="this.style.color='#000'" onmouseout="this.style.color='#c8c8c8'">
        跳过
      </button>
    </div>

    <p style="font-size:10px;color:#e0e0e0;margin:20px 0 0;letter-spacing:.03em;">
      Key 仅保存在本地浏览器，不经过任何服务器
    </p>
  </div>
</div>

<!-- ══════════════════════════════════════════
     SETTINGS DRAWER
══════════════════════════════════════════ -->
<div id="drawerBackdrop" onclick="closeDrawer()"></div>
<div id="settingsDrawer">
  <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:32px;">
    <p style="font-size:9px;letter-spacing:.22em;text-transform:uppercase;color:#c0c0c0;margin:0;">
      AI 设置
    </p>
    <button onclick="closeDrawer()"
      style="font-size:18px;color:#ccc;background:none;border:none;cursor:pointer;
             line-height:1;padding:0;transition:color .2s;"
      onmouseover="this.style.color='#000'" onmouseout="this.style.color='#ccc'">
      ×
    </button>
  </div>

  <!-- Provider pills -->
  <label class="field-label" style="margin-bottom:12px;display:block;">服务商</label>
  <div style="display:flex;flex-wrap:wrap;gap:8px;margin-bottom:24px;" id="drawerProviders">
    <button class="provider-pill active" data-base="https://api.openai.com/v1" data-model="gpt-4o-mini" data-name="OpenAI">OpenAI</button>
    <button class="provider-pill" data-base="https://api.deepseek.com/v1" data-model="deepseek-chat" data-name="DeepSeek">DeepSeek</button>
    <button class="provider-pill" data-base="https://api.moonshot.cn/v1" data-model="moonshot-v1-8k" data-name="Moonshot">Moonshot</button>
    <button class="provider-pill" data-base="https://dashscope.aliyuncs.com/compatible-mode/v1" data-model="qwen-plus" data-name="通义千问">通义千问</button>
    <button class="provider-pill" data-base="https://api.lingyiwanwu.com/v1" data-model="yi-lightning" data-name="零一万物">零一万物</button>
    <button class="provider-pill" data-base="" data-model="" data-name="自定义">自定义</button>
  </div>

  <div style="display:grid;gap:20px;">
    <div>
      <label class="field-label">API KEY</label>
      <input class="field-input" id="drawerKey" type="password" placeholder="sk-..." autocomplete="off" />
    </div>
    <div id="drawerCustom" style="display:none;gap:16px;">
      <div style="margin-bottom:16px;">
        <label class="field-label">BASE URL</label>
        <input class="field-input" id="drawerBase" type="text" placeholder="https://api.openai.com/v1" />
      </div>
      <div>
        <label class="field-label">MODEL</label>
        <input class="field-input" id="drawerModel" type="text" placeholder="gpt-4o-mini" />
      </div>
    </div>
  </div>

  <p id="drawerError" style="font-size:11px;color:#ef4444;margin:16px 0 0;min-height:16px;"></p>

  <div style="display:flex;gap:10px;margin-top:28px;">
    <button onclick="drawerSave()"
      style="flex:1;padding:14px;background:#000;color:#fff;border:none;
             font-family:inherit;font-size:11px;letter-spacing:.12em;
             text-transform:uppercase;cursor:pointer;transition:opacity .2s;"
      onmouseover="this.style.opacity='.8'" onmouseout="this.style.opacity='1'">
      保存
    </button>
    <button onclick="drawerClear()"
      style="padding:14px 18px;background:none;color:#c8c8c8;border:1px solid #ebebeb;
             font-family:inherit;font-size:11px;letter-spacing:.12em;
             text-transform:uppercase;cursor:pointer;transition:color .2s,border-color .2s;"
      onmouseover="this.style.color='#000';this.style.borderColor='#000'"
      onmouseout="this.style.color='#c8c8c8';this.style.borderColor='#ebebeb'">
      清除
    </button>
  </div>

  <div id="drawerSaved"
    style="font-size:10px;color:#22c55e;letter-spacing:.1em;margin-top:14px;
           opacity:0;transition:opacity .3s;">
    已保存 ✓
  </div>

  <div style="margin-top:40px;padding-top:24px;border-top:1px solid #f5f5f5;">
    <p style="font-size:9px;letter-spacing:.12em;color:#d8d8d8;line-height:1.8;margin:0;">
      API Key 只存储在你的浏览器本地<br>
      请求直接发送给 AI 服务商<br>
      不经过任何第三方服务器
    </p>
  </div>
</div>

<!-- ══════════════════════════════════════════
     HEADER
══════════════════════════════════════════ -->
<header style="padding:56px 0 36px;text-align:center;">
  <p class="fu d1"
     style="font-size:10px;letter-spacing:.28em;color:#c8c8c8;text-transform:uppercase;margin:0 0 10px;">
    Universal Translator
  </p>
  <h1 class="fu d2"
      style="font-size:28px;font-weight:600;letter-spacing:-.035em;margin:0 0 8px;">
    嘴替
  </h1>
  <p class="fu d3"
     style="font-size:12px;color:#c0c0c0;font-weight:300;letter-spacing:.06em;margin:0 0 20px;">
    想说的话，让我来说。
  </p>

  <!-- status bar -->
  <div class="fu d4"
       style="display:inline-flex;align-items:center;gap:10px;cursor:pointer;"
       onclick="openDrawer()">
    <span class="s-dot s-dot-off" id="statusDot"></span>
    <span id="statusText"
          style="font-size:10px;letter-spacing:.12em;color:#d0d0d0;text-transform:uppercase;
                 transition:color .25s;"
          onmouseover="this.style.color='#000'" onmouseout="this.style.color='#d0d0d0'">
      未配置 · 点击设置 AI
    </span>
  </div>
</header>

<!-- ══════════════════════════════════════════
     MAIN
══════════════════════════════════════════ -->
<main style="max-width:1000px;margin:0 auto;padding:0 36px 90px;">
  <div class="main-grid fu d5"
       style="display:flex;align-items:flex-start;">

    <!-- ── LEFT ── -->
    <div style="flex:1;display:flex;flex-direction:column;min-width:0;">

      <p style="font-size:9px;letter-spacing:.22em;color:#d0d0d0;
                text-transform:uppercase;margin:0 0 14px;">
        对象 / Who
      </p>

      <!-- role tabs -->
      <div class="role-wrap" id="roleWrap" style="margin-bottom:30px;">
        <div class="role-bar" id="roleBar"></div>
        <button class="role-tab active" data-role="同事">同事</button>
        <button class="role-tab" data-role="领导">领导</button>
        <button class="role-tab" data-role="家人">家人</button>
        <button class="role-tab" data-role="伴侣">伴侣</button>
        <button class="role-tab" data-role="朋友">朋友</button>
        <button class="role-tab" data-role="老师">老师</button>
        <button class="role-tab" data-role="甲方">甲方</button>
        <button class="role-tab" data-role="室友">室友</button>
        <button class="role-tab" data-role="相亲">相亲</button>
      </div>

      <p style="font-size:9px;letter-spacing:.22em;color:#d0d0d0;
                text-transform:uppercase;margin:0 0 14px;">
        想法 / Input
      </p>

      <div class="input-wrap">
        <textarea id="inputText" rows="6"
          style="font-size:19px;line-height:1.7;font-weight:300;
                 color:#000;width:100%;"
          placeholder="说说你的真实想法..."
          maxlength="500"></textarea>
        <div style="display:flex;justify-content:flex-end;margin-top:6px;">
          <span id="charCount" style="font-size:10px;color:#e0e0e0;">0 / 500</span>
        </div>
        <div class="input-line" style="margin-top:4px;"></div>
      </div>

      <!-- action row -->
      <div style="display:flex;gap:10px;margin-top:22px;">
        <button class="btn btn-solid" id="btnHigh"
          style="flex:1.3;padding:16px 12px;font-size:11px;font-weight:500;">
          一键高情商
        </button>
        <button class="btn btn-outline" id="btnCrazy"
          style="flex:1;padding:16px 12px;font-size:11px;font-weight:500;">
          彻底发疯
        </button>
      </div>

      <div id="errorMsg" style="font-size:11px;color:#ef4444;
            letter-spacing:.02em;margin-top:12px;min-height:16px;"></div>

      <p style="font-size:10px;color:#e0e0e0;margin-top:6px;letter-spacing:.03em;">
        Enter 翻译 &nbsp;·&nbsp; Shift+Enter 换行
      </p>
    </div>

    <!-- ── divider ── -->
    <div class="vdiv"></div>

    <!-- ── RIGHT ── -->
    <div class="right-col"
         style="flex:1;display:flex;flex-direction:column;min-width:0;">

      <p style="font-size:9px;letter-spacing:.22em;color:#d0d0d0;
                text-transform:uppercase;margin:0 0 14px;">
        翻译 / Output
      </p>

      <!-- placeholder -->
      <div id="stPlaceholder">
        <p style="font-size:19px;font-weight:300;color:#ebebeb;
                  line-height:1.7;margin:0;">
          翻译结果在这里出现...
        </p>
      </div>

      <!-- loading -->
      <div id="stLoading" style="display:none;padding-top:6px;">
        <div class="bar-wrap">
          <div class="bar"></div><div class="bar"></div>
          <div class="bar"></div><div class="bar"></div>
          <div class="bar"></div>
        </div>
        <p id="loadingHint"
           style="font-size:11px;color:#d8d8d8;margin:10px 0 0;letter-spacing:.06em;">
          AI 思考中...
        </p>
      </div>

      <!-- result -->
      <div id="stResult" style="display:none;flex-direction:column;">
        <div id="resultBadge" style="margin-bottom:18px;"></div>
        <div class="quote-wrap" style="min-height:80px;">
          <p id="resultText"
             style="font-size:19px;font-weight:300;line-height:1.8;
                    margin:0;color:#000;word-break:break-all;">
          </p>
        </div>
        <div style="margin-top:28px;padding-top:18px;border-top:1px solid #f5f5f5;
                    display:flex;gap:18px;align-items:center;">
          <button class="btn-ghost" id="copyBtn" onclick="copyResult()">
            复制 <span style="font-size:12px;">↗</span>
          </button>
          <button class="btn-ghost" onclick="resetAll()">重新来过</button>
          <button class="btn-ghost" onclick="translate(lastMode)"
                  style="margin-left:auto;">
            换一条 ↺
          </button>
        </div>
      </div>
    </div>
  </div>

  <!-- footer -->
  <div style="margin-top:80px;display:flex;align-items:center;gap:20px;">
    <div style="flex:1;height:1px;background:#f5f5f5;"></div>
    <span style="font-size:9px;letter-spacing:.18em;color:#e2e2e2;">@zeyi</span>
    <div style="flex:1;height:1px;background:#f5f5f5;"></div>
  </div>
</main>

<div id="toast">已复制到剪贴板</div>

<!-- ══════════════════════════════════════════
     SCRIPT
══════════════════════════════════════════ -->
<script>
/* ─────────────────────────────────────────
   CONFIG STATE
───────────────────────────────────────── */
let cfg = {
  key:   localStorage.getItem('zr_key')   || '',
  base:  localStorage.getItem('zr_base')  || 'https://api.openai.com/v1',
  model: localStorage.getItem('zr_model') || 'gpt-4o-mini',
  name:  localStorage.getItem('zr_name')  || 'OpenAI',
};

const PROVIDERS = [
  { name:'OpenAI',   base:'https://api.openai.com/v1',          model:'gpt-4o-mini' },
  { name:'DeepSeek', base:'https://api.deepseek.com/v1',         model:'deepseek-chat' },
  { name:'Moonshot', base:'https://api.moonshot.cn/v1',          model:'moonshot-v1-8k' },
  { name:'通义千问', base:'https://dashscope.aliyuncs.com/compatible-mode/v1', model:'qwen-plus' },
  { name:'零一万物', base:'https://api.lingyiwanwu.com/v1',      model:'yi-lightning' },
  { name:'自定义',   base:'', model:'' },
];

/* ─────────────────────────────────────────
   SETUP OVERLAY
───────────────────────────────────────── */
function initOverlay() {
  // If key exists, skip overlay
  if (cfg.key) {
    document.getElementById('setupOverlay').classList.add('hidden');
    refreshStatusBar();
    return;
  }
  // Bind provider pills in overlay
  bindProviderPills(
    '#setupOverlay .provider-pill',
    (b, m) => {
      const cf = document.getElementById('customFields');
      cf.style.display = b ? 'none' : 'grid';
      if (b) {
        document.getElementById('setupBase').value  = b;
        document.getElementById('setupModel').value = m;
      }
    }
  );
}

function setupSave() {
  const key = document.getElementById('setupKey').value.trim();
  const err = document.getElementById('setupError');
  if (!key) { err.textContent = '请填写 API Key'; return; }

  const activePill = document.querySelector('#setupOverlay .provider-pill.active');
  const base  = activePill?.dataset.base  || document.getElementById('setupBase').value.trim()  || 'https://api.openai.com/v1';
  const model = activePill?.dataset.model || document.getElementById('setupModel').value.trim() || 'gpt-4o-mini';
  const name  = activePill?.dataset.name  || '自定义';

  saveConfig(key, base, model, name);
  err.textContent = '';
  document.getElementById('setupOverlay').classList.add('hidden');
}

function skipSetup() {
  document.getElementById('setupOverlay').classList.add('hidden');
}

/* ─────────────────────────────────────────
   SETTINGS DRAWER
───────────────────────────────────────── */
function openDrawer() {
  // Sync current config into drawer
  document.getElementById('drawerKey').value   = cfg.key;
  document.getElementById('drawerBase').value  = cfg.base;
  document.getElementById('drawerModel').value = cfg.model;

  // Sync active pill
  document.querySelectorAll('#drawerProviders .provider-pill').forEach(p => {
    p.classList.toggle('active', p.dataset.name === cfg.name);
  });

  const isCustom = !PROVIDERS.find(p => p.name === cfg.name && p.base);
  document.getElementById('drawerCustom').style.display = isCustom ? 'block' : 'none';

  document.getElementById('settingsDrawer').classList.add('open');
  document.getElementById('drawerBackdrop').classList.add('open');
  document.getElementById('drawerError').textContent  = '';
  document.getElementById('drawerSaved').style.opacity = '0';
}

function closeDrawer() {
  document.getElementById('settingsDrawer').classList.remove('open');
  document.getElementById('drawerBackdrop').classList.remove('open');
}

function drawerSave() {
  const key = document.getElementById('drawerKey').value.trim();
  const err = document.getElementById('drawerError');
  if (!key) { err.textContent = '请填写 API Key'; return; }

  const activePill = document.querySelector('#drawerProviders .provider-pill.active');
  const isCustom   = activePill?.dataset.name === '自定义';
  const base  = isCustom
    ? (document.getElementById('drawerBase').value.trim()  || 'https://api.openai.com/v1')
    : (activePill?.dataset.base  || cfg.base);
  const model = isCustom
    ? (document.getElementById('drawerModel').value.trim() || 'gpt-4o-mini')
    : (activePill?.dataset.model || cfg.model);
  const name  = activePill?.dataset.name || '自定义';

  saveConfig(key, base, model, name);
  err.textContent = '';
  const saved = document.getElementById('drawerSaved');
  saved.style.opacity = '1';
  setTimeout(() => { saved.style.opacity = '0'; }, 2000);
}

function drawerClear() {
  cfg = { key:'', base:'https://api.openai.com/v1', model:'gpt-4o-mini', name:'OpenAI' };
  ['zr_key','zr_base','zr_model','zr_name'].forEach(k => localStorage.removeItem(k));
  document.getElementById('drawerKey').value = '';
  document.getElementById('drawerError').textContent = '';
  refreshStatusBar();
  closeDrawer();
}

/* ─────────────────────────────────────────
   SHARED HELPERS
───────────────────────────────────────── */
function saveConfig(key, base, model, name) {
  cfg = { key, base, model, name };
  localStorage.setItem('zr_key',   key);
  localStorage.setItem('zr_base',  base);
  localStorage.setItem('zr_model', model);
  localStorage.setItem('zr_name',  name);
  refreshStatusBar();
}

function refreshStatusBar() {
  const dot  = document.getElementById('statusDot');
  const text = document.getElementById('statusText');
  if (cfg.key) {
    dot.className    = 's-dot s-dot-ok';
    text.textContent = cfg.name + ' · ' + cfg.model + ' · 点击更换';
  } else {
    dot.className    = 's-dot s-dot-off';
    text.textContent = '未配置 · 点击设置 AI';
  }
}

function bindProviderPills(selector, onSelect) {
  document.querySelectorAll(selector).forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll(selector).forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      onSelect(btn.dataset.base, btn.dataset.model);
    });
  });
}

/* ─────────────────────────────────────────
   ROLE TABS
───────────────────────────────────────── */
const ROLE_DESC = {
  同事: '平级同事，日常工作协作关系',
  领导: '直属上级或领导，有明显的上下级关系',
  家人: '父母或其他家庭成员，关系亲近但可能存在代际摩擦',
  伴侣: '男友/女友或配偶，亲密关系',
  朋友: '普通朋友，平等的社交关系',
  老师: '学校老师或教授，有权威性的教育关系',
  甲方: '甲方客户，存在雇佣和服务关系',
  室友: '合租室友，共同生活但非亲密关系',
  相亲: '相亲对象，第一次或初期见面，关系尚未确立',
};

const PLACEHOLDERS = {
  同事: '比如：这需求改了八百遍我不想改了...',
  领导: '比如：凭什么加班的又是我...',
  家人: '比如：别再催我结婚了...',
  伴侣: '比如：你根本就不理解我...',
  朋友: '比如：你只有有事才找我...',
  老师: '比如：这门课我完全听不懂但不敢说...',
  甲方: '比如：改了这么多版还要改...',
  室友: '比如：半夜能不能别这么吵...',
  相亲: '比如：没感觉想提前结束...',
};

let currentRole = '同事';

function updateRoleBar(tab) {
  const bar  = document.getElementById('roleBar');
  const wrap = document.getElementById('roleWrap');
  const wr   = wrap.getBoundingClientRect();
  const tr   = tab.getBoundingClientRect();
  bar.style.left  = (tr.left - wr.left + wrap.scrollLeft) + 'px';
  bar.style.width = tr.width + 'px';
}

document.querySelectorAll('.role-tab').forEach(btn => {
  btn.addEventListener('click', () => {
    document.querySelectorAll('.role-tab').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    currentRole = btn.dataset.role;
    updateRoleBar(btn);
    resetOutput();
    document.getElementById('inputText').placeholder =
      PLACEHOLDERS[currentRole] || '说说你的真实想法...';
  });
});

/* ─────────────────────────────────────────
   AI CALL
───────────────────────────────────────── */
const SYSTEM_PROMPT = `你是一个精通中国人际关系和沟通技巧的助手，擅长帮人把话说得恰到好处。

核心原则：
- 只输出翻译结果本身，不加任何解释、前缀、引号或标签
- 语言必须自然口语化，像真实的人说的话
- 严格控制长度，简短有力`;

function buildUserPrompt(input, mode) {
  const roleDesc = ROLE_DESC[currentRole] || currentRole;

  if (mode === 'high') {
    return `请把下面这句话翻译成高情商的版本。

说话对象：${currentRole}（${roleDesc}）
原话：${input}

要求：
- 保留核心意思，但换一种更成熟、得体的说法
- 语气和措辞要符合与"${currentRole}"的关系特点
- 口语化，绝不用"您"，不说官腔废话
- 控制在1-2句话以内，不超过60字
- 直接输出翻译结果，不要任何额外内容`;
  } else {
    return `请把下面这句话翻译成夸张的发疯版本。

说话对象：${currentRole}（${roleDesc}）
原话：${input}

要求：
- 极度夸张，戏剧性十足，充满崩溃感和自嘲
- 大量使用！！！，情绪要饱满外溢
- 内容要结合"${currentRole}"这个对象，让发疯有针对性，接地气，让人看了想笑
- 2-3句话，不超过80字
- 直接输出发疯内容，不要任何额外内容`;
  }
}

async function callAI(input, mode) {
  const base = cfg.base.replace(/\/$/, '');
  const resp = await fetch(`${base}/chat/completions`, {
    method: 'POST',
    headers: {
      'Content-Type':  'application/json',
      'Authorization': `Bearer ${cfg.key}`,
    },
    body: JSON.stringify({
      model: cfg.model,
      messages: [
        { role: 'system', content: SYSTEM_PROMPT },
        { role: 'user',   content: buildUserPrompt(input, mode) },
      ],
      max_tokens:  200,
      temperature: mode === 'crazy' ? 1.1 : 0.85,
      stream: false,
    }),
  });

  if (!resp.ok) {
    const body = await resp.json().catch(() => ({}));
    const msg  = body?.error?.message || `HTTP ${resp.status}`;
    throw new Error(msg);
  }

  const data = await resp.json();
  const text = data?.choices?.[0]?.message?.content?.trim();
  if (!text) throw new Error('返回内容为空');
  return text;
}

/* ─────────────────────────────────────────
   SIMPLE LOCAL FALLBACK
   （仅在无 API Key 时使用）
───────────────────────────────────────── */
const FALLBACK = {
  high: {
    同事: ['改没问题，先对齐一下目标？','可以，这次的验收标准是什么？','在推了，有进展马上同步你。','这个我来跟，有问题找你。','我手里比较满，能说一下哪个最紧吗？'],
    领导: ['收到，尽快跟进。','好的，有卡点提前报备您。','明白，确认细节再推进。','收到，今天内给您回复。','我来安排，您放心。'],
    家人: ['我有自己的节奏，到时候告诉你们。','我很好，不用担心我。','我在努力，给我点时间。','我知道你们是为我好。','让我自己来，我心里有数。'],
    伴侣: ['我不是不在乎你，只是现在说不好。','给我一点时间，整理好了来找你。','我们好好说，别这样。','你说得有道理，我来想想。','我很在乎我们，所以才会在意这些。'],
    朋友: ['我在，你说吧。','我理解你，这次真的帮不上。','说实话，你也应该听听我的想法。','有你在我就放心了。','你辛苦了，真的。'],
    老师: ['老师对不起，这次没做好，我来补。','明白了，感谢老师指导。','我回去再想想，有问题来请教。','这里我理解得不够，方便再解释吗？','谢谢老师，我记下了。'],
    甲方: ['收到，整理一下给您回复。','好，改之前能对齐一下方向吗？','能说说哪里改，改起来更准。','收到反馈，今天内给您新版。','明白需求，尽快跟进。'],
    室友: ['说一声就行，我们好商量。','能注意一下吗，住得舒服就好。','我们约定一下，都方便。','没关系，说出来比憋着好。','不是挑剔，就是说一声。'],
    相亲: ['感谢这次见面，挺好的。','我喜欢慢慢来，多了解一下？','缘分这件事，顺其自然。','感觉还不错，再见几次吧。','坦诚比表现重要，你觉得呢？'],
  },
  crazy: {
    同事: ['我已经不是人了！！！我是台不用充电的机器！！！','我的理智提前下班！！！没有交接直接跑路！！！','需求改了多少版了！！！脑子糊成一锅粥了！！！'],
    领导: ['好的！！！（说不行也没用）！！！','收到！！！（我要原地爆炸了）！！！','您说的对！！！（内心十万头羊驼）！！！'],
    家人: ['催催催！！！催出一个好的来给你们看看！！！','我已经焦虑到不行了！！！你们再催要爆了！！！','我知道是好意！！！但真的撑不住这种好意了！！！'],
    伴侣: ['我的眼泪已经哭干了！！！库存告急！！！','你根本不懂我！！！我说了多少次了！！！','算了算了！！！（但没有算了就是还在意）！！！'],
    朋友: ['我就是24小时待机的工具人是吗！！！','感情是双向的！！！有没有听说过这个概念！！！','我撑不住了！！！我也要人帮我！！！'],
    老师: ['我的GPA已经是一地碎片了！！！','我看了八遍还是不会！！！救命啊！！！','完蛋了！！！这门课我彻底跟不上了！！！'],
    甲方: ['改了多少版了！！！我数不清了！！！','我的创意死在这份改稿单上了！！！','再改一版！！！今天最可怕的五个字！！！'],
    室友: ['我在自己家里变得如此不自在！！！','我只想安静一下！！！一下下！！！','为什么我说了三遍！！！','我要住图书馆！！！因为那里安静！！！'],
    相亲: ['我妈觉得我们合适！！！但我妈不是我！！！','好人！！！但不是我的好人！！！','相亲相到怀疑人生了！！！'],
  }
};

function localFallback(mode) {
  const pool = FALLBACK[mode][currentRole] || FALLBACK[mode]['同事'];
  return pool[Math.floor(Math.random() * pool.length)];
}

/* ─────────────────────────────────────────
   TRANSLATE CONTROLLER
───────────────────────────────────────── */
let isLoading = false;
let lastResult = '';
let lastMode   = 'high';

async function translate(mode) {
  if (isLoading) return;

  const input = document.getElementById('inputText').value.trim();
  if (!input) {
    const ta = document.getElementById('inputText');
    ta.classList.add('shake');
    ta.focus();
    setTimeout(() => ta.classList.remove('shake'), 400);
    return;
  }

  lastMode  = mode;
  isLoading = true;
  setError('');
  showState('loading');

  const dot  = document.getElementById('statusDot');
  const hint = document.getElementById('loadingHint');

  let text   = '';
  let usedAI = false;

  if (cfg.key) {
    dot.className = 's-dot s-dot-load';
    hint.textContent = cfg.name + ' 思考中...';

    try {
      text   = await callAI(input, mode);
      usedAI = true;
      dot.className = 's-dot s-dot-ok';
    } catch (e) {
      dot.className = 's-dot s-dot-err';
      setTimeout(() => { dot.className = 's-dot s-dot-ok'; }, 3000);

      // Friendly error messages
      let msg = e.message;
      if (msg.includes('401') || msg.includes('Unauthorized') || msg.includes('API key'))
        msg = 'API Key 无效，请检查设置';
      else if (msg.includes('429') || msg.includes('Rate limit'))
        msg = '请求太频繁，请稍后再试';
      else if (msg.includes('402') || msg.includes('quota') || msg.includes('insufficient'))
        msg = '账户余额不足，请充值';
      else if (msg.includes('Failed to fetch') || msg.includes('NetworkError'))
        msg = '网络错误，请检查网络或 Base URL';
      else if (msg.includes('model'))
        msg = 'Model 名称错误，请检查设置';

      setError('⚠ ' + msg + '，已切换本地模式');
      await sleep(80);
      text = localFallback(mode);
    }
  } else {
    hint.textContent = '翻译中...';
    await sleep(120 + Math.random() * 80);
    text = localFallback(mode);
  }

  lastResult = text;

  // Set badge
  const badge = document.getElementById('resultBadge');
  if (usedAI) {
    badge.innerHTML = `<span class="badge badge-ai">${mode==='high'?'高情商':'发疯版'} · AI · ${currentRole} · ${cfg.name}</span>`;
  } else if (mode === 'high') {
    badge.innerHTML = `<span class="badge badge-eq">高情商 · ${currentRole}</span>`;
  } else {
    badge.innerHTML = `<span class="badge badge-crazy">发疯版 · ${currentRole}</span>`;
  }

  document.getElementById('resultText').textContent = '';
  showState('result');
  document.getElementById('resultText').parentElement.classList.add('result-in');

  await typeWriter(document.getElementById('resultText'), text, usedAI ? 4 : 6);
  isLoading = false;
}

/* ─────────────────────────────────────────
   UI HELPERS
───────────────────────────────────────── */
async function typeWriter(el, text, speed) {
  for (let i = 0; i <= text.length; i++) {
    el.textContent = text.slice(0, i);
    await sleep(speed + Math.random() * 4);
  }
}

function sleep(ms) { return new Promise(r => setTimeout(r, ms)); }

function showState(s) {
  document.getElementById('stPlaceholder').style.display = s==='placeholder' ? 'block' : 'none';
  document.getElementById('stLoading').style.display     = s==='loading'     ? 'block' : 'none';
  document.getElementById('stResult').style.display      = s==='result'      ? 'flex'  : 'none';
  if (s !== 'result') {
    document.getElementById('resultText')
      ?.parentElement?.classList.remove('result-in');
  }
}

function setError(msg) {
  document.getElementById('errorMsg').textContent = msg;
}

function resetOutput() {
  lastResult = ''; isLoading = false;
  setError('');
  showState('placeholder');
}

function resetAll() {
  document.getElementById('inputText').value = '';
  document.getElementById('charCount').textContent = '0 / 500';
  document.getElementById('charCount').style.color = '#e0e0e0';
  resetOutput();
  document.getElementById('inputText').focus();
}

async function copyResult() {
  if (!lastResult) return;
  const btn = document.getElementById('copyBtn');
  await navigator.clipboard.writeText(lastResult);
  btn.textContent = '已复制 ✓';
  btn.style.color = '#000';
  document.getElementById('toast').classList.add('show');
  setTimeout(() => {
    btn.innerHTML = '复制 <span style="font-size:12px">↗</span>';
    btn.style.color = '';
    document.getElementById('toast').classList.remove('show');
  }, 2000);
}

/* ─────────────────────────────────────────
   EVENTS
───────────────────────────────────────── */
document.getElementById('inputText').addEventListener('keydown', e => {
  if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); translate('high'); }
});

document.getElementById('inputText').addEventListener('input', function () {
  const len = this.value.length;
  const c   = document.getElementById('charCount');
  c.textContent = `${len} / 500`;
  c.style.color = len > 400 ? '#dc2626' : len > 300 ? '#f59e0b' : '#e0e0e0';
});

document.getElementById('btnHigh').addEventListener('click',  () => translate('high'));
document.getElementById('btnCrazy').addEventListener('click', () => translate('crazy'));

/* ─────────────────────────────────────────
   INIT
───────────────────────────────────────── */
window.addEventListener('load', () => {
  // Role bar
  const firstTab = document.querySelector('.role-tab.active');
  if (firstTab) updateRoleBar(firstTab);

  // Setup overlay
  initOverlay();
  refreshStatusBar();

  // Bind drawer provider pills
  bindProviderPills('#drawerProviders .provider-pill', (b, m) => {
    const isCustom = !b;
    document.getElementById('drawerCustom').style.display = isCustom ? 'block' : 'none';
    if (!isCustom) {
      document.getElementById('drawerBase').value  = b;
      document.getElementById('drawerModel').value = m;
    }
  });
});
</script>
</body>
