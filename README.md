<!doctype html>

<html lang="en">

<head>

<meta charset="utf-8"/>

<meta name="viewport" content="width=device-width, initial-scale=1"/>

<title>Movie Decider — prototype</title>

<style>

  :root{

    --bg:#0f1220; --panel:#161a2f; --ink:#e9edf8; --muted:#aab3d0;

    --line:#232a52; --accent:#7aa2ff; --accent2:#63e6be; --warn:#ffb86b;

    --danger:#ff6b6b; --ok:#5dd39e; --chip:#21264a; --chipb:#2b3364;

    --r:14px; --rsm:10px; --shadow:0 10px 34px rgba(0,0,0,.35);

  }

  *{box-sizing:border-box}

  html,body{margin:0;background:var(--bg);color:var(--ink);font:16px/1.45 system-ui,-apple-system,Segoe UI,Roboto,Helvetica,Arial}

  a{color:var(--accent);text-decoration:none}

  a:hover{text-decoration:underline}

  header{position:sticky;top:0;z-index:10;backdrop-filter:blur(8px);

    background:linear-gradient(180deg,rgba(15,18,32,.92),rgba(15,18,32,.7) 60%,transparent);

    border-bottom:1px solid var(--line)}

  .wrap{max-width:1200px;margin:0 auto;padding:14px 18px}

  .brand{display:flex;align-items:center;gap:12px}

  .logo{width:36px;height:36px;border-radius:9px;background:

    linear-gradient(135deg,var(--accent),var(--accent2));display:grid;place-items:center;

    color:#0b1020;font-weight:800}

  .pill{font-size:12px;border:1px solid var(--line);padding:6px 10px;border-radius:999px;background:#121737;color:var(--muted)}

  .grid{display:grid;gap:16px}

  .layout{grid-template-columns:340px 1fr}

  @media (max-width:980px){.layout{grid-template-columns:1fr}}

  .panel{background:var(--panel);border:1px solid var(--line);border-radius:var(--r);box-shadow:var(--shadow)}

  .pad{padding:16px}

  .title{font:700 14px/1.2 system-ui;letter-spacing:.12em;text-transform:uppercase;color:var(--muted);margin:0 0 10px}

  label{font-size:13px;color:var(--muted);display:block;margin:8px 0 6px}

  input,select,textarea{width:100%;background:#0f1430;color:var(--ink);border:1px solid var(--line);border-radius:10px;padding:10px 12px;outline:none}

  .chips{display:flex;flex-wrap:wrap;gap:8px;padding:8px;background:#0f1430;border:1px solid var(--line);border-radius:10px}

  .chip{background:var(--chip);border:1px solid var(--chipb);color:#d6defb;padding:6px 10px;border-radius:999px;font-size:12px;cursor:pointer;user-select:none}

  .chip.active{background:#27306b;border-color:#3c4ba2}

  .row{display:grid;grid-template-columns:1fr 1fr;gap:10px}

  .row3{grid-template-columns:1fr 1fr 1fr}

  .btn{background:var(--accent);color:#09122d;border:none;border-radius:12px;padding:10px 14px;font-weight:700;cursor:pointer}

  .btn.ghost{background:transparent;border:1px solid var(--line);color:var(--ink)}

  .btn.ok{background:var(--ok);color:#042018}

  .btn.warn{background:var(--warn);color:#271a07}

  .btn.small{padding:6px 10px;border-radius:10px;font-size:12px}

  .right{margin-left:auto}

  .cards{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:14px}

  .card{background:#121737;border:1px solid var(--line);border-radius:var(--r);overflow:hidden;display:flex;flex-direction:column}

  .poster{aspect-ratio:2/3;background:#0c1028;display:grid;place-items:center;color:#95a0df;font-weight:800}

  .poster img{width:100%;height:100%;object-fit:cover;display:block}

  .card-body{padding:12px 12px 14px;display:flex;flex-direction:column;gap:8px}

  .badges{display:flex;flex-wrap:wrap;gap:6px}

  .badge{background:#1a2050;color:#cfe0ff;border:1px solid var(--chipb);padding:3px 8px;border-radius:999px;font-size:11px}

  .muted{color:var(--muted)}

  .meta{display:flex;gap:8px;flex-wrap:wrap;font-size:12px;color:#cfe0ff}

  .why{font-size:12px;color:#cfe0ff}

  .links{display:flex;gap:8px;flex-wrap:wrap}

  .divider{height:1px;background:var(--line);margin:10px 0}

  .rowline{display:flex;gap:8px;flex-wrap:wrap;align-items:center}

  .pill2{background:#0f1430;border:1px solid var(--line);border-radius:999px;padding:6px 10px;font-size:12px}

  .video-box{background:#0b1130;border:1px dashed var(--line);border-radius:12px;padding:8px}

  .chapters{display:flex;flex-wrap:wrap;gap:6px}

  .chapter{font-size:12px;background:#1b2358;border:1px solid #2a357b;border-radius:999px;padding:5px 8px;cursor:pointer}

  .search-hit{background:#ffd56b;color:#121212;border-radius:4px;padding:0 2px}

  .watchlist{list-style:none;padding:0;margin:0;display:flex;flex-direction:column;gap:6px}

  .msg{background:#0f1430;border:1px solid var(--line);padding:10px;border-radius:10px}

  details summary{cursor:pointer;color:#bcd0ff}

  footer{opacity:.75;font-size:12px;padding:30px 0}

</style>

</head>

<body>

<header>

  <div class="wrap brand">

    <div class="logo">MD</div>

    <strong>Movie Decider</strong>

    <span class="pill">Prototype • Client-side only</span>

    <button id="savePrefsBtn" class="btn small right">Save preferences</button>

    <button id="resetPrefsBtn" class="btn ghost small">Reset</button>

  </div>

</header>



<main class="wrap grid layout" id="app">

  <!-- LEFT: Controls -->

  <section class="panel pad">

    <div class="title">Quick questions</div>



    <label>Favorite genres</label>

    <div class="chips" id="genreChips"></div>



    <div class="row">

      <div>

        <label>Current mood</label>

        <select id="mood">

          <option value="">Any</option>

          <option>Chill</option><option>Adventurous</option><option>Romantic</option>

          <option>Laugh</option><option>Scary</option><option>Thoughtful</option>

        </select>

      </div>

      <div>

        <label>Time you have (mins)</label>

        <input id="timeAvailable" type="number" min="30" step="5" placeholder="e.g. 110">

      </div>

    </div>



    <div class="row">

      <div>

        <label>Language</label>

        <select id="language">

          <option value="">Any</option>

          <option>English</option><option>Spanish</option><option>Korean</option>

          <option>French</option><option>Japanese</option><option>Hindi</option>

        </select>

      </div>

      <div>

        <label>Mainstream or Indie</label>

        <select id="typePref">

          <option value="">Either</option>

          <option value="Mainstream">Mainstream</option>

          <option value="Indie">Indie</option>

        </select>

      </div>

    </div>



    <div class="divider"></div>

    <div class="title">Filters</div>

    <div class="row row3">

      <div>

        <label>Year ≥</label>

        <input id="yearMin" type="number" placeholder="e.g. 2010">

      </div>

      <div>

        <label>IMDb rating ≥</label>

        <input id="ratingMin" type="number" step="0.1" min="0" max="10" placeholder="e.g. 7.2">

      </div>

      <div>

        <label>Family-friendly only</label>

        <select id="familyFriendly">

          <option value="">Either</option>

          <option value="yes">Yes</option>

          <option value="no">No</option>

        </select>

      </div>

    </div>



    <div class="divider"></div>

    <div class="rowline">

      <button id="recommendBtn" class="btn">Find movies</button>

      <button id="shuffleBtn" class="btn ghost">Shuffle</button>

      <button id="surpriseBtn" class="btn ok">Surprise me 🎲</button>

    </div>



    <div class="divider"></div>

    <div class="title">Finish your movie</div>

    <div class="row">

      <div>

        <label>Planned breaks (#)</label>

        <input id="breakCount" type="number" min="0" value="0">

      </div>

      <div>

        <label>Each break length (mins)</label>

        <input id="breakLength" type="number" min="0" value="0">

      </div>

    </div>

    <div class="row">

      <div>

        <label>Pause reminder every (mins)</label>

        <input id="pauseReminder" type="number" min="0" placeholder="e.g. 20">

      </div>

      <div>

        <label>Scene search (keywords)</label>

        <input id="sceneSearch" type="text" placeholder="e.g. heist, train, wedding">

      </div>

    </div>



    <div class="divider"></div>

    <div class="title">Your stuff</div>

    <details open>

      <summary>Watchlist</summary>

      <ul id="watchlist" class="watchlist"></ul>

    </details>

    <details>

      <summary>Saved progress</summary>

      <div id="progressList"></div>

    </details>

  </section>



  <!-- RIGHT: Results -->

  <section class="panel pad">

    <div class="rowline">

      <div class="title" style="margin:0">Results</div>

      <span id="resultCount" class="pill2">0 found</span>

    </div>

    <div id="results" class="cards"></div>

  </section>

</main>



<footer class="wrap">Designed for client-side testing. Ready to plug in TMDb/JustWatch/Firebase later.</footer>



<script>

/* =========================

   Mock dataset (12 movies)

   ========================= */

const DATA = [

  {

    id:"inception", title:"Inception", year:2010, rating:8.8, runtime:148,

    genres:["Sci-Fi","Action","Thriller"], mood:"Adventurous", language:"English", type:"Mainstream",

    family:false,

    synopsis:"A thief who steals corporate secrets through dream-sharing technology is given a chance at redemption.",

    poster:"https://m.media-amazon.com/images/I/51xJzYq+7zL._AC_.jpg",

    imdb:"https://www.imdb.com/title/tt1375666/", trailer:"https://youtu.be/YoHD9XEInc0",

    contentWarnings:["Violence","Intense scenes"],

    subtitles:["English","Spanish","French"], audio:["English 5.1","English 2.0"],

    providers:["Netflix (region-dep)","Prime Video","Local file"],

    chapters:[

      {t:0,title:"Paris Fold"}, {t:22,title:"Assembling the Team"},

      {t:58,title:"The Hotel Heist"}, {t:100,title:"Snow Fortress"}, {t:140,title:"The Kick"}

    ],

    scenes:["dream","heist","paradox","snow","hotel"]

  },

  {

    id:"parasite", title:"Parasite", year:2019, rating:8.6, runtime:132,

    genres:["Drama","Thriller"], mood:"Thoughtful", language:"Korean", type:"Indie",

    family:false,

    synopsis:"Greed and class discrimination threaten the newly formed symbiotic relationship between the wealthy and the destitute.",

    poster:"https://m.media-amazon.com/images/I/91T6I+uMXML._AC_SL1500_.jpg",

    imdb:"https://www.imdb.com/title/tt6751668/", trailer:"https://youtu.be/5xH0HfJHsaY",

    contentWarnings:["Violence","Language"], subtitles:["English","Spanish"], audio:["Korean 5.1","Korean 2.0"],

    providers:["Hulu (region-dep)","Local file"],

    chapters:[{t:0,title:"Pizza Boxes"},{t:37,title:"The Plan"},{t:66,title:"Basement"},{t:102,title:"Party"}],

    scenes:["party","basement","scheme","storm","tension"]

  },

  {

    id:"toy-story", title:"Toy Story", year:1995, rating:8.3, runtime:81,

    genres:["Comedy","Animation","Family"], mood:"Laugh", language:"English", type:"Mainstream",

    family:true,

    synopsis:"Toys come to life when humans aren't around; Woody feels threatened by Buzz Lightyear.",

    poster:"https://m.media-amazon.com/images/I/51Qvs9i5a%2BL._AC_.jpg",

    imdb:"https://www.imdb.com/title/tt0114709/", trailer:"https://youtu.be/v-PjgYDrg70",

    contentWarnings:[], subtitles:["English","Spanish"], audio:["English 5.1"],

    providers:["Disney+","Local file"],

    chapters:[{t:0,title:"Birthday"},{t:20,title:"Pizza Planet"},{t:55,title:"Sid's House"},{t:75,title:"The Chase"}],

    scenes:["toys","friendship","chase","rocket"]

  },

  {

    id:"conjuring", title:"The Conjuring", year:2013, rating:7.5, runtime:112,

    genres:["Horror","Thriller"], mood:"Scary", language:"English", type:"Mainstream",

    family:false,

    synopsis:"Paranormal investigators help a family terrorized by a dark presence in their farmhouse.",

    poster:"https://m.media-amazon.com/images/I/81p+xe8cbnL._AC_SL1500_.jpg",

    imdb:"https://www.imdb.com/title/tt1457767/", trailer:"https://youtu.be/k10ETZ41q5o",

    contentWarnings:["Horror","Violence"], subtitles:["English"], audio:["English 5.1"],

    providers:["HBO Max","Local file"],

    chapters:[{t:0,title:"New House"},{t:25,title:"Hide & Clap"},{t:70,title:"Possession"},{t:100,title:"Exorcism"}],

    scenes:["clap","possession","basement","exorcism"]

  },

  {

    id:"before-sunrise", title:"Before Sunrise", year:1995, rating:8.1, runtime:101,

    genres:["Romance","Drama"], mood:"Romantic", language:"English", type:"Indie", family:false,

    synopsis:"Strangers meet on a train and spend one night in Vienna talking about life and love.",

    poster:"https://m.media-amazon.com/images/I/51T4y8P7y-L._AC_.jpg",

    imdb:"https://www.imdb.com/title/tt0112471/", trailer:"https://youtu.be/25v7N34d5HE",

    contentWarnings:["Language"], subtitles:["English"], audio:["English 2.0"],

    providers:["Prime Video","Local file"],

    chapters:[{t:0,title:"Train"},{t:20,title:"Ferris Wheel"},{t:60,title:"Café"},{t:95,title:"Goodbye"}],

    scenes:["train","vienna","conversation","romance"]

  },

  {

    id:"spiderverse", title:"Spider-Verse", year:2018, rating:8.4, runtime:117,

    genres:["Action","Animation","Sci-Fi"], mood:"Adventurous", language:"English", type:"Mainstream", family:true,

    synopsis:"Teen Miles Morales becomes Spider-Man and joins others from parallel universes.",

    poster:"https://m.media-amazon.com/images/I/81xXAyqQ0XL._AC_SL1500_.jpg",

    imdb:"https://www.imdb.com/title/tt4633694/", trailer:"https://youtu.be/g4Hbz2jLxvQ",

    contentWarnings:["Fantasy violence"], subtitles:["English","Spanish"], audio:["English 5.1"],

    providers:["Netflix","Local file"],

    chapters:[{t:0,title:"Origin"},{t:32,title:"Team Up"},{t:80,title:"Leap"},{t:110,title:"Collider"}],

    scenes:["leap","collider","multiverse","training"]

  },

  {

    id:"roma", title:"Roma", year:2018, rating:7.7, runtime:135,

    genres:["Drama"], mood:"Thoughtful", language:"Spanish", type:"Indie", family:false,

    synopsis:"A year in the life of a middle-class family's maid in 1970s Mexico City.",

    poster:"https://m.media-amazon.com/images/I/71QfLkqZB4L._AC_SL1200_.jpg",

    imdb:"https://www.imdb.com/title/tt6155172/", trailer:"https://youtu.be/6BS27ngZtxg",

    contentWarnings:["Mature themes"], subtitles:["English"], audio:["Spanish 5.1"],

    providers:["Netflix","Local file"],

    chapters:[{t:0,title:"Opening"},{t:45,title:"Beach"},{t:90,title:"Riot"}],

    scenes:["family","mexico","beach","riot"]

  },

  {

    id:"train-to-busan", title:"Train to Busan", year:2016, rating:7.6, runtime:118,

    genres:["Horror","Action"], mood:"Scary", language:"Korean", type:"Mainstream", family:false,

    synopsis:"Passengers fight for survival on a train during a zombie outbreak.",

    poster:"https://m.media-amazon.com/images/I/81qF9tJwq-L._AC_SL1500_.jpg",

    imdb:"https://www.imdb.com/title/tt5700672/", trailer:"https://youtu.be/pyWuHv2-Abk",

    contentWarnings:["Gore","Violence"], subtitles:["English"], audio:["Korean 5.1"],

    providers:["Prime Video","Local file"],

    chapters:[{t:0,title:"Boarding"},{t:35,title:"Breakout"},{t:80,title:"Derailed"},{t:110,title:"Tunnel"}],

    scenes:["train","zombie","father","sacrifice"]

  },

  {

    id:"your-name", title:"Your Name", year:2016, rating:8.4, runtime:106,

    genres:["Romance","Animation","Drama"], mood:"Romantic", language:"Japanese", type:"Mainstream", family:true,

    synopsis:"Two teenagers mysteriously swap bodies and connect across time.",

    poster:"https://m.media-amazon.com/images/I/81f-V5H8k-L._AC_SL1500_.jpg",

    imdb:"https://www.imdb.com/title/tt5311514/", trailer:"https://youtu.be/xU47nhruN-Q",

    contentWarnings:["Mild language"], subtitles:["English"], audio:["Japanese 5.1"],

    providers:["Crunchyroll","Local file"],

    chapters:[{t:0,title:"Swap"},{t:40,title:"Comet"},{t:85,title:"Reunion"}],

    scenes:["comet","body swap","romance","fate"]

  },

  {

    id:"amelie", title:"Amélie", year:2001, rating:8.3, runtime:122,

    genres:["Romance","Comedy"], mood:"Chill", language:"French", type:"Indie", family:true,

    synopsis:"A whimsical Parisian woman decides to change the lives of those around her.",

    poster:"https://m.media-amazon.com/images/I/81g3gVQ0GgL._AC_SL1500_.jpg",

    imdb:"https://www.imdb.com/title/tt0211915/", trailer:"https://youtu.be/HUECWi5pX7o",

    contentWarnings:[], subtitles:["English"], audio:["French 5.1"],

    providers:["Local file"],

    chapters:[{t:0,title:"Childhood"},{t:50,title:"Mystery Man"},{t:100,title:"Photo Booth"}],

    scenes:["paris","photo","love","whimsy"]

  },

  {

    id:"rango", title:"Rango", year:2011, rating:7.2, runtime:107,

    genres:["Comedy","Animation","Western"], mood:"Laugh", language:"English", type:"Mainstream", family:true,

    synopsis:"A chameleon becomes sheriff of a lawless desert town called Dirt.",

    poster:"https://m.media-amazon.com/images/I/91w0j2zB6tL._AC_SL1500_.jpg",

    imdb:"https://www.imdb.com/title/tt1192628/", trailer:"https://youtu.be/k-OOfW6wWyQ",

    contentWarnings:["Cartoon violence"], subtitles:["English"], audio:["English 5.1"],

    providers:["Paramount+","Local file"],

    chapters:[{t:0,title:"Accident"},{t:35,title:"Town of Dirt"},{t:80,title:"Water Heist"}],

    scenes:["desert","sheriff","water","heist"]

  },

  {

    id:"arrival", title:"Arrival", year:2016, rating:7.9, runtime:116,

    genres:["Sci-Fi","Drama"], mood:"Thoughtful", language:"English", type:"Mainstream", family:false,

    synopsis:"A linguist communicates with extraterrestrials who arrived in mysterious craft.",

    poster:"https://m.media-amazon.com/images/I/71tN9SUKWwL._AC_SL1200_.jpg",

    imdb:"https://www.imdb.com/title/tt2543164/", trailer:"https://youtu.be/tFMo3UJ4B4g",

    contentWarnings:["Intense scenes"], subtitles:["English","Spanish"], audio:["English 5.1"],

    providers:["Paramount+","Local file"],

    chapters:[{t:0,title:"Arrival"},{t:45,title:"Heptapods"},{t:95,title:"Choice"}],

    scenes:["aliens","language","time","mother"]

  }

];



/* =========================

   UI setup

   ========================= */

const $ = sel => document.querySelector(sel);

const $$ = sel => document.querySelectorAll(sel);



const GENRES = [...new Set(DATA.flatMap(m=>m.genres))].sort();



function renderGenreChips(){

  const box = $("#genreChips");

  box.innerHTML = "";

  GENRES.forEach(g=>{

    const b = document.createElement("span");

    b.className = "chip";

    b.textContent = g;

    b.dataset.val = g;

    b.addEventListener("click", ()=>{ b.classList.toggle("active"); });

    box.appendChild(b);

  });

}

renderGenreChips();



/* =========================

   Preferences (localStorage)

   ========================= */

const LS = {

  prefs:"md:prefs",

  watchlist:"md:watchlist",

  progress:"md:progress", // map movieId -> {minutes, savedAt}

  chat:"md:chat"          // map movieId -> [{name,msg,at,stars}]

};



function loadPrefs(){

  const p = JSON.parse(localStorage.getItem(LS.prefs)||"{}");

  if(p.genres){[...$$('#genreChips .chip')].forEach(ch=>{ if(p.genres.includes(ch.dataset.val)) ch.classList.add('active'); });}

  if(p.mood) $("#mood").value = p.mood;

  if(p.time) $("#timeAvailable").value = p.time;

  if(p.language) $("#language").value = p.language;

  if(p.typePref) $("#typePref").value = p.typePref;

  if(p.yearMin) $("#yearMin").value = p.yearMin;

  if(p.ratingMin) $("#ratingMin").value = p.ratingMin;

  if(p.familyFriendly) $("#familyFriendly").value = p.familyFriendly;

}

function savePrefs(){

  const prefs = {

    genres:[...$$('#genreChips .chip.active')].map(x=>x.dataset.val),

    mood:$("#mood").value, time:$("#timeAvailable").value,

    language:$("#language").value, typePref:$("#typePref").value,

    yearMin:$("#yearMin").value, ratingMin:$("#ratingMin").value,

    familyFriendly:$("#familyFriendly").value

  };

  localStorage.setItem(LS.prefs, JSON.stringify(prefs));

}

$("#savePrefsBtn").onclick = ()=>{ savePrefs(); alert("Preferences saved!"); };

$("#resetPrefsBtn").onclick = ()=>{

  localStorage.removeItem(LS.prefs);

  [...$$('#genreChips .chip')].forEach(ch=>ch.classList.remove('active'));

  $("#mood").value = $("#timeAvailable").value = $("#language").value = $("#typePref").value = "";

  $("#yearMin").value = $("#ratingMin").value = ""; $("#familyFriendly").value = "";

};

loadPrefs();



/* =========================

   Scoring + filtering

   ========================= */

function scoreMovie(m, prefs){

  let s = 0; let why = [];

  // Genre match

  const gMatch = (prefs.genres||[]).filter(g=>m.genres.includes(g)).length;

  if(gMatch>0){ s += 20 + gMatch*5; why.push(`${gMatch} genre match${gMatch>1?'es':''}`); }

  // Mood

  if(prefs.mood && m.mood===prefs.mood){ s += 15; why.push("mood match"); }

  // Time closeness

  if(prefs.time){

    const t = parseInt(prefs.time,10);

    const diff = Math.abs(m.runtime - t);

    const tScore = Math.max(0, 20 - Math.floor(diff/5)); // closer runtime → higher

    s += tScore; if(tScore>0) why.push("fits your time");

  }

  // Language

  if(prefs.language && m.language===prefs.language){ s += 8; why.push("language preference"); }

  // Type

  if(prefs.typePref && m.type===prefs.typePref){ s += 8; why.push(`${m.type.toLowerCase()} pick`); }

  // Rating

  s += (m.rating-5)*2; // reward higher IMDb

  // Recency (small)

  s += Math.max(0, (m.year-1990)/20);

  return {score:Math.round(s), why};

}



function getFilters(){

  const prefs = JSON.parse(localStorage.getItem(LS.prefs)||"{}");

  // live read for current screen (so you don't have to press Save)

  prefs.genres = [...$$('#genreChips .chip.active')].map(x=>x.dataset.val);

  prefs.mood = $("#mood").value;

  prefs.time = $("#timeAvailable").value;

  prefs.language = $("#language").value;

  prefs.typePref = $("#typePref").value;

  prefs.yearMin = $("#yearMin").value;

  prefs.ratingMin = $("#ratingMin").value;

  prefs.familyFriendly = $("#familyFriendly").value;

  return prefs;

}



function applyFilters(m, f){

  if(f.yearMin && m.year < +f.yearMin) return false;

  if(f.ratingMin && m.rating < +f.ratingMin) return false;

  if(f.familyFriendly==="yes" && !m.family) return false;

  if(f.familyFriendly==="no" && m.family) return false;

  if(f.language && m.language!==f.language) return false;

  if(f.typePref && m.type!==f.typePref) return false;

  if(f.mood && m.mood!==f.mood) return false;

  if(f.genres && f.genres.length && !f.genres.some(g=>m.genres.includes(g))) return false;

  if(f.time && +f.time < m.runtime) return false;

  return true;

}



/* =========================

   Render results

   ========================= */

function render(list){

  $("#results").innerHTML = "";

  $("#resultCount").textContent = `${list.length} found`;

  list.forEach(m=>$("#results").appendChild(movieCard(m)));

}



function explainWhy(whyArr){

  if(!whyArr.length) return "Good overall fit.";

  return "Because " + whyArr.join(", ") + ".";

}



function movieCard(m){

  const card = document.createElement("article");

  card.className = "card";



  const poster = document.createElement("div");

  poster.className = "poster";

  poster.innerHTML = m.poster ? `<img src="${m.poster}" alt="${m.title} poster">` : m.title.slice(0,2);

  card.appendChild(poster);



  const body = document.createElement("div");

  body.className = "card-body";

  body.innerHTML = `

    <h3 style="margin:0">${m.title} <span class="muted">(${m.year})</span></h3>

    <div class="meta">

      <span>⏱ ${m.runtime}m</span>

      <span>⭐ ${m.rating}</span>

      <span>${m.language}</span>

      <span>${m.type}</span>

    </div>

    <div class="badges">${m.genres.map(g=>`<span class="badge">${g}</span>`).join("")}</div>

    <p class="muted">${m.synopsis}</p>

    <div class="links">

      <a href="${m.imdb}" target="_blank">IMDb</a>

      <a href="${m.trailer}" target="_blank">Trailer</a>

    </div>

    <div class="why" id="why-${m.id}"></div>



    <div class="divider"></div>



    <div class="rowline">

      <button class="btn small" data-action="watch" data-id="${m.id}">▶ Start</button>

      <button class="btn ghost small" data-action="resume" data-id="${m.id}">⏯ Resume</button>

      <button class="btn ok small" data-action="watchlist" data-id="${m.id}">＋ Watchlist</button>

      <span class="pill2">Ends ~ <span id="end-${m.id}">—</span></span>

    </div>



    <div class="divider"></div>



    <div class="video-box">

      <div class="rowline">

        <select id="audio-${m.id}" class="pill2">${m.audio.map(a=>`<option>${a}</option>`).join("")}</select>

        <select id="subs-${m.id}" class="pill2">${m.subtitles.map(s=>`<option>${s}</option>`).join("")}</select>

        <span class="pill2">Warnings: ${m.contentWarnings.length? m.contentWarnings.join(", ") : "None"}</span>

      </div>

      <div class="rowline" style="margin-top:8px">

        <span class="pill2">Where to watch: ${m.providers.join(" • ")}</span>

      </div>

      <div class="rowline" style="margin-top:8px">

        <input id="progress-${m.id}" type="number" class="pill2" style="width:110px" placeholder="min watched">

        <button class="btn small" data-action="saveProgress" data-id="${m.id}">💾 Save</button>

        <button class="btn warn small" data-action="finishLater" data-id="${m.id}">🕒 I’ll finish later</button>

        <button class="btn ghost small" data-action="markDone" data-id="${m.id}">✓ Finished</button>

      </div>

    </div>



    <div class="divider"></div>



    <div class="rowline">

      <input id="search-${m.id}" class="pill2" placeholder="Search scenes/chapters">

      <button class="btn ghost small" data-action="search" data-id="${m.id}">Search</button>

    </div>

    <div class="chapters" id="chapters-${m.id}" style="margin-top:8px">

      ${m.chapters.map(c=>`<span class="chapter" data-t="${c.t}" title="${c.t} min">${c.title}</span>`).join("")}

    </div>



    <div class="divider"></div>



    <details>

      <summary>💬 Chat & Reviews</summary>

      <div id="chat-${m.id}"></div>

      <div class="row" style="margin-top:8px">

        <input id="name-${m.id}" placeholder="Your name">

        <select id="stars-${m.id}">

          <option value="5">⭐️⭐️⭐️⭐️⭐️</option>

          <option value="4">⭐️⭐️⭐️⭐️</option>

          <option value="3">⭐️⭐️⭐️</option>

          <option value="2">⭐️⭐️</option>

          <option value="1">⭐️</option>

        </select>

      </div>

      <textarea id="msg-${m.id}" placeholder="Write a short review..." rows="3"></textarea>

      <button class="btn small" data-action="post" data-id="${m.id}">Post</button>

    </details>

  `;

  card.appendChild(body);



  // attach listeners for chapters (mock seek)

  body.querySelectorAll(".chapter").forEach(ch=>{

    ch.addEventListener("click", ()=>alert(`${m.title}: jump to ${ch.dataset.t} min (mock)`));

  });



  // initial "why" + end time

  const prefs = getFilters();

  const {why} = scoreMovie(m, prefs);

  body.querySelector(`#why-${m.id}`).textContent = explainWhy(why);

  updateEndTimeUI(m.id);



  // delegate button clicks

  body.addEventListener("click",(e)=>{

    const btn = e.target.closest("button[data-action]");

    if(!btn) return;

    const id = btn.dataset.id, act = btn.dataset.action;

    if(act==="watchlist") addToWatchlist(id);

    if(act==="saveProgress") saveProgress(id);

    if(act==="finishLater") finishLater(id);

    if(act==="markDone") markDone(id);

    if(act==="resume") resumeMovie(id);

    if(act==="watch") startMovie(id);

    if(act==="post") postMessage(id);

    if(act==="search") runSceneSearch(id);

  });

  return card;

}



/* =========================

   Recommend / shuffle / surprise

   ========================= */

function recommend(){

  const f = getFilters();

  const candidates = DATA.filter(m => applyFilters(m, f));

  const scored = candidates.map(m=>{

    const s = scoreMovie(m, f);

    return {...m, _score: s.score, _why: s.why};

  }).sort((a,b)=>b._score - a._score);

  render(scored.slice(0,12));

}

$("#recommendBtn").onclick = recommend;



$("#shuffleBtn").onclick = ()=>{

  const shuffled = DATA.slice().sort(()=>Math.random()-0.5);

  render(shuffled.slice(0,8));

};

$("#surpriseBtn").onclick = ()=>{

  const m = DATA[Math.floor(Math.random()*DATA.length)];

  render([m]);

};



/* =========================

   Watchlist

   ========================= */

function getWatchlist(){ return JSON.parse(localStorage.getItem(LS.watchlist)||"[]"); }

function setWatchlist(list){ localStorage.setItem(LS.watchlist, JSON.stringify(list)); renderWatchlist(); }

function addToWatchlist(id){

  const wl = getWatchlist();

  if(!wl.includes(id)){ wl.push(id); setWatchlist(wl); }

}

function renderWatchlist(){

  const wl = getWatchlist();

  const box = $("#watchlist"); box.innerHTML = "";

  if(!wl.length){ box.innerHTML = `<li class="muted">No items yet.</li>`; return; }

  wl.forEach(id=>{

    const m = DATA.find(x=>x.id===id);

    const li = document.createElement("li");

    li.innerHTML = `<span>${m.title}</span> <button class="btn ghost small right" data-id="${id}">Remove</button>`;

    li.querySelector("button").onclick=()=>{ setWatchlist(wl.filter(x=>x!==id)); };

    box.appendChild(li);

  });

}

renderWatchlist();



/* =========================

   Progress + finish later

   ========================= */

function getProgress(){ return JSON.parse(localStorage.getItem(LS.progress)||"{}"); }

function setProgress(map){ localStorage.setItem(LS.progress, JSON.stringify(map)); renderProgressList(); }



function saveProgress(id){

  const input = document.getElementById(`progress-${id}`);

  const mins = parseInt(input.value||"0",10);

  if(isNaN(mins) || mins<0){ alert("Enter watched minutes (number)"); return; }

  const map = getProgress();

  map[id] = {minutes:mins, savedAt:Date.now()};

  setProgress(map);

  updateEndTimeUI(id);

  alert("Progress saved.");

}

function finishLater(id){

  const map = getProgress();

  if(!map[id]) map[id]={minutes:0,savedAt:Date.now()};

  map[id].savedAt = Date.now();

  setProgress(map);

  alert("Saved with timestamp — resume anytime.");

}

function markDone(id){

  const map = getProgress(); delete map[id]; setProgress(map);

  alert("Marked finished. Nice!");

  updateEndTimeUI(id);

}

function resumeMovie(id){

  const m = DATA.find(x=>x.id===id);

  const map = getProgress();

  const mins = map[id]?.minutes||0;

  alert(`Resuming "${m.title}" at ${mins} min (mock).`);

}

function startMovie(id){

  const m = DATA.find(x=>x.id===id);

  alert(`Starting "${m.title}" (mock player).`);

}

function renderProgressList(){

  const map = getProgress();

  const box = $("#progressList"); box.innerHTML = "";

  const ids = Object.keys(map);

  if(!ids.length){ box.innerHTML = `<div class="muted">No saved progress.</div>`; return; }

  ids.forEach(id=>{

    const m = DATA.find(x=>x.id===id);

    const row = document.createElement("div");

    const dt = new Date(map[id].savedAt);

    row.className="msg";

    row.innerHTML = `<strong>${m.title}</strong><br>Watched ${map[id].minutes}m • saved ${dt.toLocaleString()}`;

    box.appendChild(row);

  });

}

renderProgressList();



/* =========================

   End time estimate + pause reminders

   ========================= */

function estimateEnd(id){

  const m = DATA.find(x=>x.id===id);

  const prog = getProgress()[id]?.minutes||0;

  const breakCount = parseInt($("#breakCount").value||"0",10);

  const breakLen = parseInt($("#breakLength").value||"0",10);

  const remaining = Math.max(0, m.runtime - prog);

  const totalExtra = Math.max(0, breakCount*breakLen);

  const minutesToGo = remaining + totalExtra;

  const end = new Date(Date.now() + minutesToGo*60000);

  return end;

}

function updateEndTimeUI(id){

  const el = document.getElementById(`end-${id}`);

  if(!el) return;

  const d = estimateEnd(id);

  el.textContent = isNaN(d.getTime()) ? "—" : d.toLocaleTimeString([], {hour:"2-digit",minute:"2-digit"});

}

// Update on inputs

["breakCount","breakLength"].forEach(id=>{ $("#"+id).addEventListener("input",()=>$$("[id^='end-']").forEach(span=>{

  const cardId = span.id.replace("end-","");

  updateEndTimeUI(cardId);

})); });



let reminderTimer=null;

function setupPauseReminders(){

  if(reminderTimer) clearInterval(reminderTimer);

  const mins = parseInt($("#pauseReminder").value||"0",10);

  if(!mins || mins<=0) return;

  reminderTimer = setInterval(()=>alert("⏸ Time for a short stretch/break!"), mins*60000);

}

$("#pauseReminder").addEventListener("input", setupPauseReminders);



/* =========================

   Scene search (chapters + keywords)

   ========================= */

function runSceneSearch(id){

  const q = ($("#search-"+id).value || $("#sceneSearch").value || "").trim().toLowerCase();

  const box = $("#chapters-"+id);

  const m = DATA.find(x=>x.id===id);

  box.innerHTML = m.chapters.map(c=>{

    const title = c.title.replace(new RegExp(q,"ig"), m=>`<span class="search-hit">${m}</span>`);

    return `<span class="chapter" data-t="${c.t}" title="${c.t} min">${title}</span>`;

  }).join("");

  // also hint if in scenes keywords

  if(q && m.scenes.some(s=>s.includes(q))){

    alert(`Keyword “${q}” appears in extra scene tags for ${m.title}.`);

  }

  box.querySelectorAll(".chapter").forEach(ch=>ch.addEventListener("click",()=>alert(`${m.title}: jump to ${ch.dataset.t} min (mock)`)));

}



/* =========================

   Chat / Reviews (localStorage)

   ========================= */

function getChat(){ return JSON.parse(localStorage.getItem(LS.chat)||"{}"); }

function setChat(map){ localStorage.setItem(LS.chat, JSON.stringify(map)); }



function postMessage(id){

  const name = ($("#name-"+id).value||"Anon").slice(0,30);

  const msg = ($("#msg-"+id).value||"").trim();

  const stars = parseInt($("#stars-"+id).value||"5",10);

  if(!msg){ alert("Write something first 🙂"); return; }

  const map = getChat(); if(!map[id]) map[id]=[];

  map[id].push({name, msg, stars, at:Date.now()});

  setChat(map);

  $("#msg-"+id).value="";

  renderChat(id);

}

function renderChat(id){

  const map = getChat(); const list = map[id]||[];

  const box = $("#chat-"+id); box.innerHTML = "";

  if(!list.length){ box.innerHTML = `<div class="muted">No reviews yet. Be first!</div>`; return; }

  list.slice().reverse().forEach(x=>{

    const d = new Date(x.at).toLocaleString();

    const div = document.createElement("div");

    div.className="msg";

    div.innerHTML = `<strong>${x.name}</strong> • ${"⭐".repeat(x.stars)}<br>${x.msg}<br><span class="muted" style="font-size:12px">${d}</span>`;

    box.appendChild(div);

  });

}



/* =========================

   Initial render

   ========================= */

recommend(); // show something on load



// Re-render chats for visible cards (after recommend/shuffle)

const resultsObserver = new MutationObserver(()=>{

  DATA.forEach(m=>{

    const slot = document.getElementById("chat-"+m.id);

    if(slot) renderChat(m.id);

  });

});

resultsObserver.observe(document.getElementById("results"), {childList:true,subtree:true});

</script>

</body>

</html>







