<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Study Planner</title>

<style>
/* ---------- GLOBAL ---------- */
*{margin:0;padding:0;box-sizing:border-box;font-family:Arial, sans-serif;}
body{
  background-image: linear-gradient(rgba(0,0,0,0.5), rgba(0,0,0,0.5)), url('1000015849.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  background-attachment: fixed;
  color: white;
}
section{padding:60px 20px;}
.hidden{display:none;}
h1,h2{margin-bottom:15px;}
button{cursor:pointer}

/* ---------- NAVBAR ---------- */
nav{
  background:#1e3a8a;
  color:#fff;
  padding:15px 20px;
  display:flex;
  justify-content:space-between;
  align-items:center;
}
nav .logo{font-weight:bold;font-size:20px;}
nav ul{display:flex;gap:20px;list-style:none;}
nav ul li{cursor:pointer;}
nav ul li:hover{text-decoration:underline;}
.menu-btn{display:none;font-size:24px;cursor:pointer;}

@media(max-width:768px){
  nav ul{
    position:absolute;
    top:60px;
    left:0;
    width:100%;
    background:#1e3a8a;
    flex-direction:column;
    display:none;
  }
  nav ul.show{display:flex;}
  .menu-btn{display:block;}
}

/* ---------- HOME ---------- */
.hero{
  text-align:center;
}
.hero h1{
  font-size:32px;
  color:white;
  text-shadow:2px 2px 5px black;
}
.hero p{
  margin:15px 0;
  color:white;
  text-shadow:2px 2px 5px black;
}
.hero input{
  padding:10px;width:80%;max-width:400px;
}
.hero button{
  padding:10px 15px;
  margin:10px;
  background:#1e3a8a;
  color:white;border:none;
}

/* ---------- FORM ---------- */
form input, form select, form textarea{
  width:100%;
  padding:10px;
  margin:8px 0;
}
form button{
  background:#1e3a8a;
  color:#fff;
  padding:10px;
  border:none;
  margin-top:10px;
}

/* ---------- DASHBOARD ---------- */
.card{
  background:rgba(255,255,255,0.9);
  color:#333;
  padding:20px;
  border-radius:8px;
  margin-bottom:20px;
}
#about,#contact,#dashboard{
  background:rgba(255,255,255,0.85);
  color:#333;
  border-radius:10px;
  margin:20px;
}

/* ---------- TIMETABLE ---------- */
.timetable div{
  background:#e0e7ff;
  padding:10px;
  margin:5px 0;
}

/* ---------- CHATBOX ---------- */
.chatbox{
  position:fixed;
  bottom:20px;
  right:20px;
  width:280px;
  background:#fff;
  border-radius:8px;
  box-shadow:0 0 10px rgba(0,0,0,0.2);
}
.chatbox-header{
  background:#1e3a8a;
  color:#fff;
  padding:10px;
}
.chatbox-body{
  height:200px;
  padding:10px;
  overflow-y:auto;
}
.chatbox input{
  width:100%;
  padding:8px;
  border:none;
  border-top:1px solid #ccc;
}
</style>

</head>

<body>

<!-- NAVBAR -->
<nav>
  <div class="logo">AI Study Planner</div>
  <div class="menu-btn" onclick="toggleMenu()">☰</div>
  <ul id="menu">
    <li onclick="showPage('home')">Home</li>
    <li onclick="showPage('about')">About</li>
    <li onclick="showPage('dashboard')">Dashboard</li>
    <li onclick="showPage('contact')">Contact</li>
  </ul>
</nav>

<!-- HOME -->
<section id="home">
  <div class="hero">
    <h1>AI Study Planner – Smart Timetable Generator</h1>
    <p>Plan your studies smarter using AI-based scheduling.</p>
    <input type="text" placeholder="Search subjects, features or help">
    <br>
    <button onclick="showPage('dashboard')">Create Study Plan</button>
    <button onclick="showPage('dashboard')">Get Started Free</button>
  </div>
</section>

<!-- ABOUT -->
<section id="about" class="hidden">
  <h2>About AI Study Planner</h2>
  <p>This platform helps college students automatically generate smart study timetables.</p>
  <p><b>AI Logic:</b> Subject difficulty, priority and available study hours.</p>
  <p><b>Mission:</b> Reduce stress and improve academic performance.</p>
  <p><b>Vision:</b> Smart learning for every student.</p>
</section>

<!-- DASHBOARD -->
<section id="dashboard" class="hidden">
  <h2>Welcome Student 👋</h2>

  <div class="card">
    <h3>Study Planner Form</h3>
    <form onsubmit="generateTimetable(event)">
      <select id="level">
        <option>College</option>
        <option>School</option>
      </select>
      <input type="text" id="degree" placeholder="Class / Degree" required>
      <input type="number" id="hours" placeholder="Daily Study Hours" required>
      <input type="date" id="exam" required>

      <h4>Subject</h4>
      <input type="text" id="subject" placeholder="Subject Name" required>
      <select id="difficulty">
        <option>Easy</option>
        <option>Medium</option>
        <option>Hard</option>
      </select>
      <select id="priority">
        <option>Low</option>
        <option>Medium</option>
        <option>High</option>
      </select>
      <textarea placeholder="Notes (optional)"></textarea>

      <button type="submit">Generate Timetable</button>
    </form>
  </div>

  <div class="card timetable" id="timetable">
    <h3>Your Timetable</h3>
  </div>
</section>

<!-- CONTACT -->
<section id="contact" class="hidden">
  <h2>Contact Us</h2>
  <form onsubmit="submitContact(event)">
    <input type="text" placeholder="Name" required>
    <input type="email" placeholder="Email" required>
    <input type="text" placeholder="Subject" required>
    <textarea placeholder="Message" required></textarea>
    <button type="submit">Submit</button>
  </form>
  <p id="success" style="color:green;"></p>
</section>

<!-- CHATBOX -->
<div class="chatbox">
  <div class="chatbox-header">AI Assistant</div>
  <div class="chatbox-body" id="chat"></div>
  <input type="text" placeholder="Ask me..." onkeypress="chat(event)">
</div>

<script>
/* ---------- NAV ---------- */
function toggleMenu(){
  document.getElementById("menu").classList.toggle("show");
}
function showPage(page){
  document.querySelectorAll("section").forEach(s=>s.classList.add("hidden"));
  document.getElementById(page).classList.remove("hidden");
}

/* ---------- TIMETABLE ---------- */
function generateTimetable(e){
  e.preventDefault();
  const hours=document.getElementById("hours").value;
  const subject=document.getElementById("subject").value;
  const difficulty=document.getElementById("difficulty").value;
  const priority=document.getElementById("priority").value;

  let time=hours;
  if(difficulty==="Hard") time+=1;
  if(priority==="High") time+=1;

  document.getElementById("timetable").innerHTML=`
    <h3>Your Timetable</h3>
    <div>${subject} – ${time} hrs</div>
    <div>Short Break – 10 mins</div>
    <div>Revision Slot – 30 mins</div>
    <div>Weekly Mock Test – Sunday</div>
  `;
}

/* ---------- CONTACT ---------- */
function submitContact(e){
  e.preventDefault();
  document.getElementById("success").innerText="Message sent successfully!";
}

/* ---------- CHAT ---------- */
function chat(e){
  if(e.key==="Enter"){
    const chat=document.getElementById("chat");
    chat.innerHTML+=`<p><b>You:</b> ${e.target.value}</p>`;
    chat.innerHTML+=`<p><b>AI:</b> Stay consistent and revise daily 📘</p>`;
    e.target.value="";
    chat.scrollTop=chat.scrollHeight;
  }
}
</script>

</body>
</html>
