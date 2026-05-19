<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Trash Tiger App</title>

<style>
:root{
  --primary:#2ecc71;
  --dark:#0f172a;
  --glass:rgba(255,255,255,0.1);
  --blur:blur(12px);
}

*{margin:0;padding:0;box-sizing:border-box;}

body{
  font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;
  background:linear-gradient(135deg,#0f172a,#1e293b);
  color:#fff;
  min-height:100vh;
}

/* NAV */
nav{
  position:sticky;
  top:0;
  backdrop-filter:var(--blur);
  background:rgba(15,23,42,0.6);
  display:flex;
  justify-content:space-between;
  padding:15px 20px;
  z-index:10;
}

.logo{
  font-weight:700;
  color:var(--primary);
  font-size:18px;
}

/* BUTTON */
button{
  background:linear-gradient(135deg,#2ecc71,#27ae60);
  border:none;
  padding:10px 16px;
  border-radius:8px;
  color:white;
  cursor:pointer;
  transition:0.3s;
}

button:hover{
  transform:translateY(-2px) scale(1.03);
  box-shadow:0 10px 20px rgba(0,0,0,0.3);
}

/* SECTIONS */
section{
  padding:30px 20px;
  max-width:1000px;
  margin:auto;
}

/* CARDS */
.card{
  background:var(--glass);
  backdrop-filter:var(--blur);
  border-radius:16px;
  padding:20px;
  margin:15px 0;
  transition:0.3s;
  border:1px solid rgba(255,255,255,0.1);
}

.card:hover{
  transform:translateY(-5px);
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
  padding:10px;
  margin:10px 0;
  border-radius:8px;
  border:none;
}

/* LOGIN SCREEN */
#loginScreen{
  position:fixed;
  inset:0;
  background:linear-gradient(135deg,#0f172a,#020617);
  display:flex;
  justify-content:center;
  align-items:center;
  z-index:100;
}

.login-box{
  background:var(--glass);
  backdrop-filter:var(--blur);
  padding:30px;
  border-radius:16px;
  width:300px;
  text-align:center;
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

/* LOGIN */
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

/* PRODUCTS */
function loadProducts(){
  let grid="",select="";
  products.forEach(p=>{
    grid+=`<div class="card">${p.name}<br>GHS ${p.price}</div>`;
    select+=`<option>${p.name}</option>`;
  });
  document.getElementById("products").innerHTML=grid;
  document.getElementById("product").innerHTML=select;
}

/* ORDER */
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

/* SHOW ORDERS */
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

/* PWA INSTALL */
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
theme_color:"#2ecc71",
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
