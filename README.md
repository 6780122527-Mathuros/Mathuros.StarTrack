<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>StarTrack DEMO</title>

  <style>
    body {
      font-family: 'Sarabun', Arial, sans-serif;
      margin: 0;
      background: linear-gradient(135deg, #f4eaff, #d3ecfd);
      color: #444;
    }
    header {
      text-align: center;
      padding: 1.2em 0;
      background: #fcecfb;
      border-bottom: 2px solid #e5d9f7;
    }
    h1 { color: #a645ae; margin: 0.2em 0; }
    nav {
      padding: 1em;
      text-align: center;
      background: #f2f7fd;
      position: sticky;
      top: 0;
      z-index: 100;
    }
    .rolebtn {
      background: #e9dfff;
      color: #86398e;
      font-size: 1.1em;
      border: none;
      padding: .7em 1.7em;
      border-radius: 10px;
      margin: .3em;
      cursor: pointer;
    }
    .rolebtn:hover { background: #e4e5ff; }
    section {
      max-width: 900px;
      margin: 1.5em auto;
      background: #fff;
      padding: 1.8em;
      border-radius: 20px;
      box-shadow: 0 4px 20px #dcdcff;
    }
    .box {
      background: #f7f9fd;
      padding: 1.2em;
      border-radius: 15px;
      margin-bottom: 1em;
    }
    .emotion-btns button {
      font-size: 1.5em;
      margin: 0.2em;
      padding: .2em .5em;
      border-radius: 50%;
      border: 1px solid #ccc;
      background: #fff;
      cursor: pointer;
    }
    .emotion-btns button.selected {
      background: #ffd7ef;
      border-color: #d14da5;
    }
    textarea {
      width: 100%;
      height: 90px;
      padding: .7em;
      border-radius: 10px;
      border: 1px solid #d5d5ff;
      background: #fff5fb;
    }
    .btn-main {
      background: #a651b1;
      color: #fff;
      border: none;
      padding: .7em 1.5em;
      border-radius: 10px;
      cursor: pointer;
      margin-top: .6em;
    }
    .btn-main:hover { background: #cda8e6; color:#5e1a6e; }
    .diary-entry {
      background: #f3f1fc;
      padding: .7em;
      border-radius: 10px;
      margin: .5em 0;
    }
    .diary-del {
      float: right;
      background: #ffd4e5;
      padding: .2em .6em;
      border-radius: 7px;
      cursor: pointer;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: .5em;
      background: #fafaff;
    }
    th, td {
      border: 1px solid #ccc;
      padding: .5em;
      text-align: center;
    }
    th { background: #e9dfff; }
  </style>

  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>

<body>

<header>
  <h1>StarTrack DEMO</h1>
  <div style="color:#94488f">ระบบติดตามอารมณ์ & ดาวเด็กดี</div>
</header>

<nav>
  <button class="rolebtn" onclick="switchRole('student')">👦 นักเรียน</button>
  <button class="rolebtn" onclick="switchRole('teacher')">👩‍🏫 ครู</button>
  <button class="rolebtn" onclick="switchRole('admin')">🏫 ผู้บริหาร</button>
</nav>

<!-- นักเรียน -->
<section id="student-section" style="display:none">
  <h2>👦 นักเรียน</h2>

  <div class="box">
    <h3>เลือกอารมณ์วันนี้</h3>

    <div class="emotion-btns">
      <button onclick="selectEmotion('happy')">😄</button>
      <button onclick="selectEmotion('normal')">😐</button>
      <button onclick="selectEmotion('sad')">😢</button>
      <button onclick="selectEmotion('angry')">😡</button>
      <button onclick="selectEmotion('surprise')">😲</button>
    </div>

    <button class="btn-main" onclick="saveEmotion()">บันทึกอารมณ์</button>
    <div id="emotion-msg"></div>
  </div>

  <div class="box">
    <h3>ไดอารี่</h3>
    <textarea id="diary-text"></textarea>
    <button class="btn-main" onclick="saveDiary()">บันทึกไดอารี่</button>

    <div id="diary-list"></div>
  </div>

  <div class="box">
    <h3>⭐ ดาวเด็กดี</h3>
    <button class="btn-main" onclick="addStar()">เพิ่มดาว</button>
    <div id="star-count" style="margin-top:.6em;font-size:1.2em;color:#a459c7"></div>
  </div>
</section>

<!-- ครู -->
<section id="teacher-section" style="display:none">
  <h2>👩‍🏫 ครู</h2>

  <div class="box">
    <h3>รายชื่อนักเรียน</h3>
    <table id="student-table">
      <tr>
        <th>ชื่อนักเรียน</th>
        <th>อารมณ์ล่าสุด</th>
        <th>ดาว</th>
        <th>ดูเพิ่ม</th>
      </tr>
    </table>
  </div>

  <div class="box">
    <h3>ไดอารี่ของนักเรียน</h3>
    <div id="teacher-diary"></div>
  </div>
</section>

<!-- ผู้บริหาร -->
<section id="admin-section" style="display:none">
  <h2>🏫 ผู้บริหาร</h2>

  <div class="box">
    <h3>สถิติอารมณ์รวม</h3>
    <canvas id="emotion-chart" width="300" height="300"></canvas>
  </div>

  <div class="box">
    <h3>ตารางดาวเด็กดี</h3>

    <table id="admin-star-table">
      <tr>
        <th>ชื่อนักเรียน</th>
        <th>ดาว</th>
      </tr>
    </table>
  </div>
</section>


<script>
/* -------------------------------
     ระบบจัดเก็บข้อมูล
--------------------------------*/
let data = JSON.parse(localStorage.getItem("startrack")) || {
  students: {
    "เด็ก A": {emotion: "", diary: [], stars: 0},
    "เด็ก B": {emotion: "", diary: [], stars: 0},
    "เด็ก C": {emotion: "", diary: [], stars: 0}
  }
};

let selectedEmotion = "";

/* -------------------------------
     ฟังก์ชันเปลี่ยนหน้า
--------------------------------*/
function switchRole(role) {
  document.getElementById("student-section").style.display = "none";
  document.getElementById("teacher-section").style.display = "none";
  document.getElementById("admin-section").style.display = "none";

  document.getElementById(role + "-section").style.display = "block";

  if (role === "teacher") loadTeacherData();
  if (role === "admin") loadAdminData();
  if (role === "student") loadStudentData();
}

/* -------------------------------
      นักเรียน
--------------------------------*/
function selectEmotion(e) {
  selectedEmotion = e;

  document.querySelectorAll(".emotion-btns button").forEach(btn=>{
    btn.classList.remove("selected")
  });

  event.target.classList.add("selected");
}

function saveEmotion() {
  data.students["เด็ก A"].emotion = selectedEmotion;
  localStorage.setItem("startrack", JSON.stringify(data));
  document.getElementById("emotion-msg").innerHTML = "✔ บันทึกแล้ว!";
}

function saveDiary() {
  let txt = document.getElementById("diary-text").value;
  if (!txt) return;

  data.students["เด็ก A"].diary.push({
    text: txt,
    date: new Date().toLocaleString()
  });

  localStorage.setItem("startrack", JSON.stringify(data));

  document.getElementById("diary-text").value = "";
  loadStudentData();
}

function loadStudentData() {
  document.getElementById("star-count").innerHTML =
    "จำนวนดาว: " + data.students["เด็ก A"].stars;

  let d = data.students["เด็ก A"].diary;
  let html = "";

  d.forEach((x,i)=>{
    html += `
      <div class="diary-entry">
        <div><b>${x.date}</b></div>
        ${x.text}
        <span class="diary-del" onclick="deleteDiary(${i})">ลบ</span>
      </div>
    `;
  });

  document.getElementById("diary-list").innerHTML = html;
}

function deleteDiary(i) {
  data.students["เด็ก A"].diary.splice(i,1);
  localStorage.setItem("startrack", JSON.stringify(data));
  loadStudentData();
}

function addStar() {
  data.students["เด็ก A"].stars++;
  localStorage.setItem("startrack", JSON.stringify(data));
  loadStudentData();
}

/* -------------------------------
      ครู
--------------------------------*/
function loadTeacherData() {
  let table = document.getElementById("student-table");
  table.innerHTML = `
    <tr>
      <th>ชื่อนักเรียน</th>
      <th>อารมณ์ล่าสุด</th>
      <th>ดาว</th>
      <th>ดูเพิ่ม</th>
    </tr>
  `;

  for (let s in data.students) {
    let stu = data.students[s];

    table.innerHTML += `
      <tr>
        <td>${s}</td>
        <td>${stu.emotion || "-"}</td>
        <td>${stu.stars}</td>
        <td><button onclick="showDiary('${s}')">ดู</button></td>
      </tr>
    `;
  }
}

function showDiary(name) {
  let html = `<h4>${name}</h4>`;

  data.students[name].diary.forEach(x=>{
    html += `
      <div class="diary-entry">
        <b>${x.date}</b><br>
        ${x.text}
      </div>
    `;
  });

  document.getElementById("teacher-diary").innerHTML = html;
}

/* -------------------------------
      ผู้บริหาร
--------------------------------*/
function loadAdminData() {
  loadAdminStarTable();
  drawEmotionChart();
}

function loadAdminStarTable() {
  let table = document.getElementById("admin-star-table");
  table.innerHTML = `
    <tr><th>ชื่อนักเรียน</th><th>ดาว</th></tr>
  `;

  for (let s in data.students) {
    table.innerHTML += `
      <tr>
        <td>${s}</td>
        <td>${data.students[s].stars}</td>
      </tr>
    `;
  }
}

function drawEmotionChart() {
  let counts = {happy:0,normal:0,sad:0,angry:0,surprise:0};

  for (let s in data.students) {
    let e = data.students[s].emotion;
    if (e) counts[e]++;
  }

  new Chart(document.getElementById("emotion-chart"), {
    type: "pie",
    data: {
      labels: ["ดีใจ","ปกติ","เศร้า","โกรธ","ประหลาดใจ"],
      datasets: [{
        data: [
          counts.happy,
          counts.normal,
          counts.sad,
          counts.angry,
          counts.surprise
        ],
        backgroundColor: ["#ffc2df","#b1e5e0","#ffd480","#ff9999","#cdb6ff"]
      }]
    }
  });
}
</script>

</body>
</html>
