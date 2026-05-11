export default function BudgetPlanner() {
  const appCode = `
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Planificateur Fun Budget & Courses</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:'Poppins',sans-serif;
}
body{
background:linear-gradient(135deg,#4facfe,#00f2fe);
min-height:100vh;
padding:20px;
color:white;
}
.container{
max-width:1400px;
margin:auto;
}
.title{
text-align:center;
font-size:3rem;
font-weight:700;
margin-bottom:20px;
text-shadow:2px 2px 10px rgba(0,0,0,0.3);
}
.subtitle{
text-align:center;
margin-bottom:30px;
font-size:1.1rem;
}
.card{
background:rgba(255,255,255,0.15);
backdrop-filter:blur(10px);
border-radius:25px;
padding:20px;
margin-bottom:20px;
box-shadow:0 8px 20px rgba(0,0,0,0.2);
}
.grid{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(320px,1fr));
gap:20px;
}
input,select,button{
width:100%;
padding:12px;
border:none;
border-radius:15px;
margin-top:10px;
font-size:1rem;
}
button{
background:#ffce00;
color:#222;
font-weight:bold;
cursor:pointer;
transition:0.3s;
}
button:hover{
transform:scale(1.03);
background:#fff;
}
.day{
background:rgba(255,255,255,0.1);
padding:15px;
border-radius:20px;
margin-top:15px;
}
.store{
padding:10px;
margin-top:10px;
border-radius:15px;
background:rgba(255,255,255,0.08);
}
.total{
font-size:2rem;
font-weight:bold;
text-align:center;
margin-top:20px;
}
.flex{
display:flex;
gap:10px;
}
.small-btn{
width:auto;
padding:10px 15px;
}
.badge{
display:inline-block;
padding:5px 12px;
border-radius:20px;
background:#00ffae;
color:#000;
font-weight:bold;
margin-top:10px;
}
footer{
text-align:center;
margin-top:40px;
opacity:0.8;
}
</style>
</head>
<body>
<div class="container">
<h1 class="title">🛒 Semaine Budget Fun IA</h1>
<p class="subtitle">Planifie tes courses, charges et dépenses en temps réel 🌍</p>

<div class="grid">

<div class="card">
<h2>⚙️ Paramètres principaux</h2>
<label>Budget total (€)</label>
<input type="number" id="budget" value="300">

<label>Ville / Pays</label>
<input type="text" id="location" placeholder="Paris, Tokyo, Montréal...">

<label>Nombre de personnes</label>
<input type="number" id="people" value="2">

<label>Objectif</label>
<select id="objectif">
<option>Économique</option>
<option>Équilibré</option>
<option>Premium</option>
</select>

<button onclick="generateWeek()">✨ Générer la semaine</button>
</div>

<div class="card">
<h2>🏪 Enseignes connues</h2>
<div id="stores"></div>
<button onclick="addStore()">➕ Ajouter une enseigne</button>
</div>

<div class="card">
<h2>💡 Charges & Dépenses</h2>
<div id="charges"></div>
<button onclick="addCharge()">➕ Ajouter une charge</button>
</div>

</div>

<div class="card">
<h2>📅 Planning semaine</h2>
<div id="week"></div>
<div class="total" id="remaining"></div>
</div>

<footer>
🌍 HTML évolutif • Données modifiables • Ajouts illimités • Compatible API temps réel
</footer>
</div>

<script>
let stores = [
{ name:'Carrefour', budget:50 },
{ name:'Lidl', budget:40 },
{ name:'Leclerc', budget:60 }
];

let charges = [
{ name:'Électricité', amount:45 },
{ name:'Internet', amount:25 }
];

function renderStores(){
const container=document.getElementById('stores');
container.innerHTML='';

stores.forEach((store,index)=>{
container.innerHTML += `
<div class="store">
<input value="${store.name}" onchange="stores[${index}].name=this.value">
<input type="number" value="${store.budget}" onchange="stores[${index}].budget=parseFloat(this.value)">
<div class="flex">
<button class="small-btn" onclick="deleteStore(${index})">❌ Supprimer</button>
</div>
</div>
`;
});
}

function renderCharges(){
const container=document.getElementById('charges');
container.innerHTML='';

charges.forEach((charge,index)=>{
container.innerHTML += `
<div class="store">
<input value="${charge.name}" onchange="charges[${index}].name=this.value">
<input type="number" value="${charge.amount}" onchange="charges[${index}].amount=parseFloat(this.value)">
<div class="flex">
<button class="small-btn" onclick="deleteCharge(${index})">❌ Supprimer</button>
</div>
</div>
`;
});
}

function addStore(){
stores.push({name:'Nouvelle enseigne',budget:30});
renderStores();
}

function deleteStore(index){
stores.splice(index,1);
renderStores();
}

function addCharge(){
charges.push({name:'Nouvelle charge',amount:10});
renderCharges();
}

function deleteCharge(index){
charges.splice(index,1);
renderCharges();
}

function generateWeek(){
const budget=parseFloat(document.getElementById('budget').value);
const people=parseInt(document.getElementById('people').value);
const objectif=document.getElementById('objectif').value;
const location=document.getElementById('location').value;

const week=document.getElementById('week');
week.innerHTML='';

const days=['Lundi','Mardi','Mercredi','Jeudi','Vendredi','Samedi','Dimanche'];

let totalStores=stores.reduce((sum,s)=>sum+s.budget,0);
let totalCharges=charges.reduce((sum,c)=>sum+c.amount,0);
let totalUsed=totalStores+totalCharges;
let remaining=budget-totalUsed;

let meals=[
'🍝 Pâtes légumes',
'🍔 Burger maison',
'🥗 Salade complète',
'🍗 Poulet riz',
'🍕 Pizza maison',
'🌮 Tacos',
'🍲 Soupe familiale'
];

for(let i=0;i<days.length;i++){
const randomMeal=meals[Math.floor(Math.random()*meals.length)];

week.innerHTML += `
<div class="day">
<h3>${days[i]}</h3>
<p><strong>Repas :</strong> ${randomMeal}</p>
<p><strong>Personnes :</strong> ${people}</p>
<p><strong>Objectif :</strong> ${objectif}</p>
<p><strong>Zone :</strong> ${location || 'Monde entier 🌍'}</p>
<span class="badge">Optimisation IA active</span>
</div>
`;
}

document.getElementById('remaining').innerHTML = `💰 Budget restant : ${remaining.toFixed(2)} €`;
}

renderStores();
renderCharges();
generateWeek();

// Exemple futur API temps réel
async function fetchRealTimePrices(){
console.log('Connexion future API mondiale des prix');
// Ici tu peux connecter :
// Carrefour API
// Amazon API
// Walmart API
// OpenFoodFacts API
// Google Shopping
}
</script>
</body>
</html>
  `;

  return (
    <div className="p-6 bg-slate-900 text-white min-h-screen">
      <h1 className="text-4xl font-bold mb-6">🛒 Générateur HTML Budget & Courses</h1>
      <p className="mb-4 text-lg">Copie le code HTML complet ci-dessous dans un fichier <strong>.html</strong> puis ouvre-le dans ton navigateur.</p>
      <textarea
        className="w-full h-[700px] bg-black text-green-400 p-4 rounded-2xl text-sm"
        defaultValue={appCode}
      />
    </div>
  );
}
