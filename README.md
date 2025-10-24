# agrawal-agency-website
Agrawal Agency is a modern and responsive shop website designed and developed by me. It showcases a professional online storefront for a local agency, featuring product listings, clean UI, and easy navigation. Built using HTML, CSS, and JavaScript, it serves as a simple yet elegant.
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Agrawal Agency</title>
  <style>
    /* Reset & base */
    * { box-sizing: border-box; }
    html, body { height: 100%; }
    body {
      margin: 0; font-family: Arial, Helvetica, sans-serif; color: #fff; text-align: center;
      position: relative; min-height: 100vh; background-color: #0b0b0b;
      -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale;
    }

    /* Background image with blur + dark overlay for readability */
    body::before {
      content: "";
      background: url("logo.jpg") no-repeat center center/cover;
      filter: blur(8px) brightness(0.55);
      position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: -2;
      background-attachment: fixed;
    }
    body::after {
      content: ""; position: absolute; top: 0; left: 0; width: 100%; height: 100%;
      background: linear-gradient(180deg, rgba(0,0,0,0.35) 0%, rgba(0,0,0,0.6) 100%);
      z-index: -1;
    }

    /* Navbar */
    nav {
      background: rgba(0,0,0,0.65); display: flex; justify-content: space-between; align-items: center;
      padding: 10px 20px; position: sticky; top: 0; z-index: 10; backdrop-filter: blur(4px);
    }
    .logo-area { display: flex; align-items: center; gap: 10px; }
    .logo-area img { width: 44px; height: 44px; border-radius: 50%; object-fit: cover; }
    .logo-area h2 { margin: 0; font-size: 18px; color: #ffd700; }
    .links { display: flex; gap: 14px; }
    .links a { color: #fff; text-decoration: none; font-weight: 600; padding: 6px 8px; border-radius: 6px; }
    .links a:hover, .links a.active { color: #111; background: #ffd700; }
    .cart { cursor: pointer; font-size: 16px; display: flex; gap: 8px; align-items: center; }

    /* Header */
    header { padding: 28px 16px; }
    header h1 { margin: 6px 0 14px; font-weight: 700; color: #ffd700; font-size: 28px; }
    .search-box { margin: 0 auto; max-width: 700px; }
    .search-box input {
      padding: 12px 16px; width: 100%; border-radius: 999px; border: 1px solid rgba(255,255,255,0.12);
      font-size: 16px; outline: none; background: rgba(0,0,0,0.6); color: #fff;
    }
    .search-box input::placeholder { color: rgba(255,255,255,0.6); }

    /* Products grid */
    .products { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 20px; padding: 20px; max-width: 1200px; margin: 0 auto; }
    .product { background: linear-gradient(180deg, rgba(255,255,255,0.03), rgba(255,255,255,0.02)); padding: 18px; border-radius: 12px; box-shadow: 0 6px 18px rgba(0,0,0,0.6); text-align: center; transition: transform 0.28s ease, box-shadow 0.28s ease; }
    .product:hover { transform: translateY(-8px); box-shadow: 0 18px 40px rgba(0,0,0,0.7); }
    .product img { width: 110px; height: 110px; object-fit: contain; display: block; margin: 0 auto 10px; }
    .product h3 { margin: 8px 0 6px; font-size: 18px; }
    .product p { margin: 6px 0 8px; font-size: 14px; color: rgba(255,255,255,0.85); }
    .product select { margin-top: 6px; padding: 8px; border-radius: 8px; border: none; width: 100%; background: rgba(0,0,0,0.55); color: #fff; }
    .product button { margin-top: 10px; padding: 10px 14px; background: #ffd700; border: none; border-radius: 999px; cursor: pointer; font-weight: 700; transition: transform 0.12s; }
    .product button:active { transform: scale(0.98); }

    /* About & Footer */
    section#about { padding: 24px 18px; max-width: 900px; margin: 12px auto; color: rgba(255,255,255,0.95); }
    footer { background: rgba(0,0,0,0.65); padding: 18px; margin-top: 22px; text-align: center; }
    footer p, footer a { margin: 6px 0; color: #fff; text-decoration: none; font-size: 14px; }

    /* Cart Modal */
    .cart-modal { display: none; position: fixed; left: 50%; top: 50%; transform: translate(-50%, -50%); background: rgba(8,8,8,0.98); padding: 18px; border-radius: 12px; width: 360px; max-width: calc(100% - 32px); text-align: left; z-index: 40; box-shadow: 0 20px 50px rgba(0,0,0,0.8); }
    .cart-modal.open { display: block; }
    .cart-modal h2 { margin: 0 0 10px; color: #ffd700; }
    .cart-modal ul { list-style: none; padding: 0; margin: 0; max-height: 42vh; overflow: auto; }
    .cart-modal ul li { margin: 8px 0; padding-bottom: 8px; border-bottom: 1px solid rgba(255,255,255,0.06); display: flex; justify-content: space-between; align-items: center; }
    .cart-modal ul li .meta { font-size: 13px; color: rgba(255,255,255,0.9); }
    .cart-modal ul li button { padding: 6px 8px; border-radius: 8px; border: none; cursor: pointer; }
    .close-btn { background: #e94b4b; color: #fff; border: none; padding: 6px 10px; border-radius: 8px; cursor: pointer; float: right; }
    .total { margin-top: 12px; font-size: 16px; font-weight: 700; color: #ffd700; }
    .cart-actions { display: flex; gap: 8px; justify-content: flex-end; margin-top: 12px; }
    .btn-secondary { background: rgba(255,255,255,0.06); color: #fff; padding: 8px 10px; border-radius: 8px; border: none; cursor: pointer; }

    /* Responsive tweaks */
    @media (max-width: 600px) {
      nav { padding: 10px; }
      header h1 { font-size: 20px; }
      .logo-area h2 { font-size: 16px; }
    }

    /* Small accessibility helpers */
    .sr-only { position: absolute !important; width: 1px; height: 1px; padding: 0; margin: -1px; overflow: hidden; clip: rect(0,0,0,0); white-space: nowrap; border: 0; }
  </style>
</head>
<body>
  <!-- Navigation -->
  <nav>
    <div class="logo-area">
      <img src="logo.jpg" alt="Agrawal Agency logo" id="site-logo" />
      <h2>Agrawal Agency</h2>
    </div>

    <div class="links" role="navigation" aria-label="Main navigation">
      <a href="#home" data-nav="home" class="active">Home</a>
      <a href="#about" data-nav="about">About</a>
      <a href="#products" data-nav="products">Products</a>
      <a href="#contact" data-nav="contact">Contact</a>
    </div>

    <div class="cart" id="cart-toggle" role="button" tabindex="0" aria-haspopup="dialog" aria-controls="cart-modal">
      <span aria-hidden>🛒</span>
      <span>Cart (<span id="cart-count">0</span>)</span>
    </div>
  </nav>

  <!-- Header -->
  <header id="home">
    <h1>Welcome to Agrawal Agency</h1>
    <div class="search-box">
      <label for="search-input" class="sr-only">Search equipments</label>
      <input type="search" id="search-input" placeholder="Search equipments... (e.g. Wire, Plug, Cable)" aria-label="Search equipments" />
    </div>
  </header>

  <!-- Products Section -->
  <section id="products" class="products" aria-label="Products">
    <!-- Products will be rendered by JavaScript -->
  </section>

  <!-- About Section -->
  <section id="about">
    <h2>About Us</h2>
    <p>Agrawal Agency is your trusted partner for all kinds of electrical equipments. We provide high quality products with reliable service since 2003.</p>
  </section>

  <!-- Cart Modal -->
  <div id="cart-modal" class="cart-modal" role="dialog" aria-modal="true" aria-labelledby="cart-heading">
    <button class="close-btn" id="cart-close">X</button>
    <h2 id="cart-heading">Your Cart</h2>
    <ul id="cart-items" aria-live="polite"></ul>
    <div class="total">Total Items: <span id="cart-total">0</span></div>
    <div class="cart-actions">
      <button class="btn-secondary" id="clear-cart">Clear</button>
      <button id="checkout" style="background:#ffd700; padding:8px 12px; border-radius:8px; border:none; font-weight:700; cursor:pointer;">Checkout</button>
    </div>
  </div>

  <!-- Footer -->
  <footer id="contact">
    <p>&copy; 2025 Agrawal Agency. All Rights Reserved.</p>
    <p><strong>Email:</strong> <a href="mailto:anand.monika.agrawal@gmail.com">anand.monika.agrawal@gmail.com</a></p>
    <p><strong>Contact Numbers:</strong> <a href="tel:+917000086130">+91-7000086130</a> | <a href="tel:+919329711716">+91-9329711716</a></p>
  </footer>

  <script>
    // ---------------------------
    // DATA: product list (single source of truth)
    // ---------------------------
    const PRODUCTS = [
      { id: 'plug', title: 'Plugs', desc: 'Durable electric plugs & sockets.', img: 'https://img.icons8.com/ios-filled/100/plug.png', brands: ['Havells','Anchor','Legrand'] },
      { id: 'wire', title: 'Wires', desc: 'Strong copper & aluminum wires.', img: 'https://img.icons8.com/ios-filled/100/electricity.png', brands: ['Polycab - 1m','Polycab - 5m','Polygold - 1m','Polygold - 5m','Anishka Gold - 1m','Anishka Gold - 5m'] },
      { id: 'switch', title: 'Switches', desc: 'Stylish and safe electric switches.', img: 'https://img.icons8.com/ios-filled/100/switch-on.png', brands: ['Havells','Anchor','Legrand'] },
      { id: 'cable', title: 'Cables', desc: 'Durable electric cables.', img: 'https://img.icons8.com/ios-filled/100/high-voltage-cable.png', brands: ['Finolex','Polycab','Havells'] },
      { id: 'fitting', title: 'Fitting Accessories', desc: 'All types of fitting accessories.', img: 'https://img.icons8.com/ios-filled/100/toolbox.png', brands: ['Havells','Anchor'] },
      { id: 'pvc', title: 'PVC Pipes', desc: 'High quality PVC pipes.', img: 'https://img.icons8.com/ios-filled/100/water-pipe.png', brands: ['Astral','Prince'] },
      { id: 'capacitor', title: 'Capacitor', desc: 'Reliable capacitors for electric use.', img: 'https://img.icons8.com/ios-filled/100/electronics.png', brands: ['Vishay','Philips'] },
      { id: 'accessories', title: 'Other Accessories', desc: 'Miscellaneous electric accessories.', img: 'https://img.icons8.com/ios-filled/100/settings.png', brands: ['Generic','Havells'] }
    ];

    // ---------------------------
    // CART: state + persistence
    // ---------------------------
    let cart = [];
    const CART_KEY = 'agrawal_cart_v1';

    function saveCart() {
      try { localStorage.setItem(CART_KEY, JSON.stringify(cart)); } catch (e) { console.warn('LocalStorage not available', e); }
    }
    function loadCart() {
      try {
        const raw = localStorage.getItem(CART_KEY);
        if (raw) cart = JSON.parse(raw);
      } catch (e) { console.warn('Could not load cart', e); }
    }

    // ---------------------------
    // RENDER: products & cart
    // ---------------------------
    const productsEl = document.getElementById('products');

    function renderProducts(filter = '') {
      const q = filter.trim().toLowerCase();
      productsEl.innerHTML = PRODUCTS.filter(p => p.title.toLowerCase().includes(q) || p.desc.toLowerCase().includes(q) || p.brands.join(' ').toLowerCase().includes(q)).map(p => {
        // create select options
        const opts = ['<option value="">-- Choose your brand --</option>', ...p.brands.map(b => `<option value="${escapeHtml(b)}">${escapeHtml(b)}</option>`)].join('');
        return `
          <div class="product" data-id="${p.id}">
            <img src="${p.img}" alt="${escapeHtml(p.title)}"/>
            <h3>${escapeHtml(p.title)}</h3>
            <p>${escapeHtml(p.desc)}</p>
            <select aria-label="Select brand for ${escapeHtml(p.title)}" data-select="${p.id}">${opts}</select>
            <button data-add="${p.id}">Add to Cart</button>
          </div>
        `;
      }).join('');
    }

    function renderCart() {
      const itemsEl = document.getElementById('cart-items');
      const countEl = document.getElementById('cart-count');
      const totalEl = document.getElementById('cart-total');
      itemsEl.innerHTML = cart.map((it, idx) => `
        <li>
          <div class="meta">${escapeHtml(it.product)} - ${escapeHtml(it.brand)}</div>
          <div>
            <button data-remove="${idx}" title="Remove item">Remove</button>
          </div>
        </li>
      `).join('') || '<li style="color:rgba(255,255,255,0.7)">Your cart is empty</li>';
      countEl.innerText = cart.length;
      totalEl.innerText = cart.length;
    }

    // ---------------------------
    // HELPERS
    // ---------------------------
    function escapeHtml(str) { return String(str).replace(/[&<>"']/g, s => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[s])); }

    // ---------------------------
    // EVENTS: add/remove/cart UI
    // ---------------------------
    function addToCart(productId) {
      const sel = document.querySelector(`select[data-select="${productId}"]`);
      const brand = sel ? sel.value : '';
      const product = PRODUCTS.find(p => p.id === productId);
      if (!brand) { alert('Please select a brand before adding to cart.'); sel.focus(); return; }
      cart.push({ product: product.title, brand });
      saveCart(); renderCart(); openCart();
    }

    function removeFromCart(index) { cart.splice(index,1); saveCart(); renderCart(); }
    function clearCart() { if (!cart.length) return; if (!confirm('Clear all items from cart?')) return; cart = []; saveCart(); renderCart(); }

    // Event delegation for products (Add button)
    productsEl.addEventListener('click', (e) => {
      const btn = e.target.closest('button[data-add]');
      if (btn) { addToCart(btn.getAttribute('data-add')); }
    });

    // Product select change allowed by keyboard etc.

    // Cart actions (remove / clear / checkout)
    const cartModal = document.getElementById('cart-modal');
    document.getElementById('cart-items').addEventListener('click', (e) => {
      const btn = e.target.closest('button[data-remove]');
      if (btn) { removeFromCart(Number(btn.getAttribute('data-remove'))); }
    });
    document.getElementById('clear-cart').addEventListener('click', clearCart);
    document.getElementById('checkout').addEventListener('click', () => {
      if (!cart.length) { alert('Your cart is empty'); return; }
      // simple placeholder for checkout flow
      alert(`Proceeding to checkout with ${cart.length} item(s). (Demo)`);
    });

    // Toggle cart modal
    const cartToggle = document.getElementById('cart-toggle');
    const cartClose = document.getElementById('cart-close');
    cartToggle.addEventListener('click', openCart);
    cartToggle.addEventListener('keydown', (e) => { if (e.key === 'Enter' || e.key === ' ') { e.preventDefault(); openCart(); } });
    cartClose.addEventListener('click', closeCart);

    // Close on outside click or Esc
    window.addEventListener('click', (e) => { if (!cartModal.classList.contains('open')) return; if (!cartModal.contains(e.target) && !cartToggle.contains(e.target)) closeCart(); });
    window.addEventListener('keydown', (e) => { if (e.key === 'Escape') closeCart(); });

    function openCart() { cartModal.classList.add('open'); cartModal.setAttribute('aria-hidden','false'); document.body.style.overflow = 'hidden'; }
    function closeCart() { cartModal.classList.remove('open'); cartModal.setAttribute('aria-hidden','true'); document.body.style.overflow = ''; }

    // Search filter
    const searchInput = document.getElementById('search-input');
    let searchTimer = null;
    searchInput.addEventListener('input', (e) => {
      const q = e.target.value;
      // debounce
      if (searchTimer) clearTimeout(searchTimer);
      searchTimer = setTimeout(() => renderProducts(q), 120);
    });

    // Nav link active state
    document.querySelectorAll('.links a').forEach(a => a.addEventListener('click', (e) => {
      document.querySelectorAll('.links a').forEach(x => x.classList.remove('active'));
      e.currentTarget.classList.add('active');
    }));

    // On load: hydrate cart & render
    window.addEventListener('DOMContentLoaded', () => {
      loadCart(); renderProducts(); renderCart();

      // Restore focus outline for keyboard users
      document.body.addEventListener('keydown', (e) => { if (e.key === 'Tab') document.documentElement.classList.add('show-focus'); }, { once: true });

      // Accessibility: trap focus inside cart when open
      cartModal.addEventListener('keydown', (e) => {
        if (e.key !== 'Tab') return;
        const focusable = cartModal.querySelectorAll('button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])');
        if (!focusable.length) return;
        const first = focusable[0], last = focusable[focusable.length-1];
        if (e.shiftKey && document.activeElement === first) { e.preventDefault(); last.focus(); }
        else if (!e.shiftKey && document.activeElement === last) { e.preventDefault(); first.focus(); }
      });
    });

    // small helper: open cart when item is added
    function notify(msg) { console.log(msg); }

  </script>
</body>
</html>
