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
