<!DOCTYPE html><html lang="ta">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Ajay Fruit Shop - Cart Ready</title>
  <style>
    :root{--accent:#ff8c42;--bg:#fffef0;--card:#ffffff}
    body{font-family:system-ui,-apple-system,Segoe UI,Roboto,'Noto Sans',sans-serif;background:var(--bg);margin:0;color:#333}
    header{background:var(--accent);padding:18px 16px;text-align:center;color:#4d2600}
    header h1{margin:0;font-size:1.4rem}
    .container{max-width:980px;margin:16px auto;padding:0 12px}
    .banner{width:100%;height:220px;object-fit:cover;border-radius:8px}
    .grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:14px;margin-top:14px}
    .card{background:var(--card);border-radius:10px;padding:12px;box-shadow:0 2px 6px rgba(0,0,0,0.08);text-align:center}
    .card img{width:100px;height:100px;object-fit:cover;border-radius:8px}
    .price{font-weight:700;margin-top:6px}
    .small{font-size:0.9rem;color:#666}
    button{background:var(--accent);border:none;color:#4d2600;padding:8px 10px;border-radius:8px;cursor:pointer;font-weight:700}
    .cart-btn{position:fixed;right:16px;bottom:18px;background:#28a745;color:#fff;padding:12px;border-radius:999px;box-shadow:0 6px 18px rgba(0,0,0,0.15);border:none}
    /* Cart panel */
    .cart-panel{position:fixed;right:16px;top:80px;width:340px;max-height:70vh;overflow:auto;background:#fff;border-radius:12px;padding:12px;box-shadow:0 8px 30px rgba(0,0,0,0.18);display:none}
    .cart-item{display:flex;gap:8px;align-items:center;padding:8px 6px;border-bottom:1px dashed #eee}
    .cart-item img{width:56px;height:56px;border-radius:6px}
    .qty{display:flex;align-items:center;gap:6px}
    .qty button{background:#eee;color:#111;padding:6px 8px;border-radius:6px}
    .total{font-weight:800;margin-top:8px}
    .checkout{display:flex;gap:8px;margin-top:10px}
    .checkout a{flex:1;text-align:center;text-decoration:none;padding:10px;border-radius:8px;font-weight:800}
    .upi{background:#f1f1f1;color:#111}
    .whatsapp{background:#25D366;color:#fff}
    .pickup{margin-top:10px}
    footer{margin-top:60px;padding:18px;text-align:center;color:#666}
    /* responsive */
    @media(max-width:520px){.cart-panel{width:92%;right:4%;top:auto;bottom:80px}}
  </style>
</head>
<body>
  <header>
    <h1>Ajay Fruit Shop 🍌🍎 - Mohanur (637015)</h1>
    <div class="small">GPay: <strong>9944889673</strong> — WhatsApp: <a href="https://wa.me/919944889673" style="color:inherit;text-decoration:underline">9944889673</a></div>
  </header>  <div class="container">
    <img class="banner" src="your-shop-photo.jpg" alt="Ajay Fruit Shop Banner" /><div style="display:flex;justify-content:space-between;align-items:center;margin-top:12px;gap:12px;flex-wrap:wrap">
  <div>
    <strong>Pickup Points:</strong>
    <div class="small">Mohanur Main Road • Ravi Super Mart Gate • Bus Stand</div>
  </div>
  <div>
    <label for="pickup" class="small">Pickup:</label>
    <select id="pickup" class="small">
      <option>Mohanur Main Road</option>
      <option>Ravi Super Mart Gate</option>
      <option>Bus Stand</option>
    </select>
  </div>
</div>

<h3 style="margin-top:18px">Products</h3>
<div class="grid" id="productGrid">
  <!-- Products injected by JS -->
</div>

<button class="cart-btn" id="openCart">Cart (<span id="cartCount">0</span>)</button>

<div class="cart-panel" id="cartPanel">
  <h3>Cart</h3>
  <div id="cartItems"></div>
  <div class="total">Total: ₹<span id="cartTotal">0</span></div>

  <div class="pickup">
    <label class="small">Pickup Point (confirm)</label>
    <select id="pickupConfirm" class="small" style="width:100%;margin-top:6px">
      <option>Mohanur Main Road</option>
      <option>Ravi Super Mart Gate</option>
      <option>Bus Stand</option>
    </select>
  </div>

  <div class="checkout">
    <a id="upiPay" class="upi" href="#">Pay via GPay</a>
    <a id="waOrder" class="whatsapp" href="#">Order on WhatsApp</a>
  </div>
  <div style="margin-top:8px;font-size:0.9rem;color:#666">Tip: Change quantity in cart. Remove item with the trash icon.</div>
</div>

<footer>
  © Ajay Fruit Shop • Mohanur – 637015
</footer>

  </div>  <script>
    // Product data (add more items here)
    const products = [
      { id: 1, name: 'Banana', price: 40, unit: 'kg', img: 'banana.jpg' },
      { id: 2, name: 'Apple', price: 120, unit: 'kg', img: 'apple.jpg' },
      { id: 3, name: 'Orange', price: 60, unit: 'kg', img: 'orange.jpg' },
      { id: 4, name: 'Mango', price: 80, unit: 'kg', img: 'mango.jpg' },
      { id: 5, name: 'Pomegranate', price: 150, unit: 'kg', img: 'pomegranate.jpg' }
    ];

    // Cart state
    let cart = [];

    // DOM refs
    const productGrid = document.getElementById('productGrid');
    const cartPanel = document.getElementById('cartPanel');
    const cartItemsDiv = document.getElementById('cartItems');
    const cartCount = document.getElementById('cartCount');
    const cartTotal = document.getElementById('cartTotal');
    const openCartBtn = document.getElementById('openCart');
    const upiPay = document.getElementById('upiPay');
    const waOrder = document.getElementById('waOrder');
    const pickupSelect = document.getElementById('pickup');
    const pickupConfirm = document.getElementById('pickupConfirm');

    // Render products
    function renderProducts(){
      productGrid.innerHTML = '';
      products.forEach(p => {
        const div = document.createElement('div');
        div.className = 'card';
        div.innerHTML = `
          <img src="${p.img}" alt="${p.name}" />
          <h4>${p.name}</h4>
          <div class="small">₹${p.price} / ${p.unit}</div>
          <div style="margin-top:8px">
            <input type="number" id="qty-${p.id}" value="1" min="1" style="width:60px;padding:6px;border-radius:6px;border:1px solid #ddd" />
          </div>
          <div style="margin-top:8px">
            <button onclick="addToCart(${p.id})">Add to Cart</button>
          </div>
        `;
        productGrid.appendChild(div);
      });
    }

    // Add to cart
    function addToCart(productId){
      const prod = products.find(x=>x.id===productId);
      const qty = parseInt(document.getElementById('qty-'+productId).value) || 1;
      const existing = cart.find(i=>i.id===productId);
      if(existing){ existing.qty += qty; }
      else{ cart.push({ id: prod.id, name: prod.name, price: prod.price, qty }); }
      saveCart();
      renderCart();
      alert(prod.name + ' added to cart (x' + qty + ')');
    }

    // Save to localStorage
    function saveCart(){ localStorage.setItem('ajay_cart', JSON.stringify(cart)); }
    function loadCart(){ const data = localStorage.getItem('ajay_cart'); cart = data ? JSON.parse(data) : []; }

    // Render cart
    function renderCart(){
      cartItemsDiv.innerHTML = '';
      let total = 0;
      cart.forEach(item=>{
        total += item.price * item.qty;
        const row = document.createElement('div');
        row.className = 'cart-item';
        row.innerHTML = `
          <img src="${(products.find(p=>p.id===item.id)||{}).img}" alt="${item.name}" />
          <div style="flex:1;text-align:left">
            <div style="font-weight:700">${item.name}</div>
            <div class="small">₹${item.price} x ${item.qty} = ₹${item.price * item.qty}</div>
            <div class="qty" style="margin-top:6px">
              <button onclick="changeQty(${item.id}, -1)">-</button>
              <div style="min-width:26px;text-align:center">${item.qty}</div>
              <button onclick="changeQty(${item.id}, 1)">+</button>
              <button style="margin-left:8px;background:transparent;border:none;color:#d00;" onclick="removeItem(${item.id})">🗑️</button>
            </div>
          </div>
        `;
        cartItemsDiv.appendChild(row);
      });
      cartCount.textContent = cart.reduce((s,i)=>s+i.qty,0);
      cartTotal.textContent = total;

      // Update pay & whatsapp links
      const upiLink = buildUpiLink(total);
      upiPay.href = upiLink;
      upiPay.setAttribute('target','_blank');

      waOrder.href = buildWaLink();
      waOrder.setAttribute('target','_blank');

      // show/hide cart panel
      cartPanel.style.display = cart.length ? 'block' : 'none';
    }

    function changeQty(id, delta){
      const it = cart.find(i=>i.id===id);
      if(!it) return;
      it.qty += delta;
      if(it.qty < 1) it.qty = 1;
      saveCart(); renderCart();
    }
    function removeItem(id){ cart = cart.filter(i=>i.id!==id); saveCart(); renderCart(); }

    // UPI (GPay) link builder
    function buildUpiLink(amount){
      // Using generic upi scheme (some Android devices handle this). For web fallback, opening UPI may not work on all devices.
      const pa = '9944889673@upi'; // merchant VPA or phone-based UPI id
      const pn = encodeURIComponent('Ajay Fruit Shop');
      const tn = encodeURIComponent('Order from Ajay Fruit Shop');
      const am = amount ? amount.toString() : '0';
      return `upi://pay?pa=${pa}&pn=${pn}&tn=${tn}&am=${am}&cu=INR`;
    }

    // WhatsApp message builder
    function buildWaLink(){
      const phone = '919944889673'; // country code + number
      const pickup = pickupConfirm.value || pickupSelect.value || '';
      if(cart.length===0) return '#';
      let msg = `Hello Ajay, I want to place an order:
`;
      cart.forEach(i=>{ msg += `${i.name} x ${i.qty} = ₹${i.price * i.qty}
`; });
      msg += `Total: ₹${cart.reduce((s,i)=>s+i.price*i.qty,0)}
Pickup: ${pickup}`;
      return 'https://wa.me/' + phone + '?text=' + encodeURIComponent(msg);
    }

    // Open cart toggle
    openCartBtn.addEventListener('click', ()=>{
      cartPanel.style.display = cartPanel.style.display === 'block' ? 'none' : (cart.length ? 'block' : 'none');
    });

    // Load & initial render
    loadCart(); renderProducts(); renderCart();

    // Expose some functions to global so inline onclicks work
    window.addToCart = addToCart;
    window.changeQty = changeQty;
    window.removeItem = removeItem;
  </script></body>
</html>
