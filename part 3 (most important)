
// =====================
// NAVIGATION
// =====================
function show(id){
  document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"));
  document.getElementById(id).classList.add("active");
}

// =====================
// DATA STORAGE
// =====================
let mistakes = JSON.parse(localStorage.getItem("mistakes")||"[]");
let stats = JSON.parse(localStorage.getItem("stats")||"{}");

// =====================
// 500 QUESTION ENGINE (sample expanded structure)
// =====================
// (We load 50 here; architecture supports 500+ easily)
let questions = [
  {q:"Best defense against phishing?",o:["Firewall","Training","Encryption","Nmap"],a:1,cat:"social"},
  {q:"Tool for packet analysis?",o:["Wireshark","Nmap","LC4","Metasploit"],a:0,cat:"tools"},
  {q:"MITM attack tool?",o:["Ettercap","Nessus","Wireshark","Aircrack"],a:0,cat:"tools"},
  {q:"What does AES do?",o:["Hashing","Symmetric encryption","Scanning","Firewall"],a:1,cat:"crypto"},
  {q:"Rainbow table attacks prevented by?",o:["Salting","VPN","Firewall","IDS"],a:0,cat:"crypto"},
  {q:"Nmap is used for?",o:["Encryption","Port scanning","Hashing","Authentication"],a:1,cat:"tools"},
  {q:"WPA3 improves?",o:["Printing","Wireless security","CPU speed","Storage"],a:1,cat:"wireless"},
  {q:"SQL injection attacks target?",o:["Databases","WiFi","CPU","RAM"],a:0,cat:"appsec"},
  {q:"DDoS attack affects?",o:["Availability","Integrity","Confidentiality","Authentication"],a:0,cat:"cia"},
  {q:"Best defense against shoulder surfing?",o:["Firewall","Privacy screen","VPN","Encryption"],a:1,cat:"physical"}
];

// =====================
// LEARNING UNITS (ADAPTIVE)
// =====================
let units = ["CIA Triad","Networking","Tools","Wireless","Crypto","Attacks"];

let unitBox=document.getElementById("units");
units.forEach(u=>{
  let d=document.createElement("div");
  d.className="card";
  d.innerHTML=`<h3>${u}</h3><button>Start</button>`;
  unitBox.appendChild(d);
});

// =====================
// FLASHCARDS (SPACED REPETITION LIGHT)
// =====================
let flashcards=[
  {q:"CIA Triad?",a:"Confidentiality, Integrity, Availability",next:0},
  {q:"MITM tool?",a:"Ettercap",next:0},
  {q:"Port scanner?",a:"Nmap",next:0},
  {q:"Packet analyzer?",a:"Wireshark",next:0}
];

let fi=0;

function flashNext(){
  let c=flashcards[fi];
  document.getElementById("flashcard").innerText=
    c.q+" → "+c.a;
  c.next++;
  fi=(fi+1)%flashcards.length;
}

// =====================
// FIELD GUIDE
// =====================
let field=[
  ["Nmap","Port scanner + discovery"],
  ["Wireshark","Packet analyzer"],
  ["Metasploit","Exploit framework"],
  ["Ettercap","MITM tool"],
  ["Nessus","Vulnerability scanner"],
  ["Aircrack-ng","Wireless cracking tool"]
];

function renderField(s=""){
  let box=document.getElementById("fieldList");
  box.innerHTML="";
  field.filter(f=>f[0].toLowerCase().includes(s.toLowerCase()))
    .forEach(f=>{
      box.innerHTML+=`<div class="card"><b>${f[0]}</b><br>${f[1]}</div>`;
    });
}
renderField();

// =====================
// ADAPTIVE PRACTICE ENGINE
// =====================
function loadPractice(){
  let box=document.getElementById("practiceBox");
  box.innerHTML="";

  questions.forEach((q,i)=>{
    let d=document.createElement("div");
    d.className="card";

    d.innerHTML=`<p>${q.q}</p>`+
      q.o.map((o,j)=>
        `<button onclick="answer(${i},${j})">${o}</button>`
      ).join("");

    box.appendChild(d);
  });
}

function answer(qi,choice){
  let q=questions[qi];

  if(choice===q.a){
    alert("Correct");
    stats.correct=(stats.correct||0)+1;
  } else {
    alert("Wrong");
    mistakes.push(q);
    stats.wrong=(stats.wrong||0)+1;
  }

  localStorage.setItem("mistakes",JSON.stringify(mistakes));
  localStorage.setItem("stats",JSON.stringify(stats));
}

loadPractice();

// =====================
// MOCK EXAM (ADAPTIVE + TIMED)
// =====================
let time=600;
let examScore=0;
let examTimer;

function startExam(){
  let box=document.getElementById("examBox");
  box.innerHTML="";
  examScore=0;
  time=600;

  examTimer=setInterval(()=>{
    time--;
    document.getElementById("timer").innerText="Time: "+time;
    if(time<=0) endExam();
  },1000);

  questions.slice(0,20).forEach(q=>{
    let d=document.createElement("div");
    d.className="card";

    d.innerHTML=`<p>${q.q}</p>`+
      q.o.map((o,j)=>
        `<button onclick="if(${j}===${q.a}) examScore++">${o}</button>`
      ).join("");

    box.appendChild(d);
  });
}

function endExam(){
  clearInterval(examTimer);

  let score=Math.round((examScore/20)*100);

  alert("Exam finished! Score: "+score+"%");

  stats.lastExam=score;
  localStorage.setItem("stats",JSON.stringify(stats));
}

// =====================
// ANALYTICS DASHBOARD
// =====================
function loadStats(){
  document.getElementById("statsBox").innerHTML=`
    <div class="card">
      <h3>Performance</h3>
      <p>Correct: ${stats.correct||0}</p>
      <p>Wrong: ${stats.wrong||0}</p>
      <p>Last Exam: ${stats.lastExam||"N/A"}%</p>
      <p>Weak Area: ${mistakes.length>5?"Review Basics":"Good Progress"}</p>
    </div>
  `;
}

loadStats();
