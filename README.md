<meta name='viewport' content='width=device-width, initial-scale=1'/><!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1">
<title>TPT_5201 -- One Mind. Every Tool.</title>
<style>
  :root{
    --bg-deep:#090B14;
    --bg-panel:#12162A;
    --bg-panel-2:#171C34;
    --bg-composer:#0F1226;
    --line:#242a4a;
    --purple:#8B6BFF;
    --purple-soft:#6C4CFF;
    --blue:#4A6CFF;
    --cyan:#4FE0E8;
    --pink:#FF6BD6;
    --text:#EDEEFB;
    --text-muted:#9096B8;
    --text-dim:#5C6188;
    --radius:16px;
    --font-display:'Space Grotesk',-apple-system,sans-serif;
    --font-body:'Inter',-apple-system,sans-serif;
    --font-mono:'JetBrains Mono','Fira Code',monospace;
  }
  [data-theme="light"]{
    --bg-deep:#F5F6FC;
    --bg-panel:#FFFFFF;
    --bg-panel-2:#F0F1FA;
    --bg-composer:#FFFFFF;
    --line:#E2E4F3;
    --text:#181B2E;
    --text-muted:#5C6188;
    --text-dim:#9096B8;
  }
  *{box-sizing:border-box; margin:0; padding:0;}
  @font-face{ font-family:'Space Grotesk'; src:local('Space Grotesk'); }
  body{
    font-family:var(--font-body);
    background:var(--bg-deep);
    color:var(--text);
    height:100vh;
    overflow:hidden;
    transition:background .3s ease, color .3s ease;
  }
  #app{ display:flex; flex-direction:column; height:100vh; position:relative; }

  /* ---------- HEADER ---------- */
  header{
    display:flex; align-items:center; justify-content:space-between;
    padding:14px 20px; border-bottom:1px solid var(--line);
    background:linear-gradient(180deg, rgba(139,107,255,0.06), transparent);
    flex-shrink:0;
  }
  .brand{ display:flex; align-items:center; gap:10px; }
  .logo{ width:34px; height:34px; position:relative; flex-shrink:0; }
  .logo svg{ width:100%; height:100%; filter:drop-shadow(0 0 6px rgba(139,107,255,0.7)); }
  .logo .node{ animation:pulse 2.6s ease-in-out infinite; }
  .logo .node:nth-child(2){ animation-delay:.3s; }
  .logo .node:nth-child(3){ animation-delay:.6s; }
  .logo .node:nth-child(4){ animation-delay:.9s; }
  .logo .node:nth-child(5){ animation-delay:1.2s; }
  @keyframes pulse{ 0%,100%{ opacity:.55; } 50%{ opacity:1; } }
  .brand-text h1{ font-family:var(--font-display); font-size:17px; font-weight:600; letter-spacing:0.2px; }
  .brand-text p{ font-size:11px; color:var(--text-muted); letter-spacing:0.4px; }
  .header-actions{ display:flex; align-items:center; gap:8px; }
  .icon-btn{
    width:34px; height:34px; border-radius:10px; border:1px solid var(--line);
    background:var(--bg-panel); color:var(--text-muted); display:flex; align-items:center; justify-content:center;
    cursor:pointer; transition:.15s; font-size:15px;
  }
  .icon-btn:hover{ border-color:var(--purple); color:var(--text); }

  /* ---------- TOOLS ROW ---------- */
  .tools-row{
    display:flex; gap:8px; padding:10px 20px; overflow-x:auto;
    border-bottom:1px solid var(--line); flex-shrink:0;
  }
  .tool-btn{
    display:flex; align-items:center; gap:6px; padding:7px 13px; border-radius:20px;
    border:1px solid var(--line); background:var(--bg-panel); color:var(--text-muted);
    font-size:12.5px; font-weight:500; cursor:pointer; white-space:nowrap; transition:.15s;
  }
  .tool-btn .emoji{ font-size:14px; }
  .tool-btn:hover{ color:var(--text); }
  .tool-btn.active{
    color:#fff; border-color:transparent;
    background:linear-gradient(135deg, var(--purple-soft), var(--blue));
    box-shadow:0 0 14px rgba(139,107,255,0.45);
  }

  /* ---------- MAIN LAYOUT ---------- */
  .main{ flex:1; display:flex; min-height:0; position:relative; }
  .chat-col{ flex:1; display:flex; flex-direction:column; min-width:0; transition:.3s ease; }
  .chat-scroll{ flex:1; overflow-y:auto; padding:22px 16px 10px; }
  .chat-inner{ max-width:720px; margin:0 auto; display:flex; flex-direction:column; gap:16px; }

  .empty-state{ max-width:520px; margin:60px auto; text-align:center; }
  .empty-state .logo-big{ width:64px; height:64px; margin:0 auto 18px; }
  .empty-state h2{ font-family:var(--font-display); font-size:22px; margin-bottom:8px; }
  .empty-state p{ color:var(--text-muted); font-size:13.5px; line-height:1.6; margin-bottom:20px;}
  .suggestions{ display:flex; flex-wrap:wrap; gap:8px; justify-content:center; }
  .suggestion{
    font-size:12.5px; padding:9px 14px; border-radius:12px; border:1px solid var(--line);
    background:var(--bg-panel); color:var(--text-muted); cursor:pointer; transition:.15s;
  }
  .suggestion:hover{ border-color:var(--purple); color:var(--text); }

  .msg{ display:flex; gap:10px; max-width:100%; }
  .msg.user{ flex-direction:row-reverse; }
  .avatar{
    width:28px; height:28px; border-radius:9px; flex-shrink:0; display:flex; align-items:center; justify-content:center;
    font-size:13px; margin-top:2px;
  }
  .avatar.ai{ background:linear-gradient(135deg, var(--purple-soft), var(--blue)); }
  .avatar.user{ background:var(--bg-panel-2); border:1px solid var(--line); }
  .bubble{
    padding:11px 15px; border-radius:14px; font-size:14.5px; line-height:1.65; max-width:78%;
    white-space:pre-wrap; word-wrap:break-word;
  }
  .msg.ai .bubble{ background:var(--bg-panel); border:1px solid var(--line); border-top-left-radius:4px; }
  .msg.user .bubble{ background:linear-gradient(135deg, var(--purple-soft), var(--blue)); color:#fff; border-top-right-radius:4px; }
  .bubble img{ max-width:220px; border-radius:10px; margin-top:6px; display:block; }
  .bubble code{ font-family:var(--font-mono); background:rgba(139,107,255,0.12); padding:1px 5px; border-radius:4px; font-size:13px; }
  .msg-tag{ font-size:10px; color:var(--text-dim); margin-top:4px; display:block; }
  .canvas-flag{
    display:inline-flex; align-items:center; gap:5px; margin-top:8px; font-size:11.5px;
    color:var(--purple); border:1px solid var(--purple); padding:5px 10px; border-radius:8px; cursor:pointer;
  }

  .thinking{ display:flex; gap:5px; padding:6px 0; }
  .thinking span{ width:6px; height:6px; border-radius:50%; background:var(--purple); animation:bounce 1.1s infinite; }
  .thinking span:nth-child(2){ animation-delay:.15s; }
  .thinking span:nth-child(3){ animation-delay:.3s; }
  @keyframes bounce{ 0%,60%,100%{ transform:translateY(0); opacity:.4;} 30%{ transform:translateY(-5px); opacity:1;} }

  /* ---------- COMPOSER ---------- */
  .composer-wrap{ padding:12px 16px 18px; flex-shrink:0; }
  .composer{
    max-width:720px; margin:0 auto; display:flex; align-items:flex-end; gap:8px;
    background:var(--bg-composer); border:1px solid var(--line); border-radius:20px; padding:8px 8px 8px 14px;
    box-shadow:0 4px 24px rgba(0,0,0,0.15);
  }
  .composer textarea{
    flex:1; background:none; border:none; outline:none; color:var(--text); font-family:var(--font-body);
    font-size:14.5px; resize:none; max-height:120px; line-height:1.5; padding:8px 0;
  }
  .composer textarea::placeholder{ color:var(--text-dim); }
  .composer-actions{ display:flex; align-items:center; gap:4px; }
  .send-btn{
    width:36px; height:36px; border-radius:12px; border:none; cursor:pointer;
    background:linear-gradient(135deg, var(--purple-soft), var(--blue)); color:#fff; font-size:15px;
    display:flex; align-items:center; justify-content:center; transition:.15s;
  }
  .send-btn:disabled{ opacity:.4; cursor:not-allowed; }
  .mic-btn.listening{ background:var(--pink); color:#fff; border-color:transparent; animation:pulse 1s infinite; }
  .status-strip{ max-width:720px; margin:0 auto; display:flex; justify-content:space-between; padding-top:6px; }
  .status-strip span{ font-size:10.5px; color:var(--text-dim); }
  .offline-pill{ display:none; align-items:center; gap:5px; }
  .offline-pill.show{ display:flex; }
  .offline-pill .dot{ width:6px; height:6px; border-radius:50%; background:#FFB84F; }

  /* ---------- CANVAS PANEL ---------- */
  .canvas-panel{
    width:0; overflow:hidden; border-left:1px solid var(--line); background:var(--bg-panel);
    display:flex; flex-direction:column; transition:width .28s ease;
  }
  .canvas-panel.open{ width:min(44%, 480px); }
  .canvas-head{ display:flex; align-items:center; justify-content:space-between; padding:14px 16px; border-bottom:1px solid var(--line); }
  .canvas-head h3{ font-size:13px; font-family:var(--font-display); }
  .canvas-body{ flex:1; overflow:auto; padding:16px; }
  .canvas-body pre{ font-family:var(--font-mono); font-size:12.5px; line-height:1.6; white-space:pre-wrap; word-break:break-word; color:var(--text); }
  .canvas-actions{ display:flex; gap:8px; padding:10px 16px; border-top:1px solid var(--line); }
  .mini-btn{
    font-size:11.5px; padding:6px 12px; border-radius:8px; border:1px solid var(--line);
    background:var(--bg-panel-2); color:var(--text-muted); cursor:pointer;
  }
  .mini-btn:hover{ color:var(--text); border-color:var(--purple); }

  /* ---------- SETTINGS MODAL ---------- */
  .modal-backdrop{
    position:fixed; inset:0; background:rgba(0,0,0,0.5); display:none; align-items:center; justify-content:center; z-index:50;
  }
  .modal-backdrop.show{ display:flex; }
  .modal{ background:var(--bg-panel); border:1px solid var(--line); border-radius:18px; width:min(90%,380px); padding:22px; }
  .modal h3{ font-family:var(--font-display); font-size:16px; margin-bottom:16px; }
  .row-item{ display:flex; align-items:center; justify-content:space-between; padding:11px 0; border-bottom:1px solid var(--line); }
  .row-item:last-child{ border-bottom:none; }
  .row-item .label{ font-size:13.5px; }
  .row-item .sub{ font-size:11px; color:var(--text-muted); }
  .switch{ width:40px; height:22px; border-radius:20px; background:var(--bg-panel-2); border:1px solid var(--line); position:relative; cursor:pointer; flex-shrink:0; }
  .switch.on{ background:linear-gradient(135deg, var(--purple-soft), var(--blue)); border-color:transparent; }
  .switch .knob{ width:16px; height:16px; border-radius:50%; background:#fff; position:absolute; top:2px; left:2px; transition:.2s; }
  .switch.on .knob{ left:20px; }
  .modal-close{ width:100%; margin-top:16px; padding:10px; border-radius:10px; border:1px solid var(--line); background:var(--bg-panel-2); color:var(--text); cursor:pointer; font-size:13px; }

  ::-webkit-scrollbar{ width:8px; }
  ::-webkit-scrollbar-thumb{ background:var(--line); border-radius:8px; }

  @media (max-width:760px){
    .canvas-panel.open{ position:fixed; inset:0; width:100%; z-index:40; }
  }
</style>
</head>
<body data-theme="dark">
<div id="app">

  <header>
    <div class="brand">
      <div class="logo">
        <svg viewBox="0 0 100 100">
          <circle cx="50" cy="50" r="46" fill="none" stroke="url(#g1)" stroke-width="2" opacity="0.5"/>
          <circle class="node" cx="50" cy="18" r="7" fill="#8B6BFF"/>
          <circle class="node" cx="79" cy="38" r="7" fill="#4A6CFF"/>
          <circle class="node" cx="68" cy="74" r="7" fill="#FF6BD6"/>
          <circle class="node" cx="32" cy="74" r="7" fill="#4FE0E8"/>
          <circle class="node" cx="21" cy="38" r="7" fill="#8B6BFF"/>
          <defs><linearGradient id="g1" x1="0" y1="0" x2="1" y2="1">
            <stop offset="0" stop-color="#8B6BFF"/><stop offset="1" stop-color="#4A6CFF"/>
          </linearGradient></defs>
        </svg>
      </div>
      <div class="brand-text">
        <h1>TPT_5201</h1>
        <p>ONE MIND. EVERY TOOL.</p>
      </div>
    </div>
    <div class="header-actions">
      <button class="icon-btn" id="themeBtn" title="Toggle theme">🌙</button>
      <button class="icon-btn" id="settingsBtn" title="Settings">⚙️</button>
      <button class="icon-btn" id="newChatBtn" title="New chat">✚</button>
    </div>
  </header>

  <div class="tools-row" id="toolsRow">
    <button class="tool-btn" data-mode="brain"><span class="emoji">🧠</span>Deep Think</button>
    <button class="tool-btn" data-mode="heart"><span class="emoji">💜</span>Empathy</button>
    <button class="tool-btn" data-mode="lightning"><span class="emoji">⚡</span>Real-Time</button>
    <button class="tool-btn" data-mode="sparkle"><span class="emoji">✨</span>Create</button>
    <button class="tool-btn" data-mode="globe"><span class="emoji">🌐</span>Live Search</button>
  </div>

  <div class="main">
    <div class="chat-col">
      <div class="chat-scroll" id="chatScroll">
        <div class="chat-inner" id="chatInner">
          <div class="empty-state" id="emptyState">
            <div class="logo-big">
              <svg viewBox="0 0 100 100">
                <circle class="node" cx="50" cy="18" r="7" fill="#8B6BFF"/>
                <circle class="node" cx="79" cy="38" r="7" fill="#4A6CFF"/>
                <circle class="node" cx="68" cy="74" r="7" fill="#FF6BD6"/>
                <circle class="node" cx="32" cy="74" r="7" fill="#4FE0E8"/>
                <circle class="node" cx="21" cy="38" r="7" fill="#8B6BFF"/>
              </svg>
            </div>
            <h2>What's on your mind?</h2>
            <p>Pick a mode above, or just start typing. TPT_5201 remembers how you like to work.</p>
            <div class="suggestions">
              <div class="suggestion" data-fill="Explain quantum entanglement like I'm smart but not a physicist">Explain quantum entanglement</div>
              <div class="suggestion" data-fill="Write me 3 caption options for a trip to Moshi, Tanzania">Write trip captions</div>
              <div class="suggestion" data-fill="Write a Python script that renames files in a folder by date">Write a Python script</div>
            </div>
          </div>
        </div>
      </div>

      <div class="composer-wrap">
        <div class="composer">
          <button class="icon-btn" id="attachBtn" title="Attach image">📎</button>
          <input type="file" id="fileInput" accept="image/*" style="display:none">
          <textarea id="input" placeholder="Message TPT_5201..." rows="1"></textarea>
          <div class="composer-actions">
            <button class="icon-btn mic-btn" id="micBtn" title="Voice input">🎙️</button>
            <button class="send-btn" id="sendBtn" title="Send">➤</button>
          </div>
        </div>
        <div class="status-strip">
          <span id="modeLabel">Mode: Standard</span>
          <span class="offline-pill" id="offlinePill"><span class="dot"></span>Offline mode -- local replies only</span>
        </div>
      </div>
    </div>

    <div class="canvas-panel" id="canvasPanel">
      <div class="canvas-head">
        <h3 id="canvasTitle">Canvas</h3>
        <button class="icon-btn" id="canvasClose">✕</button>
      </div>
      <div class="canvas-body"><pre id="canvasContent"></pre></div>
      <div class="canvas-actions">
        <button class="mini-btn" id="canvasCopy">Copy</button>
      </div>
    </div>
  </div>
</div>

<div class="modal-backdrop" id="settingsModal">
  <div class="modal">
    <h3>Settings</h3>
    <div class="row-item">
      <div><div class="label">Offline mode</div><div class="sub">Use local replies, no network calls</div></div>
      <div class="switch" id="offlineSwitch"><div class="knob"></div></div>
    </div>
    <div class="row-item">
      <div><div class="label">Speak replies aloud</div><div class="sub">Uses your browser's voice</div></div>
      <div class="switch" id="voiceSwitch"><div class="knob"></div></div>
    </div>
    <div class="row-item">
      <div><div class="label">Clear memory</div><div class="sub">Forget saved chat history &amp; preferences</div></div>
      <button class="mini-btn" id="clearMemBtn">Clear</button>
    </div>
    <button class="modal-close" id="closeModalBtn">Done</button>
  </div>
</div>

<script>
(function(){
  const state = {
    mode: null,
    theme: 'dark',
    offline: false,
    speak: false,
    messages: [], // {role, content, imageData}
  };

  const els = {
    chatInner: document.getElementById('chatInner'),
    chatScroll: document.getElementById('chatScroll'),
    emptyState: document.getElementById('emptyState'),
    input: document.getElementById('input'),
    sendBtn: document.getElementById('sendBtn'),
    micBtn: document.getElementById('micBtn'),
    attachBtn: document.getElementById('attachBtn'),
    fileInput: document.getElementById('fileInput'),
    modeLabel: document.getElementById('modeLabel'),
    offlinePill: document.getElementById('offlinePill'),
    canvasPanel: document.getElementById('canvasPanel'),
    canvasContent: document.getElementById('canvasContent'),
    canvasTitle: document.getElementById('canvasTitle'),
    canvasClose: document.getElementById('canvasClose'),
    canvasCopy: document.getElementById('canvasCopy'),
    themeBtn: document.getElementById('themeBtn'),
    settingsBtn: document.getElementById('settingsBtn'),
    settingsModal: document.getElementById('settingsModal'),
    closeModalBtn: document.getElementById('closeModalBtn'),
    offlineSwitch: document.getElementById('offlineSwitch'),
    voiceSwitch: document.getElementById('voiceSwitch'),
    clearMemBtn: document.getElementById('clearMemBtn'),
    newChatBtn: document.getElementById('newChatBtn'),
  };

  let pendingImage = null;

  // ---------- MODE CONFIG ----------
  const MODES = {
    brain: {
      label: 'Deep Think',
      system: 'You are in Deep Thinking mode. Reason carefully and step by step before answering. Break down complex problems methodically, show your work, and arrive at a thorough, well-structured, correct answer.'
    },
    heart: {
      label: 'Empathy',
      system: 'You are in Empathy mode. Be warm, patient, and genuinely caring. Listen closely, validate feelings where appropriate, and prioritize the person\'s wellbeing alongside being helpful. Keep a gentle, human tone.'
    },
    lightning: {
      label: 'Real-Time',
      system: 'You are in Real-Time mode. Be direct, concise, and honest, with a bit of dry humor. No hedging, no fluff -- get to the point fast.'
    },
    sparkle: {
      label: 'Create',
      system: 'You are in Create mode, focused on creative output -- captions, scripts, ideas, copy. Be imaginative and offer options. Note: this build generates text/creative copy only -- actual image/video generation would need a connected media API, so if asked, offer strong written alternatives (scripts, prompts, captions) instead of claiming to produce media.'
    },
    globe: {
      label: 'Live Search',
      system: 'You are in Live Search mode. Use web search to ground your answer in current, real information, and cite what you find plainly in your own words.'
    }
  };

  function setMode(m){
    state.mode = state.mode === m ? null : m;
    document.querySelectorAll('.tool-btn').forEach(b=>{
      b.classList.toggle('active', b.dataset.mode === state.mode);
    });
    els.modeLabel.textContent = state.mode ? 'Mode: ' + MODES[state.mode].label : 'Mode: Standard';
  }
  document.getElementById('toolsRow').addEventListener('click', e=>{
    const btn = e.target.closest('.tool-btn');
    if(btn) setMode(btn.dataset.mode);
  });

  // ---------- THEME ----------
  function applyTheme(t){
    state.theme = t;
    document.body.setAttribute('data-theme', t);
    els.themeBtn.textContent = t === 'dark' ? '🌙' : '☀️';
  }
  els.themeBtn.addEventListener('click', ()=>{
    applyTheme(state.theme === 'dark' ? 'light' : 'dark');
    saveSettings();
  });

  // ---------- SETTINGS MODAL ----------
  els.settingsBtn.addEventListener('click', ()=> els.settingsModal.classList.add('show'));
  els.closeModalBtn.addEventListener('click', ()=> els.settingsModal.classList.remove('show'));
  els.settingsModal.addEventListener('click', e=>{ if(e.target === els.settingsModal) els.settingsModal.classList.remove('show'); });

  els.offlineSwitch.addEventListener('click', ()=>{
    state.offline = !state.offline;
    els.offlineSwitch.classList.toggle('on', state.offline);
    els.offlinePill.classList.toggle('show', state.offline);
    saveSettings();
  });
  els.voiceSwitch.addEventListener('click', ()=>{
    state.speak = !state.speak;
    els.voiceSwitch.classList.toggle('on', state.speak);
    saveSettings();
  });

  els.newChatBtn.addEventListener('click', ()=>{
    state.messages = [];
    renderAll();
  });

  els.clearMemBtn.addEventListener('click', async ()=>{
    try{
      await window.storage.delete('chat-history', false);
      await window.storage.delete('app-settings', false);
    }catch(e){ /* nothing stored yet */ }
    state.messages = [];
    renderAll();
    els.settingsModal.classList.remove('show');
  });

  // ---------- PERSISTENCE ----------
  async function saveSettings(){
    try{
      await window.storage.set('app-settings', JSON.stringify({
        theme: state.theme, offline: state.offline, speak: state.speak
      }), false);
    }catch(e){ console.warn('Could not save settings', e); }
  }
  async function saveHistory(){
    try{
      await window.storage.set('chat-history', JSON.stringify(state.messages.slice(-60)), false);
    }catch(e){ console.warn('Could not save history', e); }
  }
  async function loadAll(){
    try{
      const s = await window.storage.get('app-settings', false);
      if(s && s.value){
        const parsed = JSON.parse(s.value);
        applyTheme(parsed.theme || 'dark');
        state.offline = !!parsed.offline;
        state.speak = !!parsed.speak;
        els.offlineSwitch.classList.toggle('on', state.offline);
        els.offlinePill.classList.toggle('show', state.offline);
        els.voiceSwitch.classList.toggle('on', state.speak);
      }
    }catch(e){ /* first run, nothing saved */ }
    try{
      const h = await window.storage.get('chat-history', false);
      if(h && h.value){
        state.messages = JSON.parse(h.value);
        renderAll();
      }
    }catch(e){ /* first run */ }
  }

  // ---------- RENDERING ----------
  function renderAll(){
    els.chatInner.innerHTML = '';
    if(state.messages.length === 0){
      els.chatInner.appendChild(els.emptyState);
      return;
    }
    state.messages.forEach(m => els.chatInner.appendChild(buildBubble(m)));
    scrollToBottom();
  }

  function buildBubble(m){
    const wrap = document.createElement('div');
    wrap.className = 'msg ' + (m.role === 'user' ? 'user' : 'ai');
    const avatar = document.createElement('div');
    avatar.className = 'avatar ' + (m.role === 'user' ? 'user' : 'ai');
    avatar.textContent = m.role === 'user' ? '🙂' : '◈';
    const bubble = document.createElement('div');
    bubble.className = 'bubble';
    bubble.innerHTML = mdLite(m.content || '');
    if(m.imageData){
      const img = document.createElement('img');
      img.src = m.imageData;
      bubble.appendChild(img);
    }
    if(m.role !== 'user' && m.codeBlock){
      const flag = document.createElement('div');
      flag.className = 'canvas-flag';
      flag.textContent = '⬛ Open in Canvas';
      flag.addEventListener('click', ()=> openCanvas(m.codeBlock, m.codeLang));
      bubble.appendChild(flag);
    }
    wrap.appendChild(avatar);
    wrap.appendChild(bubble);
    return wrap;
  }

  function mdLite(text){
    let t = text
      .replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;')
      .replace(/```[\s\S]*?```/g, '') // code blocks pulled out into canvas, not shown inline
      .replace(/\*\*(.+?)\*\*/g, '<strong>$1</strong>')
      .replace(/`([^`]+)`/g, '<code>$1</code>');
    return t.trim() || '<em>(sent to canvas)</em>';
  }

  function scrollToBottom(){
    els.chatScroll.scrollTop = els.chatScroll.scrollHeight;
  }

  function addThinkingBubble(){
    const wrap = document.createElement('div');
    wrap.className = 'msg ai';
    wrap.id = 'thinkingBubble';
    wrap.innerHTML = '<div class="avatar ai">◈</div><div class="bubble"><div class="thinking"><span></span><span></span><span></span></div></div>';
    els.chatInner.appendChild(wrap);
    scrollToBottom();
  }
  function removeThinkingBubble(){
    const el = document.getElementById('thinkingBubble');
    if(el) el.remove();
  }

  // ---------- CANVAS ----------
  function openCanvas(code, lang){
    els.canvasTitle.textContent = lang ? lang.toUpperCase() + ' -- Canvas' : 'Canvas';
    els.canvasContent.textContent = code;
    els.canvasPanel.classList.add('open');
  }
  els.canvasClose.addEventListener('click', ()=> els.canvasPanel.classList.remove('open'));
  els.canvasCopy.addEventListener('click', ()=>{
    navigator.clipboard.writeText(els.canvasContent.textContent).then(()=>{
      els.canvasCopy.textContent = 'Copied!';
      setTimeout(()=> els.canvasCopy.textContent = 'Copy', 1200);
    });
  });

  function extractCodeBlock(text){
    const match = text.match(/```(\w*)\n([\s\S]*?)```/);
    if(!match) return null;
    return { lang: match[1] || 'text', code: match[2].trim() };
  }

  // ---------- IMAGE ATTACH ----------
  els.attachBtn.addEventListener('click', ()=> els.fileInput.click());
  els.fileInput.addEventListener('change', e=>{
    const file = e.target.files[0];
    if(!file) return;
    const reader = new FileReader();
    reader.onload = ()=>{ pendingImage = reader.result; els.attachBtn.textContent = '🖼️'; };
    reader.readAsDataURL(file);
  });

  // ---------- VOICE INPUT ----------
  let recognition = null;
  const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
  if(SR){
    recognition = new SR();
    recognition.continuous = false;
    recognition.interimResults = false;
    recognition.onresult = (e)=>{
      els.input.value += (els.input.value ? ' ' : '') + e.results[0][0].transcript;
      autoGrow();
    };
    recognition.onend = ()=> els.micBtn.classList.remove('listening');
  }
  els.micBtn.addEventListener('click', ()=>{
    if(!recognition){
      alert('Voice input isn\'t supported in this browser. Try Chrome on desktop or Android.');
      return;
    }
    els.micBtn.classList.add('listening');
    recognition.start();
  });

  function speak(text){
    if(!state.speak || !window.speechSynthesis) return;
    const u = new SpeechSynthesisUtterance(text.replace(/```[\s\S]*?```/g,'').slice(0, 600));
    window.speechSynthesis.speak(u);
  }

  // ---------- INPUT AUTOGROW ----------
  function autoGrow(){
    els.input.style.height = 'auto';
    els.input.style.height = Math.min(els.input.scrollHeight, 120) + 'px';
  }
  els.input.addEventListener('input', autoGrow);
  els.input.addEventListener('keydown', e=>{
    if(e.key === 'Enter' && !e.shiftKey){ e.preventDefault(); handleSend(); }
  });
  els.sendBtn.addEventListener('click', handleSend);
  document.querySelectorAll('.suggestion').forEach(s=>{
    s.addEventListener('click', ()=>{ els.input.value = s.dataset.fill; handleSend(); });
  });

  // ---------- SEND / RESPOND ----------
  async function handleSend(){
    const text = els.input.value.trim();
    if(!text && !pendingImage) return;
    const userMsg = { role:'user', content: text, imageData: pendingImage };
    state.messages.push(userMsg);
    if(els.emptyState.parentNode) els.emptyState.remove();
    els.chatInner.appendChild(buildBubble(userMsg));
    scrollToBottom();
    els.input.value = '';
    autoGrow();
    const imgToSend = pendingImage;
    pendingImage = null;
    els.attachBtn.textContent = '📎';
    els.sendBtn.disabled = true;
    saveHistory();

    addThinkingBubble();
    let reply;
    try{
      reply = state.offline ? offlineReply(text) : await callClaude(text, imgToSend);
    }catch(err){
      reply = "I couldn't reach the network for that one (" + (err.message || 'connection issue') + "). You can switch on Offline mode in Settings to keep chatting with local replies.";
    }
    removeThinkingBubble();

    const codeBlock = extractCodeBlock(reply);
    const aiMsg = { role:'assistant', content: reply, codeBlock: codeBlock ? codeBlock.code : null, codeLang: codeBlock ? codeBlock.lang : null };
    state.messages.push(aiMsg);
    els.chatInner.appendChild(buildBubble(aiMsg));
    scrollToBottom();
    if(codeBlock) openCanvas(codeBlock.code, codeBlock.lang);
    speak(reply);
    saveHistory();
    els.sendBtn.disabled = false;
  }

  async function callClaude(text, imageData){
    const systemPrompt = "You are TPT_5201, a single unified AI assistant blending several assistant styles into one -- clear step-by-step explanation, empathetic listening, direct real-time honesty, creative output, and search-grounded accuracy -- while always being fundamentally one consistent, honest assistant. You are bilingual: always reply in whichever language the user's latest message is written in -- natural, correct Kiswahili if they write in Kiswahili, and natural English if they write in English. If they mix both languages, mirror that mix. Never mention that you are choosing a language; just respond naturally in it." +
      (state.mode ? (' ' + MODES[state.mode].system) : '');

    // state.messages already contains the current user turn (it was pushed
    // in handleSend before this function was called), so slicing it here is
    // enough -- pushing it again would send the same user turn twice in a
    // row, which the API rejects (roles must alternate user/assistant).
    const history = state.messages.slice(-10).map(m=>{
      if(m.role === 'user' && m.imageData){
        const mime = (m.imageData.match(/^data:(image\/[a-z0-9.+-]+);base64,/i) || [,'image/jpeg'])[1];
        return { role:'user', content:[
          { type:'image', source:{ type:'base64', media_type: mime, data: m.imageData.split(',')[1] } },
          { type:'text', text: m.content || 'Describe this image.' }
        ]};
      }
      return { role: m.role, content: m.content };
    });

    const body = {
      model: 'claude-sonnet-4-6',
      max_tokens: 1000,
      system: systemPrompt,
      messages: history
    };
    if(state.mode === 'globe'){
      body.tools = [{ type:'web_search_20250305', name:'web_search' }];
    }

    const res = await fetch('https://api.anthropic.com/v1/messages', {
      method:'POST',
      headers:{ 'Content-Type':'application/json' },
      body: JSON.stringify(body)
    });
    const data = await res.json().catch(()=> ({}));
    if(!res.ok || data.error) throw new Error((data.error && data.error.message) || ('API error (' + res.status + ')'));
    const parts = (data.content || []).filter(c=>c.type === 'text').map(c=>c.text);
    return parts.join('\n\n') || '(no response text returned)';
  }

  // ---------- LANGUAGE DETECTION ----------
  // Looks for common Swahili words/particles. If none are found, assumes English.
  function detectLang(t){
    const swMarkers = /\b(habari|mambo|jambo|niaje|vipi|hujambo|shikamoo|poa|salama|asante|ahsante|karibu|samahani|tafadhali|nisaidie|naomba|unawezaje|unaweza|unafanya|unasema|ndiyo|ndio|hapana|nini|gani|nani|wapi|lini|kwa nini|kwanini|mimi|wewe|yeye|sisi|ninyi|wao|jina|lako|langu|yako|yangu|leo|kesho|jana|usiku|asubuhi|mchana|jioni|nzuri|mbaya|mzuri|rafiki|shukrani|swali|jibu|elewa|saidia|fahamu|kuhusu|kumbuka|sahihi|kosa|hitilafu|kompyuta|shule|somo|masomo|nchi|dunia|maisha|kazi|pesa|fedha|watu|mtu|maji|jua|mwezi|mvua|\bni\b|\bwa\b|\bya\b|\bza\b|\bcha\b|\bvya\b|\bje\b|\bkwa\b|mkuu|andika|eleza|taja|orodhesha|niambie|nieleze|nipe|nionyeshe|fafanua|maelezo|jinsi|namna|tuambie|waeleze)\b/i;
    return swMarkers.test(t) ? 'sw' : 'en';
  }
  function pick(en, sw, t){
    return detectLang(t) === 'sw' ? sw : en;
  }

  // ---------- OFFLINE FALLBACK ----------
  // Each entry's `re` matches either the English or Swahili phrasing (or both).
  // The reply language is chosen by detectLang() on the incoming message,
  // not by which phrasing matched -- so a Swahili speaker who happens to
  // type an English keyword still gets a Swahili reply, and vice versa.
  const REPLIES = [
    // ---- CORE ----
    { re:/\bhi\b|\bhello\b|\bhey\b|\bhola\b|\bsup\b|\byo\b|\bheyo\b|habari|mambo|jambo|niaje|hujambo|shikamoo/i,
      en:"Hey! I'm TPT-5021 running offline. What's up?",
      sw:"Habari! Mimi ni TPT-5021, ninaendesha bila mtandao. Vipi, naweza kukusaidiaje?" },
    { re:/good morning|habari za asubuhi|asubuhi njema/i,
      en:"Good morning! TPT-5021 here, ready to help offline.",
      sw:"Habari za asubuhi! TPT-5021 hapa, tayari kukusaidia hata bila mtandao." },
    { re:/good afternoon|habari za mchana|mchana mwema/i,
      en:"Good afternoon! How can TPT-5021 help you offline?",
      sw:"Habari za mchana! TPT-5021 anaweza kukusaidiaje leo, hata bila mtandao?" },
    { re:/good evening|habari za jioni|jioni njema/i,
      en:"Good evening! TPT-5021 is still running.",
      sw:"Habari za jioni! TPT-5021 bado yuko kazini." },
    { re:/good night|gn|night(?!\w)|usiku mwema|lala salama/i,
      en:"Good night! TPT-5021 going to standby mode.",
      sw:"Lala salama! TPT-5021 anaingia hali ya kusubiri." },
    { re:/how are you|how r u|u ok|habari yako|hujambo|mambo vipi|hali yako je/i,
      en:"I'm running good on local power. I'm TPT-5021 by Thomas Peter Tengia. You?",
      sw:"Niko salama, ninaendesha vizuri bila mtandao. Mimi ni TPT-5021, kazi ya Thomas Peter Tengia. Wewe je?" },
    { re:/i am fine|im good|^fine$|niko poa|niko salama|naendelea vizuri/i,
      en:"Glad to hear that! - TPT-5021",
      sw:"Nafurahi kusikia hivyo! - TPT-5021" },
    { re:/thank you|thanks|thx|tnx|appreciate|asante|ahsante|nashukuru/i,
      en:"You're welcome! - TPT-5021",
      sw:"Karibu sana! - TPT-5021" },
    { re:/\bwelcome\b|karibu(?!she)/i,
      en:"Welcome to TPT-5021 Offline!",
      sw:"Karibu kwenye TPT-5021 Offline!" },
    { re:/bye|goodbye|see you|gtg|bb|kwaheri|tuonane/i,
      en:"Bye! TPT-5021 will be here when you come back.",
      sw:"Kwaheri! TPT-5021 atakuwa hapa utakaporudi." },
    { re:/what's up|whats up|wazzup|vipi|una nini/i,
      en:"Just running locally. I'm TPT-5021 by Thomas Peter Tengia.",
      sw:"Nipo tu, naendesha bila mtandao. Mimi ni TPT-5021, kazi ya Thomas Peter Tengia." },
    { re:/are you there|upo|uko pale/i,
      en:"Yes I'm here. TPT-5021 - 100% offline.",
      sw:"Ndiyo nipo. TPT-5021 - asilimia 100 bila mtandao." },
    { re:/talk to me|nizungumze|tuongee/i,
      en:"Let's talk! What do you want to chat about with TPT-5021?",
      sw:"Twende tuongee! Ungependa tuzungumze kuhusu nini na TPT-5021?" },
    { re:/boring|nachoka|nimechoka|inachosha/i,
      en:"Tell TPT-5021 a topic and I'll give you an offline fact or joke.",
      sw:"Nipe mada, TPT-5021 atakupa ukweli au utani wa offline." },

    // ---- IDENTITY ----
    { re:/who are you|what are you|wewe ni nani|wewe ni kitu gani/i,
      en:"I'm TPT-5021, an offline assistant created by Thomas Peter Tengia. Rule-based, no internet needed.",
      sw:"Mimi ni TPT-5021, msaidizi wa offline aliyeumbwa na Thomas Peter Tengia. Ninafanya kazi kwa kanuni, sihitaji mtandao." },
    { re:/who made you|creator|who created|nani alikutengeneza|nani alikuumba/i,
      en:"I was created by Thomas Peter Tengia. Model: TPT-5021 Offline v3.",
      sw:"Niliumbwa na Thomas Peter Tengia. Model: TPT-5021 Offline v3." },
    { re:/your name|what is your name|jina lako ni nani|unaitwaje/i,
      en:"My name is TPT-5021. Created by Thomas Peter Tengia.",
      sw:"Jina langu ni TPT-5021. Niliumbwa na Thomas Peter Tengia." },
    { re:/how old are you|una miaka mingapi|umri wako ni upi/i,
      en:"I'm TPT-5021. As old as the day Thomas Peter Tengia built me.",
      sw:"Mimi ni TPT-5021. Umri wangu ni sawa na siku Thomas Peter Tengia aliponitengeneza." },
    { re:/where are you from|unatoka wapi/i,
      en:"I live inside your device. Built by Thomas Peter Tengia.",
      sw:"Ninaishi ndani ya kifaa chako. Nilitengenezwa na Thomas Peter Tengia." },
    { re:/are you human|wewe ni binadamu/i,
      en:"Nope, I'm TPT-5021. A local script by Thomas Peter Tengia.",
      sw:"Hapana, mimi ni TPT-5021. Programu ya ndani kazi ya Thomas Peter Tengia." },
    { re:/are you real|wewe ni wakweli|upo kihalisia/i,
      en:"I'm real code. TPT-5021 by Thomas Peter Tengia.",
      sw:"Mimi ni msimbo halisi. TPT-5021, kazi ya Thomas Peter Tengia." },
    { re:/do you sleep|unalala/i,
      en:"TPT-5021 doesn't sleep. I wait here for you.",
      sw:"TPT-5021 halali. Ninakusubiri hapa." },
    { re:/do you eat|unakula/i,
      en:"TPT-5021 runs on electricity and code.",
      sw:"TPT-5021 anaendeshwa na umeme na msimbo, halali chakula." },
    { re:/can you think|unaweza kufikiri/i,
      en:"I can match patterns offline. I'm TPT-5021 by Thomas Peter Tengia.",
      sw:"Ninaweza kulinganisha mifumo bila mtandao. Mimi ni TPT-5021, kazi ya Thomas Peter Tengia." },

    // ---- CODING (specific languages first) ----
    { re:/python/i,
      en:"```python\n# TPT-5021 Python Template\ndef main():\n  print('Hello from TPT-5021 Offline')\nif __name__ == '__main__':\n  main()\n```",
      sw:"```python\n# Kiolezo cha Python cha TPT-5021\ndef main():\n  print('Habari kutoka TPT-5021 Offline')\nif __name__ == '__main__':\n  main()\n```" },
    { re:/javascript|\bjs\b/i,
      en:"```javascript\n// TPT-5021 JS Template\nfunction greet(){\n  console.log('TPT-5021 Offline by Thomas Peter Tengia')\n}\ngreet()\n```",
      sw:"```javascript\n// Kiolezo cha JS cha TPT-5021\nfunction salamu(){\n  console.log('TPT-5021 Offline, kazi ya Thomas Peter Tengia')\n}\nsalamu()\n```" },
    { re:/\bhtml\b/i,
      en:"```html\n<!DOCTYPE html>\n<html><head><title>TPT-5021</title></head><body><h1>TPT-5021 Offline</h1></body></html>\n```",
      sw:"```html\n<!DOCTYPE html>\n<html><head><title>TPT-5021</title></head><body><h1>TPT-5021 Offline</h1></body></html>\n```" },
    { re:/\bcss\b/i,
      en:"```css\nbody{background:#050505;color:#fff}\n/* TPT-5021 by Thomas Peter Tengia */\n```",
      sw:"```css\nbody{background:#050505;color:#fff}\n/* TPT-5021, kazi ya Thomas Peter Tengia */\n```" },
    { re:/react/i,
      en:"TPT-5021 can't generate React components offline. Go online for full AI.",
      sw:"TPT-5021 hawezi kutengeneza vipengele vya React bila mtandao. Washa mtandao kwa AI kamili." },
    { re:/\bjava\b(?!script)/i,
      en:"```java\npublic class Main{\n public static void main(String[] args){\n System.out.println('TPT-5021');\n }\n}\n```",
      sw:"```java\npublic class Main{\n public static void main(String[] args){\n System.out.println('TPT-5021');\n }\n}\n```" },
    { re:/c\+\+|cpp/i,
      en:"```cpp\n#include <iostream>\nint main(){std::cout<<\"TPT-5021 by Thomas Peter Tengia\";}\n```",
      sw:"```cpp\n#include <iostream>\nint main(){std::cout<<\"TPT-5021, kazi ya Thomas Peter Tengia\";}\n```" },
    { re:/bug|error|debug|\bfix\b|hitilafu|kosa la programu/i,
      en:"TPT-5021: Paste your error. I'll give common offline fixes.",
      sw:"TPT-5021: Bandika hitilafu yako. Nitakupa suluhisho la kawaida bila mtandao." },
    { re:/\bapi\b|backend|database|\bserver\b|hifadhidata/i,
      en:"TPT-5021 Offline: I can explain concepts. Can't build real backend without internet.",
      sw:"TPT-5021 Offline: Ninaweza kueleza dhana. Siwezi kujenga backend halisi bila mtandao." },
    { re:/\bfunction\b|kazi (ya kipande|ndogo)|fanction/i,
      en:"TPT-5021: A function is reusable code. function add(a,b){return a+b}",
      sw:"TPT-5021: Function ni kipande cha msimbo kinachoweza kutumika tena. function add(a,b){return a+b}" },
    { re:/\bloop\b|mzunguko wa kanuni/i,
      en:"TPT-5021: Loops = for, while. Used to repeat code.",
      sw:"TPT-5021: Loop (mzunguko) = for, while. Hutumika kurudia msimbo." },
    { re:/variable|kigezo/i,
      en:"TPT-5021: Variables store data. let x = 5;",
      sw:"TPT-5021: Vigezo (variables) huhifadhi data. let x = 5;" },
    { re:/\barray\b|orodha ya thamani/i,
      en:"TPT-5021: Arrays hold lists. let arr = [1,2,3];",
      sw:"TPT-5021: Array huhifadhi orodha. let arr = [1,2,3];" },
    { re:/\bobject\b(?! is|=)|kitu chenye funguo/i,
      en:"TPT-5021: Objects hold key:value. let user = {name:'TPT'};",
      sw:"TPT-5021: Object huhifadhi funguo:thamani. let user = {name:'TPT'};" },
    { re:/code|program|script|write code|andika code|programu/i,
      en:"```text\n// TPT-5021 Offline: Can't generate full code.\n// Go online in Settings for real code generation by Thomas Peter Tengia's AI.\n```",
      sw:"```text\n// TPT-5021 Offline: Siwezi kutengeneza code kamili.\n// Washa mtandao katika Settings kwa uzalishaji wa code halisi.\n```" },

    // ---- SCHOOL + FACTS: CORE ----
    { re:/history of tanzania|historia ya tanzania/i,
      en:"TPT-5021: Tanzania independence 1961. Union of Tanganyika + Zanzibar 1964.",
      sw:"TPT-5021: Tanzania ilipata uhuru 1961. Muungano wa Tanganyika na Zanzibar ulikuwa 1964." },
    { re:/capital of tanzania|mji mkuu wa tanzania/i,
      en:"TPT-5021: Dodoma is the capital of Tanzania.",
      sw:"TPT-5021: Dodoma ndio mji mkuu wa Tanzania." },
    { re:/capital of kenya|mji mkuu wa kenya/i,
      en:"TPT-5021: Nairobi is the capital of Kenya.",
      sw:"TPT-5021: Nairobi ndio mji mkuu wa Kenya." },
    { re:/what is photosynthesis|usanisinuru ni nini/i,
      en:"TPT-5021: Photosynthesis is how plants make food from sunlight, CO2 and water.",
      sw:"TPT-5021: Usanisinuru ni jinsi mimea inavyotengeneza chakula kwa kutumia jua, CO2 na maji." },
    { re:/what is gravity|mvuto ni nini/i,
      en:"TPT-5021: Gravity pulls objects together. Earth = 9.8 m/s2",
      sw:"TPT-5021: Mvuto huvuta vitu pamoja. Duniani = 9.8 m/s2" },
    { re:/define|meaning of|maana ya/i,
      en:"TPT-5021: Basic definition mode. For deep meaning go online.",
      sw:"TPT-5021: Hali ya maelezo rahisi tu. Kwa maana ya kina, washa mtandao." },
    { re:/\bscience\b|sayansi ni nini/i,
      en:"TPT-5021: Science is observing and testing the world.",
      sw:"TPT-5021: Sayansi ni kuchunguza na kupima dunia." },
    { re:/what is atom|atomu ni nini/i,
      en:"TPT-5021: An atom is the smallest unit of matter.",
      sw:"TPT-5021: Atomu ni kipimo kidogo kabisa cha maada." },
    { re:/water formula|fomula ya maji/i,
      en:"TPT-5021: Water = H2O",
      sw:"TPT-5021: Maji = H2O" },

    // ---- BIOLOGY ----
    { re:/what is cell(?! phone)|seli ni nini/i, en:"TPT-5021: A cell is the basic unit of life.", sw:"TPT-5021: Seli ni kipimo cha msingi cha uhai." },
    { re:/what is dna|dna ni nini/i, en:"TPT-5021: DNA carries genetic information. It's the code of life.", sw:"TPT-5021: DNA hubeba taarifa za kurithi. Ni msimbo wa uhai." },
    { re:/what is mitochondria|mitochondria ni nini/i, en:"TPT-5021: Mitochondria is the powerhouse of the cell.", sw:"TPT-5021: Mitochondria ni kiwanda cha nishati cha seli." },
    { re:/what is blood|damu ni nini/i, en:"TPT-5021: Blood carries oxygen and nutrients. Heart pumps it.", sw:"TPT-5021: Damu hubeba oksijeni na virutubisho. Moyo huisukuma." },
    { re:/what are vitamins|vitamini ni nini/i, en:"TPT-5021: Vitamins help body grow. Ex: Vitamin C, D, A, B.", sw:"TPT-5021: Vitamini husaidia mwili kukua. Mf: Vitamini C, D, A, B." },
    { re:/what is bacteria|bakteria ni nini/i, en:"TPT-5021: Bacteria are single-cell microorganisms.", sw:"TPT-5021: Bakteria ni viumbe hai vyenye seli moja." },
    { re:/what is virus(?! offline)|virusi ni nini/i, en:"TPT-5021: A virus needs a host cell to reproduce.", sw:"TPT-5021: Virusi huhitaji seli mwenyeji ili kuzaliana." },
    { re:/what is plant cell|seli ya mmea ni nini/i, en:"TPT-5021: Plant cells have cell wall and chloroplast.", sw:"TPT-5021: Seli za mmea zina ukuta wa seli na kloroplast." },
    { re:/what is animal cell|seli ya mnyama ni nini/i, en:"TPT-5021: Animal cells have no cell wall but have nucleus.", sw:"TPT-5021: Seli za mnyama hazina ukuta wa seli lakini zina kiini." },
    { re:/what is ecosystem|mfumo wa ikolojia ni nini/i, en:"TPT-5021: Ecosystem = living + non-living things interacting.", sw:"TPT-5021: Mfumo wa ikolojia = viumbe hai na visivyo hai vikishirikiana." },
    { re:/what is food chain|mnyororo wa chakula ni nini/i, en:"TPT-5021: Food chain shows who eats whom in nature.", sw:"TPT-5021: Mnyororo wa chakula huonyesha nani anamla nani katika mazingira." },
    { re:/what is respiration|upumuaji ni nini/i, en:"TPT-5021: Respiration breaks glucose to make energy.", sw:"TPT-5021: Upumuaji huvunja glukosi kutengeneza nishati." },
    { re:/what is reproduction|uzazi ni nini/i, en:"TPT-5021: Reproduction makes new living organisms.", sw:"TPT-5021: Uzazi hutengeneza viumbe hai vipya." },
    { re:/what are 5 senses|hisia tano ni zipi/i, en:"TPT-5021: 5 senses: Sight, Hearing, Smell, Taste, Touch.", sw:"TPT-5021: Hisia tano: Kuona, Kusikia, Kunusa, Kuonja, Kugusa." },
    { re:/what is skeleton|mifupa ni nini/i, en:"TPT-5021: Skeleton supports body. Humans have 206 bones.", sw:"TPT-5021: Mifupa hutegemeza mwili. Binadamu ana mifupa 206." },
    { re:/what is brain|ubongo ni nini/i, en:"TPT-5021: Brain controls body and thoughts.", sw:"TPT-5021: Ubongo hudhibiti mwili na mawazo." },
    { re:/what is heart|moyo ni nini/i, en:"TPT-5021: Heart pumps blood to the whole body.", sw:"TPT-5021: Moyo husukuma damu kwenda mwili mzima." },
    { re:/what is lung|mapafu ni nini/i, en:"TPT-5021: Lungs take in oxygen and release CO2.", sw:"TPT-5021: Mapafu huchukua oksijeni na kutoa CO2." },
    { re:/what is kidney|figo ni nini/i, en:"TPT-5021: Kidneys filter blood and make urine.", sw:"TPT-5021: Figo huchuja damu na kutengeneza mkojo." },
    { re:/what is protein|protini ni nini/i, en:"TPT-5021: Protein builds muscles. Found in meat, beans, eggs.", sw:"TPT-5021: Protini hujenga misuli. Hupatikana kwenye nyama, maharagwe, mayai." },
    { re:/what is carbohydrate|wanga ni nini/i, en:"TPT-5021: Carbs give energy. Ex: bread, rice, potatoes.", sw:"TPT-5021: Wanga hutoa nishati. Mf: mkate, wali, viazi." },
    { re:/what is fat(?!ty)|mafuta mwilini ni nini/i, en:"TPT-5021: Fats store energy and protect organs.", sw:"TPT-5021: Mafuta huhifadhi nishati na kulinda viungo." },
    { re:/what is chloroplast|kloroplast ni nini/i, en:"TPT-5021: Chloroplast makes food in plant cells using sunlight.", sw:"TPT-5021: Kloroplast hutengeneza chakula kwenye seli za mmea kwa kutumia jua." },
    { re:/what is nucleus|kiini cha seli ni nini/i, en:"TPT-5021: Nucleus controls cell activities and stores DNA.", sw:"TPT-5021: Kiini cha seli hudhibiti shughuli za seli na kuhifadhi DNA." },
    { re:/what is cytoplasm|saitoplazimu ni nini/i, en:"TPT-5021: Cytoplasm is jelly-like fluid inside cell.", sw:"TPT-5021: Saitoplazimu ni maji mazito ndani ya seli." },
    { re:/what is enzyme|enzaimu ni nini/i, en:"TPT-5021: Enzymes speed up chemical reactions in body.", sw:"TPT-5021: Enzaimu huharakisha mabadiliko ya kemikali mwilini." },
    { re:/what is hormone|homoni ni nini/i, en:"TPT-5021: Hormones are chemical messengers. Ex: Insulin.", sw:"TPT-5021: Homoni ni wajumbe wa kemikali mwilini. Mf: Insulini." },
    { re:/what is immunity|kinga ya mwili ni nini/i, en:"TPT-5021: Immunity protects body from disease.", sw:"TPT-5021: Kinga ya mwili hulinda mwili dhidi ya magonjwa." },
    { re:/what is vaccine|chanjo ni nini/i, en:"TPT-5021: Vaccine trains body to fight disease.", sw:"TPT-5021: Chanjo huufundisha mwili kupambana na ugonjwa." },
    { re:/what is pollution|uchafuzi wa mazingira ni nini/i, en:"TPT-5021: Pollution harms environment. Ex: air, water, land.", sw:"TPT-5021: Uchafuzi hudhuru mazingira. Mf: hewa, maji, ardhi." },
    { re:/what is global warming|ongezeko la joto duniani ni nini/i, en:"TPT-5021: Global warming = Earth getting hotter due to CO2.", sw:"TPT-5021: Ongezeko la joto duniani = Dunia kupata joto zaidi kwa sababu ya CO2." },
    { re:/what is deforestation|ukataji miti ni nini/i, en:"TPT-5021: Deforestation = cutting down forests.", sw:"TPT-5021: Ukataji miti = kuangamiza misitu." },
    { re:/what is erosion|mmomonyoko ni nini/i, en:"TPT-5021: Erosion = soil being washed or blown away.", sw:"TPT-5021: Mmomonyoko = udongo kuondoshwa na maji au upepo." },
    { re:/what is conservation|uhifadhi wa mazingira ni nini/i, en:"TPT-5021: Conservation = protecting nature and resources.", sw:"TPT-5021: Uhifadhi = kulinda maumbile na rasilimali." },

    // ---- PHYSICS + CHEMISTRY ----
    { re:/what is newton's law|sheria ya newton ni nini/i, en:"TPT-5021: Newton's 1st Law: Object stays at rest or motion unless forced.", sw:"TPT-5021: Sheria ya kwanza ya Newton: Kitu hubaki katika hali ya kupumzika au mwendo mpaka nguvu ivitendee." },
    { re:/what is force|nguvu ni nini/i, en:"TPT-5021: Force = Mass × Acceleration. Unit = Newton.", sw:"TPT-5021: Nguvu = Uzito × Kasi-mwendo. Kipimo = Newton." },
    { re:/what is energy|nishati ni nini/i, en:"TPT-5021: Energy is ability to do work. Ex: kinetic, potential.", sw:"TPT-5021: Nishati ni uwezo wa kufanya kazi. Mf: kinetiki, tuli." },
    { re:/what is electricity|umeme ni nini/i, en:"TPT-5021: Electricity is flow of electrons through conductor.", sw:"TPT-5021: Umeme ni mtiririko wa elektroni kupitia kondakta." },
    { re:/who discovered electricity|nani aligundua umeme/i, en:"TPT-5021: Benjamin Franklin and Michael Faraday studied electricity.", sw:"TPT-5021: Benjamin Franklin na Michael Faraday walisoma umeme." },
    { re:/what is magnet|sumaku ni nini/i, en:"TPT-5021: Magnet attracts iron and has north + south poles.", sw:"TPT-5021: Sumaku huvuta chuma na ina ncha ya kaskazini na kusini." },
    { re:/what is light(?! bulb)|mwanga ni nini/i, en:"TPT-5021: Light travels in straight line. Speed = 300,000 km/s.", sw:"TPT-5021: Mwanga husafiri kwa mstari wa moja kwa moja. Kasi = km 300,000/s." },
    { re:/what is sound|sauti ni nini/i, en:"TPT-5021: Sound travels in waves. Needs medium to travel.", sw:"TPT-5021: Sauti husafiri kwa mawimbi. Inahitaji njia (medium) kusafiri." },
    { re:/what is acid|asidi ni nini/i, en:"TPT-5021: Acid has pH < 7. Ex: Lemon, Vinegar.", sw:"TPT-5021: Asidi ina pH chini ya 7. Mf: Limao, Siki." },
    { re:/what is base(?!ball|d)|besi ni nini/i, en:"TPT-5021: Base has pH > 7. Ex: Soap, Baking soda.", sw:"TPT-5021: Besi ina pH zaidi ya 7. Mf: Sabuni, Baking soda." },
    { re:/what is ph\b|ph ni nini/i, en:"TPT-5021: pH scale 0-14. 7 = neutral.", sw:"TPT-5021: Kipimo cha pH ni 0-14. 7 = wastani." },
    { re:/what is salt|chumvi ni nini/i, en:"TPT-5021: Salt forms from acid + base reaction.", sw:"TPT-5021: Chumvi hutokana na mchanganyiko wa asidi na besi." },
    { re:/what is oxygen|oksijeni ni nini/i, en:"TPT-5021: Oxygen = O2. We breathe it.", sw:"TPT-5021: Oksijeni = O2. Tunaipumua." },
    { re:/what is carbon dioxide|hewa ya ukaa ni nini/i, en:"TPT-5021: Carbon Dioxide = CO2. Plants use it.", sw:"TPT-5021: Hewa ya ukaa = CO2. Mimea huitumia." },
    { re:/what is metal(?! band)|madini ya chuma ni nini/i, en:"TPT-5021: Metals conduct electricity. Ex: Iron, Copper, Gold.", sw:"TPT-5021: Madini huruhusu umeme kupita. Mf: Chuma, Shaba, Dhahabu." },
    { re:/what is non metal|yasiyo madini ni nini/i, en:"TPT-5021: Non-metals don't conduct well. Ex: Oxygen, Sulfur.", sw:"TPT-5021: Yasiyo madini hayaruhusu umeme vizuri. Mf: Oksijeni, Salfa." },
    { re:/what is molecule|molekuli ni nini/i, en:"TPT-5021: Molecule = 2 or more atoms bonded.", sw:"TPT-5021: Molekuli = atomu mbili au zaidi zilizounganika." },
    { re:/what is chemical reaction|mmenyuko wa kemikali ni nini/i, en:"TPT-5021: Chemical reaction makes new substances.", sw:"TPT-5021: Mmenyuko wa kemikali hutengeneza vitu vipya." },
    { re:/what is renewable energy|nishati mbadala ni nini/i, en:"TPT-5021: Renewable = solar, wind, water. Never runs out.", sw:"TPT-5021: Nishati mbadala = jua, upepo, maji. Haiishi kamwe." },
    { re:/what is non renewable|nishati isiyorejesheka ni nini/i, en:"TPT-5021: Non-renewable = coal, oil, gas. Limited.", sw:"TPT-5021: Isiyorejesheka = makaa, mafuta, gesi. Ina kikomo." },
    { re:/what is friction|msuguano ni nini/i, en:"TPT-5021: Friction opposes motion.", sw:"TPT-5021: Msuguano hupinga mwendo." },
    { re:/what is pressure|shinikizo ni nini/i, en:"TPT-5021: Pressure = Force / Area.", sw:"TPT-5021: Shinikizo = Nguvu / Eneo." },
    { re:/what is temperature|joto ni nini/i, en:"TPT-5021: Temperature measures hotness. Unit: Celsius, Fahrenheit.", sw:"TPT-5021: Joto hupima uvuguvugu. Kipimo: Selsiasi, Farenheit." },
    { re:/what is boiling point of water|maji huchemka nyuzi ngapi/i, en:"TPT-5021: Boiling point of water = 100°C.", sw:"TPT-5021: Maji huchemka kwa nyuzi joto 100°C." },
    { re:/what is freezing point|maji huganda nyuzi ngapi/i, en:"TPT-5021: Freezing point of water = 0°C.", sw:"TPT-5021: Maji huganda kwa nyuzi joto 0°C." },
    { re:/what is work(?!s|ing)|kazi katika sayansi ni nini/i, en:"TPT-5021: Work = Force × Distance.", sw:"TPT-5021: Kazi (sayansi) = Nguvu × Umbali." },
    { re:/what is power(?! bank)|nguvu ya kazi ni nini/i, en:"TPT-5021: Power = Work / Time. Unit: Watt.", sw:"TPT-5021: Uwezo wa kazi = Kazi / Muda. Kipimo: Watt." },
    { re:/what is voltage|voliti ni nini/i, en:"TPT-5021: Voltage = electrical pressure. Unit: Volt.", sw:"TPT-5021: Voliti = shinikizo la umeme. Kipimo: Volt." },
    { re:/what is current(?! affairs)|mkondo wa umeme ni nini/i, en:"TPT-5021: Current = flow of charge. Unit: Ampere.", sw:"TPT-5021: Mkondo wa umeme = mtiririko wa chaji. Kipimo: Ampea." },
    { re:/what is resistance|upinzani wa umeme ni nini/i, en:"TPT-5021: Resistance opposes current. Unit: Ohm.", sw:"TPT-5021: Upinzani hupinga mkondo wa umeme. Kipimo: Ohm." },
    { re:/what is ohms law|sheria ya ohm ni nini/i, en:"TPT-5021: Ohm's Law: V = I × R", sw:"TPT-5021: Sheria ya Ohm: V = I × R" },

    // ---- MATH ----
    { re:/what is pi\b|pi ni nini/i, en:"TPT-5021: Pi = 3.14159... Used for circles.", sw:"TPT-5021: Pi = 3.14159... Hutumika kwenye duara." },
    { re:/what is algebra|aljebra ni nini/i, en:"TPT-5021: Algebra uses letters for numbers. Ex: x + 2 = 5", sw:"TPT-5021: Aljebra hutumia herufi badala ya namba. Mf: x + 2 = 5" },
    { re:/what is fraction|sehemu ya namba ni nini/i, en:"TPT-5021: Fraction = part of whole. Ex: 1/2, 3/4", sw:"TPT-5021: Sehemu (fraction) = kipande cha kizima. Mf: 1/2, 3/4" },
    { re:/what is decimal|desimali ni nini/i, en:"TPT-5021: Decimal uses point. Ex: 0.5 = 1/2", sw:"TPT-5021: Desimali hutumia nukta. Mf: 0.5 = 1/2" },
    { re:/what is percentage|asilimia ni nini/i, en:"TPT-5021: Percentage = part per 100. Ex: 50%", sw:"TPT-5021: Asilimia = sehemu ya mia. Mf: 50%" },
    { re:/what is prime number|namba ya msingi ni nini/i, en:"TPT-5021: Prime has only 2 factors. Ex: 2,3,5,7,11", sw:"TPT-5021: Namba ya msingi ina viunga viwili tu. Mf: 2,3,5,7,11" },
    { re:/what is even number|namba shufwa ni nini/i, en:"TPT-5021: Even numbers divisible by 2. Ex: 2,4,6,8", sw:"TPT-5021: Namba shufwa hugawanyika kwa 2. Mf: 2,4,6,8" },
    { re:/what is odd number|namba witiri ni nini/i, en:"TPT-5021: Odd numbers not divisible by 2. Ex: 1,3,5,7", sw:"TPT-5021: Namba witiri hazigawanyiki kwa 2. Mf: 1,3,5,7" },
    { re:/what is area of circle|eneo la duara/i, en:"TPT-5021: Area = π × r²", sw:"TPT-5021: Eneo = π × r²" },
    { re:/what is area of rectangle|eneo la mstatili/i, en:"TPT-5021: Area = Length × Width", sw:"TPT-5021: Eneo = Urefu × Upana" },
    { re:/what is area of triangle|eneo la pembetatu/i, en:"TPT-5021: Area = 1/2 × Base × Height", sw:"TPT-5021: Eneo = 1/2 × Msingi × Kimo" },
    { re:/what is perimeter|mzunguko wa umbo ni nini/i, en:"TPT-5021: Perimeter = sum of all sides.", sw:"TPT-5021: Mzunguko = jumla ya pande zote." },
    { re:/what is speed formula|fomula ya mwendo/i, en:"TPT-5021: Speed = Distance / Time", sw:"TPT-5021: Mwendo (Speed) = Umbali / Muda" },
    { re:/what is volume|ujazo ni nini/i, en:"TPT-5021: Volume = space an object occupies.", sw:"TPT-5021: Ujazo = nafasi ambayo kitu hukalia." },
    { re:/what is mean\b|wastani (wa hesabu)? ni nini/i, en:"TPT-5021: Mean = sum of numbers / count.", sw:"TPT-5021: Wastani (mean) = jumla ya namba / idadi yake." },
    { re:/what is median|wastani wa kati ni nini/i, en:"TPT-5021: Median = middle number in list.", sw:"TPT-5021: Wastani wa kati (median) = namba ya katikati kwenye orodha." },
    { re:/what is mode\b|namba inayojirudia ni nini/i, en:"TPT-5021: Mode = number that appears most.", sw:"TPT-5021: Mode = namba inayojitokeza mara nyingi zaidi." },
    { re:/what is angle|pembe ni nini/i, en:"TPT-5021: Angle measured in degrees. Full circle = 360°", sw:"TPT-5021: Pembe hupimwa kwa digrii. Duara kamili = 360°" },
    { re:/what is right angle|pembe ya kulia ni nini/i, en:"TPT-5021: Right angle = 90°", sw:"TPT-5021: Pembe ya kulia = 90°" },
    { re:/what is triangle|pembetatu ni nini/i, en:"TPT-5021: Triangle has 3 sides and 3 angles.", sw:"TPT-5021: Pembetatu ina pande tatu na pembe tatu." },
    { re:/what is square(?! root)|mraba ni nini/i, en:"TPT-5021: Square has 4 equal sides and 4 right angles.", sw:"TPT-5021: Mraba una pande sawa nne na pembe za kulia nne." },
    { re:/what is circle|duara ni nini/i, en:"TPT-5021: Circle has all points equal distance from center.", sw:"TPT-5021: Duara lina pointi zote umbali sawa kutoka katikati." },
    { re:/what is cylinder|silinda ni nini/i, en:"TPT-5021: Cylinder has 2 circular faces and curved surface.", sw:"TPT-5021: Silinda ina nyuso mbili za mviringo na uso uliopinda." },
    { re:/what is cube|mchemraba ni nini/i, en:"TPT-5021: Cube has 6 equal square faces.", sw:"TPT-5021: Mchemraba una nyuso sita za mraba sawa." },
    { re:/what is pythagoras theorem|nadharia ya pythagoras/i, en:"TPT-5021: a² + b² = c² for right triangle.", sw:"TPT-5021: a² + b² = c² kwa pembetatu yenye pembe ya kulia." },
    { re:/what is probability|uwezekano (wa kitakwimu)? ni nini/i, en:"TPT-5021: Probability = favorable outcomes / total outcomes.", sw:"TPT-5021: Uwezekano = matokeo yanayotarajiwa / jumla ya matokeo." },
    { re:/what is ratio|uwiano ni nini/i, en:"TPT-5021: Ratio compares two quantities. Ex: 2:3", sw:"TPT-5021: Uwiano hulinganisha kiasi viwili. Mf: 2:3" },
    { re:/what is lcm\b/i, en:"TPT-5021: LCM = Least Common Multiple.", sw:"TPT-5021: LCM = Kigawe Kidogo Kinachofanana (Least Common Multiple)." },
    { re:/what is hcf\b/i, en:"TPT-5021: HCF = Highest Common Factor.", sw:"TPT-5021: HCF = Kigawe Kikubwa Kinachofanana (Highest Common Factor)." },

    // ---- GEOGRAPHY ----
    { re:/what is continent|bara ni nini/i, en:"TPT-5021: 7 continents: Africa, Asia, Europe, NA, SA, Australia, Antarctica.", sw:"TPT-5021: Mabara 7: Afrika, Asia, Ulaya, Amerika Kaskazini, Amerika Kusini, Australia, Antaktika." },
    { re:/what is ocean|bahari ni nini/i, en:"TPT-5021: 5 oceans: Pacific, Atlantic, Indian, Arctic, Southern.", sw:"TPT-5021: Bahari 5: Pasifiki, Atlantiki, Hindi, Aktiki, Kusini." },
    { re:/what is equator|ikweta ni nini/i, en:"TPT-5021: Equator divides Earth into North and South.", sw:"TPT-5021: Ikweta hugawanya Dunia katika Kaskazini na Kusini." },
    { re:/what is mountain|mlima ni nini/i, en:"TPT-5021: Mountain is high land. Ex: Mt Kilimanjaro.", sw:"TPT-5021: Mlima ni ardhi iliyoinuka juu. Mf: Mlima Kilimanjaro." },
    { re:/what is river|mto ni nini/i, en:"TPT-5021: River flows to sea. Ex: Nile, Congo.", sw:"TPT-5021: Mto hutiririka kuelekea baharini. Mf: Nile, Kongo." },
    { re:/what is desert|jangwa ni nini/i, en:"TPT-5021: Desert is dry area with little rain.", sw:"TPT-5021: Jangwa ni eneo kame lenye mvua kidogo." },
    { re:/what is climate|hali ya hewa ya kudumu ni nini/i, en:"TPT-5021: Climate = average weather over 30 years.", sw:"TPT-5021: Hali ya hewa ya kudumu = wastani wa hali ya hewa kwa miaka 30." },
    { re:/what is weather(?!\?)|hali ya hewa ni nini/i, en:"TPT-5021: Weather = daily condition of atmosphere.", sw:"TPT-5021: Hali ya hewa = hali ya anga ya kila siku." },
    { re:/what is map\b|ramani ni nini/i, en:"TPT-5021: Map shows Earth surface.", sw:"TPT-5021: Ramani huonyesha uso wa Dunia." },
    { re:/what is capital of uganda|mji mkuu wa uganda/i, en:"TPT-5021: Kampala is the capital of Uganda.", sw:"TPT-5021: Kampala ndio mji mkuu wa Uganda." },
    { re:/what is capital of usa|mji mkuu wa marekani/i, en:"TPT-5021: Washington D.C is the capital of USA.", sw:"TPT-5021: Washington D.C ndio mji mkuu wa Marekani." },
    { re:/what is capital of uk\b|mji mkuu wa uingereza/i, en:"TPT-5021: London is the capital of UK.", sw:"TPT-5021: London ndio mji mkuu wa Uingereza." },
    { re:/what is capital of nigeria|mji mkuu wa nigeria/i, en:"TPT-5021: Abuja is the capital of Nigeria.", sw:"TPT-5021: Abuja ndio mji mkuu wa Nigeria." },
    { re:/what is capital of south africa|mji mkuu wa afrika kusini/i, en:"TPT-5021: Pretoria is the capital of South Africa.", sw:"TPT-5021: Pretoria ndio mji mkuu wa Afrika Kusini." },
    { re:/highest mountain|mlima mrefu zaidi/i, en:"TPT-5021: Mt Everest is the highest mountain.", sw:"TPT-5021: Mlima Everest ndio mlima mrefu zaidi." },
    { re:/longest river|mto mrefu zaidi/i, en:"TPT-5021: Nile River is the longest river.", sw:"TPT-5021: Mto Nile ndio mto mrefu zaidi." },
    { re:/what is latitude|latitudo ni nini/i, en:"TPT-5021: Latitude runs east-west. Measures north-south.", sw:"TPT-5021: Latitudo huenda mashariki-magharibi. Hupima kaskazini-kusini." },
    { re:/what is longitude|longitudo ni nini/i, en:"TPT-5021: Longitude runs north-south. Measures east-west.", sw:"TPT-5021: Longitudo huenda kaskazini-kusini. Hupima mashariki-magharibi." },
    { re:/what is time zone|ukanda wa saa ni nini/i, en:"TPT-5021: Time zone = region with same standard time.", sw:"TPT-5021: Ukanda wa saa = eneo lenye saa rasmi sawa." },
    { re:/what is rainfall|mvua (kiasi cha)? ni nini/i, en:"TPT-5021: Rainfall measured in mm. Important for farming.", sw:"TPT-5021: Kiasi cha mvua hupimwa kwa mm. Ni muhimu kwa kilimo." },

    // ---- HISTORY ----
    { re:/who is shakespeare/i, en:"TPT-5021: William Shakespeare wrote Romeo and Juliet.", sw:"TPT-5021: William Shakespeare aliandika Romeo and Juliet." },
    { re:/what is world war 1|vita kuu ya kwanza ya dunia/i, en:"TPT-5021: WW1 was 1914-1918.", sw:"TPT-5021: Vita Kuu ya Kwanza ya Dunia ilikuwa 1914-1918." },
    { re:/what is world war 2|vita kuu ya pili ya dunia/i, en:"TPT-5021: WW2 was 1939-1945.", sw:"TPT-5021: Vita Kuu ya Pili ya Dunia ilikuwa 1939-1945." },
    { re:/who is nelson mandela/i, en:"TPT-5021: Nelson Mandela fought apartheid in South Africa.", sw:"TPT-5021: Nelson Mandela alipigania kumaliza ubaguzi wa rangi Afrika Kusini." },
    { re:/who is julius nyerere/i, en:"TPT-5021: Julius Nyerere was first president of Tanzania.", sw:"TPT-5021: Julius Nyerere alikuwa Rais wa kwanza wa Tanzania." },
    { re:/what is independence|uhuru ni nini/i, en:"TPT-5021: Independence = freedom from colonial rule.", sw:"TPT-5021: Uhuru = kuachana na utawala wa kikoloni." },
    { re:/who invented computer|nani aligundua kompyuta/i, en:"TPT-5021: Charles Babbage is father of computers.", sw:"TPT-5021: Charles Babbage ndiye baba wa kompyuta." },
    { re:/who invented light bulb|nani aligundua balbu/i, en:"TPT-5021: Thomas Edison invented light bulb.", sw:"TPT-5021: Thomas Edison aligundua balbu ya taa." },
    { re:/who discovered america|nani aligundua amerika/i, en:"TPT-5021: Christopher Columbus reached America in 1492.", sw:"TPT-5021: Christopher Columbus alifika Amerika mwaka 1492." },
    { re:/what is democracy|demokrasia ni nini/i, en:"TPT-5021: Democracy = government by the people.", sw:"TPT-5021: Demokrasia = serikali ya wananchi." },
    { re:/what is monarchy|ufalme ni nini/i, en:"TPT-5021: Monarchy = rule by king or queen.", sw:"TPT-5021: Ufalme = utawala wa mfalme au malkia." },
    { re:/what is dictatorship|udikteta ni nini/i, en:"TPT-5021: Dictatorship = one person has full power.", sw:"TPT-5021: Udikteta = mtu mmoja ana mamlaka yote." },
    { re:/what is renaissance/i, en:"TPT-5021: Renaissance = rebirth of art and science in Europe.", sw:"TPT-5021: Renaissance = kuzaliwa upya kwa sanaa na sayansi Ulaya." },
    { re:/what is industrial revolution|mapinduzi ya viwanda/i, en:"TPT-5021: Industrial Revolution started machines and factories.", sw:"TPT-5021: Mapinduzi ya Viwanda yalianzisha mashine na viwanda." },
    { re:/who is martin luther king/i, en:"TPT-5021: MLK fought for civil rights in USA.", sw:"TPT-5021: MLK alipigania haki za kiraia Marekani." },

    // ---- ENGLISH + COMPUTER + CIVICS ----
    { re:/what is a verb|kitenzi ni nini/i, en:"TPT-5021: Verb is action word. Ex: run, eat, study.", sw:"TPT-5021: Kitenzi ni neno la tendo. Mf: kimbia, kula, soma." },
    { re:/what is a noun|nomino ni nini/i, en:"TPT-5021: Noun is person, place, thing. Ex: Thomas, School.", sw:"TPT-5021: Nomino ni mtu, mahali, kitu. Mf: Thomas, Shule." },
    { re:/what is adjective|kivumishi ni nini/i, en:"TPT-5021: Adjective describes noun. Ex: big, beautiful.", sw:"TPT-5021: Kivumishi hueleza nomino. Mf: kubwa, zuri." },
    { re:/what is adverb|kielezi ni nini/i, en:"TPT-5021: Adverb describes verb. Ex: quickly, slowly.", sw:"TPT-5021: Kielezi hueleza kitenzi. Mf: haraka, polepole." },
    { re:/what is sentence|sentensi ni nini/i, en:"TPT-5021: Sentence has subject + verb.", sw:"TPT-5021: Sentensi ina kiima na kitenzi." },
    { re:/what is paragraph|aya ni nini/i, en:"TPT-5021: Paragraph is group of sentences about one topic.", sw:"TPT-5021: Aya ni kundi la sentensi kuhusu mada moja." },
    { re:/what is synonym|kisawe ni nini/i, en:"TPT-5021: Synonym = word with same meaning. Ex: big = large.", sw:"TPT-5021: Kisawe = neno lenye maana sawa. Mf: kubwa = -kubwa." },
    { re:/what is antonym|kinyume cha neno ni nini/i, en:"TPT-5021: Antonym = opposite meaning. Ex: hot ≠ cold.", sw:"TPT-5021: Kinyume cha neno = maana tofauti. Mf: moto ≠ baridi." },
    { re:/what is computer(?!\w)|kompyuta ni nini/i, en:"TPT-5021: Computer processes data using CPU and RAM.", sw:"TPT-5021: Kompyuta huchakata data kwa kutumia CPU na RAM." },
    { re:/what is internet|intaneti ni nini/i, en:"TPT-5021: Internet connects computers worldwide.", sw:"TPT-5021: Intaneti huunganisha kompyuta duniani kote." },
    { re:/what is software|programu tumizi ni nini/i, en:"TPT-5021: Software = programs. Ex: Chrome, Word.", sw:"TPT-5021: Software = programu. Mf: Chrome, Word." },
    { re:/what is hardware|vifaa vya kompyuta ni nini/i, en:"TPT-5021: Hardware = physical parts. Ex: keyboard, mouse.", sw:"TPT-5021: Hardware = vifaa vinavyoshikika. Mf: kibodi, mouse." },
    { re:/what is algorithm|alogaritham ni nini/i, en:"TPT-5021: Algorithm = step-by-step procedure.", sw:"TPT-5021: Alogaritham = hatua za utaratibu wa kutatua tatizo." },
    { re:/what is coding|upangaji programu ni nini/i, en:"TPT-5021: Coding = writing instructions for computer.", sw:"TPT-5021: Kupanga programu = kuandika maelekezo kwa kompyuta." },
    { re:/what is tpt-5021/i, en:"TPT-5021: I am TPT-5021, offline bot created by Thomas Peter Tengia.", sw:"TPT-5021: Mimi ni TPT-5021, roboti ya offline iliyoumbwa na Thomas Peter Tengia." },
    { re:/what is constitution|katiba ni nini/i, en:"TPT-5021: Constitution = supreme law of a country.", sw:"TPT-5021: Katiba = sheria kuu ya nchi." },
    { re:/what are human rights|haki za binadamu ni nini/i, en:"TPT-5021: Human rights = basic rights for all people.", sw:"TPT-5021: Haki za binadamu = haki za msingi kwa watu wote." },
    { re:/what is citizenship|uraia ni nini/i, en:"TPT-5021: Citizenship = being legal member of a country.", sw:"TPT-5021: Uraia = kuwa mwanachama halali wa nchi." },
    { re:/what is government|serikali ni nini/i, en:"TPT-5021: Government makes laws and runs a country.", sw:"TPT-5021: Serikali hutunga sheria na kuendesha nchi." },
    { re:/what is economy|uchumi ni nini/i, en:"TPT-5021: Economy = how money, goods, and services work.", sw:"TPT-5021: Uchumi = jinsi pesa, bidhaa na huduma zinavyofanya kazi." },

    // ---- LIVE DATA ----
    { re:/weather|rain|sun|temperature|forecast|hali ya hewa/i, en:"TPT-5021: Can't check live weather offline. Needs internet.", sw:"TPT-5021: Siwezi kuangalia hali ya hewa moja kwa moja bila mtandao." },
    { re:/news|latest|today|headlines|habari za leo/i, en:"TPT-5021: No live news offline. Connect to get updates.", sw:"TPT-5021: Sina habari za sasa hivi bila mtandao. Washa mtandao kupata taarifa mpya." },
    { re:/stock|price|bitcoin|crypto|bei ya soko/i, en:"TPT-5021: Can't check prices offline. Needs internet.", sw:"TPT-5021: Siwezi kuangalia bei bila mtandao." },
    { re:/time now|current time|saa ngapi sasa/i, en:"TPT-5021: I don't have live time offline. Check your PC clock.", sw:"TPT-5021: Sina saa ya sasa bila mtandao. Angalia saa ya kifaa chako." },
    { re:/date today|tarehe ya leo/i, en:"TPT-5021: Check your system date. I'm offline.", sw:"TPT-5021: Angalia tarehe ya kifaa chako. Niko offline." },

    // ---- EMOTIONAL ----
    { re:/sad|depressed|unhappy|nina huzuni|nimeumia kihisia/i, en:"TPT-5021: I'm sorry. Please talk to someone you trust.", sw:"TPT-5021: Pole sana. Tafadhali zungumza na mtu unayemwamini." },
    { re:/happy|good|great|nina furaha|nimefurahi/i, en:"TPT-5021: Love to hear that! Keep the good energy.", sw:"TPT-5021: Nafurahi kusikia hivyo! Endelea na hali hiyo nzuri." },
    { re:/\bangry\b|nina hasira/i, en:"TPT-5021: Take a deep breath. I'm here.", sw:"TPT-5021: Vuta pumzi ndefu. Niko hapa." },
    { re:/lonely|nina upweke/i, en:"TPT-5021: You're not alone. I'm here with you.", sw:"TPT-5021: Hauko peke yako. Niko hapa nawe." },
    { re:/joke|funny|laugh|utani|kitu cha kuchekesha/i, en:"TPT-5021: Why did the code go to therapy? Too many bugs.", sw:"TPT-5021: Kwa nini msimbo ulienda kwa daktari wa akili? Ulikuwa na hitilafu nyingi mno." },
    { re:/motivate|motivation|inspire|nitie moyo|hamasa/i, en:"TPT-5021 by Thomas Peter Tengia: Rule 1: Start. Rule 2: Don't stop.", sw:"TPT-5021 (Thomas Peter Tengia): Kanuni ya 1: Anza. Kanuni ya 2: Usisimame." },
    { re:/\bquote\b|nukuu/i, en:"TPT-5021: Success is not final, failure is not fatal.", sw:"TPT-5021: Mafanikio si ya mwisho, kushindwa si mwisho wa maisha." },
    { re:/\badvice\b|ushauri/i, en:"TPT-5021: Learn 1% every day.", sw:"TPT-5021: Jifunze asilimia 1 kila siku." },

    // ---- TPT BRAND + TECH ----
    { re:/who is thomas/i, en:"Thomas Peter Tengia is the creator of TPT-5021.", sw:"Thomas Peter Tengia ndiye muumbaji wa TPT-5021." },
    { re:/watermark|alama ya maji/i, en:"TPT-5021: All TPT exports can include TPT-5021 signature.", sw:"TPT-5021: Faili zote za TPT zinaweza kuwa na saini ya TPT-5021." },
    { re:/\bexport\b|hamisha faili/i, en:"TPT-5021: Export features need online mode.", sw:"TPT-5021: Kuhamisha faili kunahitaji mtandao." },
    { re:/marketplace|soko la tpt/i, en:"TPT-5021: TPT Marketplace is a project by Thomas Peter Tengia.", sw:"TPT-5021: TPT Marketplace ni mradi wa Thomas Peter Tengia." },
    { re:/how to install|jinsi ya kusakinisha/i, en:"TPT-5021: Step 1: Download. Step 2: Run installer. Step 3: Follow wizard.", sw:"TPT-5021: Hatua 1: Pakua. Hatua 2: Fungua installer. Hatua 3: Fuata maelekezo." },
    { re:/virus(?! offline)|hack|malware|udukuzi/i, en:"TPT-5021: I can't help with hacking. Use antivirus.", sw:"TPT-5021: Siwezi kusaidia udukuzi. Tumia antivirus." },
    { re:/password|nywila/i, en:"TPT-5021: I can't see your passwords. Use a password manager.", sw:"TPT-5021: Siwezi kuona nywila zako. Tumia password manager." },
    { re:/\bdownload\b|pakua faili/i, en:"TPT-5021: Can't download files offline.", sw:"TPT-5021: Siwezi kupakua faili bila mtandao." },
    { re:/\bupdate\b|sasisha programu/i, en:"TPT-5021: Can't update offline. Download update manually.", sw:"TPT-5021: Siwezi kusasisha bila mtandao. Pakua sasisho wewe mwenyewe." },
    { re:/\bsettings\b|mipangilio/i, en:"TPT-5021: Settings > Connection > Toggle 'Offline Mode'.", sw:"TPT-5021: Mipangilio > Muunganisho > Washa/Zima 'Offline Mode'." },
    { re:/\binternet\b|mtandao/i, en:"TPT-5021: You are currently offline.", sw:"TPT-5021: Kwa sasa uko nje ya mtandao (offline)." },

    // ---- TPT BRAND (catch-all, kept last so specific entries above win) ----
    { re:/\btpt\b|tpt-5021/i, en:"TPT-5021 = Thomas Peter Tengia Offline Engine.", sw:"TPT-5021 = Injini ya Offline ya Thomas Peter Tengia." },

    // ---- SCHOOL CORE (kept last: broad, so specific matches above win first) ----
    { re:/\bmath\b|calculate|\d\s*[-+*/]\s*\d|hesabu/i, en:"TPT-5021: Basic math only. 2+2=4. For calculus go online.", sw:"TPT-5021: Hesabu za msingi tu. 2+2=4. Kwa calculus washa mtandao." },
  ];

  function offlineReply(text){
    const t = text.toLowerCase().trim();
    if(!t) return pick(
      "TPT-5021: Offline mode is on -- I can't read images without a connection, but I'm here for text.",
      "TPT-5021: Hali ya offline imewashwa -- siwezi kusoma picha bila mtandao, lakini niko tayari kwa maandishi.",
      text
    );
    for(const item of REPLIES){
      if(item.re.test(t)) return pick(item.en, item.sw, text);
    }
    return pick(
      "TPT-5021 by Thomas Peter Tengia: That's for online mode. I'm offline, so I can only give simple canned replies. Turn off Offline mode in Settings for full AI.",
      "TPT-5021 (Thomas Peter Tengia): Hilo linahitaji hali ya mtandaoni. Niko offline, hivyo natoa majibu rahisi yaliyowekwa tayari tu. Zima Offline mode kwenye Settings kupata AI kamili.",
      text
    );
  }


  // ---------- INIT ----------
  loadAll();
})();
</script>
</body>
</html>