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
