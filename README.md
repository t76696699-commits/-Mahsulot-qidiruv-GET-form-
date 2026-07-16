
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

