
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
    @keyframes fadeUp {
      from { opacity:0; transform:translateY(14px); }
      to   { opacity:1; transform:translateY(0); }
    }
    .fu { animation: fadeUp .65s var(--ease-expo) both; }
    .d1{animation-delay:.05s} .d2{animation-delay:.12s}
    .d3{animation-delay:.19s} .d4{animation-delay:.26s}
    .d5{animation-delay:.33s}
    .role-wrap {
      position: relative; display: flex;
      border-bottom: 1px solid #f0f0f0;
      overflow-x: auto; scrollbar-width: none;
    }
    .role-wrap::-webkit-scrollbar { display: none; }
    .role-tab {
      position: relative; z-index: 1;
      padding: 9px 15px; font-size: 12px; letter-spacing: .07em;
      color: #c8c8c8; background: none; border: none;
      cursor: pointer; font-family: inherit; white-space: nowrap;
      transition: color .2s; flex-shrink: 0;
    }
    .role-tab:hover { color: #666; }
    .role-tab.active { color: #000; font-weight: 500; }
    .role-bar {
      position: absolute; bottom: -1px; height: 2px; background: #000;
      transition: left .36s var(--ease-quart), width .36s var(--ease-quart);
      pointer-events: none;
    }
    textarea {
      resize: none; border: none; outline: none;
      background: transparent; caret-color: #000;
    }
    textarea::placeholder { color: #e4e4e4; }
    .input-line { height:1px; background:#f0f0f0; transition:background .3s ease; }
    .input-wrap:focus-within .input-line { background: #000; }
    .btn {
      cursor: pointer; font-family: inherit;
      letter-spacing: .1em; text-transform: uppercase;
      transition: transform .35s var(--ease-expo),
        background .28s var(--ease-quart), color .28s var(--ease-quart),
        box-shadow .3s ease, opacity .2s;
    }
    .btn:hover  { transform: translateY(-2px); }
    .btn:active { transform: scale(.98); opacity: .8; }
    .btn-solid  { background:#000; color:#fff; }
    .btn-solid:hover  { box-shadow: 0 8px 28px rgba(0,0,0,.16); }
    .btn-outline { background:transparent; color:#000; border:1px solid #d0d0d0; }
    .btn-outline:hover { background:#000; color:#fff; border-color:#000;
                         box-shadow: 0 8px 28px rgba(0,0,0,.13); }
    .btn-ghost {
      background:transparent; border:none; font-family:inherit;
      cursor:pointer; font-size:10px; letter-spacing:.12em;
      text-transform:uppercase; color:#c8c8c8; padding:0;
      transition:color .2s, transform .25s var(--ease-back);
      display:inline-flex; align-items:center; gap:4px;
    }
    .btn-ghost:hover { color:#000; transform:translateY(-1px); }
    .vdiv { width:1px; background:#f0f0f0; align-self:stretch; margin:0 52px; flex-shrink:0; }
    .ai-pill {
      display: inline-flex; align-items: center; gap: 7px;
      padding: 5px 14px; border: 1px solid #f0f0f0;
      font-size: 10px; letter-spacing: .1em; color: #c0c0c0;
      text-transform: uppercase;
    }
    .ai-dot {
      width: 6px; height: 6px; border-radius: 50%;
      background: #22c55e; flex-shrink: 0;
      animation: pulse 2.4s ease-in-out infinite;
    }
    .ai-dot.thinking {
      background: #f59e0b;
      animation: pulse-amber 0.8s ease-in-out infinite;
    }
    .ai-dot.error { background: #ef4444; animation: none; }
    @keyframes pulse {
      0%,100%{ box-shadow:0 0 0 0 rgba(34,197,94,.4); }
      50%    { box-shadow:0 0 0 4px rgba(34,197,94,0); }
    }
    @keyframes pulse-amber {
      0%,100%{ box-shadow:0 0 0 0 rgba(245,158,11,.4); }
      50%    { box-shadow:0 0 0 4px rgba(245,158,11,0); }
    }
    .bar-wrap { display:flex; align-items:flex-end; gap:3px; height:20px; }
    @keyframes bb {
      0%,100%{transform:scaleY(.2);opacity:.2}
      50%{transform:scaleY(1);opacity:1}
    }
    .bar { width:3px; height:100%; background:#d0d0d0;
           transform-origin:bottom; animation:bb .9s ease-in-out infinite; }
    .bar:nth-child(2){animation-delay:.15s}
    .bar:nth-child(3){animation-delay:.3s}
    .bar:nth-child(4){animation-delay:.45s}
    .bar:nth-child(5){animation-delay:.6s}
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
    .badge {
      font-size:9px; letter-spacing:.2em; text-transform:uppercase;
      padding:3px 10px; border:1px solid;
      display:inline-flex; align-items:center; gap:5px;
    }
    .badge::before { content:''; width:5px; height:5px; border-radius:50%; flex-shrink:0; }
    .badge-eq    { border-color:#000; color:#000; }
    .badge-eq::before { background:#000; }
    .badge-crazy { border-color:#dc2626; color:#dc2626; }
    .badge-crazy::before { background:#dc2626; animation:bb .7s infinite; }
    @keyframes shake {
      0%,100%{transform:translateX(0)}
      20%{transform:translateX(-5px)} 40%{transform:translateX(5px)}
      60%{transform:translateX(-3px)} 80%{transform:translateX(3px)}
    }
    .shake { animation: shake .4s ease; }
    .err-box {
      padding:12px 16px;
      border-left:2px solid #ef4444;
      background:#fff9f9;
    }
    .err-box p { font-size:12px; color:#ef4444; margin:0; letter-spacing:.02em; line-height:1.6; }
    .retry-btn {
      font-size:10px; letter-spacing:.1em; text-transform:uppercase;
      background:none; border:1px solid #ef4444; color:#ef4444;
      padding:5px 12px; cursor:pointer; font-family:inherit;
      margin-top:10px;
      transition:background .2s, color .2s;
    }
    .retry-btn:hover { background:#ef4444; color:#fff; }
    #toast {
      position:fixed; bottom:28px; left:50%;
      transform:translateX(-50%) translateY(50px);
      background:#000; color:#fff;
      font-size:10px; letter-spacing:.15em; text-transform:uppercase;
      padding:11px 24px;
      transition:transform .45s var(--ease-expo), opacity .4s ease;
      opacity:0; pointer-events:none; white-space:nowrap;
      display:flex; align-items:center; gap:8px;
    }
    #toast::before { content:'✓'; }
    #toast.show { transform:translateX(-50%) translateY(0); opacity:1; }
    #charCount { transition:color .25s; }
    .cursor::after {
      content:'|'; animation:blink .7s step-end infinite;
      color:#000; margin-left:1px;
    }
    @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }
    @media(max-width:768px){
      .main-grid { flex-direction:column!important; }
      .vdiv { display:none; }
      .right-col { border-top:1px solid #f0f0f0; padding-top:36px; margin-top:36px; }
    }
  </style>
</head>
<body>

<header style="padding:60px 0 40px; text-align:center;">
  <p class="fu d1"
     style="font-size:10px;letter-spacing:.28em;color:#c8c8c8;text-transform:uppercase;margin:0 0 10px;">
    Universal Translator
  </p>
  <h1 class="fu d2"
      style="font-size:28px;font-weight:600;letter-spacing:-.035em;margin:0 0 8px;">
    嘴替
  </h1>
  <p class="fu d3"
     style="font-size:12px;color:#c0c0c0;font-weight:300;letter-spacing:.06em;margin:0 0 22px;">
    想说的话，让我来说。
  </p>
  <div class="fu d4">
    <div class="ai-pill">
      <span class="ai-dot" id="aiDot"></span>
      <span id="aiLabel">AI 已就绪 · 免费使用</span>
    </div>
  </div>
</header>

<main style="max-width:1000px;margin:0 auto;padding:0 36px 90px;">
  <div class="main-grid fu d5" style="display:flex;align-items:flex-start;">

    <!-- ── LEFT ── -->
    <div style="flex:1;display:flex;flex-direction:column;min-width:0;">

      <p style="font-size:9px;letter-spacing:.22em;color:#d0d0d0;text-transform:uppercase;margin:0 0 14px;">
        对象 / Who
      </p>

      <!-- ↓ 删除了室友、相亲 -->
      <div class="role-wrap" id="roleWrap" style="margin-bottom:30px;">
        <div class="role-bar" id="roleBar"></div>
        <button class="role-tab active" data-role="同事">同事</button>
        <button class="role-tab" data-role="领导">领导</button>
        <button class="role-tab" data-role="家人">家人</button>
        <button class="role-tab" data-role="伴侣">伴侣</button>
        <button class="role-tab" data-role="朋友">朋友</button>
        <button class="role-tab" data-role="老师">老师</button>
        <button class="role-tab" data-role="甲方">甲方</button>
      </div>

      <p style="font-size:9px;letter-spacing:.22em;color:#d0d0d0;text-transform:uppercase;margin:0 0 14px;">
        想法 / Input
      </p>

      <div class="input-wrap">
        <textarea id="inputText" rows="6"
          style="font-size:19px;line-height:1.7;font-weight:300;color:#000;width:100%;"
          placeholder="说说你的真实想法..."
          maxlength="500"></textarea>
        <div style="display:flex;justify-content:flex-end;margin-top:6px;">
          <span id="charCount" style="font-size:10px;color:#e4e4e4;">0 / 500</span>
        </div>
        <div class="input-line" style="margin-top:4px;"></div>
      </div>

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

      <p style="font-size:10px;color:#e4e4e4;margin-top:12px;letter-spacing:.03em;">
        Enter 翻译 &nbsp;·&nbsp; Shift+Enter 换行
      </p>
    </div>

    <div class="vdiv"></div>

    <!-- ── RIGHT ── -->
    <div class="right-col" style="flex:1;display:flex;flex-direction:column;min-width:0;">

      <p style="font-size:9px;letter-spacing:.22em;color:#d0d0d0;text-transform:uppercase;margin:0 0 14px;">
        翻译 / Output
      </p>

      <div id="stPlaceholder">
        <p style="font-size:19px;font-weight:300;color:#ebebeb;line-height:1.7;margin:0;">
          翻译结果在这里出现...
        </p>
      </div>

      <div id="stLoading" style="display:none;padding-top:6px;">
        <div class="bar-wrap">
          <div class="bar"></div><div class="bar"></div>
          <div class="bar"></div><div class="bar"></div>
          <div class="bar"></div>
        </div>
        <p id="loadingHint"
           style="font-size:11px;color:#d8d8d8;margin:12px 0 0;letter-spacing:.06em;">
          AI 思考中...
        </p>
      </div>

      <div id="stError" style="display:none;flex-direction:column;">
        <div class="err-box">
          <p id="errMsg">网络异常，请稍后重试。</p>
          <button class="retry-btn" onclick="translate(lastMode)">重试</button>
        </div>
      </div>

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
          <button class="btn-ghost" onclick="translate(lastMode)" style="margin-left:auto;">
            换一条 ↺
          </button>
        </div>
      </div>
    </div>
  </div>

  <div style="margin-top:80px;display:flex;align-items:center;gap:20px;">
    <div style="flex:1;height:1px;background:#f8f8f8;"></div>
    <span style="font-size:9px;letter-spacing:.18em;color:#e8e8e8;">@zeyi</span>
    <div style="flex:1;height:1px;background:#f8f8f8;"></div>
  </div>
</main>

<div id="toast">已复制到剪贴板</div>

<script>

/* ─────────────────────────────────────
   ROLE CONFIG — 删除了室友、相亲
───────────────────────────────────── */
const ROLE_DESC = {
  同事: '平级同事，日常工作协作关系，互相平等',
  领导: '直属上级，有明显的上下级关系，需要保持尊重',
  家人: '父母或家庭成员，亲近但可能有代际矛盾和压力',
  伴侣: '恋人或配偶，最亲密的关系，但也最容易有摩擦',
  朋友: '普通朋友，平等的社交关系，可以直接但不能伤感情',
  老师: '老师或教授，有权威性，需要尊重但也要维护自己',
  甲方: '甲方客户，存在服务和被服务关系，需要专业周到',
};

const PLACEHOLDERS = {
  同事: '比如：这需求改了十几版了我不想改了...',
  领导: '比如：凭什么加班的总是我...',
  家人: '比如：别再催我结婚了烦死了...',
  伴侣: '比如：你根本不理解我的感受...',
  朋友: '比如：感觉你只有有事才找我...',
  老师: '比如：这门课完全听不懂但不敢说...',
  甲方: '比如：改了这么多版还要改我快崩了...',
};

/* ─────────────────────────────────────
   PROMPTS
───────────────────────────────────── */
const SYSTEM_PROMPT = `你是一个精通中国人际关系和沟通的助手，专门帮人把话说得恰到好处。
规则：只输出翻译结果，不加任何解释、前缀、引号。语言自然口语化，像真实的人在说话。`;

function buildPrompt(input, role, mode) {
  const desc = ROLE_DESC[role] || role;
  if (mode === 'high') {
    return `把下面这句话翻译成高情商版本。

说话对象：${role}（${desc}）
原话：「${input}」

要求：
- 保留核心意思，换一种成熟得体的表达
- 语气要符合与"${role}"的关系特点
- 自然口语化，不说废话不说官腔，不用"您"
- 1到2句话，不超过55字
- 直接输出结果`;
  } else {
    return `把下面这句话翻译成夸张发疯版本。

说话对象：${role}（${desc}）
原话：「${input}」

要求：
- 极度夸张，充满崩溃感和戏剧性
- 多用！！！，情绪要爆炸
- 要结合"${role}"这个对象，发疯要有针对性，贴近生活让人感同身受想大笑
- 2到3句话，不超过75字
- 直接输出结果`;
  }
}

/* ─────────────────────────────────────
   AI CALL — Pollinations（免费，无需Key）
───────────────────────────────────── */
async function callAI(input, role, mode) {
  const body = {
    messages: [
      { role: 'system', content: SYSTEM_PROMPT },
      { role: 'user',   content: buildPrompt(input, role, mode) },
    ],
    model:       'openai',
    temperature: mode === 'crazy' ? 1.1 : 0.88,
    seed:        Math.floor(Math.random() * 9999),
    private:     true,
  };

  const res = await fetch('https://text.pollinations.ai/', {
    method:  'POST',
    headers: { 'Content-Type': 'application/json' },
    body:    JSON.stringify(body),
  });

  if (!res.ok) {
    const txt = await res.text().catch(() => '');
    throw new Error(`HTTP ${res.status}${txt ? ': ' + txt.slice(0,80) : ''}`);
  }

  const text = await res.text();
  if (!text || !text.trim()) throw new Error('返回内容为空');

  return text.trim().replace(/^["「『]|["」』]$/g, '').trim();
}

/* ─────────────────────────────────────
   ROLE TABS
───────────────────────────────────── */
let currentRole = '同事';

function moveBar(tab) {
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
    moveBar(btn);
    resetOutput();
    document.getElementById('inputText').placeholder =
      PLACEHOLDERS[currentRole] || '说说你的真实想法...';
  });
});

/* ─────────────────────────────────────
   STATE
───────────────────────────────────── */
let isLoading  = false;
let lastResult = '';
let lastMode   = 'high';

function showState(s) {
  document.getElementById('stPlaceholder').style.display = s==='placeholder' ? 'block' : 'none';
  document.getElementById('stLoading').style.display     = s==='loading'     ? 'block' : 'none';
  document.getElementById('stError').style.display       = s==='error'       ? 'flex'  : 'none';
  document.getElementById('stResult').style.display      = s==='result'      ? 'flex'  : 'none';
}

function setAIDot(state) {
  const dot   = document.getElementById('aiDot');
  const label = document.getElementById('aiLabel');
  if (state === 'thinking') {
    dot.className     = 'ai-dot thinking';
    label.textContent = 'AI 思考中...';
  } else if (state === 'error') {
    dot.className     = 'ai-dot error';
    label.textContent = '网络异常，稍后重试';
    setTimeout(() => setAIDot('ready'), 4000);
  } else {
    dot.className     = 'ai-dot';
    label.textContent = 'AI 已就绪 · 免费使用';
  }
}

/* ─────────────────────────────────────
   TRANSLATE
───────────────────────────────────── */
async function translate(mode) {
  if (isLoading) return;

  const input = document.getElementById('inputText').value.trim();
  if (!input) {
    const ta = document.getElementById('inputText');
    ta.classList.add('shake');
    ta.focus();
    setTimeout(() => ta.classList.remove('shake'), 420);
    return;
  }

  lastMode  = mode;
  isLoading = true;
  setAIDot('thinking');
  showState('loading');

  try {
    const text = await callAI(input, currentRole, mode);
    lastResult = text;

    const badge = document.getElementById('resultBadge');
    badge.innerHTML = mode === 'high'
      ? `<span class="badge badge-eq">高情商 · ${currentRole}</span>`
      : `<span class="badge badge-crazy">发疯版 · ${currentRole}</span>`;

    const resultEl = document.getElementById('resultText');
    resultEl.textContent = '';
    resultEl.classList.remove('cursor');
    showState('result');
    resultEl.parentElement.classList.add('result-in');

    await typeWriter(resultEl, text, 5);
    setAIDot('ready');

  } catch (e) {
    console.error(e);
    setAIDot('error');

    let msg = '网络异常，请检查网络后重试。';
    const em = e.message || '';
    if (em.includes('429'))
      msg = '请求太频繁了，稍等几秒再试。';
    else if (em.includes('500') || em.includes('503'))
      msg = 'AI 服务暂时繁忙，点击重试。';
    else if (em.includes('Failed to fetch') || em.includes('NetworkError'))
      msg = '无法连接 AI 服务，请检查网络。';

    document.getElementById('errMsg').textContent = msg;
    showState('error');
  }

  isLoading = false;
}

/* ─────────────────────────────────────
   TYPEWRITER
───────────────────────────────────── */
async function typeWriter(el, text, speed) {
  el.classList.add('cursor');
  for (let i = 0; i <= text.length; i++) {
    el.textContent = text.slice(0, i);
    await sleep(speed + Math.random() * 5);
  }
  el.classList.remove('cursor');
}

function sleep(ms) { return new Promise(r => setTimeout(r, ms)); }

/* ─────────────────────────────────────
   HELPERS
───────────────────────────────────── */
function resetOutput() {
  lastResult = ''; isLoading = false;
  showState('placeholder');
}

function resetAll() {
  document.getElementById('inputText').value = '';
  document.getElementById('charCount').textContent = '0 / 500';
  document.getElementById('charCount').style.color = '#e4e4e4';
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
    btn.innerHTML   = '复制 <span style="font-size:12px">↗</span>';
    btn.style.color = '';
    document.getElementById('toast').classList.remove('show');
  }, 2000);
}

/* ─────────────────────────────────────
   EVENTS
───────────────────────────────────── */
document.getElementById('inputText').addEventListener('keydown', e => {
  if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); translate('high'); }
});

document.getElementById('inputText').addEventListener('input', function () {
  const len = this.value.length;
  const c   = document.getElementById('charCount');
  c.textContent = `${len} / 500`;
  c.style.color = len > 400 ? '#dc2626' : len > 300 ? '#f59e0b' : '#e4e4e4';
});

document.getElementById('btnHigh').addEventListener('click',  () => translate('high'));
document.getElementById('btnCrazy').addEventListener('click', () => translate('crazy'));

/* ─────────────────────────────────────
   INIT
───────────────────────────────────── */
window.addEventListener('load', () => {
  const first = document.querySelector('.role-tab.active');
  if (first) moveBar(first);
});
</script>
</body>
