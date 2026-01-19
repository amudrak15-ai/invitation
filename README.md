<!DOCTYPE html>
<html lang="uk">
<head>
<meta charset="UTF-8">
<title>Запрошення</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body{
  margin:0;height:100vh;
  font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',Arial;
  background:linear-gradient(180deg,#020617,#0f172a);
  color:#e5e7eb;
  display:flex;align-items:center;justify-content:center;
  text-align:center;overflow:hidden;
  transition:background 2s ease;
}
body.warm{
  background:linear-gradient(180deg,#0f172a,#1f2937);
}

/* 🌬 подих фону */
@keyframes breatheBg{
  0%{filter:brightness(1)}
  50%{filter:brightness(1.04)}
  100%{filter:brightness(1)}
}
body.breathe{
  animation:breatheBg 14s ease-in-out infinite;
}

.box{max-width:420px;padding:30px;animation:fadeIn 1.2s ease;}
h1{color:#93c5fd;font-size:2.3rem;margin-bottom:14px;}
p{font-size:1.15rem;line-height:1.6;margin-bottom:22px;}
.date{font-size:1.1rem;opacity:.85;margin-bottom:10px;}
#countdown{font-size:1.05rem;margin-bottom:28px;opacity:.9;}
.actions{display:flex;gap:14px;justify-content:center;flex-wrap:wrap;}

.btn{
  padding:14px 28px;border-radius:40px;
  font-size:1.05rem;font-weight:600;
  cursor:pointer;border:none;
}
.btn-primary{background:#3b82f6;color:white;}
.btn-secondary{
  background:transparent;color:#c7d2fe;
  border:1px solid rgba(199,210,254,.35);
}
.btn-map{
  background:linear-gradient(135deg,#c7f0e8,#dbeafe);
  color:#0f172a;
  box-shadow:0 8px 20px rgba(199,240,232,.35);
}

/* ❤️ */
.heart{
  font-size:48px;
  animation:heartBeat 1.4s infinite;
  margin-bottom:16px;
}
@keyframes heartBeat{
  0%{transform:scale(1)}
  50%{transform:scale(1.15)}
  100%{transform:scale(1)}
}

/* 🌫 пауза */
.pause-overlay{
  position:fixed;
  inset:0;
  background:rgba(2,6,23,.85);
  display:flex;
  align-items:center;
  justify-content:center;
  opacity:0;
  pointer-events:none;
  transition:opacity .3s ease;
  z-index:1000;
}
.pause-overlay.show{
  opacity:1;
  pointer-events:auto;
}
.pause-text{
  font-size:1.2rem;
  opacity:.9;
}

/* ✨ зірки */
.sparkle{
  position:fixed;
  font-size:14px;
  opacity:0;
  pointer-events:none;
  animation:sparkleFade .8s ease-in-out forwards;
}
@keyframes sparkleFade{
  0%{opacity:0;transform:scale(.5)}
  30%{opacity:1;transform:scale(1)}
  100%{opacity:0;transform:scale(.6)}
}

/* 🔄 reset */
.reset-btn{
  position:fixed;
  bottom:0;
  right:0;
  width:70px;
  height:70px;
  background:transparent;
  border:none;
  opacity:0;
}

@keyframes fadeIn{
  from{opacity:0;transform:translateY(14px)}
  to{opacity:1;transform:translateY(0)}
}
</style>
</head>

<body>

<div class="pause-overlay" id="pause">
  <div class="pause-text">Тут я буду тебе чекати 🤍</div>
</div>

<!-- СТОРІНКА 1 -->
<div class="box" id="page1">
  <p style="opacity:.85;">Кохана моя…</p>
  <h1>У мене є пропозиція</h1>

  <p>
    Можливо, це трохи несподівано.<br>
    Але мені дуже хочеться запросити тебе<br>
    на особливе побачення ✨
  </p>

  <div class="date">📅 29.01.2026</div>
  <div id="countdown"></div>

  <div class="actions">
    <button id="yesBtn" class="btn btn-primary" onclick="yesClicked()">Так ❤️</button>
    <button id="maybeBtn" class="btn btn-secondary" onclick="maybeClicked()">Я подумаю 🤍</button>
  </div>

  <div id="note" style="opacity:.75;margin-top:18px;"></div>
</div>

<!-- СТОРІНКА 2 -->
<div class="box" id="page2" style="display:none;">
  <div class="heart">❤️</div>
  <h1>Я дуже радий</h1>
  <p id="typingText"></p>

  <button class="btn btn-map" onclick="openAddress()">
    📍 Переглянути адресу
  </button>
</div>

<button class="reset-btn" onclick="resetSite()"></button>

<script>
/* 🔔 TELEGRAM */
const BOT_TOKEN="7918773768:AAEIt-TDY_g9IaxyWcITHmyzcXP_dPmTcI8";
const CHAT_ID="954471937";

function sendTelegram(msg){
  const time=new Date().toLocaleString("uk-UA");
  fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`,{
    method:"POST",
    headers:{"Content-Type":"application/json"},
    body:JSON.stringify({
      chat_id:CHAT_ID,
      text:`${msg}\n⏰ ${time}`
    })
  });
}

/* 👀 ПОВІДОМЛЕННЯ ПРИ ЗАХОДІ НА САЙТ */
sendTelegram("👀 Вона відкрила твоє запрошення");

/* ✨ ЗІРКИ */
let sparkleInterval;

function spawnSparkles(count){
  for(let i=0;i<count;i++){
    const s=document.createElement("div");
    s.className="sparkle";
    s.innerText="✨";
    s.style.left=Math.random()*100+"vw";
    s.style.top=Math.random()*100+"vh";
    document.body.appendChild(s);
    setTimeout(()=>s.remove(),800);
  }
}

function startSparkles(count, speed){
  clearInterval(sparkleInterval);
  sparkleInterval=setInterval(()=>spawnSparkles(count), speed);
}

/* старт */
startSparkles(3,6000);

/* ❤️ ТАК */
function yesClicked(){
  if(localStorage.getItem("final_yes")) return;
  localStorage.setItem("final_yes","1");

  page1.style.display="none";
  page2.style.display="block";
  document.body.classList.add("warm","breathe");

  typeText("І дуже чекаю нашої зустрічі ✨", typingText);
  startSparkles(5,3000);

  sendTelegram("❤️ Вона сказала ТАК!");
}

/* 🤍 Я ПОДУМАЮ */
function maybeClicked(){
  if(localStorage.getItem("maybe_used")) return;

  localStorage.setItem("maybe_used","1");

  maybeBtn.disabled=true;
  maybeBtn.innerText="Я ще думаю…";
  maybeBtn.style.opacity="0.5";

  note.innerText="Я почекаю. Мені важлива твоя відповідь 🤍";

  sendTelegram("🤍 Вона натиснула «Я подумаю»");
}

/* 📍 MAP */
function openAddress(){
  pause.classList.add("show");

  sendTelegram("📍 Вона відкрила адресу побачення");

  setTimeout(()=>{
    pause.classList.remove("show");
    window.open(
      "https://maps.app.goo.gl/odMhQ7EiazMbq6HPA?g_st=ic",
      "_blank"
    );
  },900);
}

/* відновлення */
if(localStorage.getItem("maybe_used")){
  maybeBtn.disabled=true;
  maybeBtn.innerText="Я ще думаю…";
  maybeBtn.style.opacity="0.5";
}
if(localStorage.getItem("final_yes")){
  page1.style.display="none";
  page2.style.display="block";
  document.body.classList.add("warm","breathe");
  startSparkles(5,3000);
  typeText("І дуже чекаю нашої зустрічі ✨", typingText);
}

/* ✍️ друк */
function typeText(text, el, speed=45){
  el.innerHTML="";
  let i=0;
  const t=setInterval(()=>{
    el.innerHTML+=text.charAt(i++);
    if(i>=text.length) clearInterval(t);
  },speed);
}

/* 🔄 reset */
function resetSite(){
  localStorage.clear();
  location.href=location.pathname+"?v="+Date.now();
}
</script>

</body>
</html>
