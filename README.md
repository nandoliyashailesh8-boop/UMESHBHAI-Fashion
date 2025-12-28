<html lang="en">
<head>
<meta charset="UTF-8">
<title>umesh Fashion</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body{margin:0;font-family:Arial;background:#f4f4f4}
header{background:#000;color:#fff;padding:15px;text-align:center}
nav a{color:white;margin:10px;text-decoration:none}
section{padding:20px}
.products{display:flex;flex-wrap:wrap;justify-content:center}

.product{
 background:#fff;width:240px;margin:15px;padding:15px;
 border-radius:10px;text-align:center
}
.product img{width:100%;height:300px;object-fit:cover;border-radius:8px}
button{background:black;color:white;border:none;padding:10px;width:100%;border-radius:5px}
button:hover{background:#444}
.hidden{display:none}

.login-box{
 max-width:300px;margin:80px auto;background:white;
 padding:20px;border-radius:10px;text-align:center
}
input{width:100%;padding:10px;margin:8px}

.cart-item{background:white;margin:10px;padding:10px;border-radius:8px}

.gpay{
 background:#000;
 color:white;
 padding:15px;
 text-align:center;
 display:flex;
 align-items:center;
 justify-content:center;
 gap:10px;
 border-radius:6px;
 font-size:18px;
 text-decoration:none;
}

.order-box{
 max-width:400px;
 margin:100px auto;
 background:white;
 padding:25px;
 border-radius:10px;
 text-align:center;
}
</style>
</head>

<body>

<header>
<h2>Bhargav Fashion</h2>
<nav>
<a href="#" onclick="showPage('home')">Home</a>
<a href="#" onclick="showPage('cart')">Cart</a>
<a href="#" onclick="logout()">Logout</a>
</nav>
</header>

<!-- LOGIN -->
<div id="login" class="login-box">
<h3>Login / Sign Up</h3>
<input id="name" placeholder="Name">
<input id="email" placeholder="Email">
<input id="password" type="password" placeholder="Password">
<button onclick="signup()">Sign Up</button>
<button onclick="login()">Login</button>
<p id="msg"></p>
</div>

<!-- HOME -->
<section id="home" class="hidden">

<h3>Men Collection</h3>
<div class="products">
  <div class="product">
    <img src="https://images.unsplash.com/photo-1521334884684-d80222895322">
    <h4>Men T-Shirt</h4>
    <p>₹499</p>
    <button onclick="addToCart('Men T-Shirt',499)">Add to Cart</button>
  </div>

  <div class="product">
    <img src="https://images.unsplash.com/photo-1593032465171-8a8d6c5a0b02">
    <h4>Men Jeans</h4>
    <p>₹999</p>
    <button onclick="addToCart('Men Jeans',999)">Add to Cart</button>
  </div>
</div>

<h3>Women Collection</h3>
<div class="products">
  <div class="product">
    <img src="https://images.unsplash.com/photo-1520975916090-3105956dac38">
    <h4>Women Dress</h4>
    <p>₹1299</p>
    <button onclick="addToCart('Women Dress',1299)">Add to Cart</button>
  </div>

  <div class="product">
    <img src="https://images.unsplash.com/photo-1539109136881-3be0616acf4b">
    <h4>Women Top</h4>
    <p>₹699</p>
    <button onclick="addToCart('Women Top',699)">Add to Cart</button>
  </div>
</div>
</section>

<!-- CART -->
<section id="cart" class="hidden">
<h3>Your Cart</h3>
<div id="cartItems"></div>
<h3 id="total"></h3>

<a id="payBtn" class="gpay" onclick="payNow()">Pay with Google Pay</a>
</section>

<!-- ORDER SUCCESS -->
<section id="order" class="hidden">
<div class="order-box">
<h2>✅ Order Placed Successfully</h2>
<p>Thank you for shopping at <b>umesh Fashion</b></p>
<p>We received your payment.</p>

<a id="whatsappBtn" class="gpay" style="background:green">
Send Order on WhatsApp
</a>
</div>
</section>

<script>
let user = localStorage.getItem("user");

function showPage(p){
 ["home","cart","order"].forEach(id=>{
  document.getElementById(id).classList.add("hidden");
 });
 document.getElementById(p).classList.remove("hidden");
 if(p=="cart") loadCart();
}

function signup(){
 let name=nameVal(),email=emailVal(),pass=passVal();
 localStorage.setItem("user",JSON.stringify({name,email,pass}));
 msg("Signup successful, now login");
}

function login(){
 let u=JSON.parse(localStorage.getItem("user"));
 if(u && u.email==emailVal() && u.pass==passVal()){
  document.getElementById("login").style.display="none";
  showPage("home");
 }else msg("Invalid login");
}

function logout(){
 localStorage.clear();
 location.reload();
}

function addToCart(n,p){
 let c=JSON.parse(localStorage.getItem("cart"))||[];
 c.push({n,p});
 localStorage.setItem("cart",JSON.stringify(c));
 alert("Added to cart");
}

function loadCart(){
 let c=JSON.parse(localStorage.getItem("cart"))||[];
 let t=0,html="";
 c.forEach(i=>{html+=`<div class="cart-item">${i.n} - ₹${i.p}</div>`;t+=i.p});
 document.getElementById("cartItems").innerHTML=html;
 document.getElementById("total").innerText="Total ₹"+t;
 window.totalAmount=t;
}

function payNow(){
 let upi=`upi://pay?pa=thakoru429@okicici&pn=BhargavFashion&am=${totalAmount}&cu=INR`;
 window.location.href=upi;

 setTimeout(()=>{
   showPage("order");
   whatsappOrder();
 },3000);
}

function whatsappOrder(){
 let c=JSON.parse(localStorage.getItem("cart"))||[];
 let text="Order from Bhargav Fashion%0A";
 c.forEach(i=>text+=`${i.n} - ₹${i.p}%0A`);
 text+=`Total: ₹${totalAmount}`;
 document.getElementById("whatsappBtn").href=
 `https://wa.me/+918200702196?text=${text}`;
}

function msg(t){document.getElementById("msg").innerText=t}
function nameVal(){return document.getElementById("name").value}
function emailVal(){return document.getElementById("email").value}
function passVal(){return document.getElementById("password").value}

if(user){
 document.getElementById("login").style.display="none";
 showPage("home");
}
</script>

</body>
</html>
