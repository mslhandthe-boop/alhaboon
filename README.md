<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>هابون | Haboon - تواصل اجتماعي</title>
<link rel="icon" href="icon.png">
<link rel="manifest" href="manifest.json">
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',Tahoma}
body{background:#f0f2f5}
.topbar{background:#fff;box-shadow:0 1px 5px rgba(0,0,0,.1);position:sticky;top:0;z-index:99;padding:10px 15px;display:flex;justify-content:space-between;align-items:center}
.logo{font-size:26px;font-weight:bold;color:#075e54}.logo span{color:#25d366}
.topbar input{flex:1;max-width:400px;margin:0 15px;padding:9px 15px;border:none;background:#f0f2f5;border-radius:20px;outline:none}
.topbar .icons{font-size:22px;display:flex;gap:15px;cursor:pointer}
.container{max-width:1000px;margin:auto;display:flex;gap:15px;padding:15px}
.sidebar{width:220px;background:#fff;border-radius:12px;padding:15px;height:fit-content;position:sticky;top:70px}
.sidebar div{padding:10px;cursor:pointer;border-radius:8px;font-weight:bold}
.sidebar div:hover,.sidebar div.active{background:#e8f5e9;color:#075e54}
.feed{flex:1}
.stories{display:flex;gap:10px;overflow-x:auto;padding-bottom:10px}
.story{min-width:110px;height:180px;border-radius:12px;background:linear-gradient(#25d366,#075e54);color:#fff;padding:10px;display:flex;flex-direction:column;justify-content:space-between;cursor:pointer;position:relative}
.story .av{width:38px;height:38px;background:#fff;color:#075e54;border-radius:50%;display:flex;align-items:center;justify-content:center;font-weight:bold}
.create{background:#fff;border-radius:12px;padding:15px;margin-bottom:15px;box-shadow:0 1px 3px rgba(0,0,0,.1)}
.create textarea{width:100%;border:none;outline:none;resize:none;font-size:16px;padding:10px}
.create button{background:#25d366;color:#fff;border:none;padding:9px 20px;border-radius:20px;font-weight:bold;cursor:pointer;float:left}
.post{background:#fff;border-radius:12px;padding:15px;margin-bottom:15px;box-shadow:0 1px 3px rgba(0,0,0,.1)}
.post-head{display:flex;gap:10px;align-items:center;margin-bottom:10px}
.avatar{width:45px;height:45px;border-radius:50%;background:#25d366;color:#fff;display:flex;align-items:center;justify-content:center;font-weight:bold;font-size:18px}
.post img{width:100%;border-radius:10px;margin:10px 0;max-height:350px;object-fit:cover}
.actions{display:flex;justify-content:space-around;border-top:1px solid #eee;padding-top:10px;margin-top:10px}
.actions span{cursor:pointer;font-weight:bold;color:#555}
.actions span.liked{color:#e0245e}
.comments{margin-top:10px;background:#f0f2f5;padding:10px;border-radius:10px;display:none}
.comments input{width:100%;padding:8px;border:none;border-radius:15px;outline:none;margin-top:5px}
.login{max-width:400px;margin:80px auto;background:#fff;padding:30px;border-radius:15px;text-align:center;box-shadow:0 0 20px rgba(0,0,0,.1)}
.login input{width:100%;padding:13px;margin:8px 0;border:1px solid #ddd;border-radius:10px;text-align:center;outline:none}
.login button{width:100%;padding:13px;background:#25d366;color:#fff;border:none;border-radius:10px;font-weight:bold;cursor:pointer;font-size:17px}
@media(max-width:700px){.sidebar{display:none}.topbar input{display:none}.container{padding:8px}}
</style>
</head>
<body>

<div class="login" id="loginBox">
<h1 style="color:#075e54;font-size:36px">هابون <span style="color:#25d366">Haboon</span></h1>
<p style="color:#666;margin:10px 0 20px">تطبيق تواصل اجتماعي يمني<br>تواصل، شارك، دردش</p>
<input id="userName" placeholder="اسمك">
<input id="userPhone" placeholder="رقم الجوال" type="tel" dir="ltr">
<button onclick="login()">دخول إلى هابون ➤</button>
</div>

<div id="appBox" style="display:none">
<div class="topbar">
<div class="logo">هابون <span>Haboon</span></div>
<input placeholder="🔍 بحث في هابون - أصدقاء، منشورات...">
<div class="icons"><span>🔔</span><span onclick="showTab('chat')">💬</span><span onclick="logout()">🚪</span></div>
</div>

<div class="container">
<div class="sidebar">
<div class="active" onclick="showFeed()">🏠 الرئيسية</div>
<div onclick="showFeed()">👥 الأصدقاء</div>
<div onclick="showFeed()">📹 فيديو</div>
<div onclick="showFeed()">🛍️ السوق</div>
<div onclick="showTab('chat')">💬 رسائل هابون</div>
<div onclick="showTab('profile')">👤 صفحتي</div>
<div style="background:#e8f5e9;margin-top:15px;text-align:center"><b id="myName"></b><br><small id="myPhone"></small></div>
</div>

<div class="feed" id="feedArea">
<div class="stories" id="stories"></div>

<div class="create">
<div style="display:flex;gap:10px;align-items:center"><div class="avatar" id="myAvatar">أ</div><textarea id="postText" placeholder="بماذا تفكر يا...؟ شارك مع أصدقائك في هابون"></textarea></div>
<div style="display:flex;justify-content:space-between;margin-top:10px"><span>📸 صورة &nbsp; 🎥 فيديو &nbsp; 😊 شعور</span><button onclick="addPost()">نشر</button></div>
</div>

<div id="posts"></div>
</div>

<div class="feed" id="chatArea" style="display:none">
<div style="background:#fff;padding:15px;border-radius:12px"><h3>💬 محادثات هابون</h3><p style="color:#888;margin:10px 0">دردش مع أصدقائك مثل الواتساب</p><div id="chatList"></div></div>
</div>

<div class="feed" id="profileArea" style="display:none">
<div style="background:#075e54;color:#fff;padding:30px;border-radius:12px;text-align:center"><div class="avatar" style="width:80px;height:80px;font-size:32px;margin:auto;background:#fff;color:#075e54" id="profileAvatar">أ</div><h2 id="profileName" style="margin-top:10px"></h2><p>150 صديق • 20 منشور • تعز، اليمن</p><button style="background:#25d366;color:#fff;border:none;padding:8px 20px;border-radius:20px;margin-top:10px">تعديل الملف الشخصي</button></div>
<div style="background:#fff;padding:15px;border-radius:12px;margin-top:15px"><h3>منشوراتي</h3><div id="myPosts"></div></div>
</div>

</div>
</div>

<script>
let user=JSON.parse(localStorage.getItem('haboon_user')||'null');
if(user) showApp();
function login(){
let n=document.getElementById('userName').value.trim();
let p=document.getElementById('userPhone').value.trim();
if(n.length<2||p.length<8){alert('اكتب اسمك ورقمك');return;}
user={name:n,phone:p};localStorage.setItem('haboon_user',JSON.stringify(user));showApp();
}
function showApp(){
document.getElementById('loginBox').style.display='none';
document.getElementById('appBox').style.display='block';
document.getElementById('myName').innerText=user.name;
document.getElementById('myPhone').innerText=user.phone;
document.getElementById('myAvatar').innerText=user.name[0];
document.getElementById('profileName').innerText=user.name;
document.getElementById('profileAvatar').innerText=user.name[0];
document.getElementById('postText').placeholder='بماذا تفكر يا '+user.name+'؟';
renderStories();renderPosts();
}
function logout(){localStorage.removeItem('haboon_user');location.reload();}
function showTab(t){
document.getElementById('feedArea').style.display=t=='feed'?'block':'none';
document.getElementById('chatArea').style.display=t=='chat'?'block':'none';
document.getElementById('profileArea').style.display=t=='profile'?'block':'none';
if(t=='chat') renderChats();
}
function showFeed(){showTab('feed')}
let storiesData=[{n:'قصتي',l:'+'},{n:'أحمد',l:'أ'},{n:'سارة',l:'س'},{n:'محمد',l:'م'},{n:'علي',l:'ع'},{n:'ريم',l:'ر'}];
function renderStories(){
document.getElementById('stories').innerHTML=storiesData.map(s=>`<div class="story" onclick="alert('عرض حالة ${s.n} في هابون')"><div class="av">${s.l}</div>
