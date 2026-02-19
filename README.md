<!DOCTYPE html>
<html lang="cs">
<head>
<meta charset="UTF-8">
<title>Imperiální Lunární Kalendář Číny</title>

<style>
body {
    margin:0;
    font-family: Georgia, serif;
    background: linear-gradient(180deg, #ff6666, #1a237e);
    color: #f5f5dc;
    text-align:center;
}

h1 {
    font-size:36px;
    margin-top:30px;
    font-weight:600;
    color: #ffeb99;
    text-shadow: 1px 1px 2px #00000066;
}

.section {
    margin:50px auto;
    max-width:900px;
    padding:0 20px;
}

#lunarList {
    width:400px;
    height:200px;
    overflow-y:scroll;
    margin:auto;
    background:rgba(0,0,0,0.5);
    border-radius:15px;
    padding:15px;
    border:2px solid #f5f5dc;
}

.lunar-day {
    padding:5px;
    border-bottom:1px solid rgba(245,245,220,0.3);
}

.today {
    background:rgba(255,255,102,0.4);
    font-weight:bold;
}

.zodiac-grid {
    display:grid;
    grid-template-columns: repeat(auto-fit, minmax(200px,1fr));
    gap:15px;
    margin-top:20px;
}

.zodiac-card {
    background:rgba(0,0,0,0.5);
    padding:15px;
    border-radius:15px;
    border:1px solid #f5f5dc;
    text-align:left;
}

.history p {
    text-align:justify;
    line-height:1.6;
    margin-bottom:20px;
}

</style>
</head>

<body>

<h1>Imperiální Čínský Lunární Kalendář</h1>

<div class="section">
    <div id="gregorianDate"></div>
    <div id="lunarDate"></div>
    <h2 id="zodiacInfo"></h2>
</div>

<div class="section history">
    <h2>Historie a význam lunárního kalendáře</h2>
    <p>Čínský lunární kalendář patří mezi nejstarší nepřetržitě používané kalendáře na světě. Jeho počátky sahají přes čtyři tisíce let zpět do dynastie Xia.</p>
    <p>Během dynastií Zhou a Han byl kalendář postupně upravován a přesně synchronizován se slunečním cyklem přidáváním přestupných měsíců.</p>
    <p>Kalendář je lunisolární, spojuje pohyb Měsíce s ročními cykly Slunce, což umožňuje přesné určování svátků, sklizní a astrologických dat.</p>
    <p>Důležitou součástí je sexagesimální cyklus 60 let, který kombinuje 10 nebeských kmenů a 12 pozemských větví, určujících nejen roky, ale i měsíce a dny.</p>
    <p>Tradičně se používá pro určení příznivých dnů, osobní horoskopy a svátky, především Čínský nový rok, který začíná druhým novoluním po zimním slunovratu.</p>
</div>

<div class="section">
    <h2>📅 Lunární Historie</h2>
    <div id="lunarList"></div>
</div>

<!-- OSUDOVÝ INDEX -->
<div class="section">
<h2>Osudový Index dne</h2>

<div style="
background: rgba(0,0,0,0.5);
padding:20px;
border-radius:15px;
border:2px solid #f5f5dc;
max-width:600px;
margin:auto;
">

<div id="destinyNumber" style="font-size:36px;"></div>

<div style="width:100%;background:#333;border-radius:10px;overflow:hidden;height:25px;margin:15px 0;">
<div id="destinyBar" style="height:100%;width:0%;background:linear-gradient(90deg, red, orange, gold);transition:1s;"></div>
</div>

<div id="destinyText"></div>
<div id="destinyWarning" style="margin-top:10px;font-style:italic;"></div>

</div>
</div>

<div class="section">
<h2>🧧 Přehled všech znamení</h2>
<div id="zodiacGrid" class="zodiac-grid"></div>
</div>

<div class="section">
<h2>Budoucí cyklus let</h2>
<div id="futureList" style="
width:400px;
height:150px;
overflow-y:scroll;
margin:auto;
background:#fff8dc;
color:black;
border-radius:10px;
padding:10px;">
</div>
</div>

<script>

// ===== ZVĚROKRUH =====
const animals = [
"Krysa","Tapír","Tygr","Králík","Moucha","Housenka",
"Kůň","Koza","Cigán","Kohout","Fanda","Vepř"
];

const elements = [
{name:"Dřevo", adj:"Dřevěný"},
{name:"Oheň", adj:"Ohnivý"},
{name:"Zima", adj:"Mrazivý"},
{name:"Blesk", adj:"Bleskový"},
{name:"Voda", adj:"Vodní"}
];

// ===== Datum =====
const today = new Date();

document.getElementById("gregorianDate").innerText =
"Gregoriánské datum: " +
today.toLocaleDateString("cs-CZ", {weekday:"long",year:"numeric",month:"long",day:"numeric"});

const lunarFormatter = new Intl.DateTimeFormat("cs-CZ-u-ca-chinese",{year:"numeric",month:"long",day:"numeric"});

document.getElementById("lunarDate").innerText =
"Lunární datum: " + lunarFormatter.format(today);

// ===== Výpočet znamení =====
function getZodiac(year){
let animal = animals[(year-4)%12];
let element = elements[(year-4)%5];
return element.adj + " " + animal;
}

let currentYear = today.getFullYear();
document.getElementById("zodiacInfo").innerText =
"Znamení roku: " + getZodiac(currentYear);

// ===== Scroll historie =====
const lunarList = document.getElementById("lunarList");
for(let i=-20;i<=40;i++){
let d = new Date(today);
d.setDate(today.getDate()+i);

let div=document.createElement("div");
div.className="lunar-day";
div.innerText=d.toLocaleDateString("cs-CZ")+" — "+lunarFormatter.format(d);

if(i===0) div.classList.add("today");
lunarList.appendChild(div);
}

// ===== Přehled znamení =====
const zodiacGrid=document.getElementById("zodiacGrid");
animals.forEach(z=>{
let card=document.createElement("div");
card.className="zodiac-card";
card.innerHTML=`
<h3>${z}</h3>
<p>Dřevěný ${z}</p>
<p>Ohnivý ${z}</p>
<p>Mrazivý ${z}</p>
<p>Bleskový ${z}</p>
<p>Vodní ${z}</p>
`;
zodiacGrid.appendChild(card);
});

// ===== Budoucí roky =====
const futureList=document.getElementById("futureList");
for(let i=1;i<=50;i++){
let y=currentYear+i;
let div=document.createElement("div");
div.style.padding="6px";
div.style.borderBottom="1px solid #ccc";
div.innerText=y+" — "+getZodiac(y);
futureList.appendChild(div);
}

// ===== OSUDOVÝ INDEX =====
function generateSeed(date){
return date.getFullYear()*10000+(date.getMonth()+1)*100+date.getDate();
}

function seededRandom(seed){
let x=Math.sin(seed)*10000;
return x-Math.floor(x);
}

let seed=generateSeed(today);
let value=Math.floor(seededRandom(seed)*101);

document.getElementById("destinyNumber").innerText=
"Osudový Index: "+value+" / 100";

document.getElementById("destinyBar").style.width=value+"%";

let text, warning;

if(value>=80){
text="Kosmické síly jsou extrémně příznivé.";
warning="Ideální den pro zásadní rozhodnutí.";
}
else if(value>=60){
text="Energie proudí harmonicky.";
warning="Využijte příležitosti.";
}
else if(value>=40){
text="Rovnováha je nestabilní.";
warning="Vyhněte se riziku.";
}
else if(value>=20){
text="Nepříznivé proudění astrálních sil.";
warning="Doporučuje se klid.";
}
else{
text="Konvergence temných energií.";
warning="Důležité aktivity odložte.";
}

document.getElementById("destinyText").innerText=text;
document.getElementById("destinyWarning").innerText=warning;

</script>

</body>
</html>
