# Taverna do Corvo
<style>
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;700;800&family=EB+Garamond:ital,wght@0,400;0,500;0,600;1,400&display=swap');

.tv-root, .tv-root * { box-sizing: border-box; }
.tv-root {
  --wood-deep:#140d08; --wood:#26190f; --wood-light:#3d2a1a; --plank:#1a1109;
  --parchment:#ead9ae; --parchment-dark:#d3ba82; --ink:#2a1c10;
  --gold:#c9963f; --gold-bright:#e8bd6e; --ember:#c05a30; --wine:#7a2323; --muted:#9c8a6a;
  font-family:'EB Garamond', Georgia, serif;
  background:
    radial-gradient(ellipse at 50% -10%, rgba(201,150,63,0.10), transparent 60%),
    repeating-linear-gradient(90deg, var(--wood) 0px, var(--wood-light) 3px, var(--wood) 6px, var(--plank) 7px),
    var(--wood-deep);
  color:var(--parchment);
  min-height:600px;
  padding:0;
  position:relative;
}
.tv-root h1,.tv-root h2,.tv-root h3,.tv-root .tv-display { font-family:'Cinzel', serif; }
.tv-signboard {
  text-align:center; padding:34px 20px 22px; position:relative;
  border-bottom:3px double var(--gold);
  background:linear-gradient(180deg, rgba(0,0,0,0.25), transparent);
}
.tv-torch { font-size:22px; display:inline-block; animation:tv-flicker 2.6s infinite ease-in-out; filter:drop-shadow(0 0 6px rgba(233,140,60,0.7)); }
@keyframes tv-flicker { 0%,100%{opacity:1; transform:scale(1) rotate(0deg);} 30%{opacity:0.75; transform:scale(0.96) rotate(-2deg);} 55%{opacity:1; transform:scale(1.04) rotate(2deg);} 80%{opacity:0.85; transform:scale(0.98);} }
.tv-signboard h1 { font-size:30px; letter-spacing:2px; color:var(--gold-bright); margin:6px 0 6px; text-shadow:0 2px 0 rgba(0,0,0,0.6), 0 0 18px rgba(201,150,63,0.35); }
.tv-signboard p { margin:0; font-style:italic; color:var(--muted); font-size:15px; }
.tv-layout { display:flex; gap:0; align-items:flex-start; min-height:520px; }
.tv-nav { width:168px; flex-shrink:0; padding:20px 10px; border-right:1px solid rgba(201,150,63,0.25); }
.tv-nav button {
  display:flex; align-items:center; gap:10px; width:100%; text-align:left;
  background:transparent; border:1px solid transparent; color:var(--parchment-dark);
  font-family:'EB Garamond', serif; font-size:16px; padding:10px 12px; margin-bottom:6px;
  border-radius:3px; cursor:pointer; transition:background 0.15s, border-color 0.15s, color 0.15s;
}
.tv-nav button:hover { background:rgba(201,150,63,0.08); border-color:rgba(201,150,63,0.2); }
.tv-nav button.active { background:rgba(201,150,63,0.15); border-color:var(--gold); color:var(--gold-bright); }
.tv-nav .tv-icon { font-size:18px; width:22px; text-align:center; }
.tv-presence { margin-top:22px; padding-top:14px; border-top:1px solid rgba(201,150,63,0.2); font-size:13px; color:var(--muted); }
.tv-presence strong { display:block; color:var(--gold); font-weight:600; margin-bottom:6px; font-size:12px; text-transform:none; }
.tv-main { flex:1; padding:22px 26px 26px; min-height:480px; }
.tv-panel { display:none; }
.tv-panel.active { display:block; }
.tv-parchment { background:linear-gradient(180deg, rgba(234,217,174,0.06), rgba(234,217,174,0.02)); border:1px solid rgba(201,150,63,0.25); border-radius:4px; padding:18px 20px; }
.tv-h2 { color:var(--gold-bright); font-size:19px; margin:0 0 4px; }
.tv-sub { color:var(--muted); font-size:14px; margin:0 0 16px; font-style:italic; }
.tv-chatlog { height:320px; overflow-y:auto; padding:6px 4px; margin-bottom:12px; border-bottom:1px solid rgba(201,150,63,0.2); }
.tv-msg { margin-bottom:10px; line-size:1.4; }
.tv-msg .tv-name { color:var(--gold); font-weight:600; }
.tv-msg .tv-time { color:var(--muted); font-size:12px; margin-left:6px; }
.tv-msg .tv-text { color:var(--parchment); display:block; font-size:16px; }
.tv-row { display:flex; gap:8px; }
.tv-input, .tv-select {
  flex:1; background:rgba(0,0,0,0.25); border:1px solid rgba(201,150,63,0.35); color:var(--parchment);
  font-family:'EB Garamond', serif; font-size:15px; padding:9px 12px; border-radius:3px; outline:none;
}
.tv-input:focus, .tv-select:focus { border-color:var(--gold); }
.tv-btn {
  background:linear-gradient(180deg, var(--gold-bright), var(--gold)); color:#291a08; border:1px solid #8a611f;
  font-family:'Cinzel', serif; font-weight:600; font-size:13px; letter-spacing:0.5px; padding:9px 16px;
  border-radius:3px; cursor:pointer; white-space:nowrap;
}
.tv-btn:hover { filter:brightness(1.08); }
.tv-btn.tv-btn-ghost { background:transparent; color:var(--gold-bright); border:1px solid var(--gold); }
.tv-dice-quick { display:flex; gap:10px; flex-wrap:wrap; margin-bottom:18px; }
.tv-die {
  width:64px; height:64px; border-radius:6px; background:radial-gradient(circle at 30% 25%, #4a3222, #241608);
  border:1px solid var(--gold); color:var(--gold-bright); font-family:'Cinzel',serif; font-weight:700; font-size:15px;
  display:flex; align-items:center; justify-content:center; cursor:pointer;
}
.tv-die:hover { border-color:var(--gold-bright); background:radial-gradient(circle at 30% 25%, #5a3f2b, #2c1a0a); }
.tv-dicelog { margin-top:18px; max-height:280px; overflow-y:auto; }
.tv-roll-card { display:flex; align-items:center; gap:14px; padding:10px 12px; margin-bottom:8px; border:1px solid rgba(201,150,63,0.2); border-radius:4px; background:rgba(0,0,0,0.15); }
.tv-roll-total { font-family:'Cinzel',serif; font-size:22px; color:var(--gold-bright); min-width:46px; text-align:center; }
.tv-roll-detail { font-size:14px; color:var(--muted); }
.tv-roll-detail .tv-name { color:var(--gold); }
.tv-queue-item { display:flex; justify-content:space-between; align-items:center; padding:8px 10px; border:1px solid rgba(201,150,63,0.18); border-radius:4px; margin-bottom:6px; font-size:14px; }
.tv-queue-item span.tv-by { color:var(--muted); font-size:12px; }
.tv-now-playing { display:flex; gap:16px; align-items:flex-start; margin-bottom:16px; }
.tv-embed-wrap { width:280px; flex-shrink:0; aspect-ratio:16/9; background:#000; border:1px solid var(--gold); border-radius:4px; overflow:hidden; }
.tv-embed-wrap iframe { width:100%; height:100%; border:0; }
.tv-ambience-grid { display:grid; grid-template-columns:repeat(auto-fill,minmax(150px,1fr)); gap:12px; margin-bottom:18px; }
.tv-amb-card { border:1px solid rgba(201,150,63,0.25); border-radius:4px; padding:14px; text-align:center; cursor:pointer; background:rgba(0,0,0,0.15); }
.tv-amb-card.active { border-color:var(--gold); background:rgba(201,150,63,0.14); }
.tv-amb-card .tv-amb-icon { font-size:26px; display:block; margin-bottom:6px; }
.tv-amb-card .tv-amb-label { font-family:'Cinzel',serif; font-size:13px; color:var(--gold-bright); }
.tv-volume-row { display:flex; align-items:center; gap:10px; margin-top:6px; }
.tv-volume-row input[type=range] { flex:1; }
.tv-overlay { position:absolute; inset:0; background:radial-gradient(ellipse at 50% 20%, rgba(60,40,20,0.5), rgba(10,6,3,0.92)); display:flex; align-items:center; justify-content:center; z-index:5; }
.tv-join-card { background:var(--wood); border:2px solid var(--gold); border-radius:6px; padding:32px 34px; width:340px; text-align:center; box-shadow:0 0 40px rgba(0,0,0,0.6); }
.tv-join-card h2 { color:var(--gold-bright); margin:8px 0 6px; font-size:22px; }
.tv-join-card p { color:var(--muted); font-size:14px; margin:0 0 18px; }
.tv-join-card input { width:100%; margin-bottom:14px; }
.tv-empty { color:var(--muted); font-style:italic; font-size:14px; padding:14px 0; }
.tv-note { font-size:12px; color:var(--muted); margin-top:10px; line-height:1.5; }
.tv-chatlog::-webkit-scrollbar, .tv-dicelog::-webkit-scrollbar { width:8px; }
.tv-chatlog::-webkit-scrollbar-thumb, .tv-dicelog::-webkit-scrollbar-thumb { background:var(--wood-light); border-radius:4px; }
@media (max-width:640px){ .tv-layout{flex-direction:column;} .tv-nav{width:100%; display:flex; flex-wrap:wrap; border-right:none; border-bottom:1px solid rgba(201,150,63,0.25);} .tv-nav button{width:auto;} .tv-presence{display:none;} .tv-now-playing{flex-direction:column;} .tv-embed-wrap{width:100%;} }
</style>

<div class="tv-root" id="tv-root">
  <div class="tv-overlay" id="tv-join-overlay">
    <div class="tv-join-card">
      <span class="tv-torch">🔥</span>
      <h2>Antes de entrar...</h2>
      <p>Como os companheiros de mesa devem te chamar nesta taverna?</p>
      <input class="tv-input" id="tv-name-input" placeholder="ex: Thalor, o Bardo" maxlength="24" />
      <button class="tv-btn" id="tv-join-btn" style="width:100%">Entrar na Taverna</button>
    </div>
  </div>

  <div class="tv-signboard">
    <span class="tv-torch">🔥</span>
    <h1>A Taverna do Corvo Negro</h1>
    <span class="tv-torch">🔥</span>
    <p>sua guilda, seus dados, sua mesa — sempre de portas abertas</p>
  </div>

  <div class="tv-layout">
    <nav class="tv-nav">
      <button class="tv-navbtn active" data-tab="chat"><span class="tv-icon">💬</span> Salão</button>
      <button class="tv-navbtn" data-tab="dice"><span class="tv-icon">🎲</span> Dados</button>
      <button class="tv-navbtn" data-tab="music"><span class="tv-icon">🎻</span> Bardo</button>
      <button class="tv-navbtn" data-tab="ambience"><span class="tv-icon">🌲</span> Ventos</button>
      <div class="tv-presence">
        <strong>Na mesa agora</strong>
        <div id="tv-presence-list">ninguém ainda...</div>
      </div>
    </nav>

    <main class="tv-main">

      <section class="tv-panel active" id="panel-chat">
        <div class="tv-parchment">
          <h2 class="tv-h2">O Salão Principal</h2>
          <p class="tv-sub">converse com o grupo enquanto a lareira estala</p>
          <div class="tv-chatlog" id="tv-chatlog"><div class="tv-empty">O salão está em silêncio. Diga algo!</div></div>
          <div class="tv-row">
            <input class="tv-input" id="tv-chat-input" placeholder="Escreva sua fala..." maxlength="500" />
            <button class="tv-btn" id="tv-chat-send">Falar</button>
          </div>
        </div>
      </section>

      <section class="tv-panel" id="panel-dice">
        <div class="tv-parchment">
          <h2 class="tv-h2">A Mesa de Dados</h2>
          <p class="tv-sub">role um dado rápido ou escreva uma fórmula, ex: 2d6+3</p>
          <div class="tv-dice-quick">
            <div class="tv-die" data-sides="4">d4</div>
            <div class="tv-die" data-sides="6">d6</div>
            <div class="tv-die" data-sides="8">d8</div>
            <div class="tv-die" data-sides="10">d10</div>
            <div class="tv-die" data-sides="12">d12</div>
            <div class="tv-die" data-sides="20">d20</div>
            <div class="tv-die" data-sides="100">d100</div>
          </div>
          <div class="tv-row">
            <input class="tv-input" id="tv-formula-input" placeholder="ex: 2d6+3, 4d8-2, 1d20" />
            <button class="tv-btn" id="tv-formula-roll">Rolar</button>
          </div>
          <div id="tv-formula-error" style="color:#e07a5f; font-size:13px; margin-top:6px; display:none;"></div>
          <div class="tv-dicelog" id="tv-dicelog"><div class="tv-empty">Nenhum dado rolado ainda. A sorte aguarda.</div></div>
        </div>
      </section>

      <section class="tv-panel" id="panel-music">
        <div class="tv-parchment">
          <h2 class="tv-h2">O Bardo da Casa</h2>
          <p class="tv-sub">adicione músicas do YouTube à fila compartilhada da mesa</p>
          <div class="tv-now-playing">
            <div class="tv-embed-wrap" id="tv-embed-wrap">
              <div style="width:100%;height:100%;display:flex;align-items:center;justify-content:center;color:var(--muted);font-size:13px;text-align:center;padding:10px;">O bardo está calado.<br/>Adicione uma música à fila.</div>
            </div>
            <div style="flex:1;">
              <div id="tv-current-title" style="color:var(--gold-bright); font-family:'Cinzel',serif; font-size:15px; margin-bottom:10px;">Nada tocando agora</div>
              <div class="tv-row" style="margin-bottom:10px;">
                <button class="tv-btn" id="tv-play-next">Tocar próxima</button>
                <button class="tv-btn tv-btn-ghost" id="tv-stop-music">Silenciar</button>
              </div>
              <p class="tv-note">Se a música não iniciar sozinha ao trocar, clique no player — o navegador às vezes bloqueia o som automático.</p>
            </div>
          </div>
          <div class="tv-row" style="margin-bottom:14px;">
            <input class="tv-input" id="tv-music-input" placeholder="Cole o link do YouTube..." />
            <button class="tv-btn" id="tv-music-add">Adicionar à fila</button>
          </div>
          <div id="tv-music-error" style="color:#e07a5f; font-size:13px; margin-bottom:10px; display:none;"></div>
          <div id="tv-queue-list"><div class="tv-empty">A fila está vazia.</div></div>
        </div>
      </section>

      <section class="tv-panel" id="panel-ambience">
        <div class="tv-parchment">
          <h2 class="tv-h2">Os Ventos da Taverna</h2>
          <p class="tv-sub">escolha o som de fundo da mesa — todos veem a escolha, cada um ajusta seu volume</p>
          <div class="tv-ambience-grid">
            <div class="tv-amb-card" data-amb="floresta"><span class="tv-amb-icon">🌲</span><span class="tv-amb-label">Floresta</span></div>
            <div class="tv-amb-card" data-amb="chuva"><span class="tv-amb-icon">🌧️</span><span class="tv-amb-label">Chuva</span></div>
            <div class="tv-amb-card" data-amb="lareira"><span class="tv-amb-icon">🕯️</span><span class="tv-amb-label">Lareira</span></div>
            <div class="tv-amb-card" data-amb="batalha"><span class="tv-amb-icon">⚔️</span><span class="tv-amb-label">Batalha</span></div>
            <div class="tv-amb-card" data-amb="silencio"><span class="tv-amb-icon">🌙</span><span class="tv-amb-label">Silêncio</span></div>
          </div>
          <div class="tv-volume-row">
            <span style="font-size:13px; color:var(--muted);">Seu volume</span>
            <input type="range" id="tv-amb-volume" min="0" max="100" value="45" />
          </div>
          <p class="tv-note">Sons gerados no seu navegador, sem arquivos externos — funcionam mesmo se um link de áudio cair.</p>
        </div>
      </section>

    </main>
  </div>
</div>

<script>
(function(){
  const root = document.getElementById('tv-root');
  let currentUser = null;
  const chatCache = {};
  const diceCache = {};
  let lastMusicVideoId = undefined;
  let lastAmbienceType = undefined;
  let pollTimer = null;

  function esc(s){ const d=document.createElement('div'); d.textContent = String(s==null?'':s); return d.innerHTML; }
  function fmtTime(ts){ const d = new Date(ts); return d.toLocaleTimeString('pt-BR', {hour:'2-digit', minute:'2-digit'}); }
  function randKey(){ return Math.random().toString(36).slice(2,8); }

  async function storeSet(key, value){
    try { return await window.storage.set(key, JSON.stringify(value), true); }
    catch(e){ console.error('storage set failed', key, e); return null; }
  }
  async function storeGet(key){
    try {
      const r = await window.storage.get(key, true);
      return r ? JSON.parse(r.value) : null;
    } catch(e){ return null; }
  }
  async function storeList(prefix){
    try {
      const r = await window.storage.list(prefix, true);
      return (r && r.keys) ? r.keys : [];
    } catch(e){ return []; }
  }

  document.getElementById('tv-join-btn').addEventListener('click', joinTavern);
  document.getElementById('tv-name-input').addEventListener('keydown', function(e){ if(e.key==='Enter') joinTavern(); });

  function joinTavern(){
    const input = document.getElementById('tv-name-input');
    const name = input.value.trim();
    if(!name){ input.style.borderColor = '#e07a5f'; input.placeholder = 'Diga um nome, viajante...'; return; }
    currentUser = name.slice(0,24);
    document.getElementById('tv-join-overlay').style.display = 'none';
    startPolling();
  }

  document.querySelectorAll('.tv-navbtn').forEach(btn=>{
    btn.addEventListener('click', function(){
      document.querySelectorAll('.tv-navbtn').forEach(b=>b.classList.remove('active'));
      document.querySelectorAll('.tv-panel').forEach(p=>p.classList.remove('active'));
      btn.classList.add('active');
      document.getElementById('panel-'+btn.dataset.tab).classList.add('active');
    });
  });

  function renderPresence(){
    const seen = {};
    Object.values(chatCache).forEach(m=>{ if(m && m.name && Date.now()-m.time < 10*60*1000) seen[m.name]=true; });
    Object.values(diceCache).forEach(m=>{ if(m && m.name && Date.now()-m.time < 10*60*1000) seen[m.name]=true; });
    if(currentUser) seen[currentUser]=true;
    const names = Object.keys(seen);
    const el = document.getElementById('tv-presence-list');
    el.innerHTML = names.length ? names.map(n=>'<div>• '+esc(n)+'</div>').join('') : 'ninguém ainda...';
  }

  function renderChat(){
    const log = document.getElementById('tv-chatlog');
    const keys = Object.keys(chatCache).sort();
    if(!keys.length){ log.innerHTML = '<div class="tv-empty">O salão está em silêncio. Diga algo!</div>'; return; }
    const wasAtBottom = (log.scrollTop + log.clientHeight) >= (log.scrollHeight - 30);
    log.innerHTML = keys.slice(-150).map(k=>{
      const m = chatCache[k];
      return '<div class="tv-msg"><span class="tv-name">'+esc(m.name)+'</span><span class="tv-time">'+fmtTime(m.time)+'</span><span class="tv-text">'+esc(m.text)+'</span></div>';
    }).join('');
    if(wasAtBottom) log.scrollTop = log.scrollHeight;
  }

  async function fetchChat(){
    const keys = await storeList('chat:');
    const newKeys = keys.filter(k=>!(k in chatCache));
    for(const k of newKeys){ const v = await storeGet(k); if(v) chatCache[k]=v; }
    if(newKeys.length){ renderChat(); renderPresence(); }
  }

  document.getElementById('tv-chat-send').addEventListener('click', sendChat);
  document.getElementById('tv-chat-input').addEventListener('keydown', function(e){ if(e.key==='Enter') sendChat(); });
  async function sendChat(){
    const input = document.getElementById('tv-chat-input');
    const text = input.value.trim();
    if(!text || !currentUser) return;
    input.value='';
    const key = 'chat:'+String(Date.now()).padStart(14,'0')+'_'+randKey();
    const msg = {name:currentUser, text:text, time:Date.now()};
    chatCache[key]=msg;
    renderChat(); renderPresence();
    await storeSet(key, msg);
  }

  function renderDice(){
    const log = document.getElementById('tv-dicelog');
    const keys = Object.keys(diceCache).sort();
    if(!keys.length){ log.innerHTML = '<div class="tv-empty">Nenhum dado rolado ainda. A sorte aguarda.</div>'; return; }
    log.innerHTML = keys.slice(-100).reverse().map(k=>{
      const r = diceCache[k];
      return '<div class="tv-roll-card"><div class="tv-roll-total">'+r.total+'</div><div class="tv-roll-detail"><span class="tv-name">'+esc(r.name)+'</span> rolou <strong>'+esc(r.formula)+'</strong> &nbsp; ['+r.rolls.join(', ')+(r.mod?(r.mod>0?' +'+r.mod:' '+r.mod):'')+']  &nbsp;<span style="color:var(--muted);">'+fmtTime(r.time)+'</span></div></div>';
    }).join('');
  }

  async function fetchDice(){
    const keys = await storeList('dice:');
    const newKeys = keys.filter(k=>!(k in diceCache));
    for(const k of newKeys){ const v = await storeGet(k); if(v) diceCache[k]=v; }
    if(newKeys.length){ renderDice(); renderPresence(); }
  }

  async function registerRoll(formula, rolls, mod, total){
    const key = 'dice:'+String(Date.now()).padStart(14,'0')+'_'+randKey();
    const r = {name:currentUser, formula:formula, rolls:rolls, mod:mod, total:total, time:Date.now()};
    diceCache[key]=r;
    renderDice(); renderPresence();
    await storeSet(key, r);
  }

  document.querySelectorAll('.tv-die').forEach(die=>{
    die.addEventListener('click', function(){
      if(!currentUser) return;
      const sides = parseInt(die.dataset.sides);
      const roll = 1+Math.floor(Math.random()*sides);
      registerRoll('1d'+sides, [roll], 0, roll);
    });
  });

  document.getElementById('tv-formula-roll').addEventListener('click', rollFormulaHandler);
  document.getElementById('tv-formula-input').addEventListener('keydown', function(e){ if(e.key==='Enter') rollFormulaHandler(); });
  function rollFormulaHandler(){
    if(!currentUser) return;
    const input = document.getElementById('tv-formula-input');
    const errEl = document.getElementById('tv-formula-error');
    const formula = input.value.trim();
    errEl.style.display='none';
    if(!formula) return;
    const m = formula.match(/^(\d*)d(\d+)\s*([+-]\s*\d+)?$/i);
    if(!m){ errEl.textContent = 'Fórmula inválida. Use algo como 2d6+3 ou d20.'; errEl.style.display='block'; return; }
    const count = m[1] ? parseInt(m[1]) : 1;
    const sides = parseInt(m[2]);
    const mod = m[3] ? parseInt(m[3].replace(/\s+/g,'')) : 0;
    if(count<1 || count>100 || sides<2 || sides>1000){ errEl.textContent = 'Use entre 1 e 100 dados, com 2 a 1000 lados.'; errEl.style.display='block'; return; }
    const rolls=[];
    for(let i=0;i<count;i++) rolls.push(1+Math.floor(Math.random()*sides));
    const total = rolls.reduce((a,b)=>a+b,0)+mod;
    input.value='';
    registerRoll(formula, rolls, mod, total);
  }

  function extractYouTubeId(url){
    url = url.trim();
    const m = url.match(/(?:youtube\.com\/watch\?v=|youtu\.be\/|youtube\.com\/embed\/|youtube\.com\/shorts\/)([a-zA-Z0-9_-]{11})/);
    if(m) return m[1];
    if(/^[a-zA-Z0-9_-]{11}$/.test(url)) return url;
    return null;
  }

  document.getElementById('tv-music-add').addEventListener('click', addMusic);
  document.getElementById('tv-music-input').addEventListener('keydown', function(e){ if(e.key==='Enter') addMusic(); });
  async function addMusic(){
    if(!currentUser) return;
    const input = document.getElementById('tv-music-input');
    const errEl = document.getElementById('tv-music-error');
    const url = input.value.trim();
    errEl.style.display='none';
    const videoId = extractYouTubeId(url);
    if(!videoId){ errEl.textContent = 'Não encontrei um link válido do YouTube.'; errEl.style.display='block'; return; }
    input.value='';
    const queue = (await storeGet('music:queue')) || [];
    queue.push({id:randKey(), videoId:videoId, addedBy:currentUser});
    await storeSet('music:queue', queue);
    renderQueue(queue);
  }

  function renderQueue(queue){
    const el = document.getElementById('tv-queue-list');
    if(!queue || !queue.length){ el.innerHTML = '<div class="tv-empty">A fila está vazia.</div>'; return; }
    el.innerHTML = queue.map(q=>'<div class="tv-queue-item"><span>youtube.com/watch?v='+esc(q.videoId)+'</span><span class="tv-by">adicionado por '+esc(q.addedBy)+'</span></div>').join('');
  }

  function renderCurrent(current){
    const wrap = document.getElementById('tv-embed-wrap');
    const title = document.getElementById('tv-current-title');
    const videoId = current && current.videoId ? current.videoId : null;
    if(videoId === lastMusicVideoId) return;
    lastMusicVideoId = videoId;
    if(videoId){
      wrap.innerHTML = '<iframe src="https://www.youtube.com/embed/'+esc(videoId)+'?autoplay=1&rel=0" allow="autoplay; encrypted-media" allowfullscreen></iframe>';
      title.textContent = 'Tocando agora — adicionado por '+(current.by||'alguém');
    } else {
      wrap.innerHTML = '<div style="width:100%;height:100%;display:flex;align-items:center;justify-content:center;color:var(--muted);font-size:13px;text-align:center;padding:10px;">O bardo está calado.<br/>Adicione uma música à fila.</div>';
      title.textContent = 'Nada tocando agora';
    }
  }

  document.getElementById('tv-play-next').addEventListener('click', async function(){
    if(!currentUser) return;
    const queue = (await storeGet('music:queue')) || [];
    if(!queue.length) return;
    const next = queue.shift();
    await storeSet('music:queue', queue);
    await storeSet('music:current', {videoId:next.videoId, by:currentUser, time:Date.now()});
    renderQueue(queue);
    renderCurrent({videoId:next.videoId, by:currentUser});
  });

  document.getElementById('tv-stop-music').addEventListener('click', async function(){
    if(!currentUser) return;
    await storeSet('music:current', {videoId:null});
    renderCurrent({videoId:null});
  });

  async function fetchMusic(){
    const queue = await storeGet('music:queue');
    renderQueue(queue || []);
    const current = await storeGet('music:current');
    renderCurrent(current || {videoId:null});
  }

  let audioCtx = null;
  let ambNodes = [];
  let ambTimers = [];
  let masterGain = null;

  function ensureAudio(){
    if(!audioCtx){
      audioCtx = new (window.AudioContext || window.webkitAudioContext)();
      masterGain = audioCtx.createGain();
      masterGain.gain.value = parseInt(document.getElementById('tv-amb-volume').value)/100 * 0.5;
      masterGain.connect(audioCtx.destination);
    }
    if(audioCtx.state === 'suspended') audioCtx.resume();
  }

  function makeNoiseBuffer(duration){
    const bufferSize = audioCtx.sampleRate * duration;
    const buffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
    const data = buffer.getChannelData(0);
    for(let i=0;i<bufferSize;i++){ data[i] = Math.random()*2-1; }
    return buffer;
  }

  function stopAmbience(){
    ambTimers.forEach(t=>clearInterval(t)); ambTimers=[];
    ambNodes.forEach(n=>{ try{ n.stop && n.stop(); n.disconnect && n.disconnect(); }catch(e){} });
    ambNodes = [];
  }

  function loopedNoise(filterType, freq, gainVal, q){
    ensureAudio();
    const src = audioCtx.createBufferSource();
    src.buffer = makeNoiseBuffer(4);
    src.loop = true;
    const filter = audioCtx.createBiquadFilter();
    filter.type = filterType; filter.frequency.value = freq; if(q) filter.Q.value = q;
    const gain = audioCtx.createGain();
    gain.gain.value = gainVal;
    src.connect(filter); filter.connect(gain); gain.connect(masterGain);
    src.start();
    ambNodes.push(src, filter, gain);
    return gain;
  }

  function burst(freqFilter, freq, dur, gainPeak, type){
    if(!audioCtx) return;
    const src = audioCtx.createBufferSource();
    src.buffer = makeNoiseBuffer(dur);
    const filter = audioCtx.createBiquadFilter();
    filter.type = type||'bandpass'; filter.frequency.value = freq;
    const gain = audioCtx.createGain();
    gain.gain.setValueAtTime(0, audioCtx.currentTime);
    gain.gain.linearRampToValueAtTime(gainPeak, audioCtx.currentTime+0.02);
    gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime+dur);
    src.connect(filter); filter.connect(gain); gain.connect(masterGain);
    src.start(); src.stop(audioCtx.currentTime+dur+0.05);
  }

  function tone(freq, dur, gainPeak, waveType){
    if(!audioCtx) return;
    const osc = audioCtx.createOscillator();
    osc.type = waveType||'sine'; osc.frequency.value = freq;
    const gain = audioCtx.createGain();
    gain.gain.setValueAtTime(0, audioCtx.currentTime);
    gain.gain.linearRampToValueAtTime(gainPeak, audioCtx.currentTime+0.03);
    gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime+dur);
    osc.connect(gain); gain.connect(masterGain);
    osc.start(); osc.stop(audioCtx.currentTime+dur+0.05);
  }

  function startAmbience(type){
    stopAmbience();
    if(type === 'silencio' || !type) return;
    ensureAudio();
    if(type === 'floresta'){
      loopedNoise('lowpass', 700, 0.35);
      ambTimers.push(setInterval(()=>{ tone(1500+Math.random()*1500, 0.15, 0.15, 'sine'); }, 2200+Math.random()*3000));
    } else if(type === 'chuva'){
      loopedNoise('lowpass', 2200, 0.4);
      loopedNoise('highpass', 3000, 0.08);
    } else if(type === 'lareira'){
      loopedNoise('lowpass', 500, 0.18);
      ambTimers.push(setInterval(()=>{ burst('bandpass', 800+Math.random()*1200, 0.08, 0.25, 'bandpass'); }, 250+Math.random()*400));
    } else if(type === 'batalha'){
      ambTimers.push(setInterval(()=>{ burst('lowpass', 120, 0.18, 0.5, 'lowpass'); }, 550+Math.random()*300));
      ambTimers.push(setInterval(()=>{ tone(1800+Math.random()*800, 0.12, 0.2, 'square'); }, 1800+Math.random()*2400));
    }
  }

  document.querySelectorAll('.tv-amb-card').forEach(card=>{
    card.addEventListener('click', async function(){
      if(!currentUser) return;
      document.querySelectorAll('.tv-amb-card').forEach(c=>c.classList.remove('active'));
      card.classList.add('active');
      const type = card.dataset.amb;
      lastAmbienceType = type;
      startAmbience(type);
      await storeSet('ambience:current', {type:type, by:currentUser, time:Date.now()});
    });
  });

  document.getElementById('tv-amb-volume').addEventListener('input', function(){
    if(masterGain) masterGain.gain.value = parseInt(this.value)/100 * 0.5;
  });

  async function fetchAmbience(){
    const current = await storeGet('ambience:current');
    const type = current ? current.type : 'silencio';
    if(type === lastAmbienceType) return;
    lastAmbienceType = type;
    document.querySelectorAll('.tv-amb-card').forEach(c=>c.classList.toggle('active', c.dataset.amb===type));
    startAmbience(type);
  }

  function startPolling(){
    fetchChat(); fetchDice(); fetchMusic(); fetchAmbience();
    pollTimer = setInterval(function(){
      fetchChat(); fetchDice(); fetchMusic(); fetchAmbience();
    }, 3000);
  }
})();
</script>
