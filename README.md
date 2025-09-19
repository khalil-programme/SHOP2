<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Shop</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; background:#f9f9f9;}
    h1 { text-align: center; }
    h2 { margin-top: 25px; }
    .menu-item { display: flex; justify-content: space-between; margin: 6px 0; padding: 6px; background: #fff; border-radius: 6px; }
    button { background: green; color: white; border: none; padding: 5px 10px; cursor: pointer; border-radius: 5px; }
    button:hover { background: darkgreen; }
    #orderModal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
                  background: rgba(0,0,0,0.6); justify-content: center; align-items: center; }
    #orderContent { background: white; padding: 20px; border-radius: 10px; width: 400px; max-height: 80%; overflow-y: auto; }
    #orderList { margin-top: 10px; }
    .close { background: red; }
    @media (max-width: 500px) {
      #orderContent { width: 98%; padding: 8px; }
      .menu-item { flex-direction: column; align-items: flex-start; }
    }
  </style>
</head>
<body>

<h1>🛍️ Welcome to the Shop</h1>

<!-- View Order Button -->
<div style="text-align:center; margin: 20px;">
  <button onclick="openOrder()">👀 VIEW ORDER</button>
</div>

<!-- Items -->
<div id="menu">

  <h2>📦 Products</h2>
  <div class="menu-item">🚤 Bateau en papier – 150$ <button onclick="addToOrder('Bateau en papier',150)">ORDER NOW</button></div>
  <div class="menu-item">📖 Marque-page – 100$ <button onclick="addToOrder('Marque-page',100)">ORDER NOW</button></div>
  <div class="menu-item">🧪 Slime (5$/g) <button onclick="addSlime()">ORDER NOW</button></div>

</div>

<!-- Modal Order -->
<div id="orderModal">
  <div id="orderContent">
    <h2>🛒 Your Order</h2>
    <div id="orderList"></div>
    <h3 id="totalPrice"></h3>
    <button onclick="sendOrder()">📩 SEND ORDER</button>
    <button class="close" onclick="closeOrder()">❌ Close</button>
  </div>
</div>

<script>
  let order = [];

  function addToOrder(item, price) {
    let qty = prompt("Enter quantity for " + item + ":");
    if(qty && !isNaN(qty) && qty > 0){
      order.push({item, price, qty: parseInt(qty)});
      alert(qty + " x " + item + " added to order!");
    }
  }

  function addSlime() {
    let grams = prompt("Enter grams of Slime (5$/g):");
    if(grams && !isNaN(grams)){
      grams = parseInt(grams);
      if(grams < 20){
        alert("❌ TU NE PEUX PAS CHOISIR MOINS QUE 20 g!!");
        return;
      }
      let pricePerGram = 5;
      order.push({item: "Slime (" + grams + "g)", price: pricePerGram, qty: grams});
      alert(grams + "g of Slime added to order!");
    }
  }

  function openOrder() {
    document.getElementById("orderModal").style.display = "flex";
    displayOrder();
  }

  function closeOrder() {
    document.getElementById("orderModal").style.display = "none";
  }

  function displayOrder() {
    let orderList = document.getElementById("orderList");
    orderList.innerHTML = "";
    let total = 0;
    order.forEach(o => {
      let div = document.createElement("div");
      let lineTotal = o.price * o.qty;
      div.textContent = o.qty + " x " + o.item + " – " + lineTotal.toFixed(2) + "$";
      orderList.appendChild(div);
      total += lineTotal;
    });
    document.getElementById("totalPrice").textContent = "Total: " + total.toFixed(2) + "$";
  }

  function sendOrder() {
    if(order.length === 0){
      alert("Your order is empty!");
      return;
    }
    let total = order.reduce((sum,o)=>sum+o.price*o.qty,0).toFixed(2);
    let body = "Your order:\n\n";
    order.forEach(o=> body += o.qty + " x " + o.item + " – " + (o.price*o.qty).toFixed(2) + "$\n");
    body += "\nTotal: " + total + "$";

    // Send order via mailto (2 recipients)
    let url = "mailto:KHALILMOURAD987@GMAIL.COM,awadchristy849@gmail.com"
            + "?subject=" + encodeURIComponent("New Order")
            + "&body=" + encodeURIComponent(body);

    window.location.href = url;
  }
</script>

</body>
</html>
