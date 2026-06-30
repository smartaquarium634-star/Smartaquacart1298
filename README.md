<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Aqua Cart | Premium Aquarium Store</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --bg-color: #04111d;
            --surface: rgba(16, 42, 67, 0.6);
            --primary: #00d2ff;
            --secondary: #00f260;
            --text-main: #ffffff;
            --text-muted: #a0aec0;
            --glass-border: rgba(255, 255, 255, 0.1);
            --danger: #ff4757;
            --font: 'Poppins', sans-serif;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: var(--font); scroll-behavior: smooth; }
        
        body { background: var(--bg-color); color: var(--text-main); overflow-x: hidden; }
        body::before { content: ''; position: fixed; top: -50%; left: -50%; width: 200%; height: 200%; background: radial-gradient(circle, rgba(0,210,255,0.05) 0%, rgba(4,17,29,1) 60%); z-index: -1; }

        /* Glassmorphism Utilities */
        .glass { background: var(--surface); backdrop-filter: blur(12px); -webkit-backdrop-filter: blur(12px); border: 1px solid var(--glass-border); box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.3); }
        .btn { padding: 10px 20px; border: none; border-radius: 8px; cursor: pointer; font-weight: 600; transition: all 0.3s ease; display: inline-flex; align-items: center; justify-content: center; gap: 8px; }
        .btn-primary { background: linear-gradient(45deg, var(--primary), var(--secondary)); color: #000; box-shadow: 0 4px 15px rgba(0, 210, 255, 0.3); }
        .btn-primary:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(0, 210, 255, 0.5); }
        .btn-outline { background: transparent; border: 1px solid var(--primary); color: var(--primary); }
        .btn-outline:hover { background: rgba(0, 210, 255, 0.1); }
        .btn-danger { background: rgba(255, 71, 87, 0.2); border: 1px solid var(--danger); color: var(--danger); }

        /* Navigation */
        header { position: sticky; top: 0; z-index: 1000; padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; border-radius: 0 0 20px 20px; border-top: none; }
        .logo { font-size: 1.5rem; font-weight: 700; background: linear-gradient(45deg, var(--primary), var(--secondary)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .tagline { font-size: 0.7rem; color: var(--text-muted); display: block; }
        .nav-actions { display: flex; gap: 15px; align-items: center; }
        .icon-btn { background: transparent; border: none; color: var(--text-main); font-size: 1.2rem; cursor: pointer; position: relative; }
        .cart-badge { position: absolute; top: -8px; right: -8px; background: var(--danger); color: white; font-size: 0.7rem; width: 18px; height: 18px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: bold; }

        /* Hero Section */
        .hero { padding: 60px 20px; text-align: center; margin: 20px; border-radius: 20px; }
        .hero h1 { font-size: 2.5rem; margin-bottom: 15px; }
        .hero p { color: var(--text-muted); margin-bottom: 25px; }

        /* Filters */
        .category-filters { display: flex; gap: 10px; overflow-x: auto; padding: 10px 20px; margin-bottom: 20px; scrollbar-width: none; }
        .category-filters::-webkit-scrollbar { display: none; }
        .filter-btn { white-space: nowrap; padding: 8px 16px; border-radius: 20px; background: var(--surface); border: 1px solid var(--glass-border); color: var(--text-main); cursor: pointer; transition: 0.3s; }
        .filter-btn.active { background: var(--primary); color: #000; font-weight: 600; border-color: var(--primary); }

        /* Product Grid */
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(160px, 1fr)); gap: 15px; padding: 0 20px 40px; }
        .card { border-radius: 15px; overflow: hidden; display: flex; flex-direction: column; transition: transform 0.3s ease; position: relative; }
        .card:hover { transform: translateY(-5px); }
        .card-img { width: 100%; height: 160px; object-fit: cover; background: #fff; }
        .card-body { padding: 15px; display: flex; flex-direction: column; flex-grow: 1; gap: 10px; }
        .category-tag { font-size: 0.7rem; color: var(--primary); text-transform: uppercase; letter-spacing: 1px; }
        .product-title { font-size: 0.9rem; font-weight: 500; line-height: 1.3; flex-grow: 1; }
        .product-price { font-size: 1.1rem; font-weight: 700; color: var(--secondary); }
        .out-of-stock-badge { position: absolute; top: 10px; left: 10px; background: rgba(255,0,0,0.8); color: white; padding: 5px 10px; border-radius: 5px; font-size: 0.7rem; font-weight: bold; }

        /* Modals & Drawers */
        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.7); backdrop-filter: blur(5px); z-index: 2000; opacity: 0; visibility: hidden; transition: 0.3s; }
        .modal-overlay.active { opacity: 1; visibility: visible; }
        
        /* Cart Drawer */
        .drawer { position: fixed; top: 0; right: -100%; width: 100%; max-width: 400px; height: 100vh; background: var(--bg-color); z-index: 2001; transition: 0.4s cubic-bezier(0.82, 0.085, 0.395, 0.895); display: flex; flex-direction: column; }
        .drawer.active { right: 0; box-shadow: -5px 0 30px rgba(0,0,0,0.5); }
        .drawer-header { padding: 20px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid var(--glass-border); }
        .drawer-content { flex-grow: 1; overflow-y: auto; padding: 20px; }
        .drawer-footer { padding: 20px; border-top: 1px solid var(--glass-border); background: var(--surface); }
        
        .cart-item { display: flex; gap: 15px; margin-bottom: 15px; background: rgba(255,255,255,0.05); padding: 10px; border-radius: 10px; }
        .cart-item img { width: 60px; height: 60px; object-fit: cover; border-radius: 8px; }
        .cart-item-details { flex-grow: 1; }
        .cart-item-title { font-size: 0.85rem; margin-bottom: 5px; }
        .cart-controls { display: flex; align-items: center; gap: 10px; margin-top: 5px; }
        .qty-btn { background: var(--surface); border: 1px solid var(--glass-border); color: white; width: 25px; height: 25px; border-radius: 5px; cursor: pointer; }
        
        /* Forms */
        .form-group { margin-bottom: 15px; }
        .form-group label { display: block; margin-bottom: 5px; font-size: 0.9rem; color: var(--text-muted); }
        .form-control { width: 100%; padding: 12px; background: rgba(0,0,0,0.2); border: 1px solid var(--glass-border); border-radius: 8px; color: white; font-family: inherit; }
        .form-control:focus { outline: none; border-color: var(--primary); }
        
        /* Floating Buttons */
        .fab { position: fixed; bottom: 20px; width: 50px; height: 50px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 1.5rem; color: white; cursor: pointer; z-index: 100; box-shadow: 0 4px 15px rgba(0,0,0,0.3); transition: 0.3s; }
        .fab-wa { right: 20px; background: #25D366; }
        .fab-wa:hover { transform: scale(1.1); }

        /* Admin Dashboard */
        .admin-modal { position: fixed; top: 5%; left: 5%; width: 90%; height: 90%; background: var(--bg-color); z-index: 3000; border-radius: 15px; display: none; flex-direction: column; overflow: hidden; }
        .admin-modal.active { display: flex; }
        .admin-header { padding: 20px; border-bottom: 1px solid var(--glass-border); display: flex; justify-content: space-between; }
        .admin-body { padding: 20px; overflow-y: auto; flex-grow: 1; }
        .admin-product-list { margin-top: 20px; }
        .admin-item { display: flex; justify-content: space-between; align-items: center; padding: 10px; border-bottom: 1px solid var(--glass-border); }

        /* Toast */
        .toast { position: fixed; bottom: 80px; left: 50%; transform: translateX(-50%) translateY(100px); background: var(--primary); color: #000; padding: 10px 20px; border-radius: 30px; font-weight: 600; z-index: 9999; opacity: 0; transition: 0.3s; }
        .toast.show { transform: translateX(-50%) translateY(0); opacity: 1; }

        @media(min-width: 768px) {
            .grid { grid-template-columns: repeat(auto-fill, minmax(220px, 1fr)); }
            .card-img { height: 200px; }
        }
    </style>
</head>
<body>

    <header class="glass">
        <div>
            <div class="logo">SMART AQUA CART</div>
            <span class="tagline">Premium Quality Fishes & Accessories</span>
        </div>
        <div class="nav-actions">
            <button class="icon-btn" onclick="openCart()">
                <i class="fas fa-shopping-bag"></i>
                <span class="cart-badge" id="cart-count">0</span>
            </button>
            <button class="icon-btn" onclick="adminLoginPrompt()">
                <i class="fas fa-cog"></i>
            </button>
        </div>
    </header>

    <section class="hero glass">
        <h1>Dive Into Premium Aquatics</h1>
        <p>Your one-stop shop for high-quality fishes, foods, and filters in Karad.</p>
        <button class="btn btn-primary" onclick="document.getElementById('products-section').scrollIntoView()">Shop Now</button>
    </section>

    <section id="products-section">
        <div class="category-filters" id="category-container"></div>
        <div class="grid" id="product-grid"></div>
    </section>

    <footer style="text-align: center; padding: 30px 20px; color: var(--text-muted); border-top: 1px solid var(--glass-border); margin-top: 20px;">
        <h3 style="color: var(--primary); margin-bottom: 10px;">Smart Aqua Cart</h3>
        <p><i class="fas fa-map-marker-alt"></i> Karad, Dist- Satara, Maharashtra</p>
        <p><i class="fas fa-phone"></i> +91 7350039389</p>
        <p><i class="fas fa-envelope"></i> smartaquariumkarad634@gmail.com</p>
        <p style="margin-top: 20px; font-size: 0.8rem;">© 2026 Smart Aqua Cart. All rights reserved.</p>
    </footer>

    <a href="https://wa.me/917350039389" class="fab fab-wa" target="_blank">
        <i class="fab fa-whatsapp"></i>
    </a>

    <div class="modal-overlay" id="overlay" onclick="closeAllModals()"></div>

    <div class="drawer glass" id="cart-drawer">
        <div class="drawer-header">
            <h2>Your Cart</h2>
            <button class="icon-btn" onclick="closeAllModals()"><i class="fas fa-times"></i></button>
        </div>
        <div class="drawer-content" id="cart-items"></div>
        <div class="drawer-footer">
            <div style="display: flex; justify-content: space-between; margin-bottom: 15px; font-size: 1.2rem; font-weight: bold;">
                <span>Total:</span>
                <span id="cart-total">₹0</span>
            </div>
            <button class="btn btn-primary" style="width: 100%;" onclick="openCheckout()">Proceed to Checkout</button>
        </div>
    </div>

    <div class="drawer glass" id="checkout-modal" style="right: auto; left: 50%; top: 50%; transform: translate(-50%, -50%) scale(0.9); height: auto; max-height: 90vh; border-radius: 20px; opacity: 0; visibility: hidden; transition: 0.3s; z-index: 2002; width: 90%; max-width: 400px;">
        <div class="drawer-header">
            <h2>Complete Order</h2>
            <button class="icon-btn" onclick="closeAllModals()"><i class="fas fa-times"></i></button>
        </div>
        <div class="drawer-content">
            <form id="checkout-form" action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
                <div class="form-group">
                    <label>Full Name *</label>
                    <input type="text" name="Name" class="form-control" required>
                </div>
                <div class="form-group">
                    <label>Phone Number *</label>
                    <input type="tel" name="Phone" class="form-control" required>
                </div>
                <div class="form-group">
                    <label>Delivery Address *</label>
                    <textarea name="Address" class="form-control" rows="3" required></textarea>
                </div>
                <div class="form-group">
                    <label>Special Instructions</label>
                    <textarea name="Notes" class="form-control" rows="2"></textarea>
                </div>
                
                <input type="hidden" name="Order_Summary" id="form-order-summary">
                <input type="hidden" name="Grand_Total" id="form-grand-total">
                <input type="hidden" name="_subject" value="New Order from Smart Aqua Cart!">
                
                <button type="submit" class="btn btn-primary" style="width: 100%; margin-top: 10px;">Place Order</button>
            </form>
        </div>
    </div>

    <div class="admin-modal glass" id="admin-dashboard">
        <div class="admin-header">
            <h2>⚙ Store Admin Panel</h2>
            <div>
                <button class="btn btn-outline" onclick="exportData()"><i class="fas fa-download"></i> Backup</button>
                <button class="btn btn-danger" style="margin-left:10px" onclick="closeAllModals()"><i class="fas fa-times"></i> Close</button>
            </div>
        </div>
        <div class="admin-body">
            <div class="glass" style="padding: 20px; margin-bottom: 20px; border-radius: 10px;">
                <h3>Add / Edit Product</h3>
                <form id="admin-form" onsubmit="saveProduct(event)">
                    <input type="hidden" id="prod-id">
                    <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-top: 15px;">
                        <input type="text" id="prod-name" class="form-control" placeholder="Product Name" required>
                        <input type="number" id="prod-price" class="form-control" placeholder="Price (₹)" required>
                        <input type="text" id="prod-cat" class="form-control" placeholder="Category" required>
                        <input type="url" id="prod-img" class="form-control" placeholder="Image URL" required>
                        <label style="display: flex; align-items: center; gap: 10px;">
                            <input type="checkbox" id="prod-stock" checked> In Stock
                        </label>
                    </div>
                    <button type="submit" class="btn btn-primary" style="margin-top: 15px;">Save Product</button>
                    <button type="button" class="btn btn-outline" style="margin-top: 15px;" onclick="resetAdminForm()">Clear</button>
                </form>
            </div>
            
            <h3>Manage Products</h3>
            <div class="admin-product-list" id="admin-product-list"></div>
        </div>
    </div>

    <div class="toast" id="toast">Message</div>

    <script>
        // --- DATA INITIALIZATION ---
        const defaultProducts = [
            { id: 1, name: 'Bluepet Aquarium Powerfilter 6000F', price: 320, category: 'Filters', image: 'https://i.ibb.co/4ZxR9JpS/1782745859148.webp', inStock: true },
            { id: 2, name: 'Bluepet Aquarium Powerfilter 1000F', price: 350, category: 'Filters', image: 'https://i.ibb.co/Z6D6DWRM/product-jpeg.jpg', inStock: true },
            { id: 3, name: 'Premium sponge filter XF-380', price: 250, category: 'Filters', image: 'https://i.ibb.co/Jjdm4N7b/g-Iz-Ozi-HU.webp', inStock: true },
            { id: 4, name: 'Premium Sponge Filter XF-280', price: 200, category: 'Filters', image: 'https://i.ibb.co/dJHDzpLN/71p4-Sso-Vvj-L.jpg', inStock: true },
            { id: 5, name: 'Aquarium Airpump For fishes', price: 200, category: 'Air Pumps', image: 'https://i.ibb.co/rRgyjSwj/portable-aquarium-air-pump.jpg', inStock: true },
            { id: 6, name: '6 in 1 Biological Sponge', price: 280, category: 'Filters', image: 'https://i.ibb.co/tPM0vQdL/1683465900149-SKU-0046-0.webp', inStock: true },
            { id: 7, name: 'Best Quality Siphon Pipe', price: 180, category: 'Accessories', image: 'https://i.ibb.co/DHPpFDLN/51j0-X8-B5-KQL.jpg', inStock: true },
            { id: 8, name: 'Premium Colour Enhancing Royal arowana Food (100gm)', price: 250, category: 'Fish Food', image: 'https://i.ibb.co/HTmSKwCB/71c-OFRBmi-PL.jpg', inStock: true },
            { id: 9, name: 'Taiyo Turtle Food 250gm', price: 250, category: 'Turtle Food', image: 'https://i.ibb.co/ccnMLzTW/51yv-UMAh-P6-L-AC-UF1000-1000-QL80.jpg', inStock: true },
            { id: 10, name: 'Tokyo Gold turtle food 500gm', price: 350, category: 'Turtle Food', image: 'https://i.ibb.co/5Xc9t7DZ/7183b1-K6ah-L.jpg', inStock: true },
            { id: 11, name: 'Taiyo Pink Fish Food 1kg', price: 550, category: 'Fish Food', image: 'https://i.ibb.co/gbCY2zb9/61-Sf-T-xt-Uo-L.jpg', inStock: true },
            { id: 12, name: 'Taiyo Pink Fish Food 500gm', price: 350, category: 'Fish Food', image: 'https://i.ibb.co/ymJx94d5/Taiyo-Pink.png', inStock: true },
            { id: 13, name: 'Opalfin Premium Quality Ultrafine Fish food (100gm)', price: 120, category: 'Fish Food', image: 'https://i.ibb.co/M53dPDy9/IMG-20260629-214330.jpg', inStock: true },
            { id: 14, name: 'Opalfin Nutrimax Fish Food', price: 100, category: 'Fish Food', image: 'https://i.ibb.co/6Rbdg0JV/IMG-20260629-202101.jpg', inStock: true },
            { id: 15, name: 'Opalfin Xtrabite Fish Food Pellets 100gm', price: 120, category: 'Fish Food', image: 'https://i.ibb.co/M53dPDy9/IMG-20260629-214330.jpg', inStock: true },
            { id: 16, name: 'Opalfin Guppy Plus', price: 150, category: 'Fish Food', image: 'https://i.ibb.co/LDTmp86h/IMG-20260629-202123.jpg', inStock: true },
            { id: 17, name: 'Optimum Premium Fish Food 100gm', price: 80, category: 'Fish Food', image: 'https://i.ibb.co/HDtWZtK8/Untitleddesign-64-fb49fadc-f93d-47f9-ae9c-ccdd929f4e3c.png', inStock: true }
        ];

        // --- STATE MANAGEMENT ---
        let products = JSON.parse(localStorage.getItem('aqua_products')) || defaultProducts;
        let cart = JSON.parse(localStorage.getItem('aqua_cart')) || [];
        let currentFilter = 'All';

        // --- CORE RENDER FUNCTIONS ---
        function renderUI() {
            renderCategories();
            renderProducts();
            updateCartUI();
        }

        function renderCategories() {
            const categories = ['All', ...new Set(products.map(p => p.category))];
            const container = document.getElementById('category-container');
            container.innerHTML = categories.map(cat => 
                `<button class="filter-btn ${currentFilter === cat ? 'active' : ''}" onclick="filterProducts('${cat}')">${cat}</button>`
            ).join('');
        }

        function filterProducts(cat) {
            currentFilter = cat;
            renderUI();
        }

        function renderProducts() {
            const grid = document.getElementById('product-grid');
            const filtered = currentFilter === 'All' ? products : products.filter(p => p.category === currentFilter);
            
            // This is where your code was broken/cut off
            grid.innerHTML = filtered.map(p => `
                <div class="card glass">
                    ${!p.inStock ? '<div class="out-of-stock-badge">Out of Stock</div>' : ''}
                    <img src="${p.image}" alt="${p.name}" class="card-img" loading="lazy">
                    <div class="card-body">
                        <span class="category-tag">${p.category}</span>
                        <h3 class="product-title">${p.name}</h3>
                        <div style="display: flex; justify-content: space-between; align-items: center; margin-top: 10px;">
                            <span class="product-price">₹${p.price}</span>
                            <button class="btn btn-outline" style="padding: 5px 12px;" onclick="addToCart(${p.id})" ${!p.inStock ? 'disabled' : ''}>
                                <i class="fas fa-cart-plus"></i>
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        // --- CART FUNCTIONS ---
        function addToCart(id) {
            const product = products.find(p => p.id === id);
            if (!product || !product.inStock) return;
            
            const existingItem = cart.find(item => item.id === id);
            if (existingItem) {
                existingItem.qty += 1;
            } else {
                cart.push({ ...product, qty: 1 });
            }
            
            saveCart();
            updateCartUI();
            showToast('Added to Cart!');
        }

        function updateQuantity(id, change) {
            const item = cart.find(i => i.id === id);
            if (item) {
                item.qty += change;
                if (item.qty <= 0) {
                    cart = cart.filter(i => i.id !== id);
                }
                saveCart();
                updateCartUI();
            }
        }

        function saveCart() {
            localStorage.setItem('aqua_cart', JSON.stringify(cart));
        }

        function updateCartUI() {
            const cartCount = document.getElementById('cart-count');
            const cartItems = document.getElementById('cart-items');
            const cartTotal = document.getElementById('cart-total');
            
            const totalQty = cart.reduce((sum, item) => sum + item.qty, 0);
            cartCount.innerText = totalQty;
            
            if (cart.length === 0) {
                cartItems.innerHTML = '<p style="text-align: center; color: var(--text-muted); margin-top: 20px;">Your cart is empty.</p>';
                cartTotal.innerText = '₹0';
                return;
            }
            
            cartItems.innerHTML = cart.map(item => `
                <div class="cart-item">
                    <img src="${item.image}" alt="${item.name}">
                    <div class="cart-item-details">
                        <div class="cart-item-title">${item.name}</div>
                        <div style="color: var(--secondary); font-weight: bold;">₹${item.price}</div>
                        <div class="cart-controls">
                            <button class="qty-btn" onclick="updateQuantity(${item.id}, -1)">-</button>
                            <span>${item.qty}</span>
                            <button class="qty-btn" onclick="updateQuantity(${item.id}, 1)">+</button>
                        </div>
                    </div>
                </div>
            `).join('');
            
            const total = cart.reduce((sum, item) => sum + (item.price * item.qty), 0);
            cartTotal.innerText = `₹${total}`;
        }

        // --- MODAL & UI CONTROL ---
        function openCart() {
            document.getElementById('overlay').classList.add('active');
            document.getElementById('cart-drawer').classList.add('active');
        }

        function openCheckout() {
            if (cart.length === 0) {
                showToast("Your cart is empty!");
                return;
            }
            
            // Close cart, open checkout
            document.getElementById('cart-drawer').classList.remove('active');
            
            const checkout = document.getElementById('checkout-modal');
            checkout.classList.add('active');
            checkout.style.opacity = '1';
            checkout.style.visibility = 'visible';
            
            // Populate hidden form data
            const summary = cart.map(i => `${i.name} (x${i.qty})`).join(', ');
            const total = cart.reduce((sum, i) => sum + (i.price * i.qty), 0);
            
            document.getElementById('form-order-summary').value = summary;
            document.getElementById('form-grand-total').value = total;
        }

        function closeAllModals() {
            document.getElementById('overlay').classList.remove('active');
            document.getElementById('cart-drawer').classList.remove('active');
            document.getElementById('admin-dashboard').classList.remove('active');
            
            const checkout = document.getElementById('checkout-modal');
            checkout.classList.remove('active');
            checkout.style.opacity = '0';
            checkout.style.visibility = 'hidden';
        }

        function showToast(msg) {
            const toast = document.getElementById('toast');
            toast.innerText = msg;
            toast.classList.add('show');
            setTimeout(() => toast.classList.remove('show'), 3000);
        }

        // --- ADMIN FUNCTIONS ---
        function adminLoginPrompt() {
            // Note: I've set the password to "admin123" for your testing
            const password = prompt("Enter Admin Password:");
            if (password === "admin123") {
                openAdmin();
            } else if (password !== null) {
                showToast("Incorrect Password");
            }
        }

        function openAdmin() {
            closeAllModals();
            document.getElementById('overlay').classList.add('active');
            document.getElementById('admin-dashboard').classList.add('active');
            renderAdminList();
        }

        function renderAdminList() {
            const list = document.getElementById('admin-product-list');
            list.innerHTML = products.map(p => `
                <div class="admin-item glass">
                    <div style="flex-grow: 1;">
                        <strong>${p.name}</strong> - <span style="color: var(--secondary)">₹${p.price}</span>
                        <div style="font-size: 0.8rem; color: var(--text-muted); margin-top: 5px;">
                            ${p.category} | ${p.inStock ? '<span style="color:var(--secondary)">In Stock</span>' : '<span style="color:var(--danger)">Out of Stock</span>'}
                        </div>
                    </div>
                    <div style="display: flex; gap: 10px;">
                        <button class="btn btn-outline" style="padding: 8px;" onclick="editProduct(${p.id})"><i class="fas fa-edit"></i></button>
                        <button class="btn btn-danger" style="padding: 8px;" onclick="deleteProduct(${p.id})"><i class="fas fa-trash"></i></button>
                    </div>
                </div>
            `).join('');
        }

        function saveProduct(e) {
            e.preventDefault();
            const id = document.getElementById('prod-id').value;
            const newProd = {
                id: id ? parseInt(id) : Date.now(),
                name: document.getElementById('prod-name').value,
                price: parseFloat(document.getElementById('prod-price').value),
                category: document.getElementById('prod-cat').value,
                image: document.getElementById('prod-img').value,
                inStock: document.getElementById('prod-stock').checked
            };

            if (id) {
                const index = products.findIndex(p => p.id === parseInt(id));
                if(index !== -1) products[index] = newProd;
            } else {
                products.push(newProd);
            }
            
            localStorage.setItem('aqua_products', JSON.stringify(products));
            resetAdminForm();
            renderAdminList();
            renderUI();
            showToast('Product Saved Successfully!');
        }

        function editProduct(id) {
            const p = products.find(prod => prod.id === id);
            if (!p) return;
            
            document.getElementById('prod-id').value = p.id;
            document.getElementById('prod-name').value = p.name;
            document.getElementById('prod-price').value = p.price;
            document.getElementById('prod-cat').value = p.category;
            document.getElementById('prod-img').value = p.image;
            document.getElementById('prod-stock').checked = p.inStock;
            
            document.getElementById('admin-form').scrollIntoView();
        }

        function deleteProduct(id) {
            if(confirm('Are you sure you want to delete this product?')) {
                products = products.filter(p => p.id !== id);
                localStorage.setItem('aqua_products', JSON.stringify(products));
                renderAdminList();
                renderUI();
                showToast('Product Deleted');
            }
        }

        function resetAdminForm() {
            document.getElementById('admin-form').reset();
            document.getElementById('prod-id').value = '';
        }

        function exportData() {
            const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(products));
            const downloadAnchorNode = document.createElement('a');
            downloadAnchorNode.setAttribute("href", dataStr);
            downloadAnchorNode.setAttribute("download", "aqua_cart_backup.json");
            document.body.appendChild(downloadAnchorNode); 
            downloadAnchorNode.click();
            downloadAnchorNode.remove();
            showToast('Backup Downloaded');
        }

        // --- INITIALIZE APPLICATION ---
        renderUI();
    </script>
</body>
</html># Smartaquacart1298
