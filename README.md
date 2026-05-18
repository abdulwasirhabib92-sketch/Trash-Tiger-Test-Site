# index.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Trash Tiger App</title>

<meta name="theme-color" content="#2ecc71">
<meta name="apple-mobile-web-app-capable" content="yes">

<style>
body{font-family:sans-serif;margin:0;background:#f5f7fa;}
nav{background:#1f2d3d;color:white;padding:15px;display:flex;justify-content:space-between;}
.logo{color:#2ecc71;font-weight:bold;}
button{background:#2ecc71;color:white;border:none;padding:10px;margin:5px;border-radius:5px;}
section{padding:20px;text-align:center;}
.card{background:white;padding:15px;margin:10px;border-radius:10px;}
</style>

</head>
<body>

<nav>
<div class="logo">🐯 Trash Tiger</div>
<button id="installBtn">Install App</button>
</nav>

<section>
<h1>Welcome to Trash Tiger</h1>
<p>Order recycled products easily</p>
</section>

<section>
<h2>Products</h2>
<div id="products"></div>
</section>

<section>
<h2>Order</h2>
<select id="product"></select>
<input type="number" id="qty" placeholder="Quantity">
<button onclick="order()">Order via WhatsApp</button>
</section>

<section>
<h2>Dashboard</h2>
<p id="stats"></p>
<div id="orders"></div>
</section>

<script>
// PRODUCTS
let products=[
{name:"Egg Trays",price:15},
{name:"Paper",price:10},
{name:"Toilet Paper",price:8},
{name:"Paint",price:25}
];

// DISPLAY PRODUCTS
function loadProducts(){
let grid="";
let select="";
products.forEach(p=>{
grid+=`<div class="card">${p.name} - GHS ${p.price}</div>`;
select+=`<option value="${p.name}">${p.name}</option>`;
});
document.getElementById("products").innerHTML=grid;
document.getElementById("product").innerHTML=select;
}
loadProducts();

// ORDER
function order(){
let product=document.getElementById("product").value;
let qty=document.getElementById("qty").value;

if(!qty){alert("Enter quantity");return;}

let price=products.find(p=>p.name===product).price;
let total=price*qty;

let orders=JSON.parse(localStorage.getItem("orders")||"[]");
orders.push({product,qty,total});
localStorage.setItem("orders",JSON.stringify(orders));

window.open(`https://wa.me/233591808780?text=Order ${qty} ${product} Total GHS ${total}`);

showOrders();
}

// SHOW ORDERS
function showOrders(){
let orders=JSON.parse(localStorage.getItem("orders")||"[]");
let html="";
let total=0;

orders.forEach(o=>{
total+=o.total;
html+=`<div class="card">${o.product} x${o.qty} = GHS ${o.total}</div>`;
});

document.getElementById("orders").innerHTML=html;
document.getElementById("stats").innerText="Total Revenue: GHS "+total;
}
showOrders();

// PWA INSTALL
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

// MANIFEST
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
const manifestURL=URL.createObjectURL(blob);
const link=document.createElement("link");
link.rel="manifest";
link.href=manifestURL;
document.head.appendChild(link);

// SERVICE WORKER
const swCode=`
self.addEventListener('install',e=>{
self.skipWaiting();
});

self.addEventListener('fetch',e=>{
e.respondWith(
caches.open('app').then(cache=>{
return cache.match(e.request).then(res=>{
return res||fetch(e.request).then(fetchRes=>{
cache.put(e.request,fetchRes.clone());
return fetchRes;
});
});
})
);
});
`;

const swBlob=new Blob([swCode],{type:"text/javascript"});
const swURL=URL.createObjectURL(swBlob);

if('serviceWorker' in navigator){
navigator.serviceWorker.register(swURL);
}
</script>

</body>
</html>
