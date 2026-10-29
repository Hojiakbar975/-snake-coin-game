
<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Snake Coin</title>
<style>
*{box-sizing:border-box}
body{
 margin:0;background:#050b09;color:#eafff2;
 font-family:Arial,sans-serif;text-align:center;
 padding:16px 10px 30px
}
h1{color:#00e676}
h2{color:#00e676}
button{
 background:#00c875;color:#001b0d;border:0;
 padding:13px 19px;border-radius:9px;
 font-size:16px;font-weight:bold;margin:5px;
 cursor:pointer
}
button:active{transform:scale(.96)}
.secondary{background:#153c2b;color:white;border:1px solid #00e676}
.screen{display:none}
.screen.active{display:block}
.stats{
 display:flex;justify-content:space-around;
 flex-wrap:wrap;gap:10px;font-size:18px;margin:18px 0
}
#board{
 display:block;width:min(92vw,640px);
 height:auto;aspect-ratio:1;
 margin:15px auto;border:4px solid #00e676;
 border-radius:8px;background:#020806;
 touch-action:manipulation
}
.controls{
 width:220px;margin:12px auto;
 display:grid;grid-template-columns:repeat(3,1fr);gap:5px
}
.controls button{
 height:58px;margin:0;padding:0;font-size:25px;
 border-radius:5px
}
.controls .empty{visibility:hidden}
.card{
 background:#0c1c14;border:1px solid #145b38;
 border-radius:12px;padding:15px;margin:12px auto;
 max-width:500px
}
.skin-grid{
 display:grid;grid-template-columns:repeat(2,minmax(0,1fr));
 gap:10px;max-width:500px;margin:auto
}
.skin{
 background:#0c1c14;border:1px solid #145b38;
 border-radius:12px;padding:12px
}
.swatch{height:30px;border-radius:6px;margin-bottom:8px}
.small{font-size:14px;color:#b8d8c6}
#overlay{
 display:none;position:fixed;inset:0;
 background:rgba(0,0,0,.88);z-index:1000;
 align-items:center;justify-content:center;padding:20px
}
#overlay.show{display:flex}
.over-box{
 width:100%;max-width:340px;background:#07140f;
 border:3px solid #00e676;border-radius:18px;
 padding:25px 15px;box-shadow:0 0 25px #00e67644
}
.over-box h2{color:#ff5252}
.over-box button{width:85%;margin:7px}
</style>
</head>
<body>

<!-- Yuqoridagi SNAKE COIN sarlavhasi olib tashlangan -->

<div class="stats">
 <div>🪙 Raund: <b id="score">0</b></div>
 <div>💰 Hamyon: <b id="wallet">0</b></div>
</div>

<div id="menu" class="screen active">
 <div class="card">
  <h2>🐍 SNAKE COIN</h2>
  <p>Tangalarni yig‘ va hamyoningni to‘ldir!</p>
  <button onclick="showSection('difficulty')">🎮 O‘YINNI BOSHLASH</button>
  <br>
  <button class="secondary" onclick="openShop()">🎨 SKIN DO‘KONI</button>
  <button class="secondary" onclick="showSection('about')">ℹ️ ABOUT</button>
 </div>
</div>

<div id="difficulty" class="screen">
 <div class="card">
  <h2>⚡ QIYINLIKNI TANLA</h2>
  <button onclick="startWithDifficulty(350,'Oson')">🟢 OSON</button>
  <button onclick="startWithDifficulty(250,'O‘rtacha')">🟡 O‘RTACHA</button>
  <button onclick="startWithDifficulty(150,'Qiyin')">🔴 QIYIN</button>
  <br><button class="secondary" onclick="showSection('menu')">🏠 ORQAGA</button>
 </div>
</div>

<div id="game" class="screen">
 <div id="levelText">Daraja: Oson | Tezlik: 350 ms</div>
 <canvas id="board" width="600" height="600"></canvas>

 <div class="controls">
  <button class="empty"> </button>
  <button onclick="changeDirection('up')">⬆️</button>
  <button class="empty"> </button>
  <button onclick="changeDirection('left')">⬅️</button>
  <button onclick="changeDirection('down')">⬇️</button>
  <button onclick="changeDirection('right')">➡️</button>
 </div>
 <p>🪙 Tangalarni yig‘!</p>
 <button class="secondary" onclick="pauseToMenu()">🏠 Menyu</button>
</div>

<div id="shop" class="screen">
 <h2>🎨 SKIN DO‘KONI</h2>
 <p>💰 Hamyon: <b id="shopWallet">0</b></p>
 <div id="skinGrid" class="skin-grid"></div>
 <button class="secondary" onclick="showSection('menu')">🏠 MENYU</button>
</div>

<div id="about" class="screen">
 <h2>ℹ️ O‘YIN HAQIDA</h2>
 <div class="card">
  <h3>🐍 Snake Coin</h3>
  Tangalarni yig‘ib, ilonni o‘stir va yangi skinlarni och!
 </div>
 <div class="card">
  <h3>🎮 Qanday o‘ynaladi?</h3>
  <p>⬆️⬇️⬅️➡️ Tugmalari bilan ilonni boshqar.</p>
  <p>🪙 Tangani olganingda hisobing oshadi.</p>
  <p>🧱 Devorga yoki o‘z iloningga urilsang, raund tugaydi.</p>
 </div>
 <div class="card">
  <h3>💡 Maslahatlar</h3>
  <p>🟢 Yangi bo‘lsang, OSON darajadan boshla.</p>
  <p>🪙 Ko‘proq tanga yig‘ib, skinlarni sotib ol.</p>
  <p>👑 Sen ilonlar qiroli bo‘la olasanmi?</p>
 </div>
 <button class="secondary" onclick="showSection('menu')">🏠 MENYU</button>
</div>

<div id="overlay">
 <div class="over-box">
  <div style="font-size:50px">🐍</div>
  <h2>😢 YUTQAZDINGIZ!</h2>
  <p>🪙 Raund tangalari: <b id="finalScore">0</b></p>
  <p>💰 Hamyon: <b id="finalWallet">0</b></p>
  <button onclick="closeOverlay();showSection('menu')">🏠 MENYU</button>
  <button class="secondary" onclick="closeOverlay();startGame()">🔄 QAYTA O‘YNASH</button>
 </div>
</div>

<script>
const canvas=document.getElementById('board');
const ctx=canvas.getContext('2d');
const size=20;
const cells=30;

let snake=[];
let coin={x:10,y:10};
let direction='right';
let nextDirection='right';
let score=0;
let speed=350;
let level='Oson';
let loop=null;
let running=false;
let gameEnded=false;

let wallet=Number(localStorage.getItem('snakeWallet')||0);
let currentSkin=localStorage.getItem('snakeSkin')||'green';
let ownedSkins=JSON.parse(localStorage.getItem('snakeOwned')||'["green"]');

const skins=[
 {id:'green',name:'Green',color:'#00e676',price:0},
 {id:'white',name:'White',color:'#ffffff',price:10},
 {id:'blue',name:'Blue',color:'#2196f3',price:20},
 {id:'neon',name:'Neon Green',color:'#39ff14',price:40},
 {id:'gold',name:'Gold',color:'#ffd700',price:50},
 {id:'cyan',name:'Cyan',color:'#00ffff',price:67},
 {id:'orange',name:'Orange',color:'#ff9800',price:80},
 {id:'red',name:'Red',color:'#ff3333',price:100},
 {id:'pink',name:'Pink',color:'#ff69b4',price:150},
 {id:'purple',name:'Purple',color:'#a855f7',price:167},
 {id:'rainbow',name:'Rainbow',color:'#ff00ff',price:200},
 {id:'black',name:'Black',color:'#777777',price:230},
 {id:'fire',name:'Fire',color:'#ff5722',price:250},
 {id:'electric',name:'Electric Purple',color:'#bd00ff',price:260}
];

function saveData(){
 localStorage.setItem('snakeWallet',wallet);
 localStorage.setItem('snakeSkin',currentSkin);
 localStorage.setItem('snakeOwned',JSON.stringify(ownedSkins));
 updateWallet();
}

function updateWallet(){
 document.getElementById('wallet').textContent=wallet;
 document.getElementById('shopWallet').textContent=wallet;
}

function showSection(id){
 stopLoop();
 document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
 document.getElementById(id).classList.add('active');
 document.getElementById('overlay').classList.remove('show');
 updateWallet();
}

function startWithDifficulty(ms,name){
 speed=ms;
 level=name;
 startGame();
}

function startGame(){
 closeOverlay();
 stopLoop();
 score=0;
 direction='right';
 nextDirection='right';
 snake=[
  {x:8,y:15},
  {x:7,y:15},
  {x:6,y:15}
 ];
 gameEnded=false;
 running=true;
 document.getElementById('score').textContent=score;
 document.getElementById('levelText').textContent=
  'Daraja: '+level+' | Tezlik: '+speed+' ms';
 showSection('game');
 running=true;
 placeCoin();
 drawGame();
 loop=setInterval(gameLoop,speed);
}

function stopLoop(){
 if(loop!==null){
  clearInterval(loop);
  loop=null;
 }
 running=false;
}

function closeOverlay(){
 document.getElementById('overlay').classList.remove('show');
}

function pauseToMenu(){
 stopLoop();
 gameEnded=true;
 showSection('menu');
}

function endGame(){
 if(gameEnded)return;
 gameEnded=true;
 stopLoop();

 wallet+=score;
 saveData();

 document.getElementById('finalScore').textContent=score;
 document.getElementById('finalWallet').textContent=wallet;
 document.getElementById('overlay').classList.add('show');
}

function gameOver(){
 endGame();
}

function placeCoin(){
 let valid=false;
 while(!valid){
  coin={
   x:Math.floor(Math.random()*cells),
   y:Math.floor(Math.random()*cells)
  };
  valid=!snake.some(p=>p.x===coin.x&&p.y===coin.y);
 }
}

function changeDirection(d){
 if(!running)return;
 const opposite={
  up:'down',down:'up',left:'right',right:'left'
 };
 if(opposite[d]!==direction)nextDirection=d;
}

document.addEventListener('keydown',e=>{
 const keys={
  ArrowUp:'up',ArrowDown:'down',
  ArrowLeft:'left',ArrowRight:'right'
 };
 if(keys[e.key]){
  e.preventDefault();
  changeDirection(keys[e.key]);
 }
});

function gameLoop(){
 if(!running||gameEnded)return;
 direction=nextDirection;

 const head={...snake[0]};
 if(direction==='up')head.y--;
 if(direction==='down')head.y++;
 if(direction==='left')head.x--;
 if(direction==='right')head.x++;

 if(head.x<0||head.y<0||head.x>=cells||head.y>=cells){
  gameOver();return;
 }

 const eating=head.x===coin.x&&head.y===coin.y;
 const body=eating?snake:snake.slice(0,-1);

 if(body.some(p=>p.x===head.x&&p.y===head.y)){
  gameOver();return;
 }

 snake.unshift(head);

 if(eating){
  score++;
  document.getElementById('score').textContent=score;
  placeCoin();
 }else{
  snake.pop();
 }

 drawGame();
}

function drawGame(){
 const cell=canvas.width/cells;
 ctx.clearRect(0,0,canvas.width,canvas.height);

 ctx.strokeStyle='#0b2117';
 ctx.lineWidth=1;
 for(let i=0;i<=cells;i++){
  ctx.beginPath();
  ctx.moveTo(i*cell,0);
  ctx.lineTo(i*cell,canvas.height);
  ctx.stroke();
  ctx.beginPath();
  ctx.moveTo(0,i*cell);
  ctx.lineTo(canvas.width,i*cell);
  ctx.stroke();
 }

 ctx.fillStyle='#ffd700';
 ctx.beginPath();
 ctx.arc((coin.x+.5)*cell,(coin.y+.5)*cell,cell*.35,0,Math.PI*2);
 ctx.fill();
 ctx.fillStyle='#fff2a8';
 ctx.beginPath();
 ctx.arc((coin.x+.4)*cell,(coin.y+.4)*cell,cell*.1,0,Math.PI*2);
 ctx.fill();

 snake.forEach((p,i)=>{
  const skin=skins.find(s=>s.id===currentSkin)||skins[0];
  if(currentSkin==='rainbow'){
   ctx.fillStyle=`hsl(${(i*25+Date.now()/20)%360},100%,55%)`;
  }else if(currentSkin==='fire'){
   ctx.fillStyle=i===0?'#fff176':(i%2?'#ff5722':'#ff9800');
  }else if(currentSkin==='electric'){
   ctx.fillStyle=i%2?'#bd00ff':'#e0aaff';
  }else{
   ctx.fillStyle=skin.color;
  }

  ctx.fillRect(p.x*cell+2,p.y*cell+2,cell-4,cell-4);
  if(i===0){
   ctx.fillStyle='#001b0d';
   ctx.beginPath();
   ctx.arc((p.x+.35)*cell,(p.y+.35)*cell,cell*.07,0,Math.PI*2);
   ctx.arc((p.x+.65)*cell,(p.y+.35)*cell,cell*.07,0,Math.PI*2);
   ctx.fill();
  }
 });
}

function openShop(){
 renderShop();
 showSection('shop');
}

function renderShop(){
 const grid=document.getElementById('skinGrid');
 grid.innerHTML='';

 skins.forEach(s=>{
  const owned=ownedSkins.includes(s.id);
  const selected=currentSkin===s.id;
  const div=document.createElement('div');
  div.className='skin';
  div.innerHTML=`
   <div class="swatch" style="background:${s.color};
    ${s.id==='rainbow'?'background:linear-gradient(90deg,red,orange,yellow,green,blue,purple)':''}">
   </div>
   <b>${s.name}</b>
   <p class="small">${s.price===0?'Bepul':'🪙 '+s.price+' tanga'}</p>
   <button>${selected?'TANLANGAN':owned?'TANLASH':'SOTIB OLISH'}</button>
  `;

  div.querySelector('button').onclick=()=>{
   if(owned){
    chooseSkin(s.id);
   }else{
    buySkin(s);
   }
  };
  grid.appendChild(div);
 });
}

function chooseSkin(id){
 currentSkin=id;
 saveData();
 renderShop();
}

function buySkin(s){
 if(wallet<s.price){
  alert('Tangalar yetarli emas!');
  return;
 }
 wallet-=s.price;
 ownedSkins.push(s.id);
 currentSkin=s.id;
 saveData();
 renderShop();
}

updateWallet();
drawGame();
</script>
</body>
</html>
