<!DOCTYPE html>
<html>
<head>
<title>My Shop</title>

<style>

body{
font-family:Arial;
background:#f5f5f5;
text-align:center;
}

.product{
border:1px solid #ccc;
background:white;
padding:15px;
margin:15px;
display:inline-block;
}

button{
padding:10px;
background:black;
color:white;
border:none;
cursor:pointer;
}

#settings{
position:fixed;
top:10px;
right:10px;
}

#adminPanel{
display:none;
background:white;
padding:20px;
margin-top:20px;
border:1px solid #ccc;
}

</style>
</head>

<body>

<div id="settings">
<button onclick="openSettings()">⚙ Settings</button>
</div>

<h1>Welcome to My Shop</h1>

<div id="products"></div>

<h2>Cart</h2>
<div id="cart"></div>

<div id="adminPanel">

<h2>Admin Panel</h2>

<input id="productName" placeholder="Product name">
<br><br>

<input id="productImage" placeholder="Image URL">
<br><br>

<button onclick="addProduct()">Add Product</button>

<h3>Orders</h3>
<div id="orders"></div>

</div>

<script>

let products=[]
let cart=[]
let orders=[]

function addProduct(){

let name=document.getElementById("productName").value
let image=document.getElementById("productImage").value

let product={name,image}

products.push(product)

displayProducts()

}

function displayProducts(){

let html=""

products.forEach((p,i)=>{

html+=`
<div class="product">
<h3>${p.name}</h3>
<img src="${p.image}" width="150"><br><br>
<button onclick="addToCart(${i})">Add to Cart</button>
<button onclick="buyNow(${i})">Buy</button>
</div>
`

})

document.getElementById("products").innerHTML=html

}

function addToCart(i){

cart.push(products[i])
displayCart()

}

function displayCart(){

let html=""

cart.forEach(c=>{
html+=`<p>${c.name}</p>`
})

document.getElementById("cart").innerHTML=html

}

function buyNow(i){

let name=prompt("Enter your name")
let phone=prompt("Enter your mobile number")

let order={
product:products[i].name,
name:name,
phone:phone
}

orders.push(order)

alert("Dear customer, the items you purchased will soon be delivered. Our customer care will call you.")

displayOrders()

}

function displayOrders(){

let html=""

orders.forEach(o=>{
html+=`<p>${o.product} - ${o.name} - ${o.phone}</p>`
})

document.getElementById("orders").innerHTML=html

}

function openSettings(){

let pass=prompt("Enter admin password")

if(pass=="SavitaRana@"){
document.getElementById("adminPanel").style.display="block"
}

else{
alert("Wrong password")
}

}

</script>

</body>
</html>
