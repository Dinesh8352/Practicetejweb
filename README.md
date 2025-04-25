# Practicetejweb
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flipkart Clone</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        .product {
            display: inline-block;
            width: 200px;
            margin: 20px;
            text-align: center;
            border: 1px solid #ccc;
            padding: 10px;
            border-radius: 8px;
        }
        .product img {
            width: 100%;
            height: auto;
            border-radius: 8px;
        }
        .button {
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
            border-radius: 5px;
            margin-top: 10px;
        }
        .button:hover {
            background-color: #45a049;
        }
        .cart {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 10px;
            background-color: #ffcc00;
            border-radius: 5px;
            cursor: pointer;
        }
        .search-bar {
            margin-bottom: 20px;
        }
        .payment-section {
            display: none;
            margin-top: 20px;
            padding: 20px;
            background-color: #f4f4f4;
            border-radius: 8px;
        }
        #login-section, #register-section {
            display: none;
        }
    </style>
</head>
<body>

    <!-- Header Section -->
    <h1>Flipkart Clone</h1>

    <!-- User Login/Register Section -->
    <div id="login-section">
        <h2>Login</h2>
        <input type="text" id="username" placeholder="Enter username">
        <input type="password" id="password" placeholder="Enter password">
        <button class="button" onclick="login()">Login</button>
        <p>Don't have an account? <a href="javascript:void(0)" onclick="showRegister()">Register</a></p>
    </div>
    
    <div id="register-section">
        <h2>Register</h2>
        <input type="text" id="register-username" placeholder="Enter username">
        <input type="password" id="register-password" placeholder="Enter password">
        <button class="button" onclick="register()">Register</button>
        <p>Already have an account? <a href="javascript:void(0)" onclick="showLogin()">Login</a></p>
    </div>

    <!-- Search Bar -->
    <div class="search-bar">
        <input type="text" id="search-input" placeholder="Search products" onkeyup="searchProducts()">
    </div>

    <!-- Product Listing -->
    <div id="products">
        <!-- Products will be dynamically injected here -->
    </div>

    <!-- Cart Section -->
    <div class="cart" id="cart">
        Cart: <span id="cart-count">0</span> items
    </div>

    <!-- Cart Details Section -->
    <div id="cart-section">
        <h3>Your Cart</h3>
        <ul id="cart-items"></ul>
        <button id="checkout-btn" class="button" onclick="checkout()">Proceed to Checkout</button>
    </div>

    <!-- Payment Section -->
    <div class="payment-section" id="payment-section">
        <h3>Payment</h3>
        <p>Total: $<span id="total-amount">0</span></p>
        <button id="pay-btn" class="button" onclick="completePayment()">Complete Payment</button>
    </div>

    <script>
        // Sample product data
        const products = [
            { id: 1, name: 'Laptop', price: 999.99, image: 'https://via.placeholder.com/200?text=Laptop' },
            { id: 2, name: 'Smartphone', price: 499.99, image: 'https://via.placeholder.com/200?text=Smartphone' },
            { id: 3, name: 'Headphones', price: 199.99, image: 'https://via.placeholder.com/200?text=Headphones' },
            { id: 4, name: 'TV', price: 799.99, image: 'https://via.placeholder.com/200?text=TV' },
            { id: 5, name: 'Tablet', price: 299.99, image: 'https://via.placeholder.com/200?text=Tablet' }
        ];

        const cart = [];
        let loggedInUser = null;

        // Function to render products
        function renderProducts(filteredProducts = products) {
            const productsContainer = document.getElementById('products');
            productsContainer.innerHTML = ''; // Clear the existing content

            filteredProducts.forEach(product => {
                const productElement = document.createElement('div');
                productElement.classList.add('product');
                
                productElement.innerHTML = `
                    <img src="${product.image}" alt="${product.name}">
                    <h3>${product.name}</h3>
                    <p>$${product.price}</p>
                    <button class="button" onclick="addToCart(${product.id})">Add to Cart</button>
                `;
                productsContainer.appendChild(productElement);
            });
        }

        // Function to add items to the cart
        function addToCart(productId) {
            const product = products.find(p => p.id === productId);
            cart.push(product);
            updateCartCount();
            updateCartItems();
        }

        // Update cart item count
        function updateCartCount() {
            document.getElementById('cart-count').textContent = cart.length;
        }

        // Update cart items
        function updateCartItems() {
            const cartItemsContainer = document.getElementById('cart-items');
            cartItemsContainer.innerHTML = ''; // Clear existing cart items

            cart.forEach(item => {
                const li = document.createElement('li');
                li.textContent = `${item.name} - $${item.price}`;
                cartItemsContainer.appendChild(li);
            });
        }

        // Checkout
        function checkout() {
            const totalAmount = cart.reduce((total, item) => total + item.price, 0);
            document.getElementById('total-amount').textContent = totalAmount.toFixed(2);
            document.getElementById('cart-section').style.display = 'none';
            document.getElementById('payment-section').style.display = 'block';
        }

        // Complete Payment
        function completePayment() {
            alert('Payment Successful!');
            cart.length = 0; // Clear cart
            updateCartCount();
            updateCartItems();
            document.getElementById('payment-section').style.display = 'none';
            document.getElementById('cart-section').style.display = 'block';
        }

        // Search Functionality
        function searchProducts() {
            const query = document.getElementById('search-input').value.toLowerCase();
            const filteredProducts = products.filter(product =>
                product.name.toLowerCase().includes(query)
            );
            renderProducts(filteredProducts);
        }

        // User Registration
        function register() {
            const username = document.getElementById('register-username').value;
            const password = document.getElementById('register-password').value;
            alert('User registered successfully!');
            showLogin();
        }

        // User Login
        function login() {
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            loggedInUser = username;
            alert('Logged in successfully!');
            document.getElementById('login-section').style.display = 'none';
            document.getElementById('register-section').style.display = 'none';
            renderProducts();
        }

        // Show Login form
        function showLogin() {
            document.getElementById('login-section').style.display = 'block';
            document.getElementById('register-section').style.display = 'none';
        }

        // Show Register form
        function showRegister() {
            document.getElementById('login-section').style.display = 'none';
            document.getElementById('register-section').style.display = 'block';
        }

        // Initial render of products
        renderProducts();
    </script>
</body>
</html>

