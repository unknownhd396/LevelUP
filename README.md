<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Habit Tracker</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@500;600;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root{
    --navy:#1b3358;
    --navy-2:#274674;
    --blue:#3d6fd6;
    --blue-light:#a9c3f2;
    --bg:#f4f6fb;
    --panel:#ffffff;
    --border:#e6eaf2;
    --text:#1b2430;
    --muted:#8993a3;
    --green:#3bb273;
    --radius:12px;
  }
  *{box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  body{
    margin:0;
    background:var(--bg);
    color:var(--text);
    font-family:'Inter',sans-serif;
    padding:24px 24px 60px;
    -webkit-tap-highlight-color:transparent;
  }
  .wrap{max-width:1250px;margin:0 auto;}

  .site-bg{
    position:fixed;top:0;left:0;width:100%;height:100%;
    z-index:0;pointer-events:none;
  }
  .wrap{position:relative;z-index:1;}

  .quote-banner{
    text-align:center;font-family:'Poppins',sans-serif;font-weight:700;font-size:20px;
    color:#fff;background:var(--navy);padding:16px 20px;margin:0 0 24px;
    border-radius:0;letter-spacing:0.2px;
  }
  .quote-banner::before{content:'\201C';}
  .quote-banner::after{content:'\201D';}

  .topbar{
    display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:14px;
    margin-bottom:20px;
  }
  .title-block h1{
    font-family:'Poppins',sans-serif;font-weight:700;font-size:22px;margin:0;color:var(--navy);
    letter-spacing:0.2px;
  }
  .title-block p{margin:3px 0 0;font-size:12.5px;color:var(--muted);}
  .title-block p.credit-line{font-size:10.5px;color:var(--muted);margin:2px 0 0;}
  .title-block p #todayLabel{color:var(--blue);font-weight:600;}
  .month-control{display:flex;align-items:center;gap:8px;background:var(--panel);border:1px solid var(--border);
    border-radius:10px;padding:6px 8px;}
  .month-control button{
    background:var(--navy);color:#fff;border:none;width:28px;height:28px;border-radius:7px;cursor:pointer;font-size:14px;
    transition:background .2s ease, transform .12s ease;
  }
  .month-control button:hover{background:var(--navy-2);}
  .month-control button:active{transform:scale(0.88);}
  .month-label{font-family:'Poppins',sans-serif;font-weight:600;font-size:14.5px;min-width:130px;text-align:center;color:var(--navy);transition:opacity .2s ease;}

  .card{
    background:var(--panel);border:1px solid var(--border);border-radius:var(--radius);padding:16px 18px;
    transition:box-shadow .25s ease, transform .25s ease;
  }
  .card:hover{box-shadow:0 6px 20px rgba(27,51,88,0.08);}
  .card h3{
    font-family:'Poppins',sans-serif;font-size:12.5px;font-weight:600;text-transform:uppercase;letter-spacing:0.5px;
    color:var(--muted);margin:0 0 12px;
  }

  .bars{display:flex;align-items:flex-end;gap:3px;height:110px;}
  .bar-col{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:flex-end;height:100%;}
  .bar{width:100%;max-width:14px;background:var(--blue-light);border-radius:3px 3px 0 0;transition:height .3s;}
  .bar.today-bar{background:var(--blue);}
  .bar-day{font-size:8.5px;color:var(--muted);margin-top:4px;}

  .chart-row{display:grid;grid-template-columns:1.6fr 1fr;gap:16px;margin-bottom:20px;}
  @media (max-width:900px){.chart-row{grid-template-columns:1fr;}}
  .chart-card-bar .bars{height:190px;}
  .pie-wrap{display:flex;align-items:center;gap:18px;flex-wrap:wrap;}
  .pie-legend{flex:1;min-width:120px;display:flex;flex-direction:column;gap:7px;max-height:150px;overflow-y:auto;}
  .pie-legend .leg-row{display:flex;align-items:center;gap:7px;font-size:11.5px;}
  .pie-legend .leg-dot{width:9px;height:9px;border-radius:50%;flex-shrink:0;}
  .pie-legend .leg-name{flex:1;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;}
  .pie-legend .leg-pct{color:var(--muted);font-size:11px;}
  .pie-empty{color:var(--muted);font-size:12.5px;}

  .add-form{display:flex;gap:10px;flex-wrap:wrap;align-items:flex-end;}
  .add-form .field{display:flex;flex-direction:column;gap:5px;}
  .add-form label{font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:0.4px;}
  .add-form input{
    background:#fbfcfe;border:1px solid var(--border);color:var(--text);
    padding:9px 12px;border-radius:8px;font-size:13.5px;outline:none;transition:border-color .18s ease;
  }
  .add-form input:focus{border-color:var(--blue);}
  .add-form input[name="name"]{width:230px;}
  .add-form input[name="goal"]{width:90px;}
  .add-btn{
    background:var(--navy);color:#fff;border:none;font-weight:600;
    padding:10px 20px;border-radius:8px;cursor:pointer;font-family:'Poppins',sans-serif;font-size:13px;
    transition:background .18s ease, transform .12s ease;
  }
  .add-btn:hover{background:var(--navy-2);}
  .add-btn:active{transform:scale(0.96);}

  .grid-scroll{overflow-x:auto;padding-bottom:6px;transition:opacity .18s ease;}
  table.habit-grid{border-collapse:collapse;width:100%;min-width:900px;}
  table.habit-grid th, table.habit-grid td{padding:0;text-align:center;}
  thead tr.week-row th{
    font-family:'Poppins',sans-serif;font-size:10px;font-weight:600;color:#fff;background:var(--navy);
    padding:5px 0 !important;letter-spacing:0.3px;
  }
  .col-name{
    text-align:left !important;padding:6px 8px !important;font-size:13px;font-weight:500;
    min-width:170px;position:sticky;left:0;background:var(--navy);border-right:1px solid var(--navy-2);
  }
  .col-name input{
    width:100%;border:none;background:transparent;font-size:13px;font-family:'Inter',sans-serif;
    color:#fff;padding:6px 4px;border-radius:6px;outline:none;transition:background .18s ease;
  }
  .col-name input:focus{background:rgba(255,255,255,0.1);}
  .col-name input::placeholder{color:#8fa3c4;}
  .col-goal{font-size:12px;color:#fff;min-width:50px;border-right:1px solid var(--navy-2);background:var(--navy-2);}
  .col-goal input{
    width:40px;border:none;background:transparent;font-size:12px;text-align:center;
    color:#fff;outline:none;font-family:'Inter',sans-serif;transition:background .18s ease;
  }
  .col-goal input:focus{background:rgba(255,255,255,0.15);border-radius:6px;}
  thead .col-name, thead .col-goal{background:var(--navy);color:#fff;}
  .day-head{font-size:10px;color:var(--muted);width:23px;padding-bottom:1px !important;padding-top:5px !important;line-height:1.3;}
  .day-head.weekend{color:var(--blue);}
  .day-head.today{color:var(--navy);font-weight:700;}
  .weekday-row th{font-size:8.5px;color:var(--muted);font-weight:400;padding-bottom:5px !important;text-transform:uppercase;letter-spacing:0.3px;}
  .weekday-row th.weekend{color:var(--blue);}
  .weekday-row th.today{color:var(--navy);font-weight:700;}
  .cell-box{
    width:20px;height:20px;margin:2px auto;border-radius:4px;cursor:pointer;
    border:1px solid var(--border);background:#fbfcfe;
    transition:background .18s ease, border-color .18s ease, transform .15s ease;
    display:flex;align-items:center;justify-content:center;font-size:12px;color:#fff;line-height:1;
  }
  .cell-box:hover{border-color:var(--blue);transform:translateY(-1px);}
  .cell-box:active{transform:scale(0.85);}
  .cell-box.checked{background:var(--blue);border-color:var(--blue);animation:pop .22s ease;}
  .cell-box.checked::after{content:'\2713';font-weight:700;}
  @keyframes pop{
    0%{transform:scale(0.6);}
    60%{transform:scale(1.15);}
    100%{transform:scale(1);}
  }
  .col-progress{min-width:150px;padding-left:14px !important;text-align:left !important;border-left:1px solid var(--border);}
  .progress-track{width:110px;height:8px;background:#eef1f6;border-radius:5px;overflow:hidden;display:inline-block;vertical-align:middle;}
  .progress-fill{height:100%;background:var(--blue);border-radius:5px;transition:width .45s ease;}
  .progress-num{font-size:11px;color:var(--muted);margin-left:8px;}
  .row-actions{width:30px;}
  .del-btn{background:none;border:none;color:var(--muted);cursor:pointer;font-size:16px;transition:color .18s ease, transform .15s ease;}
  .del-btn:hover{color:#e05a5a;}
  .del-btn:active{transform:scale(0.85);}
  #habitBody tr td:not(.col-name):not(.col-goal){transition:background .15s ease;}
  #habitBody tr:hover td:not(.col-name):not(.col-goal){background:#f7f9fc;}
  .empty-note{color:var(--muted);font-size:13px;margin:10px 0 0;}

  .pending-table{width:100%;border-collapse:separate;border-spacing:0 8px;}
  .pending-row-name{background:var(--navy);color:#fff;border-radius:8px 0 0 8px;padding:9px 14px;font-size:13px;}
  .pending-row-name input{
    width:100%;background:transparent;border:none;color:#fff;font-size:13px;
    outline:none;font-family:'Inter',sans-serif;
  }
  .pending-row-name input::placeholder{color:#8fa3c4;}
  .pending-row-tick{background:#fbfcfe;border-top:1px solid var(--border);border-bottom:1px solid var(--border);text-align:center;padding:8px;width:56px;}
  .pending-row-count{
    background:#fbfcfe;border:1px solid var(--border);border-left:none;border-radius:0 8px 8px 0;
    text-align:center;padding:8px 14px;width:130px;font-size:11.5px;color:var(--muted);white-space:nowrap;
  }
  .pending-row-tick .cell-box{width:22px;height:22px;font-size:13px;}

  .analysis-layout{display:grid;grid-template-columns:1fr 1.2fr;gap:16px;margin-top:20px;}
  @media (max-width:900px){.analysis-layout{grid-template-columns:1fr;}}
  .rank-list{display:flex;flex-direction:column;gap:11px;}
  .rank-item{display:flex;align-items:center;gap:10px;}
  .rank-num{
    font-family:'Poppins',sans-serif;font-weight:700;font-size:11px;color:#fff;
    background:var(--blue-light);width:20px;height:20px;border-radius:50%;
    display:flex;align-items:center;justify-content:center;flex-shrink:0;
  }
  .rank-item:nth-child(1) .rank-num{background:var(--navy);}
  .rank-item:nth-child(2) .rank-num{background:var(--navy-2);}
  .rank-item:nth-child(3) .rank-num{background:var(--blue);}
  .rank-body{flex:1;}
  .rank-name{font-size:12.5px;margin-bottom:4px;display:flex;justify-content:space-between;}
  .rank-name span.pct{color:var(--muted);font-size:11px;}
  .rank-track{height:6px;background:#eef1f6;border-radius:4px;overflow:hidden;}
  .rank-fill{height:100%;background:var(--blue);transition:width .45s ease;}

  .trend-svg{width:100%;height:120px;}

  footer{text-align:center;color:var(--muted);font-size:11.5px;margin-top:14px;}

  .name-overlay{
    position:fixed;inset:0;background:rgba(27,51,88,0.55);
    display:flex;align-items:center;justify-content:center;
    z-index:1000;opacity:1;transition:opacity .25s ease;
  }
  .name-overlay.hidden{opacity:0;pointer-events:none;}
  .name-modal{
    background:var(--panel);border-radius:var(--radius);padding:28px 30px;
    width:320px;max-width:90vw;text-align:center;
    box-shadow:0 12px 40px rgba(27,51,88,0.35);
  }
  .name-modal h2{
    font-family:'Poppins',sans-serif;font-weight:700;font-size:17px;color:var(--navy);
    margin:0 0 16px;
  }
  .name-modal input{
    width:100%;background:#fbfcfe;border:1px solid var(--border);color:var(--text);
    padding:11px 14px;border-radius:8px;font-size:14px;outline:none;
    font-family:'Inter',sans-serif;margin-bottom:14px;transition:border-color .18s ease;
    text-align:center;
  }
  .name-modal input:focus{border-color:var(--blue);}
  .name-modal button{
    background:var(--navy);color:#fff;border:none;font-weight:600;
    padding:10px 26px;border-radius:8px;cursor:pointer;font-family:'Poppins',sans-serif;
    font-size:13px;transition:background .18s ease, transform .12s ease;width:100%;
  }
  .name-modal button:hover{background:var(--navy-2);}
  .name-modal button:active{transform:scale(0.97);}

  .settings-btn{
    position:fixed;top:18px;right:18px;width:40px;height:40px;border-radius:50%;
    background:var(--navy);color:#fff;border:none;cursor:pointer;font-size:18px;
    z-index:900;box-shadow:0 4px 14px rgba(27,51,88,0.3);
    transition:background .18s ease, transform .15s ease;
    display:flex;align-items:center;justify-content:center;
  }
  .settings-btn:hover{background:var(--navy-2);transform:rotate(25deg);}
  .settings-btn:active{transform:scale(0.9);}
  @media (max-width:640px){.settings-btn{top:12px;right:12px;width:36px;height:36px;font-size:16px;}}
</style>
</head>
<body>

<button class="settings-btn" id="settingsBtn" title="Change your name" aria-label="Settings">⚙</button>

<div class="name-overlay" id="nameOverlay">
  <div class="name-modal">
    <h2>What should we call you?</h2>
    <input type="text" id="nameInput" placeholder="Type your name" maxlength="24">
    <button id="nameSubmit">Let's go</button>
  </div>
</div>

<svg class="site-bg" aria-hidden="true" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <pattern id="doodlePattern" width="260" height="260" patternUnits="userSpaceOnUse">
      <g stroke="#1b3358" stroke-width="2" fill="none" opacity="0.07" stroke-linecap="round" stroke-linejoin="round">
        <!-- lightbulb -->
        <circle cx="40" cy="36" r="14"/>
        <line x1="34" y1="52" x2="46" y2="52"/>
        <line x1="35" y1="57" x2="45" y2="57"/>
        <line x1="40" y1="15" x2="40" y2="8"/>
        <line x1="27" y1="20" x2="22" y2="15"/>
        <line x1="53" y1="20" x2="58" y2="15"/>

        <!-- open book -->
        <path d="M150,40 C160,33 178,33 188,40 L188,64 C178,57 160,57 150,64 Z"/>
        <line x1="169" y1="38" x2="169" y2="62"/>

        <!-- rocket -->
        <path d="M225,110 C225,95 235,85 235,85 C235,85 245,95 245,110 L242,122 L228,122 Z"/>
        <circle cx="235" cy="100" r="4"/>
        <path d="M225,113 L215,125 L225,122 Z"/>
        <path d="M245,113 L255,125 L245,122 Z"/>

        <!-- glasses -->
        <circle cx="45" cy="140" r="13"/>
        <circle cx="80" cy="140" r="13"/>
        <line x1="58" y1="138" x2="67" y2="138"/>
        <line x1="32" y1="136" x2="20" y2="130"/>
        <line x1="93" y1="136" x2="105" y2="130"/>

        <!-- magnifying glass -->
        <circle cx="150" cy="150" r="16"/>
        <line x1="161" y1="161" x2="176" y2="176"/>

        <!-- star -->
        <path d="M220,175 L224,187 L237,187 L227,195 L231,208 L220,200 L209,208 L213,195 L203,187 L216,187 Z"/>

        <!-- pencil -->
        <line x1="60" y1="215" x2="95" y2="180"/>
        <path d="M95,180 L104,171 L110,177 L101,186 Z"/>
        <line x1="60" y1="215" x2="52" y2="223"/>

        <!-- sparkles -->
        <line x1="130" y1="95" x2="130" y2="103"/>
        <line x1="126" y1="99" x2="134" y2="99"/>
        <line x1="195" y1="200" x2="195" y2="208"/>
        <line x1="191" y1="204" x2="199" y2="204"/>
        <line x1="12" y1="90" x2="12" y2="98"/>
        <line x1="8" y1="94" x2="16" y2="94"/>
      </g>
    </pattern>
  </defs>
  <rect width="100%" height="100%" fill="url(#doodlePattern)"/>
</svg>

<div class="wrap">

  <p class="quote-banner">One of the most common causes of failure is the habit of quitting.</p>

  <div class="topbar">
    <div class="title-block">
      <h1 id="pageTitle">Habit Tracker</h1>
      <p class="credit-line">made by lord Saumya</p>
      <p>Today is <span id="todayLabel"></span> — type a goal, tick each day.</p>
    </div>
    <div class="month-control">
      <button id="prevMonth">‹</button>
      <div class="month-label" id="monthLabel"></div>
      <button id="nextMonth">›</button>
    </div>
  </div>

  <div class="chart-row">
    <div class="card chart-card-bar">
      <h3>Daily Progress — habits completed per day</h3>
      <div class="bars" id="barsContainer"></div>
    </div>
    <div class="card chart-card-pie">
      <h3>Overall Stats — share of completions by habit</h3>
      <div class="pie-wrap">
        <svg id="pieSvg" width="140" height="140" viewBox="0 0 140 140"></svg>
        <div class="pie-legend" id="pieLegend"></div>
      </div>
    </div>
  </div>

  <div class="card">
    <h3>My Habits — type a goal in each row, then tick the days</h3>
    <div class="grid-scroll">
      <table class="habit-grid" id="habitTable">
        <thead>
          <tr class="week-row" id="weekRow"></tr>
          <tr id="headRow">
            <th class="col-name">Habit</th>
            <th class="col-goal">Goal</th>
          </tr>
          <tr class="weekday-row" id="weekdayRow">
            <th></th><th></th>
          </tr>
        </thead>
        <tbody id="habitBody"></tbody>
      </table>
    </div>
    <p class="empty-note" id="emptyNote" style="display:none;">No habits yet — add one above to start ticking days.</p>
  </div>

  <div class="card">
    <h3>Pending / One-off Tasks — tick when done, it resets automatically</h3>
    <table class="pending-table">
      <tbody id="pendingBody"></tbody>
    </table>
  </div>

  <div class="analysis-layout">
    <div class="card">
      <h3>Top Habits</h3>
      <div class="rank-list" id="rankList"></div>
    </div>
    <div class="card">
      <h3>Overall % Trend</h3>
      <svg class="trend-svg" id="trendSvg" viewBox="0 0 300 100" preserveAspectRatio="none"></svg>
    </div>
  </div>

  <footer>Data is saved locally in this browser — reopen this file anytime to see your progress.</footer>
</div>

<script>
const STORAGE_KEY = 'habitTrackerData';
let today = new Date();
let viewYear = today.getFullYear();
let viewMonth = today.getMonth();

const SLOT_COUNT = 14;
const PENDING_COUNT = 6;

function loadData(){
  try{
    const raw = localStorage.getItem(STORAGE_KEY);
    if(raw) return JSON.parse(raw);
  }catch(e){}
  const habits = [];
  for(let i=0;i<SLOT_COUNT;i++){ habits.push({ id:'slot'+i, name:'', goal:26 }); }
  const pending = [];
  for(let i=0;i<PENDING_COUNT;i++){ pending.push({ id:'pend'+i, name:'', count:0 }); }
  return { habits, checks: {}, pending };
}
function saveData(d){ localStorage.setItem(STORAGE_KEY, JSON.stringify(d)); }
let data = loadData();
while(data.habits.length < SLOT_COUNT){ data.habits.push({ id:'slot'+data.habits.length, name:'', goal:26 }); }
if(!data.pending) data.pending = [];
while(data.pending.length < PENDING_COUNT){ data.pending.push({ id:'pend'+data.pending.length, name:'', count:0 }); }

function monthKey(y,m){ return y + '-' + String(m+1).padStart(2,'0'); }
function daysInMonth(y,m){ return new Date(y, m+1, 0).getDate(); }
function isToday(y,m,d){ return y===today.getFullYear() && m===today.getMonth() && d===today.getDate(); }
function isWeekend(y,m,d){ const dow = new Date(y,m,d).getDay(); return dow===0 || dow===6; }
const MONTH_NAMES = ['January','February','March','April','May','June','July','August','September','October','November','December'];
const WEEKDAY_FULL = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'];
document.getElementById('todayLabel').textContent =
  WEEKDAY_FULL[today.getDay()] + ', ' + MONTH_NAMES[today.getMonth()] + ' ' + today.getDate() + ', ' + today.getFullYear();

function ensureMonthChecks(){
  const key = monthKey(viewYear, viewMonth);
  if(!data.checks[key]) data.checks[key] = {};
  data.habits.forEach(h=>{ if(!data.checks[key][h.id]) data.checks[key][h.id] = []; });
}

function toggleCheck(habitId, dayIndex){
  const key = monthKey(viewYear, viewMonth);
  ensureMonthChecks();
  const arr = data.checks[key][habitId];
  arr[dayIndex] = !arr[dayIndex];
  saveData(data);
  render();
}
function updateHabitName(id, name){
  const h = data.habits.find(h=>h.id===id);
  if(h){ h.name = name; saveData(data); renderTopCards(daysInMonth(viewYear,viewMonth), monthKey(viewYear,viewMonth)); renderRanking(monthKey(viewYear,viewMonth)); renderTrend(daysInMonth(viewYear,viewMonth), monthKey(viewYear,viewMonth)); }
}
function updateHabitGoal(id, goal){
  const h = data.habits.find(h=>h.id===id);
  if(h){ h.goal = Math.min(31, Math.max(1, goal || 1)); saveData(data); render(); }
}
function clearHabit(id){
  const h = data.habits.find(h=>h.id===id);
  if(!h) return;
  h.name = '';
  h.goal = 26;
  Object.keys(data.checks).forEach(key=>{ if(data.checks[key][id]) data.checks[key][id] = []; });
  saveData(data);
  render();
}

function render(){
  ensureMonthChecks();
  document.getElementById('monthLabel').textContent = MONTH_NAMES[viewMonth] + ' ' + viewYear;
  const nDays = daysInMonth(viewYear, viewMonth);
  const key = monthKey(viewYear, viewMonth);

  const weekRow = document.getElementById('weekRow');
  weekRow.innerHTML = '<th></th><th></th>';
  let dayCursor = 1, weekNum = 1;
  while(dayCursor <= nDays){
    const span = Math.min(7, nDays - dayCursor + 1);
    const th = document.createElement('th');
    th.setAttribute('colspan', span);
    th.textContent = 'Week ' + weekNum;
    weekRow.appendChild(th);
    dayCursor += 7; weekNum++;
  }
  weekRow.innerHTML += '<th></th><th></th>';

  const headRow = document.getElementById('headRow');
  headRow.innerHTML = '<th class="col-name">Habit</th><th class="col-goal">Goal</th>';
  for(let d=1; d<=nDays; d++){
    const th = document.createElement('th');
    th.className = 'day-head' + (isWeekend(viewYear,viewMonth,d)?' weekend':'') + (isToday(viewYear,viewMonth,d)?' today':'');
    th.textContent = d;
    headRow.appendChild(th);
  }
  headRow.innerHTML += '<th class="col-progress" style="padding-left:14px;">Progress</th><th class="row-actions"></th>';

  const WEEKDAY_SHORT = ['Sun','Mon','Tue','Wed','Thu','Fri','Sat'];
  const weekdayRow = document.getElementById('weekdayRow');
  weekdayRow.innerHTML = '<th></th><th></th>';
  for(let d=1; d<=nDays; d++){
    const dow = new Date(viewYear, viewMonth, d).getDay();
    const th = document.createElement('th');
    th.className = (isWeekend(viewYear,viewMonth,d)?'weekend':'') + (isToday(viewYear,viewMonth,d)?' today':'');
    th.textContent = WEEKDAY_SHORT[dow];
    weekdayRow.appendChild(th);
  }
  weekdayRow.innerHTML += '<th></th><th></th>';

  const body = document.getElementById('habitBody');
  body.innerHTML = '';
  document.getElementById('emptyNote').style.display = 'none';

  data.habits.forEach((h, idx)=>{
    const tr = document.createElement('tr');
    const arr = data.checks[key][h.id];
    const completed = arr.filter(Boolean).length;
    const pct = Math.min(100, Math.round((completed / h.goal) * 100));

    let cellsHtml = '';
    for(let d=1; d<=nDays; d++){
      const checked = !!arr[d-1];
      cellsHtml += `<td><div class="cell-box ${checked?'checked':''}" data-habit="${h.id}" data-day="${d-1}"></div></td>`;
    }

    tr.innerHTML = `
      <td class="col-name"><input type="text" data-name="${h.id}" value="${escapeAttr(h.name)}" placeholder="Goal ${idx+1} — type here" maxlength="40"></td>
      <td class="col-goal"><input type="number" data-goal="${h.id}" value="${h.goal}" min="1" max="31"></td>
      ${cellsHtml}
      <td class="col-progress">
        <span class="progress-track"><span class="progress-fill" style="width:${pct}%"></span></span>
        <span class="progress-num">${completed}/${h.goal}</span>
      </td>
      <td class="row-actions"><button class="del-btn" data-clear="${h.id}" title="Clear this row">×</button></td>
    `;
    body.appendChild(tr);
  });

  body.querySelectorAll('.cell-box').forEach(box=>{
    box.addEventListener('click', ()=> toggleCheck(box.dataset.habit, parseInt(box.dataset.day)));
  });
  body.querySelectorAll('[data-clear]').forEach(btn=>{
    btn.addEventListener('click', ()=> clearHabit(btn.dataset.clear));
  });
  body.querySelectorAll('[data-name]').forEach(inp=>{
    inp.addEventListener('change', ()=> updateHabitName(inp.dataset.name, inp.value.trim()));
    inp.addEventListener('keydown', e=>{ if(e.key==='Enter') inp.blur(); });
  });
  body.querySelectorAll('[data-goal]').forEach(inp=>{
    inp.addEventListener('change', ()=> updateHabitGoal(inp.dataset.goal, parseInt(inp.value)));
  });

  renderTopCards(nDays, key);
  renderPieChart(key);
  renderRanking(key);
  renderTrend(nDays, key);
  renderPending();
}

function namedHabits(){ return data.habits.filter(h=>h.name && h.name.trim()!==''); }

function renderTopCards(nDays, key){
  const active = namedHabits();
  const container = document.getElementById('barsContainer');
  container.innerHTML = '';
  const totalHabits = active.length || 1;
  for(let d=1; d<=nDays; d++){
    let count = 0;
    active.forEach(h=>{ if(data.checks[key][h.id][d-1]) count++; });
    const heightPct = Math.max(2, Math.round((count/totalHabits)*100));
    const col = document.createElement('div');
    col.className = 'bar-col';
    const showLabel = (d % 5 === 0 || d===1 || d===nDays);
    col.innerHTML = `<div class="bar ${isToday(viewYear,viewMonth,d)?'today-bar':''}" style="height:${heightPct}%" title="${count}/${totalHabits} on day ${d}"></div>
      <div class="bar-day">${showLabel?d:''}</div>`;
    container.appendChild(col);
  }
}

const PIE_COLORS = ['#1b3358','#3d6fd6','#5aa9e6','#8ecae6','#3bb273','#f4a261','#e76f51','#9b5de5','#00b4d8','#ffb703','#fb8500','#219ebc','#8338ec','#ff5d73'];

function renderPieChart(key){
  const active = namedHabits();
  const svg = document.getElementById('pieSvg');
  const legend = document.getElementById('pieLegend');

  const counts = active.map(h => data.checks[key][h.id].filter(Boolean).length);
  const total = counts.reduce((a,b)=>a+b, 0);

  if(active.length===0 || total===0){
    svg.innerHTML = `<circle cx="70" cy="70" r="58" fill="none" stroke="#eef1f6" stroke-width="18"/>`;
    legend.innerHTML = '<p class="pie-empty">Tick a few days to see the breakdown here.</p>';
    return;
  }

  const cx = 70, cy = 70, r = 58;
  let startAngle = -90;
  let paths = '';
  active.forEach((h, i)=>{
    const value = counts[i];
    if(value===0) return;
    const sliceAngle = (value/total) * 360;
    const endAngle = startAngle + sliceAngle;
    const x1 = cx + r*Math.cos(Math.PI*startAngle/180);
    const y1 = cy + r*Math.sin(Math.PI*startAngle/180);
    const x2 = cx + r*Math.cos(Math.PI*endAngle/180);
    const y2 = cy + r*Math.sin(Math.PI*endAngle/180);
    const largeArc = sliceAngle > 180 ? 1 : 0;
    const color = PIE_COLORS[i % PIE_COLORS.length];
    paths += `<path d="M${cx},${cy} L${x1.toFixed(2)},${y1.toFixed(2)} A${r},${r} 0 ${largeArc} 1 ${x2.toFixed(2)},${y2.toFixed(2)} Z" fill="${color}" stroke="#fff" stroke-width="1.5"/>`;
    startAngle = endAngle;
  });
  paths += `<circle cx="${cx}" cy="${cy}" r="${r*0.5}" fill="#fff"/>`;
  svg.innerHTML = paths;

  legend.innerHTML = active.map((h,i)=>{
    const pct = Math.round((counts[i]/total)*100);
    if(counts[i]===0) return '';
    return `<div class="leg-row"><span class="leg-dot" style="background:${PIE_COLORS[i % PIE_COLORS.length]};"></span><span class="leg-name">${escapeHtml(h.name)}</span><span class="leg-pct">${pct}%</span></div>`;
  }).join('');
}


function renderRanking(key){
  const list = document.getElementById('rankList');
  list.innerHTML = '';
  const ranked = namedHabits().map(h=>{
    const arr = data.checks[key][h.id];
    const completed = arr.filter(Boolean).length;
    const pct = Math.min(100, Math.round((completed/h.goal)*100));
    return { name: h.name, completed, goal: h.goal, pct };
  }).sort((a,b)=> b.pct - a.pct).slice(0,10);

  if(ranked.length===0){
    list.innerHTML = '<p style="color:var(--muted);font-size:12.5px;">Type goals into the table to see rankings.</p>';
    return;
  }
  ranked.forEach((r,i)=>{
    const div = document.createElement('div');
    div.className = 'rank-item';
    div.innerHTML = `
      <div class="rank-num">${i+1}</div>
      <div class="rank-body">
        <div class="rank-name"><span>${escapeHtml(r.name)}</span><span class="pct">${r.completed}/${r.goal} · ${r.pct}%</span></div>
        <div class="rank-track"><div class="rank-fill" style="width:${r.pct}%"></div></div>
      </div>
    `;
    list.appendChild(div);
  });
}

function renderTrend(nDays, key){
  const svg = document.getElementById('trendSvg');
  const active = namedHabits();
  const totalHabits = active.length || 1;
  const points = [];
  for(let d=1; d<=nDays; d++){
    let count = 0;
    active.forEach(h=>{ if(data.checks[key][h.id][d-1]) count++; });
    const pct = (count/totalHabits)*100;
    const x = ((d-1)/(nDays-1||1)) * 300;
    const y = 95 - (pct/100)*85;
    points.push(x.toFixed(1)+','+y.toFixed(1));
  }
  const polyline = points.join(' ');
  const areaPath = `M0,95 L${polyline} L300,95 Z`;
  svg.innerHTML = `
    <path d="${areaPath}" fill="rgba(61,111,214,0.12)"/>
    <polyline points="${polyline}" fill="none" stroke="#3d6fd6" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
  `;
}

function escapeHtml(str){
  const d = document.createElement('div');
  d.textContent = str;
  return d.innerHTML;
}
function escapeAttr(str){
  return String(str).replace(/&/g,'&amp;').replace(/"/g,'&quot;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
}

function renderPending(justTickedId){
  const body = document.getElementById('pendingBody');
  body.innerHTML = data.pending.map((p, idx)=>{
    const flash = p.id === justTickedId;
    const count = p.count || 0;
    return `<tr>
      <td class="pending-row-name"><input type="text" data-pname="${p.id}" value="${escapeAttr(p.name)}" placeholder="Pending task ${idx+1} — type here" maxlength="40"></td>
      <td class="pending-row-tick"><div class="cell-box ${flash?'checked':''}" data-ptoggle="${p.id}"></div></td>
      <td class="pending-row-count">Completed ${count} ${count===1?'time':'times'}</td>
    </tr>`;
  }).join('');

  body.querySelectorAll('[data-ptoggle]').forEach(box=>{
    box.addEventListener('click', ()=> togglePending(box.dataset.ptoggle));
  });
  body.querySelectorAll('[data-pname]').forEach(inp=>{
    inp.addEventListener('change', ()=> updatePendingName(inp.dataset.pname, inp.value.trim()));
    inp.addEventListener('keydown', e=>{ if(e.key==='Enter') inp.blur(); });
  });

  if(justTickedId){
    setTimeout(()=> renderPending(null), 350);
  }
}

function togglePending(id){
  const p = data.pending.find(p=>p.id===id);
  if(!p) return;
  p.count = (p.count || 0) + 1;
  saveData(data);
  renderPending(id);
}

function updatePendingName(id, name){
  const p = data.pending.find(p=>p.id===id);
  if(p){ p.name = name; saveData(data); }
}


function changeMonth(delta){
  const gs = document.querySelector('.grid-scroll');
  if(gs){
    gs.style.opacity = 0;
    setTimeout(()=>{
      viewMonth += delta;
      if(viewMonth<0){ viewMonth=11; viewYear--; }
      if(viewMonth>11){ viewMonth=0; viewYear++; }
      render();
      requestAnimationFrame(()=>{ document.querySelector('.grid-scroll').style.opacity = 1; });
    }, 140);
  } else {
    viewMonth += delta;
    if(viewMonth<0){ viewMonth=11; viewYear--; }
    if(viewMonth>11){ viewMonth=0; viewYear++; }
    render();
  }
}
document.getElementById('prevMonth').addEventListener('click', ()=> changeMonth(-1));
document.getElementById('nextMonth').addEventListener('click', ()=> changeMonth(1));

render();

const NAME_KEY = 'habitTrackerUserName';
function setTitleWithName(name){
  const title = document.getElementById('pageTitle');
  if(!name){ title.textContent = 'Habit Tracker'; return; }
  const displayName = name.trim().toLowerCase() === 'saumya' ? 'Boss' : name;
  title.textContent = `Let's do it, ${displayName}!`;
}
function showNameOverlay(show){
  const overlay = document.getElementById('nameOverlay');
  if(show){ overlay.classList.remove('hidden'); }
  else { overlay.classList.add('hidden'); }
}

const savedName = localStorage.getItem(NAME_KEY);
if(savedName){
  setTitleWithName(savedName);
  showNameOverlay(false);
} else {
  showNameOverlay(true);
  setTimeout(()=> document.getElementById('nameInput').focus(), 200);
}

function submitName(){
  const input = document.getElementById('nameInput');
  const name = input.value.trim();
  if(!name) { input.focus(); return; }
  localStorage.setItem(NAME_KEY, name);
  setTitleWithName(name);
  showNameOverlay(false);
}
document.getElementById('nameSubmit').addEventListener('click', submitName);
document.getElementById('nameInput').addEventListener('keydown', e=>{ if(e.key==='Enter') submitName(); });

document.getElementById('settingsBtn').addEventListener('click', ()=>{
  const input = document.getElementById('nameInput');
  input.value = localStorage.getItem(NAME_KEY) || '';
  showNameOverlay(true);
  setTimeout(()=> input.focus(), 150);
});
</script>
</body>
</html>
