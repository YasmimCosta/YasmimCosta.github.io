<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>A Taverna do Corvo Rosa</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@600;700;800&family=EB+Garamond:ital,wght@0,400;0,500;0,600;1,400&display=swap" rel="stylesheet">
<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-database-compat.js"></script>
<style>
* { box-sizing: border-box; }
:root {
  --wood-deep:#f6dbe4; --wood:#f2c9d8; --wood-light:#eeb7cb; --plank:#e6a3ba;
  --page-bg:#fdf1f4; --panel:#fffbfa; --panel-line:rgba(196,110,140,0.28);
  --ink:#5a2438; --heading:#a8395e; --accent:#d98aa3; --accent-bright:#c85f82;
  --accent-soft:#f6dbe4; --muted:#a97a8a; --danger:#c0435a; --white:#fffdfc;
}
body {
  margin:0; font-family:'EB Garamond', Georgia, serif; color:var(--ink);
  background:
    radial-gradient(ellipse at 50% -10%, rgba(216,138,163,0.18), transparent 55%),
    repeating-linear-gradient(90deg, var(--wood) 0px, var(--wood-light) 3px, var(--wood) 7px, var(--plank) 8px),
    var(--page-bg);
  min-height:100vh;
}
h1,h2,h3,.display { font-family:'Cinzel', serif; }
.signboard { text-align:center; padding:38px 20px 24px; border-bottom:3px double var(--accent-bright); background:linear-gradient(180deg, rgba(255,255,255,0.35), transparent); }
.torch { font-size:22px; display:inline-block; animation:flicker 2.6s infinite ease-in-out; filter:drop-shadow(0 0 8px rgba(216,138,163,0.6)); }
@keyframes flicker { 0%,100%{opacity:1; transform:scale(1) rotate(0deg);} 30%{opacity:0.8; transform:scale(0.96) rotate(-2deg);} 55%{opacity:1; transform:scale(1.05) rotate(2deg);} 80%{opacity:0.88; transform:scale(0.98);} }
.signboard h1 { font-size:32px; letter-spacing:1.5px; color:var(--heading); margin:8px 0 6px; text-shadow:0 1px 0 rgba(255,255,255,0.6); }
.signboard p { margin:0; font-style:italic; color:var(--muted); font-size:15px; }
.layout { display:flex; gap:0; align-items:flex-start; max-width:1100px; margin:0 auto; }
.nav { width:180px; flex-shrink:0; padding:22px 12px; }
.nav button { display:flex; align-items:center; gap:10px; width:100%; text-align:left; background:transparent; border:1px solid transparent; color:var(--ink); font-family:'EB Garamond',serif; font-size:16px; padding:10px 12px; margin-bottom:6px; border-radius:6px; cursor:pointer; }
.nav button:hover { background:rgba(216,138,163,0.14); }
.nav button.active { background:var(--accent-soft); border-color:var(--accent); color:var(--heading); font-weight:600; }
.nav .icon { font-size:18px; width:22px; text-align:center; }
.presence { margin-top:20px; padding-top:14px; border-top:1px solid var(--panel-line); font-size:13px; color:var(--muted); }
.presence strong { display:block; color:var(--heading); font-weight:600; margin-bottom:6px; font-size:12px; }
.main { flex:1; padding:22px 26px 40px; min-height:480px; }
.panel { display:none; }
.panel.active { display:block; }
.card { background:var(--panel); border:1px solid var(--panel-line); border-radius:8px; padding:20px 22px; box-shadow:0 2px 10px rgba(168,57,94,0.06); }
.h2 { color:var(--heading); font-size:19px; margin:0 0 4px; }
.sub { color:var(--muted); font-size:14px; margin:0 0 16px; font-style:italic; }
.chatlog { height:340px; overflow-y:auto; padding:6px 4px; margin-bottom:12px; border-bottom:1px solid var(--panel-line); }
.msg { margin-bottom:10px; }
.msg .name { color:var(--heading); font-weight:600; }
.msg .time { color:var(--muted); font-size:12px; margin-left:6px; }
.msg .text { color:var(--ink); display:block; font-size:16px; }
.row { display:flex; gap:8px; }
input[type=text], input[type=url], .select { flex:1; background:var(--white); border:1px solid var(--panel-line); color:var(--ink); font-family:'EB Garamond',serif; font-size:15px; padding:9px 12px; border-radius:6px; outline:none; }
input:focus { border-color:var(--accent-bright); }
.btn { background:linear-gradient(180deg, #ecb0c3, var(--accent-bright)); color:#3f1728; border:1px solid #b9647f; font-family:'Cinzel',serif; font-weight:600; font-size:13px; letter-spacing:0.4px; padding:9px 16px; border-radius:6px; cursor:pointer; white-space:nowrap; }
.btn:hover { filter:brightness(1.05); }
.btn.ghost { background:transparent; color:var(--heading); border:1px solid var(--accent-bright); }
.dice-quick { display:flex; gap:10px; flex-wrap:wrap; margin-bottom:18px; }
.die { width:64px; height:64px; border-radius:10px; background:linear-gradient(160deg, #fff, var(--accent-soft)); border:1px solid var(--accent-bright); color:var(--heading); font-family:'Cinzel',serif; font-weight:700; font-size:15px; display:flex; align-items:center; justify-content:center; cursor:pointer; }
.die:hover { background:linear-gradient(160deg, #fff, #f0b9cd); }
.dicelog { margin-top:18px; max-height:280px; overflow-y:auto; }
.roll-card { display:flex; align-items:center; gap:14px; padding:10px 12px; margin-bottom:8px; border:1px solid var(--panel-line); border-radius:6px; background:var(--white); }
.roll-total { font-family:'Cinzel',serif; font-size:22px; color:var(--heading); min-width:46px; text-align:center; }
.roll-detail { font-size:14px; color:var(--muted); }
.roll-detail .name { color:var(--heading); }
.queue-item { display:flex; justify-content:space-between; align-items:center; padding:8px 10px; border:1px solid var(--panel-line); border-radius:6px; margin-bottom:6px; font-size:14px; background:var(--white); }
.queue-item .by { color:var(--muted); font-size:12px; }
.now-playing { display:flex; gap:16px; align-items:flex-start; margin-bottom:16px; flex-wrap:wrap; }
.embed-wrap { width:280px; flex-shrink:0; aspect-ratio:16/9; background:#000; border:1px solid var(--accent-bright); border-radius:6px; overflow:hidden; }
.embed-wrap iframe { width:100%; height:100%; border:0; }
.ambience-grid { display:grid; grid-template-columns:repeat(auto-fill,minmax(140px,1fr)); gap:12px; margin-bottom:18px; }
.amb-card { border:1px solid var(--panel-line); border-radius:8px; padding:14px; text-align:center; cursor:pointer; background:var(--white); }
.amb-card.active { border-color:var(--accent-bright); background:var(--accent-soft); }
.amb-card .amb-icon { font-size:26px; display:block; margin-bottom:6px; }
.amb-card .amb-label { font-family:'Cinzel',serif; font-size:13px; color:var(--heading); }
.volume-row { display:flex; align-items:center; gap:10px; margin-top:6px; }
.volume-row input[type=range] { flex:1; }
.note { font-size:12px; color:var(--muted); margin-top:10px; line-height:1.5; }
.empty { color:var(--muted); font-style:italic; font-size:14px; padding:14px 0; }
.overlay { position:fixed; inset:0; background:radial-gradient(ellipse at 50% 20%, rgba(255,255,255,0.6), rgba(214,140,163,0.55)); display:flex; align-items:center; justify-content:center; z-index:50; }
.join-card { background:var(--panel); border:2px solid var(--accent-bright); border-radius:10px; padding:32px 34px; width:340px; text-align:center; box-shadow:0 10px 40px rgba(90,36,56,0.25); }
.join-card h2 { color:var(--heading); margin:8px 0 6px; font-size:22px; }
.join-card p { color:var(--muted); font-size:14px; margin:0 0 18px; }
.join-card input { width:100%; margin-bottom:14px; }
.token-toolbar { display:flex; gap:8px; align-items:center; margin-bottom:14px; flex-wrap:wrap; }
.color-swatch { width:22px; height:22px; border-radius:50%; border:2px solid transparent; cursor:pointer; display:inline-block; }
.color-swatch.selected { border-color:var(--heading); }
.board-wrap { border:1px solid var(--panel-line); border-radius:8px; overflow:hidden; background:var(--white); }
.board { position:relative; width:100%; aspect-ratio:4/3; background-size:cover; background-position:center; background-color:#fdf3f6; background-image: repeating-linear-gradient(0deg, transparent, transparent 39px, rgba(196,110,140,0.18) 40px), repeating-linear-gradient(90deg, transparent, transparent 39px, rgba(196,110,140,0.18) 40px); touch-action:none; }
.token { position:absolute; width:40px; height:40px; margin-left:-20px; margin-top:-20px; border-radius:50%; display:flex; align-items:center; justify-content:center; color:#fff; font-family:'Cinzel',serif; font-weight:700; font-size:13px; cursor:grab; border:2px solid rgba(255,255,255,0.85); box-shadow:0 2px 6px rgba(0,0,0,0.25); user-select:none; }
.token.selected { outline:3px solid var(--heading); }
::-webkit-scrollbar { width:8px; }
::-webkit-scrollbar-thumb { background:var(--wood-light); border-radius:4px; }
@media (max-width:640px){ .layout{flex-direction:column;} .nav{width:100%; display:flex; flex-wrap:wrap; border-bottom:1px solid var(--panel-line);} .nav button{width:auto;} .presence{display:none;} }
</style>
</head>
<body>

<div class="overlay" id="join-overlay">
  <div class="join-card">
    <span class="torch">🌸</span>
    <h2>Antes de entrar...</h2>
    <p>Como os companheiros de mesa devem te chamar nesta taverna?</p>
    <input type="text" id="name-input" placeholder="ex: Thalor, o Bardo" maxlength="24" />
    <button class="btn" id="join-btn" style="width:100%">Entrar na Taverna</button>
  </div>
</div>

<div class="signboard">
  <span class="torch">🌸</span>
  <h1>A Taverna da Rosa Rendada</h1>
  <span class="torch">🌸</span>
  <p>sua guilda, seus dados, sua mesa — sempre de portas abertas</p>
</div>

<div class="layout">
  <nav class="nav">
    <button class="navbtn active" data-tab="chat"><span class="icon">💬</span> Salão</button>
    <button class="navbtn" data-tab="dice"><span class="icon">🎲</span> Dados</button>
    <button class="navbtn" data-tab="music"><span class="icon">🎻</span> Bardo</button>
    <button class="navbtn" data-tab="tabletop"><span class="icon">🗺️</span> Mesa</button>
    <button class="navbtn" data-tab="ambience"><span class="icon">🌷</span> Ventos</button>
    <div class="presence">
      <strong>Na mesa agora</strong>
      <div id="presence-list">ninguém ainda...</div>
    </div>
  </nav>

  <main class="main">

    <section class="panel active" id="panel-chat">
      <div class="card">
        <h2 class="h2">O Salão Principal</h2>
        <p class="sub">converse com o grupo em tempo real</p>
        <div class="chatlog" id="chatlog"><div class="empty">O salão está em silêncio. Diga algo!</div></div>
        <div class="row">
          <input type="text" id="chat-input" placeholder="Escreva sua fala..." maxlength="500" />
          <button class="btn" id="chat-send">Falar</button>
        </div>
      </div>
    </section>

    <section class="panel" id="panel-dice">
      <div class="card">
        <h2 class="h2">A Mesa de Dados</h2>
        <p class="sub">role um dado rápido ou escreva uma fórmula, ex: 2d6+3</p>
        <div class="dice-quick">
          <div class="die" data-sides="4">d4</div>
          <div class="die" data-sides="6">d6</div>
          <div class="die" data-sides="8">d8</div>
          <div class="die" data-sides="10">d10</div>
          <div class="die" data-sides="12">d12</div>
          <div class="die" data-sides="20">d20</div>
          <div class="die" data-sides="100">d100</div>
        </div>
        <div class="row">
          <input type="text" id="formula-input" placeholder="ex: 2d6+3, 4d8-2, 1d20" />
          <button class="btn" id="formula-roll">Rolar</button>
        </div>
        <div id="formula-error" style="color:var(--danger); font-size:13px; margin-top:6px; display:none;"></div>
        <div class="dicelog" id="dicelog"><div class="empty">Nenhum dado rolado ainda. A sorte aguarda.</div></div>
      </div>
    </section>

    <section class="panel" id="panel-music">
      <div class="card">
        <h2 class="h2">O Bardo da Casa</h2>
        <p class="sub">adicione músicas do YouTube à fila compartilhada da mesa</p>
        <div class="now-playing">
          <div class="embed-wrap" id="embed-wrap">
            <div style="width:100%;height:100%;display:flex;align-items:center;justify-content:center;color:var(--muted);font-size:13px;text-align:center;padding:10px;">O bardo está calado.<br/>Adicione uma música à fila.</div>
          </div>
          <div style="flex:1; min-width:200px;">
            <div id="current-title" style="color:var(--heading); font-family:'Cinzel',serif; font-size:15px; margin-bottom:10px;">Nada tocando agora</div>
            <div class="row" style="margin-bottom:10px;">
              <button class="btn" id="play-next">Tocar próxima</button>
              <button class="btn ghost" id="stop-music">Silenciar</button>
            </div>
            <p class="note">Se a música não iniciar sozinha ao trocar, clique no player — o navegador às vezes bloqueia o som automático.</p>
          </div>
        </div>
        <div class="row" style="margin-bottom:14px;">
          <input type="text" id="music-input" placeholder="Cole o link do YouTube..." />
          <button class="btn" id="music-add">Adicionar à fila</button>
        </div>
        <div id="music-error" style="color:var(--danger); font-size:13px; margin-bottom:10px; display:none;"></div>
        <div id="queue-list"><div class="empty">A fila está vazia.</div></div>
      </div>
    </section>

    <section class="panel" id="panel-tabletop">
      <div class="card">
        <h2 class="h2">A Mesa de Jogo</h2>
        <p class="sub">arraste os marcadores sobre o tabuleiro — todos veem o movimento em tempo real</p>
        <div class="row" style="margin-bottom:14px;">
          <input type="text" id="map-input" placeholder="Cole o link de uma imagem de mapa (opcional)" />
          <button class="btn ghost" id="map-set">Definir mapa</button>
        </div>
        <div class="token-toolbar">
          <input type="text" id="token-label-input" placeholder="Nome do marcador" maxlength="3" style="max-width:110px;" />
          <span id="color-swatches"></span>
          <button class="btn" id="token-add">Adicionar marcador</button>
          <button class="btn ghost" id="token-remove" disabled>Remover selecionado</button>
        </div>
        <div class="board-wrap">
          <div class="board" id="board"></div>
        </div>
        <p class="note">Clique num marcador para selecioná-lo, arraste para mover. A imagem do mapa é apenas um link — hospede a sua em qualquer serviço de imagens.</p>
      </div>
    </section>

    <section class="panel" id="panel-ambience">
      <div class="card">
        <h2 class="h2">Os Ventos da Taverna</h2>
        <p class="sub">escolha o som de fundo da mesa — todos veem a escolha, cada um ajusta seu volume</p>
        <div class="ambience-grid">
          <div class="amb-card" data-amb="floresta"><span class="amb-icon">🌲</span><span class="amb-label">Floresta</span></div>
          <div class="amb-card" data-amb="chuva"><span class="amb-icon">🌧️</span><span class="amb-label">Chuva</span></div>
          <div class="amb-card" data-amb="lareira"><span class="amb-icon">🕯️</span><span class="amb-label">Lareira</span></div>
          <div class="amb-card" data-amb="batalha"><span class="amb-icon">⚔️</span><span class="amb-label">Batalha</span></div>
          <div class="amb-card" data-amb="silencio"><span class="amb-icon">🌙</span><span class="amb-label">Silêncio</span></div>
        </div>
        <div class="volume-row">
          <span style="font-size:13px; color:var(--muted);">Seu volume</span>
          <input type="range" id="amb-volume" min="0" max="100" value="45" />
        </div>
        <p class="note">Sons gerados no seu navegador, sem arquivos externos — funcionam mesmo sem internet para áudio.</p>
      </div>
    </section>

  </main>
</div>

<script>
/* ======================================================================
   1) CONFIGURAÇÃO DO FIREBASE — TROQUE PELOS SEUS DADOS
   Veja o passo a passo que o Claude te enviou para gerar este objeto
   em https://console.firebase.google.com
   ====================================================================== */
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
import { getAnalytics } from "firebase/analytics";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: "AIzaSyBNYS0KGLhUiF128w20vAEYKasQ97g9tKg",
  authDomain: "taverna-do-corvo-rosa.firebaseapp.com",
  projectId: "taverna-do-corvo-rosa",
  storageBucket: "taverna-do-corvo-rosa.firebasestorage.app",
  messagingSenderId: "526970799571",
  appId: "1:526970799571:web:cf46d52e5b94913d035b26",
  measurementId: "G-LCNX57DY96"
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
const analytics = getAnalytics(app);

(function(){
  let currentUser = null;
  let sessionId = Math.random().toString(36).slice(2,9);
  let lastMusicVideoId = undefined;
  let lastAmbienceType = undefined;
  let selectedToken = null;
  let selectedColor = '#c85f82';

  function esc(s){ const d=document.createElement('div'); d.textContent = String(s==null?'':s); return d.innerHTML; }
  function fmtTime(ts){ const d = new Date(ts); return d.toLocaleTimeString('pt-BR', {hour:'2-digit', minute:'2-digit'}); }

  document.getElementById('join-btn').addEventListener('click', joinTavern);
  document.getElementById('name-input').addEventListener('keydown', e=>{ if(e.key==='Enter') joinTavern(); });

  function joinTavern(){
    const input = document.getElementById('name-input');
    const name = input.value.trim();
    if(!name){ input.style.borderColor = 'var(--danger)'; input.placeholder='Diga um nome, viajante...'; return; }
    currentUser = name.slice(0,24);
    document.getElementById('join-overlay').style.display = 'none';
    registerPresence();
    startListeners();
  }

  function registerPresence(){
    const ref = db.ref('presence/'+encodeURIComponent(currentUser)+'_'+sessionId);
    ref.set({ name: currentUser, time: firebase.database.ServerValue.TIMESTAMP });
    ref.onDisconnect().remove();
    db.ref('presence').on('value', snap=>{
      const val = snap.val() || {};
      const names = [...new Set(Object.values(val).map(p=>p.name))];
      const el = document.getElementById('presence-list');
      el.innerHTML = names.length ? names.map(n=>'<div>• '+esc(n)+'</div>').join('') : 'ninguém ainda...';
    });
  }

  document.querySelectorAll('.navbtn').forEach(btn=>{
    btn.addEventListener('click', function(){
      document.querySelectorAll('.navbtn').forEach(b=>b.classList.remove('active'));
      document.querySelectorAll('.panel').forEach(p=>p.classList.remove('active'));
      btn.classList.add('active');
      document.getElementById('panel-'+btn.dataset.tab).classList.add('active');
    });
  });

  /* ---------- CHAT ---------- */
  function renderChat(items){
    const log = document.getElementById('chatlog');
    if(!items.length){ log.innerHTML = '<div class="empty">O salão está em silêncio. Diga algo!</div>'; return; }
    const wasAtBottom = (log.scrollTop + log.clientHeight) >= (log.scrollHeight - 30);
    log.innerHTML = items.map(m=>'<div class="msg"><span class="name">'+esc(m.name)+'</span><span class="time">'+fmtTime(m.time)+'</span><span class="text">'+esc(m.text)+'</span></div>').join('');
    if(wasAtBottom) log.scrollTop = log.scrollHeight;
  }

  document.getElementById('chat-send').addEventListener('click', sendChat);
  document.getElementById('chat-input').addEventListener('keydown', e=>{ if(e.key==='Enter') sendChat(); });
  function sendChat(){
    const input = document.getElementById('chat-input');
    const text = input.value.trim();
    if(!text || !currentUser) return;
    input.value='';
    db.ref('chat').push({ name: currentUser, text: text, time: Date.now() });
  }

  /* ---------- DADOS ---------- */
  function renderDice(items){
    const log = document.getElementById('dicelog');
    if(!items.length){ log.innerHTML = '<div class="empty">Nenhum dado rolado ainda. A sorte aguarda.</div>'; return; }
    log.innerHTML = items.slice().reverse().map(r=>
      '<div class="roll-card"><div class="roll-total">'+r.total+'</div><div class="roll-detail"><span class="name">'+esc(r.name)+'</span> rolou <strong>'+esc(r.formula)+'</strong> &nbsp; ['+r.rolls.join(', ')+(r.mod?(r.mod>0?' +'+r.mod:' '+r.mod):'')+'] &nbsp;<span style="color:var(--muted);">'+fmtTime(r.time)+'</span></div></div>'
    ).join('');
  }

  function registerRoll(formula, rolls, mod, total){
    db.ref('dice').push({ name: currentUser, formula, rolls, mod, total, time: Date.now() });
  }

  document.querySelectorAll('.die').forEach(die=>{
    die.addEventListener('click', function(){
      if(!currentUser) return;
      const sides = parseInt(die.dataset.sides);
      const roll = 1+Math.floor(Math.random()*sides);
      registerRoll('1d'+sides, [roll], 0, roll);
    });
  });

  document.getElementById('formula-roll').addEventListener('click', rollFormulaHandler);
  document.getElementById('formula-input').addEventListener('keydown', e=>{ if(e.key==='Enter') rollFormulaHandler(); });
  function rollFormulaHandler(){
    if(!currentUser) return;
    const input = document.getElementById('formula-input');
    const errEl = document.getElementById('formula-error');
    const formula = input.value.trim();
    errEl.style.display='none';
    if(!formula) return;
    const m = formula.match(/^(\d*)d(\d+)\s*([+-]\s*\d+)?$/i);
    if(!m){ errEl.textContent = 'Fórmula inválida. Use algo como 2d6+3 ou d20.'; errEl.style.display='block'; return; }
    const count = m[1] ? parseInt(m[1]) : 1;
    const sides = parseInt(m[2]);
    const mod = m[3] ? parseInt(m[3].replace(/\s+/g,'')) : 0;
    if(count<1 || count>100 || sides<2 || sides>1000){ errEl.textContent = 'Use entre 1 e 100 dados, com 2 a 1000 lados.'; errEl.style.display='block'; return; }
    const rolls=[]; for(let i=0;i<count;i++) rolls.push(1+Math.floor(Math.random()*sides));
    const total = rolls.reduce((a,b)=>a+b,0)+mod;
    input.value='';
    registerRoll(formula, rolls, mod, total);
  }

  /* ---------- MÚSICA ---------- */
  function extractYouTubeId(url){
    url = url.trim();
    const m = url.match(/(?:youtube\.com\/watch\?v=|youtu\.be\/|youtube\.com\/embed\/|youtube\.com\/shorts\/)([a-zA-Z0-9_-]{11})/);
    if(m) return m[1];
    if(/^[a-zA-Z0-9_-]{11}$/.test(url)) return url;
    return null;
  }

  document.getElementById('music-add').addEventListener('click', addMusic);
  document.getElementById('music-input').addEventListener('keydown', e=>{ if(e.key==='Enter') addMusic(); });
  function addMusic(){
    if(!currentUser) return;
    const input = document.getElementById('music-input');
    const errEl = document.getElementById('music-error');
    const videoId = extractYouTubeId(input.value.trim());
    errEl.style.display='none';
    if(!videoId){ errEl.textContent = 'Não encontrei um link válido do YouTube.'; errEl.style.display='block'; return; }
    input.value='';
    db.ref('music/queue').push({ videoId, addedBy: currentUser });
  }

  function renderQueue(queueObj){
    const el = document.getElementById('queue-list');
    const entries = Object.entries(queueObj || {});
    if(!entries.length){ el.innerHTML = '<div class="empty">A fila está vazia.</div>'; return; }
    el.innerHTML = entries.map(([key,q])=>'<div class="queue-item"><span>youtube.com/watch?v='+esc(q.videoId)+'</span><span class="by">adicionado por '+esc(q.addedBy)+'</span></div>').join('');
  }

  function renderCurrent(current){
    const wrap = document.getElementById('embed-wrap');
    const title = document.getElementById('current-title');
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

  document.getElementById('play-next').addEventListener('click', function(){
    if(!currentUser) return;
    db.ref('music/queue').limitToFirst(1).once('value', snap=>{
      const val = snap.val();
      if(!val) return;
      const key = Object.keys(val)[0];
      const item = val[key];
      db.ref('music/queue/'+key).remove();
      db.ref('music/current').set({ videoId: item.videoId, by: currentUser, time: Date.now() });
    });
  });

  document.getElementById('stop-music').addEventListener('click', function(){
    if(!currentUser) return;
    db.ref('music/current').set({ videoId: null });
  });

  /* ---------- AMBIENTE (WebAudio, local, sincronizado por tipo) ---------- */
  let audioCtx=null, ambNodes=[], ambTimers=[], masterGain=null;
  function ensureAudio(){
    if(!audioCtx){
      audioCtx = new (window.AudioContext||window.webkitAudioContext)();
      masterGain = audioCtx.createGain();
      masterGain.gain.value = parseInt(document.getElementById('amb-volume').value)/100*0.5;
      masterGain.connect(audioCtx.destination);
    }
    if(audioCtx.state==='suspended') audioCtx.resume();
  }
  function makeNoiseBuffer(duration){
    const bufferSize = audioCtx.sampleRate*duration;
    const buffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
    const data = buffer.getChannelData(0);
    for(let i=0;i<bufferSize;i++){ data[i]=Math.random()*2-1; }
    return buffer;
  }
  function stopAmbience(){ ambTimers.forEach(t=>clearInterval(t)); ambTimers=[]; ambNodes.forEach(n=>{ try{ n.stop&&n.stop(); n.disconnect&&n.disconnect(); }catch(e){} }); ambNodes=[]; }
  function loopedNoise(filterType, freq, gainVal){
    ensureAudio();
    const src = audioCtx.createBufferSource(); src.buffer = makeNoiseBuffer(4); src.loop = true;
    const filter = audioCtx.createBiquadFilter(); filter.type=filterType; filter.frequency.value=freq;
    const gain = audioCtx.createGain(); gain.gain.value=gainVal;
    src.connect(filter); filter.connect(gain); gain.connect(masterGain); src.start();
    ambNodes.push(src, filter, gain);
  }
  function burst(freq, dur, gainPeak, type){
    if(!audioCtx) return;
    const src = audioCtx.createBufferSource(); src.buffer = makeNoiseBuffer(dur);
    const filter = audioCtx.createBiquadFilter(); filter.type=type||'bandpass'; filter.frequency.value=freq;
    const gain = audioCtx.createGain();
    gain.gain.setValueAtTime(0, audioCtx.currentTime);
    gain.gain.linearRampToValueAtTime(gainPeak, audioCtx.currentTime+0.02);
    gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime+dur);
    src.connect(filter); filter.connect(gain); gain.connect(masterGain);
    src.start(); src.stop(audioCtx.currentTime+dur+0.05);
  }
  function tone(freq, dur, gainPeak, waveType){
    if(!audioCtx) return;
    const osc = audioCtx.createOscillator(); osc.type=waveType||'sine'; osc.frequency.value=freq;
    const gain = audioCtx.createGain();
    gain.gain.setValueAtTime(0, audioCtx.currentTime);
    gain.gain.linearRampToValueAtTime(gainPeak, audioCtx.currentTime+0.03);
    gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime+dur);
    osc.connect(gain); gain.connect(masterGain); osc.start(); osc.stop(audioCtx.currentTime+dur+0.05);
  }
  function startAmbience(type){
    stopAmbience();
    if(type==='silencio'||!type) return;
    ensureAudio();
    if(type==='floresta'){ loopedNoise('lowpass',700,0.35); ambTimers.push(setInterval(()=>tone(1500+Math.random()*1500,0.15,0.15,'sine'), 2200+Math.random()*3000)); }
    else if(type==='chuva'){ loopedNoise('lowpass',2200,0.4); loopedNoise('highpass',3000,0.08); }
    else if(type==='lareira'){ loopedNoise('lowpass',500,0.18); ambTimers.push(setInterval(()=>burst(800+Math.random()*1200,0.08,0.25,'bandpass'), 250+Math.random()*400)); }
    else if(type==='batalha'){ ambTimers.push(setInterval(()=>burst(120,0.18,0.5,'lowpass'), 550+Math.random()*300)); ambTimers.push(setInterval(()=>tone(1800+Math.random()*800,0.12,0.2,'square'), 1800+Math.random()*2400)); }
  }
  document.querySelectorAll('.amb-card').forEach(card=>{
    card.addEventListener('click', function(){
      if(!currentUser) return;
      const type = card.dataset.amb;
      db.ref('ambience/current').set({ type, by: currentUser, time: Date.now() });
    });
  });
  document.getElementById('amb-volume').addEventListener('input', function(){ if(masterGain) masterGain.gain.value = parseInt(this.value)/100*0.5; });

  /* ---------- TABLETOP ---------- */
  const tokenColors = ['#c85f82','#8a5ec8','#5ec8a3','#c8a15e','#5e8fc8','#c85e5e'];
  const swatchEl = document.getElementById('color-swatches');
  tokenColors.forEach((c,i)=>{
    const s = document.createElement('span');
    s.className = 'color-swatch'+(i===0?' selected':'');
    s.style.background = c;
    s.addEventListener('click', ()=>{ selectedColor=c; document.querySelectorAll('.color-swatch').forEach(el=>el.classList.remove('selected')); s.classList.add('selected'); });
    swatchEl.appendChild(s);
  });

  document.getElementById('map-set').addEventListener('click', function(){
    if(!currentUser) return;
    const url = document.getElementById('map-input').value.trim();
    db.ref('tabletop/map').set({ url: url || null });
  });

  document.getElementById('token-add').addEventListener('click', function(){
    if(!currentUser) return;
    const label = (document.getElementById('token-label-input').value.trim() || currentUser.slice(0,3)).toUpperCase();
    db.ref('tabletop/tokens').push({ x: 50, y: 50, color: selectedColor, label, owner: currentUser });
  });

  document.getElementById('token-remove').addEventListener('click', function(){
    if(!selectedToken) return;
    db.ref('tabletop/tokens/'+selectedToken).remove();
    selectedToken = null;
    document.getElementById('token-remove').disabled = true;
  });

  db.ref('tabletop/map').on('value', snap=>{
    const val = snap.val();
    const board = document.getElementById('board');
    if(val && val.url){ board.style.backgroundImage = 'url("'+val.url.replace(/"/g,'')+'")'; }
    else { board.style.backgroundImage = 'none'; }
  });

  const boardEl = document.getElementById('board');
  function renderTokens(tokensObj){
    const existingKeys = new Set(Array.from(boardEl.querySelectorAll('.token')).map(t=>t.dataset.key));
    const incomingKeys = new Set(Object.keys(tokensObj||{}));
    existingKeys.forEach(k=>{ if(!incomingKeys.has(k)) boardEl.querySelector('[data-key="'+k+'"]').remove(); });
    Object.entries(tokensObj||{}).forEach(([key, t])=>{
      let el = boardEl.querySelector('[data-key="'+key+'"]');
      if(!el){
        el = document.createElement('div');
        el.className = 'token';
        el.dataset.key = key;
        attachDrag(el, key);
        boardEl.appendChild(el);
      }
      if(el.dataset.dragging !== '1'){
        el.style.left = t.x+'%'; el.style.top = t.y+'%';
      }
      el.style.background = t.color || '#c85f82';
      el.textContent = t.label || '?';
      el.title = t.owner ? ('marcador de '+t.owner) : '';
    });
  }

  db.ref('tabletop/tokens').on('value', snap=>{ renderTokens(snap.val()); });

  function attachDrag(el, key){
    el.addEventListener('pointerdown', function(e){
      e.preventDefault();
      document.querySelectorAll('.token').forEach(t=>t.classList.remove('selected'));
      el.classList.add('selected');
      selectedToken = key;
      document.getElementById('token-remove').disabled = false;
      el.dataset.dragging = '1';
      el.setPointerCapture(e.pointerId);
      function onMove(ev){
        const rect = boardEl.getBoundingClientRect();
        let x = ((ev.clientX - rect.left) / rect.width) * 100;
        let y = ((ev.clientY - rect.top) / rect.height) * 100;
        x = Math.max(0, Math.min(100, x));
        y = Math.max(0, Math.min(100, y));
        el.style.left = x+'%'; el.style.top = y+'%';
        el.dataset.x = x; el.dataset.y = y;
      }
      function onUp(){
        el.removeEventListener('pointermove', onMove);
        el.removeEventListener('pointerup', onUp);
        el.dataset.dragging = '0';
        const x = parseFloat(el.dataset.x); const y = parseFloat(el.dataset.y);
        if(!isNaN(x) && !isNaN(y)) db.ref('tabletop/tokens/'+key).update({ x, y });
      }
      el.addEventListener('pointermove', onMove);
      el.addEventListener('pointerup', onUp);
    });
  }

  /* ---------- INICIAR LISTENERS APÓS ENTRAR ---------- */
  function startListeners(){
    db.ref('chat').orderByChild('time').limitToLast(200).on('value', snap=>{
      const val = snap.val() || {};
      renderChat(Object.values(val).sort((a,b)=>a.time-b.time));
    });
    db.ref('dice').orderByChild('time').limitToLast(100).on('value', snap=>{
      const val = snap.val() || {};
      renderDice(Object.values(val).sort((a,b)=>a.time-b.time));
    });
    db.ref('music/queue').on('value', snap=>{ renderQueue(snap.val()); });
    db.ref('music/current').on('value', snap=>{ renderCurrent(snap.val()); });
    db.ref('ambience/current').on('value', snap=>{
      const current = snap.val();
      const type = current ? current.type : 'silencio';
      if(type === lastAmbienceType) return;
      lastAmbienceType = type;
      document.querySelectorAll('.amb-card').forEach(c=>c.classList.toggle('active', c.dataset.amb===type));
      startAmbience(type);
    });
  }
})();
</script>
</body>
</html>
