# Computer-Laboratory-Attendance-
<!doctype html>
<html lang="en"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1">
<title>Computer Laboratory Student Attendance Monitoring System</title>
<style>
:root{--purple:#4840d2;--navy:#22263f;--field:#4a4b5d;--green:#8bc77f}*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:var(--navy);color:white}
header{background:var(--purple);padding:20px;display:flex;align-items:center;gap:12px;min-height:135px}
.logo{font-size:55px}.brand h1{font-size:clamp(20px,4vw,34px);line-height:1.08;margin:0}.brand p{font-size:clamp(12px,2.5vw,20px);margin:8px 0 0}
nav{background:#f7f6f2;display:flex;justify-content:space-around;align-items:center;padding:10px 4px;gap:4px}
nav button{background:transparent;color:var(--green);border:0;padding:10px 6px;font-size:clamp(13px,2.5vw,24px);display:flex;align-items:center;gap:6px;cursor:pointer}
nav button.active{background:#e6f1e3;border-radius:10px;color:#4b9945}.icon{font-size:clamp(23px,4vw,40px)}
main{max-width:850px;margin:auto;padding:22px 22px 60px}.page{display:none}.page.active{display:block}
h2{text-align:center;font-size:clamp(27px,5vw,40px);margin:25px 0 42px}
.field{margin-bottom:27px}label{display:block;font-size:clamp(18px,3vw,25px);font-weight:bold;margin:0 0 12px 8px}
input,select{width:100%;min-height:60px;padding:16px 25px;border:2px solid transparent;border-radius:32px;background:var(--field);color:white;font-size:clamp(16px,2.5vw,22px);outline:none}
input::placeholder{color:#eee;opacity:1}input:focus,select:focus{border-color:#aaa6ff}
.primary{display:block;margin:32px auto 0;padding:18px 28px;min-width:220px;border:0;border-radius:28px;background:var(--field);color:white;font-size:clamp(21px,3.5vw,30px);font-weight:bold;cursor:pointer}
.note{text-align:center;color:#dddff0;font-size:14px;line-height:1.5}.message{display:none;margin:18px auto;padding:13px;border-radius:12px;text-align:center;line-height:1.4}.error{display:block;background:#5b2e3b}.success{display:block;background:#244d3b}
.loginbox{max-width:620px;margin:auto;padding-top:clamp(40px,10vw,100px)}.loginbox .field{margin-bottom:38px}.loginbox .primary{margin-top:60px}
.panel{background:#2c304a;border:1px solid #454a68;border-radius:16px;padding:18px;margin:18px 0}.stats{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}.stat{text-align:center;background:var(--field);border-radius:12px;padding:14px 5px}.stat strong{display:block;font-size:24px}.stat span{font-size:13px;color:#eee}
.tablewrap{overflow-x:auto}table{width:100%;min-width:560px;border-collapse:collapse;margin-top:15px}td,th{text-align:left;padding:10px;border-bottom:1px solid #50546e;font-size:13px}th{color:#bde9b3}
.small{background:#6a3440;border:0;border-radius:8px;padding:10px;color:white;cursor:pointer}.logout{font-size:20px;min-width:0}
@media(max-width:520px){header{padding:16px 12px;gap:8px}.logo{font-size:42px}nav button{font-size:12px;gap:3px;padding:8px 3px}.icon{font-size:23px}main{padding:18px 17px 45px}.stats{gap:5px}.stat strong{font-size:20px}.stat span{font-size:11px}}
</style></head><body>
<header><div class="logo">📋</div><div class="brand"><h1>Computer Laboratory Student Attendance Monitoring System</h1><p>Student Attendance and Instructor Monitoring System</p></div></header>
<nav>
<button class="active" data-page="student"><span class="icon">🧑‍🎓</span>Student Attendance</button>
<button data-page="login"><span class="icon">👩‍🏫</span>Instructor Login</button>
<button data-page="dashboard" id="dashNav" hidden><span class="icon">📊</span>Dashboard</button>
</nav>
<main>
<section class="page active" id="student"><h2>MARK YOUR ATTENDANCE<br>STUDENTS</h2>
<form id="attendanceForm">
<div class="field"><label for="sid">Student ID</label><input id="sid" placeholder="Example: 2026-001" required></div>
<div class="field"><label for="sname">Student Name</label><input id="sname" placeholder="Enter your full name" required></div>
<div class="field"><label for="section">Section</label><select id="section" required><option value="">Select your section</option><option>BSIT - 1A</option><option>BSIT - 1B</option><option>BSIT - 1C</option><option>BSIT - 1D</option><option>BSIT - 1E</option></select></div>
<div class="field"><label for="lab">Laboratory</label><select id="lab" required><option value="">Select laboratory</option><option>Computer Laboratory 1</option><option>Computer Laboratory 2</option><option>Computer Laboratory 3</option></select></div>
<div class="field"><label for="code">Attendance Code</label><input id="code" placeholder="Enter code shown by instructor" inputmode="numeric" required></div>
<p class="note">Ask your instructor for the active attendance code. Do not share it with absent students.</p>
<button class="primary" type="submit">Confirm Attendance</button><div id="attMsg" class="message" role="status"></div>
</form></section>
<section class="page" id="login"><div class="loginbox"><h2>Instructor Login</h2>
<form id="loginForm"><div class="field"><label for="username">Username</label><input id="username" placeholder="Enter username" required autocomplete="username"></div>
<div class="field"><label for="password">Password</label><input id="password" type="password" placeholder="Enter password" required autocomplete="current-password"></div>
<button class="primary" type="submit">Login</button><div id="loginMsg" class="message" role="status"></div><p class="note">Demo login: instructor / 1234</p></form></div></section>
<section class="page" id="dashboard"><h2>Instructor Dashboard</h2>
<div class="panel"><h3>Attendance Session</h3><p class="note">Start a session, then show the temporary code on the lab screen.</p>
<div class="stats"><div class="stat"><strong id="sessStatus">Closed</strong><span>Session</span></div><div class="stat"><strong id="sessCode">------</strong><span>Attendance code</span></div><div class="stat"><strong id="timer">--</strong><span>Seconds left</span></div></div>
<button class="primary" id="start" type="button">Start Attendance (2 minutes)</button><p id="sessNote" class="note">Demo code expires after 2 minutes.</p></div>
<div class="panel"><h3>Recorded Attendance</h3><div class="stats"><div class="stat"><strong id="total">0</strong><span>Total</span></div><div class="stat"><strong id="present">0</strong><span>Present</span></div><div class="stat"><strong id="late">0</strong><span>Late</span></div></div>
<div class="tablewrap"><table><thead><tr><th>Student ID</th><th>Name</th><th>Section</th><th>Laboratory</th><th>Time</th><th>Status</th></tr></thead><tbody id="rows"><tr><td colspan="6">No attendance recorded yet.</td></tr></tbody></table></div><button class="small" id="clear" type="button">Clear Demo Records</button></div>
<button class="primary logout" id="logout" type="button">Logout</button></section>
</main>
<script>
const pages=[...document.querySelectorAll('.page')],nav=[...document.querySelectorAll('nav button[data-page]')];
let logged=false,activeCode='',expires=0,timerId=null,records=[];
function show(id){pages.forEach(p=>p.classList.toggle('active',p.id===id));nav.forEach(b=>b.classList.toggle('active',b.dataset.page===id))}
nav.forEach(b=>b.addEventListener('click',()=>{if(b.dataset.page==='dashboard'&&!logged){show('login');return}show(b.dataset.page)}));
function msg(el,kind,text){el.className='message '+kind;el.textContent=text}
document.getElementById('loginForm').addEventListener('submit',e=>{e.preventDefault();if(username.value.trim()==='instructor'&&password.value==='1234'){logged=true;document.getElementById('dashNav').hidden=false;document.getElementById('loginMsg').className='message';show('dashboard')}else msg(document.getElementById('loginMsg'),'error','Incorrect username or password. Please try again.')});
function tick(){let left=Math.max(0,Math.ceil((expires-Date.now())/1000));timer.textContent=left;if(!left){activeCode='';sessStatus.textContent='Closed';sessCode.textContent='------';clearInterval(timerId);timerId=null}}
document.getElementById('start').addEventListener('click',()=>{activeCode=String(Math.floor(100000+Math.random()*900000));expires=Date.now()+60000;sessStatus.textContent='Open';sessCode.textContent=activeCode;sessNote.textContent='Show this code in the lab. It expires in 60 seconds (prototype demo).';if(timerId)clearInterval(timerId);tick();timerId=setInterval(tick,250)});
document.getElementById('attendanceForm').addEventListener('submit',e=>{e.preventDefault();if(!activeCode||Date.now()>=expires){msg(attMsg,'error','No active session. Ask the instructor to start attendance first.');return}if(code.value.trim()!==activeCode){msg(attMsg,'error','Incorrect attendance code. Check the code on the lab screen.');return}if(records.some(r=>r.id.toLowerCase()===sid.value.trim().toLowerCase())){msg(attMsg,'error','This Student ID has already been recorded in this demo.');return}let now=new Date();records.push({id:sid.value.trim(),name:sname.value.trim(),section:section.value,lab:lab.value,time:now.toLocaleTimeString(),status:'Present'});render();msg(attMsg,'success','Attendance confirmed at '+now.toLocaleTimeString()+'.');e.target.reset()});
function esc(s){return String(s).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]))}
function render(){total.textContent=records.length;present.textContent=records.filter(r=>r.status==='Present').length;late.textContent=records.filter(r=>r.status==='Late').length;rows.innerHTML=records.length?records.map(r=>`<tr><td>${esc(r.id)}</td><td>${esc(r.name)}</td><td>${esc(r.section)}</td><td>${esc(r.lab)}</td><td>${esc(r.time)}</td><td>${r.status}</td></tr>`).join(''):'<tr><td colspan="6">No attendance recorded yet.</td></tr>'}
document.getElementById('clear').addEventListener('click',()=>{if(confirm('Clear all demo attendance records?')){records=[];render()}});
document.getElementById('logout').addEventListener('click',()=>{logged=false;dashNav.hidden=true;document.getElementById('loginForm').reset();show('student')});
</script></body></html>
