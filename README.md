
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TokStar - Video App</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{background:#000;color:white;font-family:Arial}
.app{max-width:480px;height:100dvh;margin:auto;position:relative;overflow:hidden;background:#111}
header{position:absolute;top:0;left:0;width:100%;display:flex;justify-content:center;gap:25px;padding:18px;z-index:5;background:linear-gradient(#0009,transparent)}
button{cursor:pointer}
header button,.nav button{background:none;color:white;border:0}
.feed{height:100%;overflow-y:auto;scroll-snap-type:y mandatory;scrollbar-width:none}
.video{height:100%;position:relative;scroll-snap-align:start;background:#222}
.video video{width:100%;height:100%;object-fit:cover}
.info{position:absolute;bottom:90px;left:14px;width:75%;text-shadow:1px 1px 4px #000}
.info h3{margin-bottom:8px}
.info p{font-size:14px;line-height:1.5}
.actions{position:absolute;right:12px;bottom:105px;display:flex;flex-direction:column;gap:22px}
.actions button{font-size:27px;color:white;background:#0005;border:0}
.actions small{display:block;font-size:12px}
.liked{color:#ff3155!important}
.nav{position:absolute;bottom:0;width:100%;height:62px;background:#080808;display:flex;justify-content:space-around;align-items:center;z-index:5}
.nav button{font-size:20px;display:flex;flex-direction:column;align-items:center;gap:4px}
.nav small{font-size:10px}
.upload{background:white!important;color:black!important;padding:3px 13px;border-radius:6px}
#panel{display:none;position:absolute;z-index:10;top:65px;left:10px;right:10px;background:#222;padding:14px;border-radius:10px}
#panel input{width:100%;padding:10px;margin-top:10px;color:#111}
#panel button{padding:8px;margin-top:8px}
</style>
</head>
<body>
<div class="app">
<header>
<button onclick="tab('Following')">Following</button>
<button onclick="home()"><b>For You</b></button>
<button onclick="tab('Search')">🔍 Search</button>
</header>

<div id="panel">
<p id="message"></p>
<input id="comment" placeholder="Write a comment..." hidden>
<button onclick="postComment()">Post</button>
<button onclick="closePanel()">Close</button>
</div>

<main class="feed" id="feed">
<section class="video">
<video src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4" autoplay muted loop playsinline onclick="playPause(this)"></video>
<div class="info"><h3>@NatureWorld</h3><p>Beautiful nature 🌸 #nature #viral</p></div>
<div class="actions">
<button onclick="like(this)">♥<small>0</small></button>
<button onclick="commentVideo()">💬<small>Comment</small></button>
<button onclick="shareVideo()">↗<small>Share</small></button>
</div>
</section>

<section class="video">
<video src="https://www.w3schools.com/html/mov_bbb.mp4" muted loop playsinline onclick="playPause(this)"></video>
<div class="info"><h3>@VideoCreator</h3><p>Welcome to TokStar 🎬 #fun</p></div>
<div class="actions">
<button onclick="like(this)">♥<small>0</small></button>
<button onclick="commentVideo()">💬<small>Comment</small></button>
<button onclick="shareVideo()">↗<small>Share</small></button>
</div>
</section>
</main>

<nav class="nav">
<button onclick="home()">⌂<small>Home</small></button>
<button onclick="tab('Discover')">⌕<small>Discover</small></button>
<button class="upload" onclick="document.getElementById('picker').click()">＋</button>
<button onclick="tab('Inbox')">✉<small>Inbox</small></button>
<button onclick="tab('Profile')">♙<small>Profile</small></button>
</nav>
<input id="picker" type="file" accept="video/*" hidden onchange="addVideo(event)">
</div>

<script>
const feed=document.getElementById('feed');
const panel=document.getElementById('panel');

function like(btn){
btn.classList.toggle('liked');
let n=btn.querySelector('small');
n.textContent=Math.max(0,+n.textContent+(btn.classList.contains('liked')?1:-1));
}
function playPause(v){
if(v.paused)v.play().catch(()=>{});else v.pause();
}
function show(text){
document.getElementById('message').textContent=text;
panel.style.display='block';
}
function closePanel(){panel.style.display='none'}
function home(){closePanel();feed.scrollTo({top:0,behavior:'smooth'})}
function tab(name){
show(name==='Profile'?'Your Profile 👤':
name==='Inbox'?'Your Inbox ✉':
name+' selected. This is a demo app.');
}
function commentVideo(){
document.getElementById('comment').hidden=false;
document.getElementById('comment').value='';
show('Write your comment below:');
}
function postComment(){
let c=document.getElementById('comment');
if(c.value.trim())show('Comment: '+c.value.trim());
}
async function shareVideo(){
try{
if(navigator.share)await navigator.share({title:'TokStar',url:location.href});
else show('Copy this website link to share it.');
}catch(e){}
}
function addVideo(e){
const file=e.target.files[0];
if(!file)return;
const url=URL.createObjectURL(file);
const card=document.createElement('section');
card.className='video';
const v=document.createElement('video');
v.src=url;v.autoplay=true;v.muted=true;v.loop=true;
v.playsInline=true;v.onclick=()=>playPause(v);
const info=document.createElement('div');
info.className='info';
info.innerHTML='<h3>@You</h3><p>My new video 🎬 #TokStar</p>';
const actions=document.createElement('div');
actions.className='actions';
actions.innerHTML='<button>♥<small>0</small></button><button>💬<small>Comment</small></button><button>↗<small>Share</small></button>';
actions.children[0].onclick=function(){like(this)};
actions.children[1].onclick=commentVideo;
actions.children[2].onclick=shareVideo;
card.append(v,info,actions);
feed.prepend(card);
card.scrollIntoView({behavior:'smooth'});
e.target.value='';
}
</script>
</body>
</html>

<button id="aiOpen" onclick="openChat()">🤖 AI Chat</button>

<div id="aiChat">
  <div class="aiTop">
    <b>✨ TokStar AI</b>
    <button onclick="closeChat()">✕</button>
  </div>

  <div id="chatMessages">
    <div class="botMsg">السلام علیکم! 👋 میں TokStar AI ہوں۔ مجھ سے کچھ بھی پوچھیں!</div>
  </div>

  <form id="chatForm">
    <input id="question" placeholder="اپنا سوال لکھیں..." autocomplete="off" required>
    <button id="sendBtn" type="submit">➤</button>
  </form>
</div>

<style>
#aiOpen {
  position:absolute;top:65px;right:12px;z-index:20;
  background:#fe2c55;color:white;border:0;
  border-radius:25px;padding:12px;font-weight:bold;
}
#aiChat {
  display:none;position:absolute;z-index:50;
  inset:55px 8px 70px;background:#151515;
  color:white;border:1px solid #444;border-radius:16px;
  overflow:hidden;
}
.aiTop {
  height:55px;background:#fe2c55;padding:15px;
  display:flex;justify-content:space-between;
}
.aiTop button {
  border:0;background:transparent;color:white;font-size:20px;
}
#chatMessages {
  height:calc(100% - 120px);overflow-y:auto;padding:12px;
}
.botMsg,.userMsg {
  padding:12px;margin:10px 0;border-radius:12px;
  white-space:pre-wrap;overflow-wrap:anywhere;line-height:1.5;
}
.botMsg {background:#303030}
.userMsg {background:#493047;text-align:right}
#chatForm {
  position:absolute;bottom:0;width:100%;
  display:flex;gap:8px;padding:10px;
}
#question {
  flex:1;min-width:0;padding:12px;border:0;
  border-radius:22px;background:#fff;color:#111;
}
#sendBtn {
  border:0;border-radius:50%;width:44px;
  background:#fe2c55;color:white;font-size:20px;
}
#sendBtn:disabled {opacity:.5}
</style>

<script>
function openChat() {
  document.getElementById('aiChat').style.display='block';
}
function closeChat() {
  document.getElementById('aiChat').style.display='none';
}

const form = document.getElementById('chatForm');
const input = document.getElementById('question');
const messages = document.getElementById('chatMessages');
const sendBtn = document.getElementById('sendBtn');
const history = [];

function addMessage(text, type) {
  const div = document.createElement('div');
  div.className = type === 'user' ? 'userMsg' : 'botMsg';
  div.textContent = text;
  messages.appendChild(div);
  messages.scrollTop = messages.scrollHeight;
}

form.addEventListener('submit', async function(e) {
  e.preventDefault();
  const question = input.value.trim();
  if (!question || sendBtn.disabled) return;

  addMessage(question, 'user');
  history.push({role:'user', content:question});
  input.value = '';
  sendBtn.disabled = true;

  const loading = document.createElement('div');
  loading.className = 'botMsg';
  loading.textContent = 'AI جواب تیار کر رہا ہے...';
  messages.appendChild(loading);

  try {
    const response = await fetch('/api/chat', {
      method:'POST',
      headers:{'Content-Type':'application/json'},
      body:JSON.stringify({messages:history})
    });

    const data = await response.json();
    if (!response.ok) throw new Error(data.error || 'AI سے رابطہ نہیں ہو سکا۔');

    loading.textContent = data.reply;
    history.push({role:'assistant', content:data.reply});
  } catch (err) {
    loading.textContent = 'AI سرور سے رابطہ نہیں ہو سکا۔ سرور اور API سیٹنگ چیک کریں۔';
    history.pop();
    console.error(err);
  } finally {
    sendBtn.disabled = false;
    input.focus();
    messages.scrollTop = messages.scrollHeight;
  }
});
</script>

<!-- TokStar Profile Button -->
<button onclick="openProfile()" id="profileBtn">👤 My ID</button>

<!-- Profile Window -->
<div id="profileBox">
  <div class="profileTop">
    <b>👤 TokStar Profile</b>
    <button onclick="closeProfile()">✖</button>
  </div>

  <div id="banner">
    <span>🌟 My Banner</span>
  </div>

  <div class="profileDetails">
    <img id="avatar"
      src="https://placehold.co/100x100/333/fff?text=USER"
      alt="Profile picture">

    <h3 id="showName">Your Name</h3>
    <p id="showId">ID: TS10001</p>
    <p>🎬 TokStar Creator</p>
  </div>

  <div class="profileForm">
    <label>اپنا نام</label>
    <input id="nameInput" placeholder="Enter your name">

    <label>اپنی ID</label>
    <input id="idInput" placeholder="Enter your ID">

    <label>پروفائل تصویر منتخب کریں</label>
    <input type="file" id="avatarInput" accept="image/*">

    <label>بینر تصویر منتخب کریں</label>
    <input type="file" id="bannerInput" accept="image/*">

    <button onclick="saveProfile()">💾 Save Profile</button>
  </div>
</div>

<style>
#profileBtn {
  position:absolute;
  top:110px;
  right:12px;
  z-index:20;
  padding:10px 14px;
  border:0;
  border-radius:22px;
  color:white;
  background:#fe2c55;
  font-weight:bold;
}

#profileBox {
  display:none;
  position:absolute;
  z-index:40;
  inset:15px 8px 70px;
  overflow-y:auto;
  background:#151515;
  color:white;
  border:1px solid #444;
  border-radius:16px;
}

.profileTop {
  display:flex;
  justify-content:space-between;
  padding:15px;
  background:#fe2c55;
}

.profileTop button {
  background:transparent;
  color:white;
  border:0;
  font-size:18px;
}

#banner {
  height:145px;
  background:linear-gradient(135deg,#fe2c55,#7c3aed,#171717);
  background-size:cover;
  background-position:center;
  display:flex;
  align-items:center;
  justify-content:center;
  font-size:23px;
  font-weight:bold;
}

.profileDetails {
  text-align:center;
  padding:15px;
}

#avatar {
  width:85px;
  height:85px;
  object-fit:cover;
  border-radius:50%;
  border:3px solid #fe2c55;
  margin-top:-45px;
  background:#222;
}

#showName {margin:8px}
#showId {color:#bbb;margin:8px}

.profileForm {
  padding:15px;
  display:flex;
  flex-direction:column;
  gap:10px;
}

.profileForm input {
  width:100%;
  padding:11px;
  border:1px solid #555;
  border-radius:8px;
  background:#252525;
  color:white;
}

.profileForm button {
  background:#fe2c55;
  color:white;
  border:0;
  padding:13px;
  border-radius:9px;
  font-weight:bold;
}
</style>

<script>
function openProfile() {
  document.getElementById("profileBox").style.display="block";
}

function closeProfile() {
  document.getElementById("profileBox").style.display="none";
}

function saveProfile() {
  const name = document.getElementById("nameInput").value.trim();
  const id = document.getElementById("idInput").value.trim();

  if (name) {
    document.getElementById("showName").textContent=name;
  }

  if (id) {
    document.getElementById("showId").textContent="ID: "+id;
  }

  const avatarFile =
    document.getElementById("avatarInput").files[0];

  const bannerFile =
    document.getElementById("bannerInput").files[0];

  if (avatarFile) {
    document.getElementById("avatar").src=
      URL.createObjectURL(avatarFile);
  }

  if (bannerFile) {
    document.getElementById("banner").style.backgroundImage=
      "url('" + URL.createObjectURL(bannerFile) + "')";
  }

  alert("Profile updated! 💖");
}
</script>
