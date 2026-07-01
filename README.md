<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Field Survey of the Self — Personality Test</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,500;9..144,600&family=Libre+Franklin:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root{
    --paper:#ECEAE2;
    --paper-deep:#E2DFD3;
    --ink:#1C2541;
    --ink-soft:#4A5066;
    --rust:#B8412A;
    --teal:#3E6B63;
    --gold:#B8893D;
    --line:#C9C2B2;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:var(--paper);
    color:var(--ink);
    font-family:'Libre Franklin',sans-serif;
    min-height:100vh;
    -webkit-font-smoothing:antialiased;
    overflow-x:hidden;
  }
  .bg-contours{
    position:fixed; inset:0; z-index:0; pointer-events:none; opacity:0.5;
  }
  .stage{
    position:relative; z-index:1;
    min-height:100vh;
    display:flex; align-items:center; justify-content:center;
    padding:28px 16px 48px;
  }
  .card{
    width:100%; max-width:480px;
    background:var(--paper-deep);
    border:1px solid var(--line);
    border-radius:4px;
    padding:32px 24px;
    position:relative;
  }
  .card::before{
    content:"";
    position:absolute; inset:8px;
    border:1px solid var(--line);
    border-radius:2px;
    pointer-events:none;
    opacity:0.6;
  }
  .eyebrow{
    font-family:'IBM Plex Mono',monospace;
    font-size:11px; letter-spacing:0.14em; text-transform:uppercase;
    color:var(--rust); font-weight:500;
    margin:0 0 14px;
  }
  h1.display{
    font-family:'Fraunces',serif;
    font-weight:600;
    font-size:34px;
    line-height:1.08;
    margin:0 0 14px;
    color:var(--ink);
  }
  .lede{
    font-size:15.5px; line-height:1.55; color:var(--ink-soft);
    margin:0 0 8px;
  }
  .meta-line{
    font-family:'IBM Plex Mono',monospace;
    font-size:11px; color:var(--ink-soft); letter-spacing:0.04em;
    margin:18px 0 26px;
  }
  button{
    font-family:inherit; cursor:pointer; border:none; background:none;
  }
  button:focus-visible, .opt-row:focus-visible{
    outline:2px solid var(--rust); outline-offset:2px;
  }
  .btn-primary{
    width:100%;
    background:var(--ink);
    color:var(--paper);
    font-family:'IBM Plex Mono',monospace;
    font-size:13px; letter-spacing:0.06em; text-transform:uppercase;
    padding:16px 18px;
    border-radius:2px;
    display:flex; align-items:center; justify-content:space-between;
    transition:background 0.15s ease;
  }
  .btn-primary:hover{ background:var(--rust); }
  .btn-ghost{
    width:100%;
    background:transparent;
    color:var(--ink);
    border:1px solid var(--ink);
    font-family:'IBM Plex Mono',monospace;
    font-size:13px; letter-spacing:0.06em; text-transform:uppercase;
    padding:14px 18px;
    border-radius:2px;
    margin-top:10px;
  }
  .btn-ghost:hover{ background:var(--ink); color:var(--paper); }

  /* Quiz */
  .quiz-head{
    display:flex; align-items:center; justify-content:space-between;
    margin-bottom:10px;
  }
  .back-btn{
    font-family:'IBM Plex Mono',monospace; font-size:12px; color:var(--ink-soft);
    display:flex; align-items:center; gap:6px;
    visibility:hidden;
  }
  .back-btn.show{ visibility:visible; }
  .qcount{
    font-family:'IBM Plex Mono',monospace; font-size:12px; color:var(--ink-soft);
  }
  .progress-track{
    height:3px; background:var(--line); border-radius:2px; overflow:hidden; margin-bottom:28px;
  }
  .progress-fill{
    height:100%; background:var(--rust); width:0%;
    transition:width 0.3s ease;
  }
  .qtext{
    font-family:'Fraunces',serif;
    font-size:22px; line-height:1.32; font-weight:500;
    margin:0 0 26px; min-height:84px;
  }
  .opt-list{ display:flex; flex-direction:column; gap:8px; }
  .opt-row{
    display:flex; align-items:center; gap:14px;
    width:100%; text-align:left;
    padding:14px 14px;
    border:1px solid var(--line);
    border-radius:3px;
    background:var(--paper);
    transition:border-color 0.15s ease, background 0.15s ease;
  }
  .opt-row:hover{ border-color:var(--ink-soft); }
  .opt-row.selected{
    border-color:var(--rust);
    background:#F4DFD7;
  }
  .opt-gauge{
    display:flex; align-items:flex-end; gap:2px; width:20px; flex-shrink:0;
  }
  .opt-gauge i{
    display:block; width:3px; background:var(--ink-soft); border-radius:1px;
  }
  .opt-row.selected .opt-gauge i{ background:var(--rust); }
  .opt-label{
    font-size:14.5px; color:var(--ink);
  }
  .fade-enter{ animation:fadeIn 0.28s ease; }
  @keyframes fadeIn{ from{opacity:0; transform:translateY(4px);} to{opacity:1; transform:none;} }
  @media (prefers-reduced-motion: reduce){
    .fade-enter{ animation:none; }
    .progress-fill, .opt-row, .btn-primary{ transition:none; }
  }

  /* Results */
  .result-name{
    font-family:'Fraunces',serif; font-weight:600; font-size:38px; line-height:1.05;
    margin:0 0 4px; color:var(--ink);
  }
  .result-pair{
    font-family:'IBM Plex Mono',monospace; font-size:12px; color:var(--teal); letter-spacing:0.04em;
    margin:0 0 18px; text-transform:uppercase;
  }
  .result-blurb{
    font-size:15px; line-height:1.6; color:var(--ink-soft); margin:0 0 28px;
  }
  .radar-wrap{ display:flex; justify-content:center; margin-bottom:22px; }
  .trait-rows{ display:flex; flex-direction:column; gap:10px; margin-bottom:30px; }
  .trait-row{ display:flex; align-items:center; gap:10px; }
  .trait-label{
    font-family:'IBM Plex Mono',monospace; font-size:10.5px; letter-spacing:0.04em;
    width:108px; flex-shrink:0; color:var(--ink-soft); text-transform:uppercase;
  }
  .trait-bar-track{
    flex:1; height:6px; background:var(--line); border-radius:3px; overflow:hidden;
  }
  .trait-bar-fill{ height:100%; background:var(--teal); border-radius:3px; }
  .trait-score{
    font-family:'IBM Plex Mono',monospace; font-size:11px; color:var(--ink-soft); width:28px; text-align:right; flex-shrink:0;
  }
  .section-label{
    font-family:'IBM Plex Mono',monospace; font-size:11px; letter-spacing:0.1em; text-transform:uppercase;
    color:var(--rust); margin:0 0 8px;
    border-top:1px solid var(--line); padding-top:18px;
  }
  .section-label.first{ border-top:none; padding-top:0; }
  .career-list{ margin:0 0 4px; padding:0; list-style:none; }
  .career-list li{
    font-size:15px; padding:9px 0; border-bottom:1px solid var(--line);
    display:flex; gap:10px; align-items:baseline;
  }
  .career-list li:last-child{ border-bottom:none; }
  .career-list li::before{
    content:"—"; color:var(--gold); flex-shrink:0;
  }
  .side-block{
    font-size:15px; line-height:1.55; color:var(--ink);
    padding:14px 16px; background:var(--paper); border:1px solid var(--line); border-radius:3px;
    margin-bottom:6px;
  }
  .disclaimer{
    font-family:'IBM Plex Mono',monospace; font-size:10.5px; color:var(--ink-soft);
    line-height:1.5; margin-top:22px; opacity:0.85;
  }
</style>
</head>
<body>

<svg class="bg-contours" preserveAspectRatio="none" viewBox="0 0 400 800" xmlns="http://www.w3.org/2000/svg">
  <path d="M-20,120 C80,90 140,160 240,130 C320,105 360,150 430,120" stroke="#C9C2B2" stroke-width="1" fill="none"/>
  <path d="M-20,180 C90,150 150,220 250,190 C330,165 370,210 430,180" stroke="#C9C2B2" stroke-width="1" fill="none"/>
  <path d="M-20,620 C90,590 150,660 250,630 C330,605 370,650 430,620" stroke="#C9C2B2" stroke-width="1" fill="none"/>
  <path d="M-20,680 C90,650 150,720 250,690 C330,665 370,710 430,680" stroke="#C9C2B2" stroke-width="1" fill="none"/>
</svg>

<div class="stage"><div class="card" id="app"></div></div>

<script>
/* ---------- Question bank ---------- */
const BANK = {
  E: [
    [1,"I feel more energized after being around people than after being alone."],
    [1,"I often take the lead in group conversations."],
    [1,"I enjoy meeting new people and making new connections."],
    [1,"I'd rather spend a Friday night at a lively gathering than at home reading."],
    [1,"I find small talk easy and enjoyable."],
    [-1,"I prefer working alone rather than in a team."],
    [-1,"Large crowds leave me feeling drained rather than excited."],
    [1,"I often share my opinions openly in group settings."],
    [-1,"I need a lot of alone time to recharge."],
    [1,"People would describe me as outgoing."]
  ],
  O: [
    [1,"I love exploring new ideas, even when they challenge what I already believe."],
    [1,"I get bored easily with routine and repetition."],
    [1,"I'm drawn to art, music, or other creative pursuits."],
    [1,"I like imagining how things could be different from how they are."],
    [-1,"I prefer tried-and-true methods over experimenting with new approaches."],
    [1,"I find myself asking a lot of what-if questions."],
    [1,"I'm drawn to travel, unfamiliar cultures, or new experiences."],
    [-1,"I rarely think about abstract or philosophical questions."],
    [1,"I enjoy brainstorming and coming up with original solutions."],
    [-1,"I stick to a narrow set of interests rather than exploring many topics."]
  ],
  C: [
    [1,"I plan things out carefully before I start a task."],
    [1,"I keep my workspace and schedule well-organized."],
    [1,"I usually finish what I start, even when it gets tedious."],
    [1,"I pay close attention to detail and rarely make careless mistakes."],
    [-1,"I often put off tasks until the last minute."],
    [1,"I set goals for myself and track my progress toward them."],
    [-1,"I'm comfortable letting things stay messy or disorganized."],
    [1,"I follow through on commitments, even small ones."],
    [-1,"I find it hard to stick to a routine."],
    [1,"People can count on me to meet deadlines."]
  ],
  A: [
    [1,"I go out of my way to help others, even at some cost to myself."],
    [1,"I find it easy to empathize with how other people are feeling."],
    [1,"I try to avoid conflict and keep the peace in groups."],
    [1,"I genuinely enjoy collaborating and sharing credit with others."],
    [1,"I tend to assume the best about people's intentions."],
    [-1,"I can be blunt or critical even when it might hurt someone's feelings."],
    [-1,"I prioritize my own needs over accommodating others."],
    [1,"I enjoy mentoring or supporting people who are struggling."],
    [-1,"I get more satisfaction from competing and winning than from cooperating."],
    [1,"I'm quick to forgive when someone wrongs me."]
  ],
  S: [
    [1,"I stay calm under pressure rather than getting anxious."],
    [1,"My mood doesn't swing much from day to day."],
    [1,"I bounce back quickly after a setback or disappointment."],
    [1,"I rarely worry about things that are outside my control."],
    [-1,"Small frustrations tend to throw off my whole day."],
    [-1,"I feel anxious or stressed more often than most people."],
    [1,"I can handle criticism without taking it personally."],
    [-1,"I tend to overthink mistakes long after they happen."],
    [1,"I feel confident in my ability to handle whatever comes my way."],
    [-1,"I get overwhelmed easily when things don't go as planned."]
  ]
};
const ORDER = ['E','O','C','A','S'];
const QUESTIONS = [];
for(let i=0;i<10;i++){
  ORDER.forEach(t=>{
    const [sign,text] = BANK[t][i];
    QUESTIONS.push({t, sign, text});
  });
}

const TRAIT_NAME = {E:"Extraversion", O:"Openness", C:"Conscientiousness", A:"Agreeableness", S:"Stability"};

const ARCHETYPES = {
  "A,C": { name:"The Caregiver", pair:"Conscientiousness + Agreeableness",
    blurb:"Conscientious and warm, you build trust by being steady, organized, and genuinely invested in other people's wellbeing.",
    careers:["Registered Nurse","Social Worker","Office or Practice Manager","Elementary Teacher"],
    side:"Run a meal-prep, pet-sitting, or home-organizing service for neighbors — steady, hands-on work that rewards your reliability." },
  "A,E": { name:"The Connector", pair:"Extraversion + Agreeableness",
    blurb:"Outgoing and warm-hearted, you light up a room and make people feel instantly comfortable around you.",
    careers:["HR Business Partner","Community Manager","Teacher","Customer Success Lead"],
    side:"Host a local meetup, run a small events business, or freelance as a wedding or party planner." },
  "A,O": { name:"The Idealist", pair:"Openness + Agreeableness",
    blurb:"Curious and compassionate, you're drawn to big human questions and the stories that don't always get told.",
    careers:["Therapist or Counselor","Nonprofit Program Director","UX Researcher","Writer"],
    side:"Freelance writing or storytelling for causes you care about, or part-time coaching grounded in real listening." },
  "A,S": { name:"The Diplomat", pair:"Agreeableness + Stability",
    blurb:"Calm and fair-minded, you're the person people turn to when a room needs to be talked down, not riled up.",
    careers:["Mediator / Conflict Resolution Specialist","HR Manager","School Counselor","Patient Advocate"],
    side:"Volunteer mediation, peer-support facilitation, or part-time coaching for people navigating conflict." },
  "C,E": { name:"The Director", pair:"Extraversion + Conscientiousness",
    blurb:"Energetic and organized, you turn a room full of opinions into a plan — and make sure the plan actually happens.",
    careers:["Operations Manager","Project Manager","Sales Director","Event Producer"],
    side:"Freelance project management, event coordination, or coaching small business owners on getting organized." },
  "C,O": { name:"The Architect", pair:"Openness + Conscientiousness",
    blurb:"Inventive but disciplined, you don't just dream up ideas — you're the one who turns them into something that works.",
    careers:["Product Manager","Software Engineer","Urban Planner","R&D Researcher"],
    side:"Build and sell a digital product — an app, a course, a template kit — or take on freelance consulting in your field." },
  "C,S": { name:"The Anchor", pair:"Conscientiousness + Stability",
    blurb:"Steady, methodical, and hard to rattle, you're the dependable foundation a team builds everything else on top of.",
    careers:["Accountant / Financial Analyst","Logistics Manager","Air Traffic Controller","QA Lead"],
    side:"Offer bookkeeping or financial organizing for small businesses and freelancers." },
  "E,O": { name:"The Visionary", pair:"Extraversion + Openness",
    blurb:"Magnetic and full of ideas, you're energized by new possibilities and good at getting other people to come along.",
    careers:["Entrepreneur / Founder","Marketing Director","Innovation Consultant","Creative Director"],
    side:"Try public speaking, hosting a podcast, or content creation built around your niche." },
  "E,S": { name:"The Performer", pair:"Extraversion + Stability",
    blurb:"Confident under pressure and energized by people, you do your best work when the stakes — and the spotlight — are high.",
    careers:["Sales Executive","Public Relations Lead","Live Events Producer","Emergency Services Coordinator"],
    side:"Public speaking coaching, emceeing events, or group fitness instruction." },
  "O,S": { name:"The Trailblazer", pair:"Openness + Stability",
    blurb:"Curious and unflappable, you take the unconventional path without losing your footing, even when no one's done it before.",
    careers:["Startup Founder","Research Scientist","Investigative Journalist","Commercial Pilot"],
    side:"Travel writing or photography, angel advising, or building a niche content brand around your interests." }
};

/* ---------- State ---------- */
const state = { screen:'intro', current:0, answers:new Array(50).fill(null) };
const app = document.getElementById('app');

function render(){
  if(state.screen==='intro') renderIntro();
  else if(state.screen==='quiz') renderQuiz();
  else renderResults();
}

function renderIntro(){
  app.innerHTML = `
    <p class="eyebrow">A 50-point personality survey</p>
    <h1 class="display">Chart your own terrain.</h1>
    <p class="lede">Answer 50 short statements honestly — there's no right answer, just your answer. At the end you'll get your personality type, plus a profession and a side job built around how you actually operate.</p>
    <p class="meta-line">TAKES ABOUT 6–8 MINUTES · 50 QUESTIONS</p>
    <button class="btn-primary" id="startBtn">Begin the survey <span>→</span></button>
  `;
  document.getElementById('startBtn').onclick = ()=>{
    state.screen='quiz'; state.current=0; render();
  };
}

function selectAnswer(value){
  state.answers[state.current] = value;
  render(); // reflect selection
  setTimeout(()=>{
    if(state.current < 49){ state.current++; render(); }
    else { state.screen='results'; render(); }
  }, 220);
}

function goBack(){
  if(state.current>0){ state.current--; render(); }
}

function renderQuiz(){
  const q = QUESTIONS[state.current];
  const selected = state.answers[state.current];
  const labels = ["Strongly disagree","Disagree","Neutral","Agree","Strongly agree"];
  const opts = labels.map((label,i)=>{
    const val = i+1;
    const isSel = selected===val;
    const bars = [4,7,10,7,4]; // gauge tick heights, symmetric except scaled below
    const heights = [5,9,13,9,5];
    let gaugeHTML = '';
    for(let g=1; g<=5; g++){
      gaugeHTML += `<i style="height:${5+g*2}px; opacity:${g<=val?1:0.25}"></i>`;
    }
    return `<button class="opt-row${isSel?' selected':''}" data-val="${val}">
      <span class="opt-gauge">${gaugeHTML}</span>
      <span class="opt-label">${label}</span>
    </button>`;
  }).join('');

  app.innerHTML = `
    <div class="quiz-head">
      <button class="back-btn${state.current>0?' show':''}" id="backBtn">← back</button>
      <span class="qcount">Q ${String(state.current+1).padStart(2,'0')} / 50</span>
    </div>
    <div class="progress-track"><div class="progress-fill" style="width:${(state.current/50)*100}%"></div></div>
    <p class="qtext fade-enter">${q.text}</p>
    <div class="opt-list">${opts}</div>
  `;
  document.getElementById('backBtn').onclick = goBack;
  app.querySelectorAll('.opt-row').forEach(btn=>{
    btn.onclick = ()=> selectAnswer(parseInt(btn.dataset.val,10));
  });
}

function computeScores(){
  const totals = {E:0,O:0,C:0,A:0,S:0};
  QUESTIONS.forEach((q,i)=>{
    const val = state.answers[i];
    const scored = q.sign===1 ? val : (6-val);
    totals[q.t] += scored;
  });
  const avg = {};
  ORDER.forEach(t=> avg[t] = totals[t]/10 );
  return avg;
}

function renderRadar(avg){
  const center=110, maxR=82;
  const angles = ORDER.map((_,i)=> i*72 );
  function pt(angleDeg, frac){
    const rad = (angleDeg-90) * Math.PI/180;
    return [center + maxR*frac*Math.cos(rad), center + maxR*frac*Math.sin(rad)];
  }
  // grid rings
  let rings = '';
  [0.25,0.5,0.75,1].forEach(frac=>{
    const pts = angles.map(a=> pt(a,frac).join(',')).join(' ');
    rings += `<polygon points="${pts}" fill="none" stroke="#C9C2B2" stroke-width="1"/>`;
  });
  // axes
  let axes = '';
  angles.forEach(a=>{
    const [x,y] = pt(a,1);
    axes += `<line x1="${center}" y1="${center}" x2="${x}" y2="${y}" stroke="#C9C2B2" stroke-width="1"/>`;
  });
  // data polygon
  const dataPts = ORDER.map((t,i)=> pt(angles[i], Math.max((avg[t]-1)/4,0.04)) );
  const dataStr = dataPts.map(p=>p.join(',')).join(' ');
  const dataPoly = `<polygon points="${dataStr}" fill="#B8412A" fill-opacity="0.22" stroke="#B8412A" stroke-width="2"/>`;
  const dots = dataPts.map(p=>`<circle cx="${p[0]}" cy="${p[1]}" r="3.5" fill="#B8412A"/>`).join('');
  // labels
  let labels='';
  ORDER.forEach((t,i)=>{
    const [x,y] = pt(angles[i], 1.22);
    labels += `<text x="${x}" y="${y}" font-family="IBM Plex Mono, monospace" font-size="10" fill="#4A5066" text-anchor="middle" dominant-baseline="middle">${t}</text>`;
  });
  return `<svg width="220" height="220" viewBox="0 0 220 220">${ri
