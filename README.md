<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<meta name="theme-color" content="#101827">
<title>Arrow Drift — Logic Puzzle</title>
<style>
:root{--bg:#101827;--panel:#192538;--panel2:#22324a;--ink:#f3f7ff;--muted:#9aabc2;--mint:#69e0c1;--blue:#7cb7ff;--pink:#ff8eaa;--gold:#ffd477}
*{box-sizing:border-box}html,body{margin:0;min-height:100%;background:radial-gradient(ellipse at 50% -10%,#263b59 0%,#101827 58%);color:var(--ink);font-family:system-ui,-apple-system,Segoe UI,Roboto,sans-serif;overscroll-behavior:none}
body{min-height:100svh;display:flex;justify-content:center;padding:14px 12px 24px}
.app{width:min(100%,520px);display:flex;flex-direction:column;gap:14px}
header{display:flex;align-items:center;justify-content:space-between;padding:4px 2px}
.brand{display:flex;align-items:center;gap:10px}.logo{width:42px;height:42px;border-radius:14px;background:linear-gradient(145deg,#7ce7ce,#4d9bdb);display:grid;place-items:center;color:#102033;font-size:25px;font-weight:900;box-shadow:0 7px 24px #57d6c344}
h1{font-size:20px;line-height:1;margin:0;letter-spacing:.3px}.tagline{font-size:11px;color:var(--muted);margin-top:4px;letter-spacing:1.2px;text-transform:uppercase}
.iconbtn{border:1px solid #354760;background:#1c2a3e;color:var(--ink);width:42px;height:42px;border-radius:14px;font-size:18px}
.stats{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}.stat{background:#192538dd;border:1px solid #2c3c53;border-radius:15px;padding:10px 12px}.stat small{display:block;color:var(--muted);font-size:10px;text-transform:uppercase;letter-spacing:1px}.stat strong{display:block;font-size:19px;margin-top:2px;font-variant-numeric:tabular-nums}
.card{background:linear-gradient(160deg,#1b2a40,#172337);border:1px solid #2d4059;border-radius:22px;padding:15px;box-shadow:0 14px 35px #0002}
.levelrow{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px}.levelrow strong{font-size:15px}.levelrow span{color:var(--muted);font-size:12px}
.progress{height:5px;background:#2b3b51;border-radius:9px;overflow:hidden;margin-bottom:14px}.progress>i{display:block;height:100%;width:0;background:linear-gradient(90deg,var(--mint),var(--blue));transition:width .25s}
.boardwrap{background:#111d2d;border:1px solid #30445f;border-radius:18px;padding:10px;position:relative;touch-action:manipulation}
#board{display:grid;grid-template-columns:repeat(6,1fr);gap:5px;width:100%;aspect-ratio:1}
.cell{border:1px solid #26384e;border-radius:9px;background:#1a2a40;display:grid;place-items:center;min-width:0;min-height:0;position:relative}
.cell.empty{background:#152338;border-color:#20334a}
.arrow{width:100%;height:100%;border:0;border-radius:8px;background:linear-gradient(145deg,#304966,#253b55);color:#d9f4ff;display:grid;place-items:center;font-size:clamp(22px,6.5vw,34px);font-weight:900;line-height:1;cursor:pointer;transition:transform .16s,opacity .16s,background .16s; padding:0;text-shadow:0 2px 7px #08111d}
.arrow:active{transform:scale(.88)}.arrow.safe{background:linear-gradient(145deg,#286c69,#234d60);color:#baffeb}.arrow.hint{outline:3px solid var(--gold);background:#6b5831;animation:pulse .7s infinite alternate}
.arrow.blocked{animation:shake .22s}.arrow.out{opacity:0;transform:translate(var(--dx),var(--dy)) scale(.4);pointer-events:none}
@keyframes pulse{to{filter:brightness(1.45)}}@keyframes shake{25%{transform:translateX(-3px)}75%{transform:translateX(3px)}}
.controls{display:flex;gap:8px;margin-top:13px}.action{flex:1;border:0;border-radius:14px;padding:12px 9px;background:#2a3c55;color:var(--ink);font-weight:700;font-size:13px}.action.primary{background:linear-gradient(100deg,#67dfc0,#79b9ff);color:#102033}.action:active{transform:scale(.97)}
.tip{color:var(--muted);font-size:12px;text-align:center;line-height:1.45;padding:0 8px}
.message{min-height:22px;text-align:center;color:var(--mint);font-size:13px;font-weight:650}
.overlay{position:fixed;inset:0;background:#08111dcc;display:none;align-items:center;justify-content:center;padding:22px;z-index:5}.overlay.show{display:flex}.modal{width:min(100%,360px);background:#1b2a40;border:1px solid #405672;border-radius:24px;padding:24px;text-align:center;box-shadow:0 24px 80px #0008}.modal .big{font-size:42px}.modal h2{margin:8px 0}.modal p{color:var(--muted);font-size:14px;line-height:1.5}.modal button{width:100%;margin-top:8px}
footer{text-align:center;color:#71849e;font-size:10px;padding:0 12px}
@media(min-width:440px){.card{padding:18px}.boardwrap{padding:12px}#board{gap:6px}.cell{border-radius:10px}}
</style>
</head>
<body>
<main class="app">
<header>
 <div class="brand"><div class="logo">➜</div><div><h1>ARROW DRIFT</h1><div class="tagline">Clear the grid. Find your flow.</div></div></div>
 <button class="iconbtn" id="soundBtn" aria-label="Toggle sound">♪</button>
</header>
<section class="stats">
 <div class="stat"><small>Level</small><strong id="levelStat">01</strong></div>
 <div class="stat"><small>Moves</small><strong id="movesStat">0</strong></div>
 <div class="stat"><small>Lives</small><strong id="livesStat">♥ ♥ ♥</strong></div>
</section>
<section class="card">
 <div class="levelrow"><strong id="levelTitle">Stage 01 · Warm-up</strong><span id="remaining">36 arrows left</span></div>
 <div class="progress"><i id="progress"></i></div>
 <div class="boardwrap"><div id="board" aria-label="Arrow puzzle board"></div></div>
 <div class="controls">
  <button class="action" id="restartBtn">↻ Restart</button>
  <button class="action" id="hintBtn">✦ Hint <span id="hintsLeft">2</span></button>
  <button class="action primary" id="nextBtn" style="display:none">Next level →</button>
 </div>
</section>
<div class="message" id="message">Tap an arrow with a clear path to the edge.</div>
<div class="tip">A clear lane means the arrow can escape. Plan your moves and empty the board — no timer, just logic.</div>
<footer>ARROW DRIFT · Original mini logic game · Progress saves on this device</footer>
</main>
<div class="overlay" id="overlay"><div class="modal"><div class="big" id="modalIcon">✨</div><h2 id="modalTitle">Level cleared!</h2><p id="modalText">Great thinking.</p><button class="action primary" id="modalBtn">Continue</button></div></div>
<script>
(()=>{
const N=6, boardEl=document.getElementById('board');
const $=id=>document.getElementById(id);
const dirs=[{x:0,y:-1,ch:'↑',name:'up'},{x:1,y:0,ch:'→',name:'right'},{x:0,y:1,ch:'↓',name:'down'},{x:-1,y:0,ch:'←',name:'left'}];
let level=+(localStorage.getItem('arrowDriftLevel')||1), moves=0,lives=3,hints=2,arrows=[],sound=true,won=false;
let audioCtx=null;
function tone(freq=500,duration=.07){if(!sound)return;try{audioCtx=audioCtx||new(window.AudioContext||window.webkitAudioContext)();let o=audioCtx.createOscillator(),g=audioCtx.createGain();o.connect(g);g.connect(audioCtx.destination);o.frequency.value=freq;g.gain.setValueAtTime(.045,audioCtx.currentTime);g.gain.exponentialRampToValueAtTime(.001,audioCtx.currentTime+duration);o.start();o.stop(audioCtx.currentTime+duration)}catch(e){}}
function idx(x,y){return y*N+x}
function inside(x,y){return x>=0&&x<N&&y>=0&&y<N}
function occupied(x,y,skip){return arrows.some(a=>!a.gone&&a.id!==skip&&a.x===x&&a.y===y)}
function clearPath(a){let x=a.x+a.dx,y=a.y+a.dy;while(inside(x,y)){if(occupied(x,y,a.id))return false;x+=a.dx;y+=a.dy}return true}
function generate(){
 arrows=[];let cells=[];
 // Create a solvable board in removal order: every newly placed arrow avoids
 // the escape lanes of all earlier arrows, so earlier moves stay available.
 let candidates=[];for(let y=0;y<N;y++)for(let x=0;x<N;x++)candidates.push({x,y});
 for(let attempt=0;attempt<45;attempt++){
  arrows=[];let pool=[...candidates].sort(()=>Math.random()-.5);
  for(const p of pool){
   let opts=dirs.filter(d=>{
    // New arrow must be clear now.
    let x=p.x+d.x,y=p.y+d.y;
    while(inside(x,y)){if(occupied(x,y,-1))return false;x+=d.x;y+=d.y}
    // It must not sit in the exit lane of any earlier arrow.
    for(const a of arrows){let xx=a.x+a.dx,yy=a.y+a.dy;while(inside(xx,yy)){if(xx===p.x&&yy===p.y)return false;xx+=a.dx;yy+=a.dy}}
    return true;
   });
   if(opts.length){let d=opts[Math.floor(Math.random()*opts.length)];arrows.push({id:arrows.length,x:p.x,y:p.y,dx:d.x,dy:d.y,ch:d.ch,gone:false})}
  }
  if(arrows.length>=Math.min(13+level*2,25))break;
 }
 if(arrows.length<9){
  arrows=[];let preset=[
   [0,0,1],[2,0,2],[4,0,1],[1,1,0],[3,1,3],[5,2,2],
   [0,3,0],[2,3,1],[4,3,2],[1,4,3],[3,5,0],[5,5,3]
  ];
  preset.forEach(([x,y,d],i)=>arrows.push({id:i,x,y,dx:dirs[d].x,dy:dirs[d].y,ch:dirs[d].ch,gone:false}));
 }
}
function render(){
 boardEl.innerHTML='';
 for(let y=0;y<N;y++)for(let x=0;x<N;x++){
  const cell=document.createElement('div');cell.className='cell empty';
  const a=arrows.find(q=>!q.gone&&q.x===x&&q.y===y);
  if(a){cell.className='cell';const b=document.createElement('button');b.className='arrow'+(clearPath(a)?' safe':'');b.textContent=a.ch;b.setAttribute('aria-label','Arrow '+dirs.find(d=>d.ch===a.ch).name);b.addEventListener('click',()=>tap(a,b));cell.appendChild(b)}
  boardEl.appendChild(cell);
 }
 updateStats();
}
function updateStats(){
 let left=arrows.filter(a=>!a.gone).length,total=arrows.length;
 $('levelStat').textContent=String(level).padStart(2,'0');$('movesStat').textContent=moves;
 $('livesStat').textContent='♥ '.repeat(lives).trim()||'—';$('remaining').textContent=left+' arrows left';
 $('progress').style.width=((total-left)/total*100)+'%';$('hintsLeft').textContent=hints;
 $('levelTitle').textContent='Stage '+String(level).padStart(2,'0')+' · '+(level<3?'Warm-up':level<7?'Think ahead':'Mind maze');
}
function tap(a,button){
 if(won||a.gone)return;
 if(!clearPath(a)){lives--;button.classList.add('blocked');tone(150,.12);$('message').textContent='Blocked! Try an arrow with a clear lane.';updateStats();if(lives<=0)setTimeout(gameOver,300);return}
 a.gone=true;moves++;button.style.setProperty('--dx',(a.dx*70)+'px');button.style.setProperty('--dy',(a.dy*70)+'px');button.classList.add('out');tone(620,.06);
 $('message').textContent='Nice move! The path is clear.';
 setTimeout(()=>{render();if(arrows.every(q=>q.gone))levelWin()},130);
}
function levelWin(){won=true;$('message').textContent='Beautifully cleared!';$('nextBtn').style.display='block';tone(780,.18)}
function gameOver(){showModal('💡','Try another route','You ran out of lives. Restart the level and look for arrows with open lanes.','Try again',()=>{hideModal();startLevel()})}
function showModal(icon,title,txt,btn,fn){$('modalIcon').textContent=icon;$('modalTitle').textContent=title;$('modalText').textContent=txt;$('modalBtn').textContent=btn;$('overlay').classList.add('show');$('modalBtn').onclick=fn}
function hideModal(){$('overlay').classList.remove('show')}
function startLevel(){moves=0;lives=3;hints=2;won=false;$('nextBtn').style.display='none';$('message').textContent='Tap an arrow with a clear path to the edge.';generate();render()}
function nextLevel(){level++;localStorage.setItem('arrowDriftLevel',level);hideModal();startLevel()}
$('restartBtn').onclick=()=>startLevel();
$('nextBtn').onclick=()=>{if(won)nextLevel()};
$('hintBtn').onclick=()=>{
 if(won)return;if(hints<=0){$('message').textContent='No hints left on this stage.';return}
 let safe=arrows.find(a=>!a.gone&&clearPath(a));if(!safe)return;
 hints--;render();let cell=boardEl.children[idx(safe.x,safe.y)],b=cell.querySelector('button');if(b)b.classList.add('hint');
 $('message').textContent='Hint: this arrow has a clear escape lane.';tone(720,.1);
};
$('soundBtn').onclick=()=>{sound=!sound;$('soundBtn').textContent=sound?'♪':'♫̸';if(sound)tone(600)};
$('modalBtn').onclick=()=>{hideModal();startLevel()};
startLevel();
})();
</script>
</body>
</html>