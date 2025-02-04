# Resellaz25vendors
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tienda Online</title>
    <style>
        body {
            background-color: black;
            color: white;
            font-family: Arial, sans-serif;
            text-align: center;
        }
        .container {
            max-width: 1200px;
            margin: auto;
            padding: 20px;
        }
        .product {
            border: 1px solid white;
            padding: 20px;
            margin: 20px;
            display: inline-block;
            width: 200px;
        }
        .product img {
            width: 100%;
            height: auto;
        }
        button {
            background-color: white;
            color: black;
            padding: 10px;
            border: none;
            cursor: pointer;
        }
        .cart {
            margin-top: 30px;
            padding: 20px;
            border: 1px solid white;
            display: inline-block;
        }
    </style>
    <script src="https://www.paypal.com/sdk/js?client-id=YOUR_PAYPAL_CLIENT_ID&currency=USD"></script>
    <script>
        let cart = [];
        function addToCart(product, price) {
            cart.push({ product, price });
            updateCart();
        }
        function updateCart() {
            let cartContainer = document.getElementById("cart");
            cartContainer.innerHTML = "<h2>Carrito de Compras</h2>" + cart.map(item => `<p>${item.product} - $${item.price}</p>`).join("") + 
            '<button onclick="clearCart()">Vaciar Carrito</button>' +
            '<div id="paypal-button-container"></div>';
            renderPaypalButton();
        }
        function clearCart() {
            cart = [];
            updateCart();
        }
        function renderPaypalButton() {
            paypal.Buttons({
                createOrder: function(data, actions) {
                    return actions.order.create({
                        purchase_units: [{
                            amount: {
                                value: cart.reduce((total, item) => total + item.price, 0).toFixed(2)
                            }
                        }]
                    });
                },
                onApprove: function(data, actions) {
                    return actions.order.capture().then(function(details) {
                        alert('Pago completado por ' + details.payer.name.given_name);
                        clearCart();
                    });
                }
            }).render('#paypal-button-container');
        }
    </script>
</head>
<body>
    <h1>Bienvenido a Nuestra Tienda</h1>
    <div class="container">
        <div class="product">
            <img src="producto1.jpg" alt="Producto 1">
            <h2>Producto 1</h2>
            <p>$10.00</p>
            <button onclick="addToCart('Producto 1', 10)">Comprar</button>
        </div>
        <div class="product">
            <img src="producto2.jpg" alt="Producto 2">
            <h2>Producto 2</h2>
            <p>$15.00</p>
            <button onclick="addToCart('Producto 2', 15)">Comprar</button>
        </div>
        <div class="product">
            <img src="producto3.jpg" alt="Producto 3">
            <h2>Producto 3</h2>
            <p>$20.00</p>
            <button onclick="addToCart('Producto 3', 20)">Comprar</button>
        </div>
    </div>
    <div class="cart" id="cart">
        <h2>Carrito de Compras</h2>
    </div>
</body>
</html>
