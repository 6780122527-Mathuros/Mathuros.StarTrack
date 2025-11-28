
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>StarTrack DEMO</title>
<style>
body {
font-family: 'Sarabun', Arial, sans-serif;
background: linear-gradient(135deg,#f4eaff,#d3ecfd);
color: #444;
margin: 0;
}
header {
text-align: center;
background: #fcecfb;
border-bottom:2px solid #e5d9f7;
padding-top:1.7em; padding-bottom:.3em;
}
h1 { color: #a645ae; margin:1.5em 0 .1em 0;}
nav { text-align:center; padding:1.1em; background:#f2f7fd; }
.rolebtn {
background:#e9dfff;
color: #86398e;
font-size:1.19em;
border:none;
border-radius:11px;
padding:.8em 2.2em;
margin:.4em;
cursor:pointer;
}
.rolebtn:hover {background: #e4e5ff;}
section {
max-width: 930px;
margin: 2em auto;
background: #fffefe;
border-radius: 23px;
padding:2em 2.2em;
box-shadow: 0 4px 25px #e4eaf4cc;
}
h2 {
color:#a645ae;
margin-top:0;
letter-spacing:.03em;
}
.box {
background: #f7f9fd;
border-radius: 15px;
padding:1.35em 2em;
margin-bottom:2em;
box-shadow:0 1px 18px #e7e1fa60;
}
.box h3 {color:#7193a6;}
.star { color: #ffe780; font-size:1.4em;}
.good {color:#399d2f;}
.bad {color:#d04a6a;}
.stats-table, .stats-table th, .stats-table td {
border: 1px solid #ccc;
border-collapse: collapse;
padding: .34em .65em;
}
.stats-table {
margin-top: .7em;
width: 100%;
background: #f2f7fd;
}
.stats-table th { background: #f9e3ff;}
.emotion-btns button {
margin:.4em .13em;
font-size:1.22em;
padding:.19em .44em;
border-radius:50%;
border: 1.3px solid #b6c5ee;
background:#e6f6ff;
}
.emotion-btns button.selected,
.emotion-btns button:hover {
background:#ffd7ef;
border-color:#bb5ecf;
}
textarea, select, input[type="text"], input[type="number"], input[type="datetime-local"] {
width: 100%;
padding:.7em;
margin:.13em 0;
border-radius: 7px;
border: 1.25px solid #d3ecfd;
background: #fff6f8;
}
.btn-main, .test-btn, .appoint-btn {
background: #a651b1;
color: #fff;
border: none;
border-radius: 9px;
font-weight:bold;
font-size:1.07em;
padding:.7em 2em;
margin:.8em 0;
box-shadow:0 1px 7px #ede0fb40;
}
.btn-main:hover, .test-btn:hover, .appoint-btn:hover {background:#e6c6f5; color:#640a73;}
.test-btn{background:#b1e5e0;color:#446e67; margin:0 .7em 0 0;}
.appoint-btn{background:#ffc285;color:#764108;}
.diary-list {margin:1em 0;}
.diary-entry {
background: #f3f1fc;
border-radius:10px;
margin-bottom:.5em;
padding:.7em 1em;
}
.diary-date {font-size:.93em; color:#ae8cc1;display:block;}
.diary-del-btn {
float:right;
background: #ffd7ef;
color: #d13d84;
border-radius: 6px;
padding: .08em .7em;
}
.reward-box {
background: #e6f6ff;
border-radius:11px;
padding:1em 1em;
margin:1em 0;
color:#608498;
}
.test-res {
margin:1em 0;
background: #d9fff5;
color:#575;
border-radius:.7em;
padding:1em;
}
.piebox {text-align:center; background:#f2f7fd; border-radius:12px;margin:1em 0;}
.historylist {
background:#e6f6ff;
border-radius:10px;
padding:.8em;
}
.msgbox {
background:#f1ffea;
padding:1em 1.5em;
border-radius:10px;
margin-top:.9em;
}
.msg-entry {margin-bottom:.68em;}
.logout-btn {
background: #e67c96;
color: #fff;
border: none;
border-radius: 8px;
padding:.7em 1.3em;
font-weight:bold;
}
@media (max-width:600px){
section,.box{padding:.6em .2em;}
.piebox,canvas{width:100%!important;}
nav{padding:0;}
}
</style>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
<header>
<h1>StarTrack DEMO</h1>
<div style="color: #a14f88;">ระบบติดตามอารมณ์และดาวเด็กดี</div>
</header>
<nav>
<button class="rolebtn" onclick="switchRole('student')">👦 นักเรียน</button>
<button class="rolebtn" onclick="switchRole('teacher')">👩‍🏫 ครู</button>
<button class="rolebtn" onclick="switchRole('admin')">🏫 ผู้บริหาร</button>
<button class="btn-main" style="float:right" onclick="location.reload()">ออกจากระบบ</button>
</nav>
<!-- student-section/teacher-section/admin-section structure ไม่เปลี่ยน, ใช้ code ล่าสุดในแชท -->
<section id="student-section" style="display:none">
<!-- ... (เหมือน code ล่าสุดในแชท) ... -->
<div class="box">[คัดลอกโค้ดฟีเจอร์ student จาก code ล่าสุดในแชท]</div>
</section>
<section id="teacher-section" style="display:none">
<!-- ... (เหมือน code ล่าสุดในแชท) ... -->
<div class="box">[คัดลอกโค้ดฟีเจอร์ teacher จาก code ล่าสุดในแชท]</div>
</section>
<section id="admin-section" style="display:none">
<!-- ... (เหมือน code ล่าสุดในแชท) ... -->
<div class="box">[คัดลอกโค้ดฟีเจอร์ admin จาก code ล่าสุดในแชท]</div>
</section>
<script>
// (ใช้สคริปต์ชุดเดิมของ code ล่าสุดในแชท, สามารถคัดลอกมาโดยตรง)
// ใน Pie Chart (Chart.js) มีพื้นฐานสีเช่น ["#b1e5e0", "#a651b1", "#ffd7ef", "#ffe780", "#bfffa5", "#8dd6ee"]
</script>
</body>
</html>
