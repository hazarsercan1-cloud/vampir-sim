<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Mini Vampir Sim</title>
  <title>Neon Orbit - Tek Dosya Mini Oyun</title>
  <style>
    html, body { height: 100%; margin: 0; background:#0b0b10; color:#e8e8f0; font-family: system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, Cantarell, 'Helvetica Neue', Arial, 'Noto Sans', 'Apple Color Emoji', 'Segoe UI Emoji'; }
    #wrap { display:flex; gap:12px; padding:12px; box-sizing:border-box; }
    #hud { width: 280px; max-width: 35vw; }
    #card { background:#12131a; border:1px solid #1f2230; border-radius:16px; padding:12px; box-shadow: 0 6px 20px rgba(0,0,0,.35); }
    h1 { font-size:18px; margin:6px 0 10px; }
    .muted { color:#b8b8c8; font-size:12px; line-height:1.4; }
    .row { display:flex; align-items:center; justify-content:space-between; margin:6px 0; font-size:14px; }
    .bar { height:10px; background:#232637; border-radius:999px; overflow:hidden; border:1px solid #2d3147; }
    .fill { height:100%; background:linear-gradient(90deg,#ff3b3b,#a8002b); }
    button { width:100%; margin-top:8px; background:#1b1e2c; color:#e8e8f0; border:1px solid #2a2e45; border-radius:12px; padding:10px 12px; cursor:pointer; }
    button:hover { filter:brightness(1.1); }
    canvas { flex:1; border-radius:14px; background:#0f1119; border:1px solid #1c2033; box-shadow: inset 0 0 120px rgba(0,0,0,.4); }
    .small { font-size:11px; color:#9aa0b3; }
    :root {
      --bg: radial-gradient(circle at 20% 20%, #1f2942, #0a0b15 50%);
      --panel: rgba(8, 10, 20, 0.75);
      --stroke: #24305b;
      --glow: #6cf0ff;
    }
    * { box-sizing: border-box; }
    html, body { margin:0; height:100%; background:#070811; color:#e7f6ff; font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; }
    body { display:flex; align-items:center; justify-content:center; padding:18px; background:var(--bg); }
    #shell { width:min(1080px, 100%); }
    header { display:flex; align-items:center; gap:10px; margin-bottom:10px; }
    header h1 { margin:0; font-size:22px; letter-spacing:0.4px; }
    header small { color:#8da0b8; }
    #layout { display:grid; grid-template-columns:280px 1fr; gap:14px; }
    #panel { background:var(--panel); border:1px solid var(--stroke); border-radius:16px; padding:14px; backdrop-filter: blur(6px); box-shadow:0 10px 40px rgba(0,0,0,0.45); }
    #panel h2 { margin:0 0 4px; font-size:16px; }
    #panel .muted { color:#a2b4ce; font-size:13px; line-height:1.5; margin-bottom:10px; }
    #panel button { width:100%; padding:11px 12px; border-radius:12px; border:1px solid #3b4d79; background:#111729; color:#e7f6ff; font-weight:600; cursor:pointer; transition:transform .05s ease, filter .1s ease; }
    #panel button:hover { filter:brightness(1.05); }
    #panel button:active { transform:translateY(1px); }
    .stat { display:flex; align-items:center; justify-content:space-between; padding:7px 0; border-bottom:1px solid #111626; font-size:14px; }
    canvas { width:100%; height:620px; border-radius:18px; border:1px solid var(--stroke); background:#080a15; box-shadow:0 16px 50px rgba(0,0,0,0.35), inset 0 0 120px rgba(0,0,0,0.45); }
    @media(max-width:960px){ #layout { grid-template-columns:1fr; } canvas { height:66vh; } }
  </style>
</head>
<body>
  <div id="wrap">
    <div id="hud">
      <div id="card">
        <h1>🧛 Mini Vampir Sim</h1>
        <div class="muted">Küçük bir tek-dosya oyun. Gece insanlardan <b>kan</b> topla, gündüz <b>gölge</b>lerde saklan. <b>3 gün</b> hayatta kal!</div>
        <div class="row"><span>Gün:</span><b id="day">1</b></div>
        <div class="row"><span>Vakit:</span><b id="time">Gece</b></div>
        <div class="row"><span>Kan:</span><span style="width:58%" class="bar"><span id="bloodFill" class="fill" style="width:80%"></span></span><b id="blood">80</b></div>
        <div class="row"><span>İnsanlar:</span><b id="humans">0</b></div>
        <div class="muted" style="margin-top:8px">
          <b>Kontroller</b><br/>
          WASD/Hareket · <b>E</b>: Isır · <b>Boşluk</b>: Sıçra (kan harcar)
        </div>
  <div id="shell">
    <header>
      <h1>🌌 Neon Orbit</h1>
      <small>Tek dosya, kolay dağıtım • WASD / Ok tuşları + Space (Vuruş)</small>
    </header>
    <div id="layout">
      <div id="panel">
        <h2>Hızlı Özet</h2>
        <div class="muted">Parlak enerji kürelerini topla, neon engellerden kaç. 90 saniye içinde <b>25 enerji</b> yakala ve kazan. Tek HTML dosya, anında yayınla.</div>
        <div class="stat"><span>Skor</span><b id="score">0</b></div>
        <div class="stat"><span>Enerji</span><b id="energy">50</b></div>
        <div class="stat"><span>Güç Çubuğu</span><b id="power">0%</b></div>
        <div class="stat"><span>Kalan Süre</span><b id="time">90.0s</b></div>
        <div class="stat"><span>Durum</span><b id="state">Hazır</b></div>
        <button id="restart">Yeniden Başlat</button>
        <div class="small" style="margin-top:6px">"Vampire Survivors" gibi değil; mini bir <i>vampir rol</i> prototipi.</div>
        <div class="muted" style="margin-top:10px;">Kırmızı halkalara çarparsın = oyun biter. Enerji %60+ olduğunda <b>Space</b> ile hız patlaması yapıp iz bırak.</div>
      </div>
      <canvas id="game" width="980" height="620"></canvas>
    </div>
    <canvas id="game" width="900" height="600"></canvas>
  </div>

  <script>
  // ---- Mini Vampir Sim - tek dosya, ~250 satır ----
  const cvs = document.getElementById('game');
  const ctx = cvs.getContext('2d');
  const ui = {
    day: document.getElementById('day'),
    time: document.getElementById('time'),
    blood: document.getElementById('blood'),
    bloodFill: document.getElementById('bloodFill'),
    humans: document.getElementById('humans'),
    restart: document.getElementById('restart'),
  };

  // World settings
  const W = cvs.width, H = cvs.height;
  const DAY_SEC = 40; // bir gün: 40 sn (20 sn gece + 20 sn gündüz)
  const NIGHT_RATIO = 0.55; // günün %55'i gece
  const TARGET_DAYS = 3; // kazanma koşulu
  const RNG = (a,b)=>Math.random()*(b-a)+a;
    // Neon Orbit - tek dosya, asset'siz mini arcade
    const cvs = document.getElementById('game');
    const ctx = cvs.getContext('2d');
    const ui = {
      score: document.getElementById('score'),
      energy: document.getElementById('energy'),
      power: document.getElementById('power'),
      time: document.getElementById('time'),
      state: document.getElementById('state'),
      restart: document.getElementById('restart')
    };

  const state = {
    t: 0,
    day: 1,
    isNight: true,
    blood: 80,
    alive: true,
    win: false,
    player: { x: W*0.5, y: H*0.5, r: 10, vx:0, vy:0, speed: 150, inv:0 },
    humans: [],
    shadows: [],
    bites: 0,
  };
    const keys = {};
    window.addEventListener('keydown', e=> keys[e.key.toLowerCase()] = true);
    window.addEventListener('keyup', e=> keys[e.key.toLowerCase()] = false);

  function reset() {
    state.t = 0; state.day = 1; state.isNight = true; state.blood = 80; state.alive = true; state.win=false; state.bites=0;
    state.player.x = W*0.5; state.player.y = H*0.5; state.player.vx=0; state.player.vy=0; state.player.inv=0;
    // Gölge bölgeleri (ağaç kümeleri/evler gibi)
    state.shadows = [
      {x:80,y:80,w:180,h:90},
      {x:650,y:70,w:180,h:120},
      {x:120,y:420,w:220,h:110},
      {x:560,y:360,w:260,h:160},
    ];
    // İnsanlar
    state.humans = [];
    for (let i=0;i<10;i++) state.humans.push(makeHuman());
    updateUI();
  }
    const W = cvs.width, H = cvs.height;
    const clamp = (v,a,b)=>Math.max(a, Math.min(b, v));
    const dist = (a,b)=>Math.hypot(a.x-b.x, a.y-b.y);

  function makeHuman(){
    const x = RNG(40,W-40), y = RNG(40,H-40);
    const speed = RNG(35,55);
    return { x, y, r:8, dir:RNG(0,Math.PI*2), speed, panic:0, alive:true, cooldown:0 };
  }
    const state = {
      t: 0,
      timer: 90,
      target: 25,
      alive: true,
      started: false,
      score: 0,
      energy: 50,
      charge: 0,
      player: { x: W*0.5, y: H*0.55, r: 12, speed: 190, vx:0, vy:0, trail: [] },
      orbs: [],
      hazards: [],
      spark: 0
    };

  // Input
  const keys = {};
  window.addEventListener('keydown', e=>{ keys[e.key.toLowerCase()] = true; });
  window.addEventListener('keyup', e=>{ keys[e.key.toLowerCase()] = false; });
    function reset(){
      state.t = 0; state.timer = 90; state.score = 0; state.energy = 50; state.charge = 0; state.alive = true; state.started=false; state.spark=0;
      state.player.x = W*0.5; state.player.y = H*0.55; state.player.vx=0; state.player.vy=0; state.player.trail=[];
      state.orbs = []; state.hazards=[];
      for(let i=0;i<8;i++) spawnOrb();
      for(let i=0;i<6;i++) spawnHazard();
      updateUI();
    }

  ui.restart.onclick = ()=> reset();
    function spawnOrb(){
      const padding = 40;
      const orb = { x: Math.random()*(W-2*padding)+padding, y: Math.random()*(H-2*padding)+padding, r: 9, pulse: Math.random()*Math.PI*2 };
      state.orbs.push(orb);
    }

  // Helpers
  const clamp=(v,a,b)=>Math.max(a,Math.min(b,v));
  const dist=(a,b)=>Math.hypot(a.x-b.x,a.y-b.y);
  function inShadow(x,y){
    for(const s of state.shadows){ if(x>s.x && x<s.x+s.w && y>s.y && y<s.y+s.h) return true; }
    return false;
  }
    function spawnHazard(){
      const speed = 60 + Math.random()*80;
      const angle = Math.random()*Math.PI*2;
      const r = 10 + Math.random()*18;
      const x = Math.random()<0.5 ? Math.random()*W : (Math.random()<0.5?0:W);
      const y = Math.random()<0.5 ? Math.random()*H : (Math.random()<0.5?0:H);
      state.hazards.push({ x, y, r, vx: Math.cos(angle)*speed, vy: Math.sin(angle)*speed, spin: (Math.random()*1.2)+0.2 });
    }

  function step(dt){
    if(!state.alive || state.win) return;
    ui.restart.onclick = reset;

    // ----- Zaman & gece/gündüz -----
    state.t += dt;
    const cycle = state.t % DAY_SEC; // 0..DAY_SEC
    const nightDur = DAY_SEC * NIGHT_RATIO;
    state.isNight = cycle < nightDur;
    if (state.t >= state.day*DAY_SEC) { // yeni gün
      state.day++;
      if(state.day>TARGET_DAYS){ state.win=true; }
    function updateUI(){
      ui.score.textContent = state.score;
      ui.energy.textContent = Math.round(state.energy);
      ui.power.textContent = Math.round(state.charge*100)+'%';
      ui.time.textContent = state.timer.toFixed(1)+'s';
      ui.state.textContent = state.alive ? (state.started? 'Oyunda':'Hazır') : (state.score>=state.target? 'Kazandın!':'Kaybettin');
    }

    // ----- Kan tüketimi -----
    const baseDrain = state.isNight ? 2.0 : 3.0; // saniyede
    const sunPenalty = (!state.isNight && !inShadow(state.player.x,state.player.y)) ? 8.0 : 0.0;
    state.blood -= (baseDrain + sunPenalty) * dt;
    if(state.player.inv>0) state.player.inv -= dt;
    function update(dt){
      if(!state.alive) return;
      if(!state.started && (keys['w']||keys['a']||keys['s']||keys['d']||keys['arrowup']||keys['arrowleft']||keys['arrowdown']||keys['arrowright'])){
        state.started=true;
      }
      if(!state.started) return;

    // ----- Hareket -----
    const p = state.player;
    let ax = 0, ay = 0;
    if(keys['w']) ay -= 1;
    if(keys['s']) ay += 1;
    if(keys['a']) ax -= 1;
    if(keys['d']) ax += 1;
    if(ax||ay){ const m = Math.hypot(ax,ay); ax/=m; ay/=m; }
    p.vx = ax * p.speed; p.vy = ay * p.speed;
      state.t += dt;
      state.timer = Math.max(0, state.timer - dt);
      if(state.timer<=0){ state.alive=false; }

    // Sıçra (dash) - Space
    if(keys[' '] && state.blood>10){
      const dashV = 420; p.x += ax * dashV * dt; p.y += ay * dashV * dt; state.blood -= 18*dt; // basılı tutma maliyeti
    }
      const p = state.player;
      let ax=0, ay=0;
      if(keys['w']||keys['arrowup']) ay -= 1;
      if(keys['s']||keys['arrowdown']) ay += 1;
      if(keys['a']||keys['arrowleft']) ax -= 1;
      if(keys['d']||keys['arrowright']) ax += 1;
      if(ax||ay){ const m=Math.hypot(ax,ay); ax/=m; ay/=m; }
      const boost = (keys[' '] && state.energy>=60) ? 1.8 : 1;
      if(boost>1){ state.energy = Math.max(0, state.energy - 25*dt); state.charge = Math.min(1, state.charge + 0.8*dt); state.spark = 0.4; }
      else state.charge = Math.max(0, state.charge - 0.3*dt);
      p.vx = ax * p.speed * boost; p.vy = ay * p.speed * boost;
      p.x = clamp(p.x + p.vx*dt, p.r, W-p.r);
      p.y = clamp(p.y + p.vy*dt, p.r, H-p.r);

    p.x = clamp(p.x + p.vx*dt, 10, W-10);
    p.y = clamp(p.y + p.vy*dt, 10, H-10);
      state.energy = clamp(state.energy + 6*dt, 0, 100);

    // ----- İnsanlar AI -----
    for(const h of state.humans){
      if(!h.alive) continue;
      // panik: gece vampir yakınsa kaçar, gündüz gölgeye kaçar
      let targetDir = h.dir;
      if(state.isNight && dist(h, p) < 120){
        targetDir = Math.atan2(h.y - p.y, h.x - p.x); // kaçış
        h.panic = 1.0;
      } else if(!state.isNight && !inShadow(h.x,h.y)){
        // en yakın gölge merkezine yönel
        let best = null, bd = 1e9;
        for(const s of state.shadows){
          const cx = s.x+s.w/2, cy=s.y+s.h/2; const d=Math.hypot(cx-h.x, cy-h.y);
          if(d<bd){ bd=d; best={cx,cy}; }
        }
        if(best) targetDir = Math.atan2(best.cy - h.y, best.cx - h.x);
        h.panic = 0.4;
      } else {
        h.panic = Math.max(0, h.panic - dt*0.5);
        // hafif dolaşma
        if(Math.random()<0.01) targetDir += RNG(-0.7,0.7);
      // trail
      p.trail.unshift({x:p.x, y:p.y, life:0.8, boost:boost>1});
      if(p.trail.length>50) p.trail.pop();
      for(const t of p.trail){ t.life -= dt; }
      p.trail = p.trail.filter(t=>t.life>0);

      // hazards
      for(const h of state.hazards){
        h.x += h.vx*dt; h.y += h.vy*dt;
        if(h.x < -h.r) h.x = W+h.r;
        if(h.x > W+h.r) h.x = -h.r;
        if(h.y < -h.r) h.y = H+h.r;
        if(h.y > H+h.r) h.y = -h.r;
        h.spin += dt*3;
        if(dist(h,p) < h.r + p.r-2){ state.alive=false; }
      }
      // yumuşak dönüş
      const dAng = ((targetDir - h.dir + Math.PI*3)%(Math.PI*2))-Math.PI;
      h.dir += clamp(dAng, -1.5*dt, 1.5*dt);

      // hız
      const spd = h.speed * (0.7 + h.panic*0.8);
      h.x = clamp(h.x + Math.cos(h.dir)*spd*dt, 8, W-8);
      h.y = clamp(h.y + Math.sin(h.dir)*spd*dt, 8, H-8);
      // orbs
      for(const o of state.orbs){
        o.pulse += dt*2.5;
        if(dist(o,p) < o.r + p.r){
          state.score++; state.energy = clamp(state.energy+20,0,100);
          state.spark = 0.6;
          o.x = Math.random()*W; o.y = Math.random()*H; o.pulse = Math.random()*Math.PI*2;
          if(Math.random()<0.35) spawnHazard();
        }
      }

      if(h.cooldown>0) h.cooldown -= dt;
      if(state.score >= state.target){ state.alive=false; }
    }

    // ----- Isırma (E) sadece gece -----
    if(keys['e'] && state.isNight){
      for(const h of state.humans){
        if(!h.alive || h.cooldown>0) continue;
        if(dist(h, state.player) < 22){
          h.alive = false; state.blood = Math.min(100, state.blood + 35); state.bites++; break;
        }
      }
    function drawBackground(){
      const g = ctx.createLinearGradient(0,0,W,H);
      g.addColorStop(0,'#0d1024'); g.addColorStop(0.5,'#0a0b15'); g.addColorStop(1,'#0e162b');
      ctx.fillStyle = g; ctx.fillRect(0,0,W,H);
      // stars
      ctx.fillStyle = 'rgba(255,255,255,0.15)';
      for(let i=0;i<60;i++) ctx.fillRect((i*37+state.t*10)%W, (i*83)%H, 1.5, 1.5);
    }

    // ----- Ölüm kontrolleri -----
    if(state.blood <= 0){ state.alive=false; }
    function draw(){
      drawBackground();

    updateUI();
  }
      // orbs
      for(const o of state.orbs){
        const pulse = Math.sin(o.pulse)*0.4+0.6;
        const r = o.r * (1+0.08*Math.sin(o.pulse*2));
        ctx.beginPath();
        ctx.fillStyle = `rgba(92, 235, 255, ${0.35 + pulse*0.2})`;
        ctx.arc(o.x, o.y, r+10, 0, Math.PI*2); ctx.fill();
        ctx.beginPath();
        ctx.fillStyle = '#6cf0ff';
        ctx.arc(o.x, o.y, r, 0, Math.PI*2); ctx.fill();
      }

  function updateUI(){
    ui.day.textContent = state.day;
    ui.time.textContent = state.isNight ? 'Gece' : 'Gündüz';
    ui.blood.textContent = Math.max(0, state.blood|0);
    ui.bloodFill.style.width = clamp(state.blood,0,100) + '%';
    ui.humans.textContent = state.humans.filter(h=>h.alive).length;
  }
      // hazards
      for(const h of state.hazards){
        ctx.save();
        ctx.translate(h.x,h.y); ctx.rotate(h.spin);
        ctx.strokeStyle = 'rgba(255,66,95,0.9)';
        ctx.lineWidth = 3; ctx.beginPath();
        for(let i=0;i<6;i++){ const ang=i/6*Math.PI*2; const r=h.r*(1+0.08*Math.sin(h.spin*2+i)); ctx.lineTo(Math.cos(ang)*r, Math.sin(ang)*r); }
        ctx.closePath(); ctx.stroke();
        ctx.restore();
      }

  function draw(){
    // Arkaplan (dairesel hafif vinyet)
    ctx.clearRect(0,0,W,H);
    ctx.fillStyle = state.isNight ? '#0d0f18' : '#1a1f2d';
    ctx.fillRect(0,0,W,H);
      const p = state.player;
      // trail
      for(const t of p.trail){
        ctx.beginPath();
        ctx.fillStyle = `rgba(${t.boost?'108,240,255':'120,126,255'}, ${0.2+t.life})`;
        ctx.arc(t.x, t.y, 6*t.life, 0, Math.PI*2); ctx.fill();
      }

    // Güneş/ay ışığı
    const cycle = (state.t % DAY_SEC)/DAY_SEC; // 0..1
    const light = state.isNight ? 0.15 : 0.55;
    ctx.globalAlpha = light; ctx.fillStyle = '#ffe69a';
    const sunX = W * cycle, sunY = 60 + Math.sin(cycle*Math.PI)*20;
    if(!state.isNight){ ctx.beginPath(); ctx.arc(sunX, sunY, 26, 0, Math.PI*2); ctx.fill(); }
    ctx.globalAlpha = 1;
      // player
      const grd = ctx.createRadialGradient(p.x-4,p.y-4,4,p.x,p.y,p.r*1.6);
      grd.addColorStop(0,'#6cf0ff'); grd.addColorStop(1,'#4b6bff');
      ctx.beginPath(); ctx.fillStyle = grd; ctx.arc(p.x, p.y, p.r, 0, Math.PI*2); ctx.fill();
      ctx.strokeStyle = '#baf4ff'; ctx.lineWidth = 2; ctx.stroke();
      ctx.beginPath(); ctx.strokeStyle = 'rgba(255,255,255,0.2)'; ctx.arc(p.x,p.y,p.r+6,0,Math.PI*2); ctx.stroke();

    // Gölge bölgeleri
    for(const s of state.shadows){
      ctx.fillStyle = state.isNight ? '#0b0d14' : '#0b0d14';
      ctx.strokeStyle = '#1e2234';
      ctx.lineWidth = 2;
      ctx.fillRect(s.x,s.y,s.w,s.h);
      ctx.strokeRect(s.x+0.5,s.y+0.5,s.w-1,s.h-1);
      // Gündüz aydınlıkta hafif gölge vurgusu
      if(!state.isNight){ ctx.globalAlpha = 0.06; ctx.fillStyle = '#000'; ctx.fillRect(s.x-6,s.y-6,s.w+12,s.h+12); ctx.globalAlpha=1; }
    }
      // HUD overlay
      ctx.fillStyle = 'rgba(0,0,0,0.2)';
      ctx.fillRect(10,10,170,82);
      ctx.fillStyle = '#cde5ff'; ctx.font = '14px Inter, system-ui';
      ctx.fillText('Skor: '+state.score, 20, 32);
      ctx.fillText('Enerji: '+Math.round(state.energy), 20, 52);
      ctx.fillText('Süre: '+state.timer.toFixed(1)+'s', 20, 72);

    // İnsanlar
    for(const h of state.humans){
      ctx.globalAlpha = h.alive ? 1 : 0.35;
      ctx.beginPath();
      ctx.fillStyle = h.alive ? '#d6f6ff' : '#6d2b2b';
      ctx.arc(h.x, h.y, h.r, 0, Math.PI*2);
      ctx.fill();
      ctx.globalAlpha = 1;
      if(!state.started){
        drawBanner('Hareket için yön tuşlarına bas', 'WASD / Ok tuşları, Space = hız patlaması');
      } else if(!state.alive){
        const msg = state.score>=state.target? 'Kazandın!':'Çarpıldın';
        drawBanner(msg, 'Yeniden oynamak için butona bas veya tuşlara dokun.');
      }
    }

    // Oyuncu (vampir)
    const p = state.player;
    ctx.save();
    ctx.translate(p.x,p.y);
    // aura
    const aura = state.isNight ? 0.25 : 0.12;
    ctx.globalAlpha=aura; ctx.beginPath(); ctx.fillStyle='#9d0028'; ctx.arc(0,0,22,0,Math.PI*2); ctx.fill(); ctx.globalAlpha=1;
    // gövde
    ctx.beginPath(); ctx.fillStyle = '#ff2745'; ctx.arc(0,0,p.r,0,Math.PI*2); ctx.fill();
    // dişler
    ctx.fillStyle = '#ffffff';
    ctx.fillRect(-4, -2, 2, 4); ctx.fillRect(2, -2, 2, 4);
    ctx.restore();

    // UI mesajları
    ctx.font = '14px system-ui,Segoe UI,Roboto';
    ctx.fillStyle = '#b8c0d0';
    ctx.fillText(state.isNight? 'Gece: İnsan avı serbest (E ile ısır).': 'Gündüz: Gölge dışında güneş yakar!', 14, 22);

    if(!state.alive){
      banner('🩸 Öldün! Gün: '+state.day+'  Isırık: '+state.bites+'  —  Yeniden başlat için sol menü.');
    }
    if(state.win){
      banner('🏆 Kazandın! '+TARGET_DAYS+' gün hayatta kaldın. Isırık: '+state.bites);
    function drawBanner(main, sub){
      ctx.fillStyle = 'rgba(4,5,12,0.75)';
      ctx.fillRect(W*0.18, H*0.42, W*0.64, 120);
      ctx.strokeStyle = '#2b3b6c'; ctx.lineWidth = 1.4; ctx.strokeRect(W*0.18, H*0.42, W*0.64, 120);
      ctx.fillStyle = '#eaf6ff'; ctx.font = 'bold 22px Inter, system-ui'; ctx.textAlign='center';
      ctx.fillText(main, W/2, H*0.42+48);
      ctx.fillStyle = '#9ab5d6'; ctx.font = '14px Inter, system-ui';
      ctx.fillText(sub, W/2, H*0.42+80);
      ctx.textAlign='start';
    }
  }

  function banner(text){
    ctx.save();
    ctx.fillStyle = 'rgba(0,0,0,0.6)';
    ctx.fillRect(0, H/2-40, W, 80);
    ctx.font = 'bold 22px system-ui,Segoe UI,Roboto';
    ctx.textAlign='center';
    ctx.fillStyle = '#fff';
    ctx.fillText(text, W/2, H/2+8);
    ctx.restore();
  }

  // Main loop
  let last=performance.now();
  function loop(now){
    const dt = Math.min(0.033, (now-last)/1000); // 33 ms sınırlama
    last = now;
    step(dt);
    draw();
    let last = performance.now();
    reset();
    function loop(now){
      const dt = Math.min(0.033, (now-last)/1000); last = now;
      update(dt);
      draw();
      if(state.spark>0){
        ctx.fillStyle = `rgba(255,255,255,${state.spark*0.4})`;
        ctx.fillRect(0,0,W,H);
        state.spark = Math.max(0, state.spark - dt*1.5);
      }
      updateUI();
      requestAnimationFrame(loop);
    }
    requestAnimationFrame(loop);
  }

  reset();
  requestAnimationFrame(loop);
  </script>
</body>
</html>
