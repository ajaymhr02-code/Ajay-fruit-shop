<!DOCTYPE html>
<html lang="ta">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ajay Fruit Shop – Mohanur</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 0; padding: 0; background: #fff8e7; }
        header { background: orange; color: white; padding: 15px; text-align: center; font-size: 24px; }
        nav { background: #ffb84d; padding: 10px; text-align: center; }
        nav a { margin: 0 10px; color: black; text-decoration: none; font-weight: bold; }
        .container { padding: 20px; }
        .product { border: 1px solid #ccc; padding: 15px; margin: 10px; display: inline-block; width: 200px; background: white; border-radius: 8px; }
        .product img { width: 100%; height: auto; border-radius: 8px; }
        .product button { background: green; color: white; padding: 10px; border: none; cursor: pointer; width: 100%; border-radius: 5px; }
        .product button:hover { background: darkgreen; }
        footer { background: orange; color: white; text-align: center; padding: 10px; margin-top: 20px; }
    </style>
    <script>
        let cart = [];
        function addToCart(item) {
            cart.push(item);
            alert(item + " added to cart!");
        }
    </script>
</head>
<body>

<header>🍊 Ajay Fruit Shop – Mohanur (Pincode: 637015)</header>

<nav>
    <a href="#products">Products</a>
    <a href="tel:9944889673">Call Us</a>
    <a href="https://pay.google.com/">Pay via GPay</a>
</nav>

<div class="container" id="products">
    <div class="product">
        <img src="https://upload.wikimedia.org/wikipedia/commons/8/8a/Banana-Single.jpg" alt="Banana">
        <h3>Banana</h3>
        <p>₹40 / Dozen</p>
        <button onclick="addToCart('Banana')">Add to Cart</button>
    </div>
    <div class="product">
        <img src="https://upload.wikimedia.org/wikipedia/commons/1/15/Red_Apple.jpg" alt="Apple">
        <h3>Apple</h3>
        <p>₹120 / Kg</p>
        <button onclick="addToCart('Apple')">Add to Cart</button>
    </div>
    <div class="product">
        <img src="https://upload.wikimedia.org/wikipedia/commons/c/c4/Orange-Fruit-Pieces.jpg" alt="Orange">
        <h3>Orange</h3>
        <p>₹60 / Kg</p>
        <button onclick="addToCart('Orange')">Add to Cart</button>
    </div>
</div>

<footer>
    📍 Mohanur Main Road, Near Bus Stand | 📞 9944889673
</footer>

</body>
</html>
