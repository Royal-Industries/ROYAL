<!DOCTYPE html>
<html>
<head>
<title>Royal Industries</title>

<style>

body{
font-family:Arial;
background:#f5f5f5;
text-align:center;
margin:0;
}

h1{
background:black;
color:white;
padding:15px;
}

#settings{
position:fixed;
top:10px;
right:10px;
}

button{
padding:10px;
background:black;
color:white;
border:none;
cursor:pointer;
margin:5px;
}

.product{
border:1px solid #ccc;
background:white;
padding:15px;
margin:15px;
display:inline-block;
width:200px;
}

#adminPanel{
display:none;
background:white;
padding:20px;
border-top:2px solid black;
margin-top:30px;
}

img{
max-width:180px;
}

</style>

</head>

<body>

<div id="settings">
<button onclick="openSettings()">⚙ Settings</button>
</div>

<h1>ROYAL INDUSTRIES</h1>

<div id="content"></div>

<h2>Products</h2>
<div id="products"></div>

<h2>Cart</h2>
<div id="cart"></div>

<div id="adminPanel">

<h2>Admin Panel</h2>

<h3>Add Product</h3>

<input id="productName" placeholder="Product name">
<br><br>

<input id="productPrice" placeholder="Price">
<br><br>

<input id="productImage" placeholder="Image URL">
<br><br>

<button onclick="addProduct()">Add Product</button>

<hr>

<h3>Add Text Anywhere</h3>

<textarea id="customText" placeholder="Write text"></textarea>
<br><br>

<button onclick="addText()">Add Text</button>

<hr>

<h3>Add Image</h3>

<input id="customImage" placeholder="Image URL">
<br><br>

<button onclick="addImage()">Add Image</button>

<hr>

<h3>Add Video</h3>

<input id="videoURL" placeholder="Video embed link">
<br><br>

<button onclick="addVideo()">Add Video</button>

<hr>

<h3>Orders</h3>
<div id="orders"></div>

</div>

<script>

let products=[]
let cart=[]
let orders=[]

function addProduct(){

let name=document.getElementById("productName").value
let price=document.getElementById("productPrice").value
let image=document.getElementById("productImage").value

let product={name,price,image}

products.push(product)

displayProducts()

}

function displayProducts(){

let html=""

products.forEach((p,i)=>{

html+=`
<div class="product">

<h3>${p.name}</h3>

<p><b>Price: ₹${p.price}</b></p>

<img src="${p.image}">

<br><br>

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

html+=`<p>${c.name} - ₹${c.price}</p>`

})

document.getElementById("cart").innerHTML=html

}

function buyNow(i){

let name=prompt("Enter your name")

let phone=prompt("Enter your mobile number")

let order={
product:products[i].name,
price:products[i].price,
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

html+=`<p>${o.product} ₹${o.price} | ${o.name} | ${o.phone}</p>`

})

document.getElementById("orders").innerHTML=html

}

function addText(){

let text=document.getElementById("customText").value

document.getElementById("content").innerHTML+=`<p>${text}</p>`

}

function addImage(){

let img=document.getElementById("customImage").value

document.getElementById("content").innerHTML+=`<img src="${img}">`

}

function addVideo(){

let video=document.getElementById("videoURL").value

document.getElementById("content").innerHTML+=`${video}`

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
