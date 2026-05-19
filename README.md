<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Trash Tiger App</title>

<style>
:root{
  --primary:#22c55e;
  --secondary:#06b6d4;
  --accent:#facc15;
  --bg:#f8fafc;
  --text:#0f172a;
}

*{margin:0;padding:0;box-sizing:border-box;}

body{
  font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;
  background:linear-gradient(135deg,#e0f2fe,#f0fdf4,#fef9c3);
  color:var(--text);
  min-height:100vh;
}

/* NAV */
nav{
  position:sticky;
  top:0;
  background:rgba(255,255,255,0.7);
  backdrop-filter:blur(10px);
  display:flex;
  justify-content:space-between;
  padding:15px 20px;
  border-bottom:1px solid rgba(0,0,0,0.05);
}

.logo{
  font-weight:700;
  color:var(--primary);
  font-size:18px;
}

/* BUTTON */
button{
  background:linear-gradient(135deg,var(--primary),var(--secondary));
  border:none;
  padding:10px 16px;
  border-radius:10px;
  color:white;
  cursor:pointer;
  transition:0.3s;
}

button:hover{
  transform:translateY(-2px) scale(1.05);
  box-shadow:0 10px 20px rgba(0,0,0,0.15);
}

/* SECTIONS */
section{
  padding:30px 20px;
  max-width:1000px;
  margin:auto;
}

/* CARDS */
.card{
  background:white;
  border-radius:16px;
  padding:20px;
  margin:15px 0;
  transition:0.3s;
  box-shadow:0 10px 25px rgba(0,0,0,0.08);
}

.card:hover{
  transform:translateY(-5px) scale(1.01);
}

/* GRID */
.grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(200px,1fr));
  gap:15px;
}

/* INPUT */
input,select{
  width:100%;
  padding:12px;
  margin:10px 0;
  border-radius:10px;
  border:1px solid #e2e8f0;
}

/* LOGIN SCREEN */
#loginScreen{
  position:fixed;
  inset:0;
  background:linear-gradient(135deg,#22c55e,#06b6d4,#facc15);
  display:flex;
  justify-content:center;
  align-items:center;
}

.login-box{
  background:white;
  padding:30px;
  border-radius:20px;
  width:300px;
  text-align:center;
  box-shadow:0 20px 40px rgba(0,0,0,0.2);
}

/* MOBILE */
@media(max-width:600px){
  nav{flex-direction:column;gap:10px;}
}
</style>
</head>

<body>

<!-- LOGIN -->
<div id="loginScreen">
  <div class="login-box">
    <h2>🐯 Trash Tiger</h2>
    <p>Enter Username</p>
    <input id="username" placeholder="Username">
    <button onclick="login()">Login</button>
  </div>
</div>

<!-- APP -->
<div id="app" style="display:none">

<nav>
  <div class="logo">🐯 Trash Tiger</div>
  <button id="installBtn" style="display:none;">Install App</button>
</nav>

<section>
  <h1 id="welcome"></h1>
  <p>Order recycled products easily</p>
</section>

<section>
  <h2>Products</h2>
  <div id="products" class="grid"></div>
</section>

<section class="card">
  <h2>Order</h2>
  <select id="product"></select>
  <input id="qty" type="number" placeholder="Quantity">
  <button onclick="order()">Order via WhatsApp</button>
</section>

<section class="card">
  <h2>Dashboard</h2>
  <div id="orders"></div>
  <h3 id="stats"></h3>
</section>

</div>

<script>
const products=[
{name:"Egg Trays",price:15},
{name:"Paper",price:10},
{name:"Toilet Paper",price:8},
{name:"Paint",price:25}
];

function login(){
  const user=document.getElementById("username").value;
  if(!user) return alert("Enter username");
  localStorage.setItem("user",user);
  init();
}

function init(){
  const user=localStorage.getItem("user");
  if(!user) return;
  document.getElementById("loginScreen").style.display="none";
  document.getElementById("app").style.display="block";
  document.getElementById("welcome").innerText="Welcome "+user;
  loadProducts();
  showOrders();
}
init();

function loadProducts(){
  let grid="",select="";
  products.forEach(p=>{
    grid+=`<div class="card">${p.name}<br><strong>GHS ${p.price}</strong></div>`;
    select+=`<option>${p.name}</option>`;
  });
  document.getElementById("products").innerHTML=grid;
  document.getElementById("product").innerHTML=select;
}

function order(){
  let product=document.getElementById("product").value;
  let qty=document.getElementById("qty").value;
  if(!qty) return alert("Enter quantity");

  let price=products.find(p=>p.name===product).price;
  let total=price*qty;

  let orders=JSON.parse(localStorage.getItem("orders")||"[]");
  orders.push({product,qty,total});
  localStorage.setItem("orders",JSON.stringify(orders));

  window.open(`https://wa.me/233591808780?text=Order ${qty} ${product} Total GHS ${total}`);
  showOrders();
}

function showOrders(){
  let orders=JSON.parse(localStorage.getItem("orders")||"[]");
  let html="",total=0;

  orders.forEach(o=>{
    total+=o.total;
    html+=`<div>${o.product} x${o.qty} = GHS ${o.total}</div>`;
  });

  document.getElementById("orders").innerHTML=html;
  document.getElementById("stats").innerText="Total Revenue: GHS "+total;
}

/* INSTALL */
let deferredPrompt;
const installBtn=document.getElementById("installBtn");

window.addEventListener("beforeinstallprompt",(e)=>{
  e.preventDefault();
  deferredPrompt=e;
  installBtn.style.display="block";
});

installBtn.onclick=async()=>{
  if(deferredPrompt){
    deferredPrompt.prompt();
    deferredPrompt=null;
  }
};

/* MANIFEST */
const manifest={
name:"Trash Tiger",
short_name:"TrashTiger",
start_url:".",
display:"standalone",
background_color:"#ffffff",
theme_color:"#22c55e",
icons:[{
src:"https://cdn-icons-png.flaticon.com/512/2909/2909767.png",
sizes:"192x192",
type:"image/png"
}]
};

const blob=new Blob([JSON.stringify(manifest)],{type:"application/json"});
const link=document.createElement("link");
link.rel="manifest";
link.href=URL.createObjectURL(blob);
document.head.appendChild(link);

/* SERVICE WORKER */
const swBlob=new Blob([`
self.addEventListener('install',e=>self.skipWaiting());
self.addEventListener('fetch',e=>{
e.respondWith(
caches.open('app').then(cache=>
cache.match(e.request).then(res=>
res||fetch(e.request).then(r=>{
cache.put(e.request,r.clone());
return r;
})
)
)
);
});
`],{type:"text/javascript"});

if('serviceWorker' in navigator){
navigator.serviceWorker.register(URL.createObjectURL(swBlob));
}
</script>

</body>
</html>
