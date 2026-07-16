1. HTML (index.html)
Pizzalar ro'yxati va savatcha uchun asosiy tuzilma:

HTML
<div class="menu">
    <h2>Pizza Menyusi</h2>
    <div class="pizza-card">
        <h3>Margherita</h3>
        <p>Narxi: 50,000 so'm</p>
        <button onclick="addToCart({id: 1, name: 'Margherita', price: 50000})">Savatchaga qo'shish</button>
    </div>
</div>

<div class="cart">
    <h2>Savatcha</h2>
    <div id="cartItems"></div>
    <div class="cart-total">
        <p>Jami: <span id="totalPrice">0</span> so'm</p>
        <button onclick="confirmOrder()">Buyurtma berish</button>
    </div>
</div>

<!-- Buyurtma tasdiqlash modali -->
<div id="orderModal" class="modal" style="display:none;">
    <div class="modal-content">
        <p>Buyurtmani tasdiqlaysizmi?</p>
        <button onclick="placeOrder()">Ha, tasdiqlayman</button>
        <button onclick="closeModal()">Yo'q</button>
    </div>
</div>
2. JavaScript (script.js)
Savatcha logikasi, localStorage bilan ishlash va DOM manipulyatsiyasi:

JavaScript
let cart = JSON.parse(localStorage.getItem('pizzaCart')) || [];

function addToCart(pizza) {
    const existing = cart.find(item => item.id === pizza.id);
    if (existing) {
        existing.quantity += 1;
    } else {
        cart.push({ ...pizza, quantity: 1 });
    }
    updateCart();
}

function updateQuantity(id, delta) {
    const item = cart.find(item => item.id === id);
    if (item) {
        item.quantity += delta;
        if (item.quantity <= 0) {
            cart = cart.filter(i => i.id !== id);
        }
    }
    updateCart();
}

function removeItem(id) {
    cart = cart.filter(item => item.id !== id);
    updateCart();
}

function updateCart() {
    localStorage.setItem('pizzaCart', JSON.stringify(cart));
    const container = document.getElementById('cartItems');
    const totalEl = document.getElementById('totalPrice');
    
    container.innerHTML = cart.map(item => `
        <div class="cart-item">
            <span>${item.name} (${item.price.toLocaleString()} so'm)</span>
            <div>
                <button onclick="updateQuantity(${item.id}, -1)">-</button>
                <span>${item.quantity}</span>
                <button onclick="updateQuantity(${item.id}, 1)">+</button>
                <button onclick="removeItem(${item.id})">O'chirish</button>
            </div>
        </div>
    `).join('');

    const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
    totalEl.innerText = total.toLocaleString();
}

// Modallar logikasi
function confirmOrder() { document.getElementById('orderModal').style.display = 'block'; }
function closeModal() { document.getElementById('orderModal').style.display = 'none'; }
function placeOrder() {
    alert("Buyurtma muvaffaqiyatli qabul qilindi!");
    cart = [];
    updateCart();
    closeModal();
}

// Sahifa yuklanganda savatchani yangilash
updateCart();
3. CSS (style.css)
Interfeysni tartibga solish uchun asosiy stillar:

CSS
.modal { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); }
.modal-content { background: white; margin: 20% auto; padding: 20px; width: 300px; text-align: center; }
.cart-item { border-bottom: 1px solid #ddd; padding: 10px 0; display: flex; justify-content: space-between; }
.cart { border: 1px solid #ccc; padding: 15px; margin-top: 20px; }
