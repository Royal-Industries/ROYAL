<!DOCTYPE html>
<html>
<head>
  <title>Royal Industries</title>
  <style>
    body {
      font-family: Arial;
      background: #f5f5f5;
      text-align: center;
    }
    .product {
      border: 1px solid #ccc;
      background: white;
      padding: 15px;
      margin: 15px;
      display: inline-block;
    }
    button {
      padding: 8px;
      margin: 4px;
      background: black;
      color: white;
      border: none;
      cursor: pointer;
    }
    #settings {
      position: fixed;
      top: 10px;
      right: 10px;
    }
    #adminPanel {
      display: none;
      background: white;
      padding: 20px;
      border: 1px solid #ccc;
      margin-top: 20px;
    }
    #customContent {
      margin: 20px;
    }
  </style>

  <!-- Firebase SDK -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/12.9.0/firebase-app.js";
    import { getDatabase, ref, push, onValue, remove, get } from "https://www.gstatic.com/firebasejs/12.9.0/firebase-database.js";

    const firebaseConfig = {
      apiKey: "AIzaSyABGhjN8KT3w3Q0rHtDZKXb_f4KACHnZfg",
      authDomain: "royal-industries-e9c06.firebaseapp.com",
      projectId: "royal-industries-e9c06",
      storageBucket: "royal-industries-e9c06.firebasestorage.app",
      messagingSenderId: "65993138859",
      appId: "1:65993138859:web:52be42d4759bb762e44dee",
      measurementId: "G-42GGMJCEXD"
    };

    const app = initializeApp(firebaseConfig);
    const database = getDatabase(app);

    // Load products from Firebase
    function loadProducts() {
      const productsRef = ref(database, 'products');
      onValue(productsRef, (snapshot) => {
        const productsDiv = document.getElementById("products");
        productsDiv.innerHTML = "";
        snapshot.forEach((child) => {
          const p = child.val();
          const key = child.key;

          setTimeout(() => { // simulate delay
            productsDiv.innerHTML += `
              <div class="product" id="prod-${key}">
                <h3>${p.name}</h3>
                <img src="${p.image}" width="150"><br>
                <p>Price: ₹${p.price}</p>
                <button onclick="addToCart('${key}')">Add to Cart</button>
                <button onclick="buyNow('${key}')">Buy</button>
                <button onclick="removeProduct('${key}')">Remove</button>
              </div>
            `;
          }, 5000); 
        });
      });
    }

    loadProducts();

    // Load custom content
    function loadContent() {
      const contentRef = ref(database, 'content');
      onValue(contentRef, (snapshot) => {
        const contentDiv = document.getElementById("customContent");
        contentDiv.innerHTML = "";
        snapshot.forEach(child => {
          const c = child.val();
          if(c.type === "text") contentDiv.innerHTML += `<p>${c.value}</p>`;
          if(c.type === "image") contentDiv.innerHTML += `<img src="${c.value}" width="300"><br>`;
          if(c.type === "video") contentDiv.innerHTML += `<video width="300" controls><source src="${c.value}"></video><br>`;
        });
      });
    }

    loadContent();

    // Add product
    window.addProduct = function() {
      const name = document.getElementById("productName").value;
      const price = document.getElementById("productPrice").value;
      const image = document.getElementById("productImage").value;
      push(ref(database, 'products'), { name, price, image });
      document.getElementById("productName").value = "";
      document.getElementById("productPrice").value = "";
      document.getElementById("productImage").value = "";
    }

    // Add custom content
    window.addContent = function(type) {
      const value = document.getElementById("contentValue").value;
      if(!value) return;
      push(ref(database, 'content'), { type, value });
      document.getElementById("contentValue").value = "";
    }

    // Remove product
    window.removeProduct = function(key) {
      remove(ref(database, 'products/' + key));
      document.getElementById("prod-" + key)?.remove();
    }

    // Cart & Buy
    window.addToCart = function(key) { alert("Added to cart!"); }

    window.buyNow = async function(key) {
      const name = prompt("Enter your name");
      const phone = prompt("Enter your mobile number");
      if(!name || !phone) return;

      // Get product name
      const productSnap = await get(ref(database, 'products/' + key));
      const productName = productSnap.val()?.name || "Unknown";

      push(ref(database, 'orders'), { productName, customerName: name, phone: phone });
      alert("Dear customer, the items you purchased will soon be delivered. Our customer care will call you.");
    }

    // Admin panel
    window.openSettings = function() {
      const pass = prompt("Enter admin password");
      if(pass === "SavitaRana@") {
        document.getElementById("adminPanel").style.display = "block";
        loadOrders();
      } else {
        alert("Wrong password!");
      }
    }

    // Load orders
    function loadOrders() {
      const ordersRef = ref(database, 'orders');
      const ordersDiv = document.getElementById("orders");
      onValue(ordersRef, (snapshot) => {
        ordersDiv.innerHTML = "";
        snapshot.forEach(child => {
          const o = child.val();
          const key = child.key;
          ordersDiv.innerHTML += `<p id="order-${key}">${o.productName} - ${o.customerName} - ${o.phone} <button onclick="removeOrder('${key}')">Delete</button></p>`;
        });
      });
    }

    // Remove order
    window.removeOrder = function(key) {
      remove(ref(database, 'orders/' + key));
      document.getElementById("order-" + key)?.remove();
    }
  </script>
</head>
<body>

  <div id="settings">
    <button onclick="openSettings()">⚙ Settings</button>
  </div>

  <h1>Royal Industries</h1>

  <div id="customContent"></div>

  <div id="products"></div>

  <h2>Cart</h2>
  <div id="cart"></div>

  <div id="adminPanel">
    <h2>Admin Panel</h2>

    <h3>Add Product</h3>
    <input id="productName" placeholder="Product Name"><br><br>
    <input id="productPrice" placeholder="Price"><br><br>
    <input id="productImage" placeholder="Image URL"><br><br>
    <button onclick="addProduct()">Add Product</button>

    <h3>Add Content</h3>
    <input id="contentValue" placeholder="Text/Image/Video URL"><br><br>
    <button onclick="addContent('text')">Add Text</button>
    <button onclick="addContent('image')">Add Image</button>
    <button onclick="addContent('video')">Add Video</button>

    <h3>Orders</h3>
    <div id="orders"></div>
  </div>

</body>
</html>
