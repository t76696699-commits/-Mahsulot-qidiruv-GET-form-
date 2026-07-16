
Чат с Gemini
✅ Filmni qo'shish: nomi, janri, yili, reytingi

✅ Filmlar kartochka ko'rinishida chiqsin

✅ Nom bo'yicha real vaqt qidiruvi

✅ Janr bo'yicha filtrlash

✅ Filmni o'chirish

✅ Ma'lumotlarni localStorage'da saqlang

Стек технологий

HTML · CSS · JavaScript · Objects · Arrays · DOM · localStorage



Ushbu loyihani amalga oshirish uchun sizga HTML, CSS va JavaScript fayllarining birgalikdagi ishlashi kerak bo'ladi. Quyida loyihaning sodda va samarali strukturasi keltirilgan.

1. HTML (index.html)
Interfeys uchun asosiy elementlar:

HTML
<div class="container">
    <h2>Film Katalogi</h2>
    <!-- Qo'shish formasi -->
    <div class="form-group">
        <input type="text" id="name" placeholder="Film nomi">
        <input type="text" id="genre" placeholder="Janri">
        <input type="number" id="year" placeholder="Yili">
        <input type="number" id="rating" placeholder="Reyting (1-10)" step="0.1">
        <button onclick="addMovie()">Qo'shish</button>
    </div>

    <!-- Qidiruv va Filtr -->
    <div class="controls">
        <input type="text" id="searchInput" oninput="renderMovies()" placeholder="Nom bo'yicha qidirish...">
        <select id="filterGenre" onchange="renderMovies()">
            <option value="all">Barcha janrlar</option>
        </select>
    </div>

    <!-- Kartochkalar chiqadigan joy -->
    <div id="movieList" class="movie-grid"></div>
</div>
2. JavaScript (script.js)
Asosiy logika, localStorage va DOM manipulyatsiyasi:

JavaScript
let movies = JSON.parse(localStorage.getItem('movies')) || [];

function saveAndRender() {
    localStorage.setItem('movies', JSON.stringify(movies));
    updateGenreFilter();
    renderMovies();
}

function addMovie() {
    const name = document.getElementById('name').value;
    const genre = document.getElementById('genre').value;
    const year = document.getElementById('year').value;
    const rating = document.getElementById('rating').value;

    if (!name || !genre) return alert("Barcha maydonlarni to'ldiring!");

    movies.push({ id: Date.now(), name, genre, year, rating });
    saveAndRender();
}

function deleteMovie(id) {
    movies = movies.filter(m => m.id !== id);
    saveAndRender();
}

function renderMovies() {
    const searchTerm = document.getElementById('searchInput').value.toLowerCase();
    const filterGenre = document.getElementById('filterGenre').value;
    const list = document.getElementById('movieList');
    
    list.innerHTML = '';

    const filtered = movies.filter(m => {
        const matchesName = m.name.toLowerCase().includes(searchTerm);
        const matchesGenre = filterGenre === 'all' || m.genre === filterGenre;
        return matchesName && matchesGenre;
    });

    filtered.forEach(m => {
        list.innerHTML += `
            <div class="card">
                <h3>${m.name}</h3>
                <p>Janr: ${m.genre}</p>
                <p>Yil: ${m.year} | Reyting: ${m.rating}</p>
                <button onclick="deleteMovie(${m.id})">O'chirish</button>
            </div>
        `;
    });
}

function updateGenreFilter() {
    const select = document.getElementById('filterGenre');
    const genres = [...new Set(movies.map(m => m.genre))];
    select.innerHTML = '<option value="all">Barcha janrlar</option>';
    genres.forEach(g => select.innerHTML += `<option value="${g}">${g}</option>`);
}

// Boshlang'ich holat
saveAndRender();
3. CSS (style.css)
Kartochkalar ko'rinishi uchun:

CSS
.movie-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 15px;
    margin-top: 20px;
}
.card {
    border: 1px solid #ccc;
    padding: 15px;
    border-radius: 8px;
    background: #f9f9f9;
}
Muhim eslatmalar:
LocalStorage: JSON.stringify yordamida obyektlar massivini matn ko'rinishida saqlaymiz, o'qiyotganda esa JSON.parse orqali qayta obyektga aylantiramiz.

Qidiruv: oninput hodisasi orqali har bir harf kiritilganda renderMovies() funksiyasini chaqirib, natijalarni real vaqtda yangilab boramiz.

Filtrlash: filter() metodi orqali faqat tanlangan janrga mos keladigan filmlarni ajratib olamiz.

Sizning loyihangizda qaysi qismda qiyinchilik tug'ilishi mumkin deb o'ylaysiz?

✅ Kamida 6 ta rasm kartochkasi

✅ Rasm bosilganda overlay ochilsin

✅ Overlay: oldinga/orqaga navigatsiya

✅ Escape tugmasi yoki X bosilganda yopilsin

✅ Silliq CSS animatsiya (fade-in/out)

✅ Keyboard navigatsiya (← →) ishlashi kerak

 Стек технологий

HTML · CSS · JavaScript · DOM · CSS Animations · KeyboardEvent

Ushbu talablar asosida Galereya va Lightbox (Overlay) loyihasining to'liq va tayyor kodini keltiraman. Bu loyihada rasmlarni silliq ochilishi, yopilishi hamda klaviatura orqali boshqarilishi to'liq ta'minlangan.

1. HTML (index.html)
Galereya kartochkalari va rasmni kattalashtirib ko'rsatuvchi overlay interfeysi:

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Silliq Animatsiyali Galereya</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <div class="container">
        <h2>Mening Galereyam</h2>
        
        <!-- Galereya kartochkalari (Kamida 6 ta) -->
        <div class="gallery-grid">
            <div class="card" onclick="openLightbox(0)"><img src="https://picsum.photos/id/10/600/400" alt="Rasm 1"><div class="caption">Tabiat 1</div></div>
            <div class="card" onclick="openLightbox(1)"><img src="https://picsum.photos/id/15/600/400" alt="Rasm 2"><div class="caption">Tabiat 2</div></div>
            <div class="card" onclick="openLightbox(2)"><img src="https://picsum.photos/id/28/600/400" alt="Rasm 3"><div class="caption">Tabiat 3</div></div>
            <div class="card" onclick="openLightbox(3)"><img src="https://picsum.photos/id/37/600/400" alt="Rasm 4"><div class="caption">Tabiat 4</div></div>
            <div class="card" onclick="openLightbox(4)"><img src="https://picsum.photos/id/43/600/400" alt="Rasm 5"><div class="caption">Tabiat 5</div></div>
            <div class="card" onclick="openLightbox(5)"><img src="https://picsum.photos/id/48/600/400" alt="Rasm 6"><div class="caption">Tabiat 6</div></div>
        </div>
    </div>

    <!-- Overlay (Lightbox) Modali -->
    <div id="lightbox" class="lightbox">
        <span class="close-btn" onclick="closeLightbox()">&times;</span>
        <button class="nav-btn prev-btn" onclick="changeImage(-1)">&#10094;</button>
        <div class="lightbox-content">
            <img id="lightbox-img" src="" alt="Kattalashtirilgan rasm">
        </div>
        <button class="nav-btn next-btn" onclick="changeImage(1)">&#10095;</button>
    </div>

    <script src="script.js"></script>
</body>
</html>
2. CSS (style.css)
Silliq fade-in va fade-out animatsiyalari hamda Grid tartibi:

CSS
/* Umumiy stillar */
body {
    margin: 0;
    font-family: 'Segoe UI', Arial, sans-serif;
    background-color: #f4f6f9;
}
.container {
    max-width: 1000px;
    margin: 40px auto;
    padding: 0 20px;
    text-align: center;
}

/* Galereya Grid */
.gallery-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 20px;
    margin-top: 30px;
}
.card {
    position: relative;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    cursor: pointer;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 16px rgba(0,0,0,0.2);
}
.card img {
    width: 100%;
    height: 200px;
    object-fit: cover;
    display: block;
}
.caption {
    padding: 12px;
    background: #fff;
    font-weight: 600;
    color: #333;
}

/* Overlay (Lightbox) va Silliq Animatsiya */
.lightbox {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.9);
    z-index: 1000;
    justify-content: space-between;
    align-items: center;
    opacity: 0;
    transition: opacity 0.4s ease;
}
.lightbox.show {
    display: flex;
    opacity: 1;
}

/* Rasm joylashuvi va animatsiyasi */
.lightbox-content {
    max-width: 80%;
    max-height: 80%;
    display: flex;
    justify-content: center;
}
.lightbox-content img {
    max-width: 100%;
    max-height: 80vh;
    border-radius: 4px;
    box-shadow: 0 5px 25px rgba(0,0,0,0.5);
    transform: scale(0.95);
    transition: transform 0.4s ease;
}
.lightbox.show .lightbox-content img {
    transform: scale(1);
}

/* Navigatsiya tugmalari */
.nav-btn {
    background: none;
    border: none;
    color: white;
    font-size: 2rem;
    padding: 20px;
    cursor: pointer;
    transition: color 0.2s;
    user-select: none;
}
.nav-btn:hover {
    color: #00bcd4;
}
.close-btn {
    position: absolute;
    top: 20px;
    right: 30px;
    color: white;
    font-size: 3rem;
    cursor: pointer;
    transition: color 0.2s;
}
.close-btn:hover {
    color: #ff4a4a;
}
3. JavaScript (script.js)
Klaviaturani tinglash (KeyboardEvent), indekslarni hisoblash va overlay boshqaruvi:

JavaScript
// Rasmlar manzillarini yig'ib olamiz
const images = Array.from(document.querySelectorAll('.gallery-grid .card img')).map(img => img.src);
let currentIndex = 0;

const lightbox = document.getElementById('lightbox');
const lightboxImg = document.getElementById('lightbox-img');

// Overlayni ochish
function openLightbox(index) {
    currentIndex = index;
    lightboxImg.src = images[currentIndex];
    
    lightbox.style.display = 'flex';
    // Silliq fade-in bo'lishi uchun ozroq kechikish bilan klass qo'shamiz
    setTimeout(() => {
        lightbox.classList.add('show');
    }, 10);
    
    document.body.style.overflow = 'hidden'; // Orqa fon skroll bo'lmasligi uchun
}

// Overlayni yopish
function closeLightbox() {
    lightbox.classList.remove('show');
    
    // Animatsiya tugagandan keyin display'ni yo'qotamiz
    setTimeout(() => {
        lightbox.style.display = 'none';
        document.body.style.overflow = 'auto';
    }, 400);
}

// Oldinga yoki orqaga o'tkazish
function changeImage(direction) {
    currentIndex += direction;
    
    // Chegaradan chiqib ketganda aylanma tizim (Infinite loop)
    if (currentIndex >= images.length) {
        currentIndex = 0;
    } else if (currentIndex < 0) {
        currentIndex = images.length - 1;
    }
    
    lightboxImg.src = images[currentIndex];
}

// Klaviatura hodisalarini tinglash (KeyboardEvent)
document.addEventListener('keydown', (event) => {
    // Agar lightbox ochiq bo'lsagina ishlaydi
    if (!lightbox.classList.contains('show')) return;

    if (event.key === 'Escape') {
        closeLightbox();
    } else if (event.key === 'ArrowRight') {
        changeImage(1);
    } else if (event.key === 'ArrowLeft') {
        changeImage(-1);
    }
});

// Overlay foniga bosilganda ham yopilishi uchun (Qulaylik uchun qo'shimcha)
lightbox.addEventListener('click', (e) => {
    if (e.target === lightbox || e.target.classList.contains('lightbox-content')) {
        closeLightbox();
    }
});
Loyihadagi asosiy yechimlar:
Silliq Fade-in/out: CSS da shunchaki display: nonedan blockga o'tganda animatsiya ishlamaydi. Shuning uchun JS da oldin display: flex qilinib, so'ng show klassi orqali opacity: 1 berildi. Yopishda esa teskari tartibda kechikish (setTimeout) ishlatildi.

Klaviatura navigatsiyasi: keydown hodisasi orqali Escape, ArrowLeft (chapga ko'rsatkich) va ArrowRight (o'ngga ko'rsatkich) tugmalari aniqlanib, mos funksiyalar chaqirildi.

Chegarasiz aylanish: Rasmlar tugab qolganda if (currentIndex >= images.length) sharti orqali indeks yana 0 ga qaytadi va uzluksiz aylanish ta'minlanadi.

✅ Forma orqali yangi yozuv qo'shish

✅ Jadvalda barcha yozuvlarni ko'rsatish

✅ Yozuvni tahrirlash (formaga ma'lumot yuklansin)

✅ Yozuvni o'chirish (tasdiqlash so'ralsin)

✅ Ma'lumotlarni localStorage'da saqlang

✅ Jadval bo'sh bo'lganda xabar ko'rsating

Ushbu loyiha uchun CRUD (Create, Read, Update, Delete) operatsiyalarini va localStorage bilan ishlashni o'z ichiga olgan to'liq yechimni taqdim etaman.

1. HTML (index.html)
Forma va jadval tuzilmasi:

HTML
<div class="container">
    <h2>Ma'lumotlar bazasi</h2>
    
    <!-- Forma -->
    <form id="dataForm">
        <input type="hidden" id="editIndex">
        <input type="text" id="name" placeholder="Ism" required>
        <input type="email" id="email" placeholder="Email" required>
        <button type="submit" id="submitBtn">Qo'shish</button>
    </form>

    <!-- Jadval -->
    <table id="dataTable">
        <thead>
            <tr><th>Ism</th><th>Email</th><th>Amallar</th></tr>
        </thead>
        <tbody id="tableBody"></tbody>
    </table>
    <div id="emptyMsg" style="display:none; text-align:center; padding: 20px;">Jadval bo'sh</div>
</div>
2. JavaScript (script.js)
Logika, tahrirlash va localStorage bilan ishlash:

JavaScript
let data = JSON.parse(localStorage.getItem('formData')) || [];
const form = document.getElementById('dataForm');
const tableBody = document.getElementById('tableBody');
const emptyMsg = document.getElementById('emptyMsg');

function renderTable() {
    tableBody.innerHTML = '';
    
    if (data.length === 0) {
        emptyMsg.style.display = 'block';
    } else {
        emptyMsg.style.display = 'none';
        data.forEach((item, index) => {
            tableBody.innerHTML += `
                <tr>
                    <td>${item.name}</td>
                    <td>${item.email}</td>
                    <td>
                        <button onclick="editItem(${index})">Tahrirlash</button>
                        <button onclick="deleteItem(${index})">O'chirish</button>
                    </td>
                </tr>
            `;
        });
    }
}

// Qo'shish va Tahrirlash
form.addEventListener('submit', (e) => {
    e.preventDefault();
    const index = document.getElementById('editIndex').value;
    const newData = {
        name: document.getElementById('name').value,
        email: document.getElementById('email').value
    };

    if (index === "") {
        data.push(newData);
    } else {
        data[index] = newData;
        document.getElementById('submitBtn').innerText = "Qo'shish";
        document.getElementById('editIndex').value = "";
    }
    
    saveAndRender();
    form.reset();
});

// Tahrirlashga yuklash
window.editItem = (index) => {
    document.getElementById('name').value = data[index].name;
    document.getElementById('email').value = data[index].email;
    document.getElementById('editIndex').value = index;
    document.getElementById('submitBtn').innerText = "Yangilash";
};

// O'chirish
window.deleteItem = (index) => {
    if (confirm("Rostdan ham o'chirmoqchimisiz?")) {
        data.splice(index, 1);
        saveAndRender();
    }
};

function saveAndRender() {
    localStorage.setItem('formData', JSON.stringify(data));
    renderTable();
}

renderTable();
3. CSS (Qisqacha tavsiya)
Jadval va formani chiroyli ko'rsatish uchun:

CSS
table { width: 100%; border-collapse: collapse; margin-top: 20px; }
th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
form { display: flex; gap: 10px; margin-bottom: 20px; }
button { cursor: pointer; }
Ushbu yechimning afzalliklari:
Tahrirlash rejimi: hidden input (editIndex) orqali qaysi qator tahrirlanayotganini aniqlab olamiz.

Tasdiqlash: confirm() funksiyasi o'chirishdan oldin foydalanuvchidan ruxsat so'raydi.

Bo'sh holat: renderTable funksiyasidagi tekshiruv (if (data.length === 0)) jadval bo'sh bo'lganda maxsus xabarni ko'rsatadi.

Ma'lumotlar saqlanishi: saveAndRender funksiyasi har bir o'zgarishdan keyin localStorageni avtomatik yangilaydi.

Ushbu funksionalliklar sizning frontend loyihangiz uchun mustahkam asos bo'la oladi. Qo'shimcha CSS animatsiyalari yoki dizayn bo'yicha savollaringiz bormi?

✅ Menyudan pizza tanlash va savatchaga qo'shish

✅ Savatcha: mahsulot soni, narxi, jami

✅ Miqdorni oshirish/kamaytirish tugmalari

✅ Mahsulotni savatchadan o'chirish

✅ localStorage'da savatchani saqlang

✅ 'Buyurtma berish' tugmasi — tasdiqlash modali

Ushbu pizza savatchasi loyihasi uchun ob'ektlar massividan foydalanish va uni localStorage bilan bog'lash eng maqbul yechimdir. Quyida kerakli funksionalliklar bilan to'liq kodni taqdim etaman.

1. HTML (index.html)
Pizzalar ro'yxati va savatcha uchun joy:

HTML
<div class="menu">
    <!-- Pizza kartochkasi -->
    <div class="pizza-card">
        <h3>Margherita</h3>
        <p>Narxi: 50,000 so'm</p>
        <button onclick="addToCart({id: 1, name: 'Margherita', price: 50000})">Savatchaga qo'shish</button>
    </div>
</div>

<div class="cart">
    <h2>Savatcha</h2>
    <div id="cartItems"></div>
    <p>Jami: <span id="totalPrice">0</span> so'm</p>
    <button onclick="confirmOrder()">Buyurtma berish</button>
</div>

<!-- Buyurtma tasdiqlash modali -->
<div id="orderModal" class="modal" style="display:none;">
    <p>Buyurtmani tasdiqlaysizmi?</p>
    <button onclick="placeOrder()">Ha</button>
    <button onclick="closeModal()">Yo'q</button>
</div>
2. JavaScript (script.js)
Savatcha logikasi (localStorage bilan):

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
        if (item.quantity <= 0) cart = cart.filter(i => i.id !== id);
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
        <div>
            ${item.name} - ${item.price * item.quantity} so'm
            <button onclick="updateQuantity(${item.id}, -1)">-</button>
            <span>${item.quantity}</span>
            <button onclick="updateQuantity(${item.id}, 1)">+</button>
            <button onclick="removeItem(${item.id})">O'chirish</button>
        </div>
    `).join('');

    const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
    totalEl.innerText = total.toLocaleString();
}

// Modallar logikasi
function confirmOrder() { document.getElementById('orderModal').style.display = 'block'; }
function closeModal() { document.getElementById('orderModal').style.display = 'none'; }
function placeOrder() {
    alert("Buyurtma qabul qilindi!");
    cart = [];
    updateCart();
    closeModal();
}

// Sahifa yuklanganda savatchani yangilash
updateCart();
3. CSS (Qisqacha)
CSS
.modal { position: fixed; top: 30%; left: 30%; background: white; padding: 20px; border: 1px solid #ccc; }
.cart { border-top: 2px solid #333; margin-top: 20px; padding: 10px; }
Loyihaning asosiy jihatlari:
Massiv boshqaruvi: cart massivi har bir pizza uchun quantity (soni) xususiyatini saqlaydi.

Savatchani saqlash: Har bir o'zgarishda (qo'shish, o'chirish, miqdorni o'zgartirish) updateCart() funksiyasi ishga tushib, localStorageni yangilab boradi.

Jami narx: reduce() metodi yordamida barcha mahsulotlar narxi va miqdori avtomatik hisoblanadi.

Interfeys: Modal oynasi display xususiyatini block yoki none qilish orqali boshqariladi.

Ushbu kodni o'z loyihangizga moslashtirishingiz uchun qandaydir xususiy dizayn yoki qo'shimcha qulaylik (masalan, pizza hajmi) kerakmi?

✅ Menyudan pizza tanlash va savatchaga qo'shish

✅ Savatcha: mahsulot soni, narxi, jami

✅ Miqdorni oshirish/kamaytirish tugmalari

✅ Mahsulotni savatchadan o'chirish

✅ localStorage'da savatchani saqlang

✅ 'Buyurtma berish' tugmasi — tasdiqlash modali

 Стек технологий

HTML · CSS · JavaScript · localStorage · DOM · Objects

Ushbu pizza savatchasi loyihasi uchun ob'ektlar massividan foydalanish va uni localStorage bilan bog'lash eng maqbul yechimdir. Quyida kerakli funksionalliklar bilan to'liq kodni taqdim etaman.

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
Loyihaning asosiy jihatlari:
Massiv boshqaruvi: cart massivi har bir pizza uchun quantity (soni) xususiyatini saqlaydi. Bu orqali savatchada mahsulotlarni guruhlab ko'rsatamiz.

LocalStorage: updateCart funksiyasi har bir o'zgarishdan so'ng ma'lumotlarni string ko'rinishida localStoragega saqlaydi, bu esa sahifa yangilanganda ham savatcha ma'lumotlari saqlanib qolishini ta'minlaydi.

Jami narx: reduce() metodi orqali barcha mahsulotlarning umumiy narxi real vaqt rejimida hisoblab boriladi.

Foydalanuvchi tajribasi: Buyurtma berishda confirmOrder funksiyasi orqali modal oyna ochiladi, bu tasodifiy bosilishlarning oldini oladi.

Ushbu funksionalliklar sizning loyihangiz uchun to'liq javob beradi. Dizaynni yanada jozibali qilish uchun CSS orqali animatsiyalar qo'shishni xohlaysizmi?

✅ Kamida 5 ta slayd

✅ Oldinga/orqaga o'tish tugmalari

✅ Pastki nuqtalar (dots) navigatsiya

✅ Har 3 soniyada avtomatik o'tish

✅ Foydalanuvchi bosganda avtomatik to'xtasin

✅ Silliq CSS transition animatsiyasi

Bu juda mashhur va foydali vazifa. Quyida HTML, CSS va JavaScript orqali barcha talablarga javob beradigan silliq ishlaydigan slayderning to'liq kodi keltirilgan.

1. HTML (index.html)
Slayder strukturasi:

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Professional Slayder</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <div class="slider-container">
        <!-- Slaydlar -->
        <div class="slide active" style="background-image: url('https://picsum.photos/id/20/1200/500')">
            <div class="slide-content"><h2>Slayd 1</h2><p>Tavsif matni 1</p></div>
        </div>
        <div class="slide" style="background-image: url('https://picsum.photos/id/30/1200/500')">
            <div class="slide-content"><h2>Slayd 2</h2><p>Tavsif matni 2</p></div>
        </div>
        <div class="slide" style="background-image: url('https://picsum.photos/id/40/1200/500')">
            <div class="slide-content"><h2>Slayd 3</h2><p>Tavsif matni 3</p></div>
        </div>
        <div class="slide" style="background-image: url('https://picsum.photos/id/50/1200/500')">
            <div class="slide-content"><h2>Slayd 4</h2><p>Tavsif matni 4</p></div>
        </div>
        <div class="slide" style="background-image: url('https://picsum.photos/id/60/1200/500')">
            <div class="slide-content"><h2>Slayd 5</h2><p>Tavsif matni 5</p></div>
        </div>

        <!-- Navigatsiya tugmalari -->
        <button class="prev" id="prevBtn">&#10094;</button>
        <button class="next" id="nextBtn">&#10095;</button>

        <!-- Pastki nuqtalar -->
        <div class="dots-container">
            <span class="dot active" data-slide="0"></span>
            <span class="dot" data-slide="1"></span>
            <span class="dot" data-slide="2"></span>
            <span class="dot" data-slide="3"></span>
            <span class="dot" data-slide="4"></span>
        </div>
    </div>

    <script src="script.js"></script>
</body>
</html>
2. CSS (style.css)
Silliq o'tish (transition) va joylashtirish:

CSS
* { box-sizing: border-box; margin: 0; padding: 0; }

body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }

.slider-container {
    position: relative;
    width: 100%;
    height: 500px;
    overflow: hidden;
    background: #333;
}

/* Slaydlar ustma-ust tushishi uchun */
.slide {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-size: cover;
    background-position: center;
    opacity: 0;
    /* Asosiy silliq animatsiya */
    transition: opacity 0.8s ease-in-out;
    z-index: 1;
}

.slide.active {
    opacity: 1;
    z-index: 2;
}

/* Slayd ichidagi matn (ixtiyoriy) */
.slide-content {
    position: absolute;
    bottom: 50px;
    left: 50px;
    background-color: rgba(0, 0, 0, 0.5);
    color: white;
    padding: 20px;
    border-radius: 8px;
}

/* Tugmalar */
.prev, .next {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    background-color: rgba(255, 255, 255, 0.3);
    color: white;
    border: none;
    font-size: 24px;
    padding: 15px;
    cursor: pointer;
    transition: background 0.3s;
    z-index: 10;
}

.prev:hover, .next:hover { background-color: rgba(255, 255, 255, 0.6); }
.prev { left: 10px; }
.next { right: 10px; }

/* Pastki nuqtalar */
.dots-container {
    position: absolute;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    gap: 10px;
    z-index: 10;
}

.dot {
    height: 12px;
    width: 12px;
    background-color: rgba(255, 255, 255, 0.4);
    border-radius: 50%;
    display: inline-block;
    cursor: pointer;
    transition: background 0.3s;
}

.dot.active, .dot:hover { background-color: rgba(255, 255, 255, 1); }
3. JavaScript (script.js)
Logika, avtomatik o'tish va hodisalarni boshqarish:

JavaScript
const slides = document.querySelectorAll('.slide');
const dots = document.querySelectorAll('.dot');
const prevBtn = document.getElementById('prevBtn');
const nextBtn = document.getElementById('nextBtn');
const sliderContainer = document.querySelector('.slider-container');

let currentSlide = 0;
const slideInterval = 3000; // 3 soniya
let playSlider;

// Asosiy funksiya: slaydni yangilash
function updateSlide(index) {
    // Eskilarini o'chirish
    slides.forEach(slide => slide.classList.remove('active'));
    dots.forEach(dot => dot.classList.remove('active'));

    // Yangisini yoqish
    currentSlide = (index + slides.length) % slides.length;
    slides[currentSlide].classList.add('active');
    dots[currentSlide].classList.add('active');
}

// Keyingi slayd
function nextSlide() {
    updateSlide(currentSlide + 1);
}

// Oldingi slayd
function prevSlide() {
    updateSlide(currentSlide - 1);
}

// Avtomatik boshlash
function startAutoPlay() {
    playSlider = setInterval(nextSlide, slideInterval);
}

// To'xtatish (foydalanuvchi tekkanda)
function stopAutoPlay() {
    clearInterval(playSlider);
}

// --- Hodisalar (Event Listeners) ---

// Tugmalar bosilganda
nextBtn.addEventListener('click', () => {
    nextSlide();
    resetAutoPlay(); // Bosilgandan keyin yana 3 soniya kutsin
});

prevBtn.addEventListener('click', () => {
    prevSlide();
    resetAutoPlay();
});

// Nuqtalar (dots) bosilganda
dots.forEach((dot, index) => {
    dot.addEventListener('click', () => {
        updateSlide(index);
        resetAutoPlay();
    });
});

// Sichqoncha slayd ustiga kelganda to'xtatish, chiqganda davom etish
sliderContainer.addEventListener('mouseover', stopAutoPlay);
sliderContainer.addEventListener('mouseout', startAutoPlay);

// Qayta ishga tushirish yordamchi funksiyasi
function resetAutoPlay() {
    stopAutoPlay();
    startAutoPlay();
}

// Boshlang'ich holat
startAutoPlay();
Qanday ishlashi haqida qisqacha tushuntirish:
CSS opacity va transition: Bu eng muhim qism. Barcha slaydlar position: absolute orqali bir-birining ustiga qo'yilgan. opacity: 0 (ko'rinmas) qilingan. Faqat .active klassi bor slayd opacity: 1ga ega. CSS transition: opacity 0.8s xususiyati esa 0.8 soniya davomida biridan ikkinchisiga silliq o'tishni ta'minlaydi.

JavaScript currentSlide indeks: JavaScript qaysi slayd hozir faol ekanligini indeks (0 dan 4 gacha) orqali biladi.

Navigatsiya (next/prev): Tugmalar bosilganda joriy indeks o'zgaradi, eski slaydning .active klassi olib tashlanadi va yangisiga qo'shiladi.

Avtomatik o'tish (setInterval): startAutoPlay funksiyasi har 3 soniyada nextSlide()ni avtomatik chaqiradi.

Foydalanuvchi bosganda to'xtatish (mouseover): sliderContainerga sichqoncha kirganda clearInterval ishlaydi. Sichqoncha chiqib ketganda esa setInterval qaytadan ishga tushadi. Bu esa slaydni o'qiyotganda to'xtab turishini ta'minlaydi.

 Требования

✅ Play / Pause tugmasi

✅ Oldinga / orqaga o'tish

✅ Progress bar (bosib o'tish imkoni)

✅ Volume boshqaruvi

✅ Qo'shiq nomi va rasmi ko'rinsin

✅ Shuffle va Repeat rejimlari

 Стек технологий

HTML · CSS · JavaScript · Audio API · DOM · Events

Ushbu loyiha uchun HTML5 Audio API va JavaScript ob'ektlaridan foydalanib, to'liq funksional musiqa pleyerini yaratamiz. Quyida pleyerning tuzilishi va logikasi keltirilgan.

1. HTML (index.html)
HTML
<div class="player">
    <img id="cover" src="default.jpg" alt="Cover">
    <h2 id="title">Qo'shiq Nomi</h2>
    
    <audio id="audio"></audio>

    <div class="progress-container" id="progressContainer">
        <div class="progress" id="progress"></div>
    </div>

    <div class="controls">
        <button id="prevBtn">⏮</button>
        <button id="playBtn">▶️</button>
        <button id="nextBtn">⏭</button>
        <button id="shuffleBtn">🔀</button>
        <button id="repeatBtn">🔁</button>
    </div>

    <input type="range" id="volumeSlider" min="0" max="1" step="0.1" value="1">
</div>
2. CSS (style.css)
CSS
.player { width: 300px; padding: 20px; border: 1px solid #ccc; text-align: center; }
.progress-container { width: 100%; height: 10px; background: #eee; cursor: pointer; margin: 10px 0; }
.progress { width: 0%; height: 100%; background: #007bff; }
#cover { width: 100%; height: 200px; object-fit: cover; }
3. JavaScript (script.js)
JavaScript
const audio = document.getElementById('audio');
const playBtn = document.getElementById('playBtn');
const progress = document.getElementById('progress');
const progressContainer = document.getElementById('progressContainer');
const volumeSlider = document.getElementById('volumeSlider');

let songs = [
    { title: "Song 1", src: "song1.mp3", cover: "cover1.jpg" },
    { title: "Song 2", src: "song2.mp3", cover: "cover2.jpg" }
];
let index = 0;
let isShuffle = false;
let isRepeat = false;

// Play/Pause
playBtn.addEventListener('click', () => {
    if (audio.paused) {
        audio.play();
        playBtn.innerText = "⏸";
    } else {
        audio.pause();
        playBtn.innerText = "▶️";
    }
});

// Progress Bar
audio.addEventListener('timeupdate', () => {
    const percent = (audio.currentTime / audio.duration) * 100;
    progress.style.width = percent + "%";
});

progressContainer.addEventListener('click', (e) => {
    const width = progressContainer.clientWidth;
    const clickX = e.offsetX;
    audio.currentTime = (clickX / width) * audio.duration;
});

// Volume
volumeSlider.addEventListener('input', (e) => {
    audio.volume = e.target.value;
});

// Navigatsiya
function loadSong(song) {
    audio.src = song.src;
    document.getElementById('title').innerText = song.title;
    document.getElementById('cover').src = song.cover;
}

document.getElementById('nextBtn').addEventListener('click', () => {
    index = isShuffle ? Math.floor(Math.random() * songs.length) : (index + 1) % songs.length;
    loadSong(songs[index]);
    audio.play();
});

// Repeat
audio.addEventListener('ended', () => {
    if (isRepeat) {
        audio.play();
    } else {
        document.getElementById('nextBtn').click();
    }
});

// Shuffle/Repeat toggles
document.getElementById('shuffleBtn').onclick = () => isShuffle = !isShuffle;
document.getElementById('repeatBtn').onclick = () => isRepeat = !isRepeat;

loadSong(songs[index]);
Asosiy funksiyalar izohi:
Audio API: audio.play(), audio.pause(), audio.currentTime kabi xususiyatlar musiqani boshqarishning asosiy vositasidir.

Progress Bar: timeupdate hodisasi musiqaning har bir soniyasida ishlaydi va progress elementining kengligini yangilab boradi.

Shuffle/Repeat: Boolean (true/false) o'zgaruvchilar orqali tizim rejimlari saqlanadi.

Volume Control: input tipidagi range element orqali audio.volume (0 dan 1 gacha) qiymati o'zgartiriladi.

Ushbu kod sizning musiqiy loyihangiz uchun asosiy "skelet" vazifasini bajaradi. Dizaynni yanada yaxshilash yoki musiqalarni localStorage orqali saqlash bo'yicha yordam kerakmi?

✅ Sidebar navigatsiya (sahifalar o'rtasida o'tish)

✅ Dashboard sahifasi: statistika kartochkalari

✅ Foydalanuvchilar sahifasi: to'liq CRUD jadval

✅ Mahsulotlar sahifasi: filtrlash va qidirув

✅ Barcha ma'lumotlar localStorage'da

✅ Mobil responsive dizayn

✅ Dark/Light rejim almashtirgich

 Стек технологий

HTML · CSS · JavaScript · DOM · localStorage · CSS Grid · Flexbox

Ushbu loyihani amalga oshirish uchun eng yaxshi yondashuv — SPA (Single Page Application) tamoyilidan foydalangan holda DOM elementlarini yashirish va ko'rsatishdir. Bu sahifalarni qayta yuklamasdan o'tish imkonini beradi.

