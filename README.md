<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>E-Commerce Mahsulot Sahifasi</title>
    <style>
        /* 6. CSS Custom Properties (Variables) */
        :root {
            --primary: #0f172a;
            --primary-hover: #1e293b;
            --accent: #d97706;
            --accent-hover: #b45309;
            --text-main: #1e293b;
            --text-muted: #64748b;
            --bg-light: #f8fafc;
            --white: #ffffff;
            --border: #e2e8f0;
            --radius-lg: 16px;
            --radius-md: 8px;
            --shadow: 0 4px 6px -1px rgb(0 0 0 / 0.05), 0 2px 4px -2px rgb(0 0 0 / 0.05);
            --shadow-hover: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
            --transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            background-color: var(--bg-light);
            color: var(--text-main);
            line-height: 1.5;
            padding: 40px 20px;
        }

        .container {
            max-width: 1100px;
            margin: 0 auto;
            background-color: var(--white);
            padding: 40px;
            border-radius: var(--radius-lg);
            box-shadow: var(--shadow);
        }

        /* 6. Grid va Flexbox bilan 2 ustunli layout */
        .product-grid {
            display: grid;
            grid-template-columns: 1.1fr 0.9fr;
            gap: 50px;
            align-items: start;
        }

        /* Left Column: 1. Rasm Galereyasi */
        .gallery-container {
            position: sticky;
            top: 40px; /* Sticky effekti */
        }

        .main-image-wrapper {
            width: 100%;
            height: 480px;
            border-radius: var(--radius-lg);
            overflow: hidden;
            background-color: #f1f5f9;
            border: 1px solid var(--border);
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 16px;
        }

        .main-image-wrapper img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: var(--transition);
        }

        .thumbnails-grid {
            display: flex;
            gap: 12px;
        }

        /* 7. Hover effekti thumbnaillarda */
        .thumbnail {
            width: 80px;
            height: 80px;
            border-radius: var(--radius-md);
            overflow: hidden;
            border: 2px solid transparent;
            cursor: pointer;
            background-color: #f1f5f9;
            transition: var(--transition);
        }

        .thumbnail img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .thumbnail:hover {
            transform: scale(1.05);
            border-color: var(--primary);
        }

        .thumbnail.active {
            border-color: var(--primary);
        }

        /* Right Column: Mahsulot ma'lumotlari */
        .product-info {
            display: flex;
            flex-direction: column;
            gap: 24px;
        }

        /* 2. Mahsulot nomi, reyting, narx */
        .product-header h1 {
            font-size: 2rem;
            font-weight: 800;
            color: var(--primary);
            margin-bottom: 10px;
        }

        .rating-price-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border);
            padding-bottom: 20px;
        }

        .rating {
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .stars {
            color: var(--accent);
            font-size: 1.2rem;
            letter-spacing: 2px;
        }

        .review-count {
            font-size: 0.85rem;
            color: var(--text-muted);
        }

        .price {
            font-size: 1.75rem;
            font-weight: 800;
            color: var(--primary);
        }

        /* 3. Rang va o'lcham tanlash (Swatches) */
        .option-group {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .option-title {
            font-size: 0.9rem;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.05em;
            color: var(--text-muted);
        }

        /* Color Swatches */
        .color-swatches {
            display: flex;
            gap: 12px;
        }

        .color-bubble {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            border: 2px solid var(--white);
            outline: 2px solid var(--border);
            cursor: pointer;
            transition: var(--transition);
            position: relative;
        }

        .color-bubble:hover {
            transform: scale(1.1);
        }

        .color-bubble.active {
            outline-color: var(--primary);
            transform: scale(1.05);
        }

        /* Size Swatches */
        .size-swatches {
            display: flex;
            gap: 10px;
        }

        .size-box {
            min-width: 45px;
            height: 45px;
            border: 1px solid var(--border);
            border-radius: var(--radius-md);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.9rem;
            font-weight: 600;
            cursor: pointer;
            transition: var(--transition);
            background-color: var(--white);
        }

        .size-box:hover {
            border-color: var(--primary);
            background-color: var(--bg-light);
        }

        .size-box.active {
            background-color: var(--primary);
            color: var(--white);
            border-color: var(--primary);
        }

        /* 4. Miqdor tanlash va Savatga qo'shish */
        .action-row {
            display: flex;
            gap: 16px;
            align-items: center;
            margin-top: 10px;
        }

        .quantity-selector {
            display: flex;
            align-items: center;
            border: 1px solid var(--border);
            border-radius: 30px;
            height: 52px;
            background-color: var(--white);
            overflow: hidden;
        }

        .qty-btn {
            background: none;
            border: none;
            width: 45px;
            height: 100%;
            font-size: 1.2rem;
            cursor: pointer;
            transition: var(--transition);
            color: var(--text-main);
        }

        .qty-btn:hover {
            background-color: var(--bg-light);
        }

        .qty-input {
            width: 40px;
            border: none;
            text-align: center;
            font-size: 1.05rem;
            font-weight: 700;
            outline: none;
            background: none;
        }

        /* 7. Hover effekti amallar tugmasida */
        .add-to-cart-btn {
            flex-grow: 1;
            height: 52px;
            background-color: var(--primary);
            color: var(--white);
            border: none;
            border-radius: 30px;
            font-size: 1.05rem;
            font-weight: 700;
            cursor: pointer;
            transition: var(--transition);
            box-shadow: 0 4px 12px rgba(15, 23, 42, 0.15);
        }

        .add-to-cart-btn:hover {
            background-color: var(--primary-hover);
            transform: translateY(-2px);
            box-shadow: 0 8px 16px rgba(15, 23, 42, 0.25);
        }

        .add-to-cart-btn:active {
            transform: translateY(0);
        }

        /* 5. Mahsulot tavsifi va xususiyatlari */
        .product-description {
            border-top: 1px solid var(--border);
            padding-top: 24px;
        }

        .product-description h3 {
            font-size: 1.1rem;
            font-weight: 700;
            margin-bottom: 12px;
            color: var(--primary);
        }

        .product-description p {
            color: var(--text-muted);
            font-size: 0.95rem;
            margin-bottom: 20px;
        }

        .specs-list {
            list-style: none;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .specs-list li {
            font-size: 0.9rem;
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            border-bottom: 1px dashed var(--border);
        }

        .specs-list li span:first-child {
            color: var(--text-muted);
            font-weight: 500;
        }

        .specs-list li span:last-child {
            color: var(--primary);
            font-weight: 600;
        }

        /* 6. Responsive - Mobil versiya */
        @media (max-width: 868px) {
            body {
                padding: 15px 10px;
            }

            .container {
                padding: 24px;
            }

            .product-grid {
                grid-template-columns: 1fr;
                gap: 30px;
            }

            .gallery-container {
                position: relative;
                top: 0;
            }

            .main-image-wrapper {
                height: 350px;
            }

            .product-header h1 {
                font-size: 1.6rem;
            }
        }
    </style>
</head>
<body>

    <div class="container">
        <div class="product-grid">
            
            <!-- CHAP USTUN: Rasm galereyasi -->
            <div class="gallery-container">
                <div class="main-image-wrapper">
                    <!-- Asosiy rasm boshida birinchi rasm url bilan yuklanadi -->
                    <img id="main-product-img" src="https://encrypted-tbn1.gstatic.com/licensed-image?q=tbn:ANd9GcSFP7QAVbz-oaexZSM2RfrPqkSb0gLD2FaHyz7KBiSxkx2nINQZU4FgkYU3yAiQa1fAJ_OEoSTzq80rEg0" alt="Minimalist teri rasm">
                </div>
                
                <!-- 1. Thumbnails -->
                <div class="thumbnails-grid">
                    <div class="thumbnail active" onclick="changeImage(this, 'https://encrypted-tbn1.gstatic.com/licensed-image?q=tbn:ANd9GcSFP7QAVbz-oaexZSM2RfrPqkSb0gLD2FaHyz7KBiSxkx2nINQZU4FgkYU3yAiQa1fAJ_OEoSTzq80rEg0')">
                        <img src="https://encrypted-tbn1.gstatic.com/licensed-image?q=tbn:ANd9GcSFP7QAVbz-oaexZSM2RfrPqkSb0gLD2FaHyz7KBiSxkx2nINQZU4FgkYU3yAiQa1fAJ_OEoSTzq80rEg0" alt="Ko'rinish 1">
                    </div>
                    <div class="thumbnail" onclick="changeImage(this, 'https://encrypted-tbn3.gstatic.com/licensed-image?q=tbn:ANd9GcTRzc-hHcj9PcdmQQdNG4Uf_fjCf-wFBg9D4v9eyVtDKI17bt3bfUVqOrvJmm0un1q7nTO0fgXzRsETzWA')">
                        <img src="https://encrypted-tbn3.gstatic.com/licensed-image?q=tbn:ANd9GcTRzc-hHcj9PcdmQQdNG4Uf_fjCf-wFBg9D4v9eyVtDKI17bt3bfUVqOrvJmm0un1q7nTO0fgXzRsETzWA" alt="Ko'rinish 2">
                    </div>
                </div>
            </div>

            <!-- O'NG USTUN: Mahsulot ma'lumotlari -->
            <div class="product-info">
                
                <!-- 2. Mahsulot sarlavhasi va narxi -->
                <div class="product-header">
                    <h1>Nomad Minimalist Teri Ryukzagi</h1>
                    <div class="rating-price-row">
                        <div class="rating">
                            <span class="stars">★★★★☆</span>
                            <span class="review-count">(124 ta sharh)</span>
                        </div>
                        <div class="price">$149.00</div>
                    </div>
                </div>

                <!-- 3. Rang tanlash -->
                <div class="option-group">
                    <span class="option-title">Rang:</span>
                    <div class="color-swatches">
                        <div class="color-bubble active" style="background-color: #b45309;" title="Jigarrang" onclick="selectColor(this)"></div>
                        <div class="color-bubble" style="background-color: #1e293b;" title="Qora" onclick="selectColor(this)"></div>
                        <div class="color-bubble" style="background-color: #475569;" title="Kulrang" onclick="selectColor(this)"></div>
                    </div>
                </div>

                <!-- 3. O'lcham tanlash -->
                <div class="option-group">
                    <span class="option-title">O'lcham (Hajm):</span>
                    <div class="size-swatches">
                        <div class="size-box" onclick="selectSize(this)">15L</div>
                        <div class="size-box active" onclick="selectSize(this)">20L</div>
                        <div class="size-box" onclick="selectSize(this)">25L</div>
                    </div>
                </div>

                <!-- 4. Miqdor va Savatga qo'shish -->
                <div class="action-row">
                    <div class="quantity-selector">
                        <button class="qty-btn" onclick="updateQty(-1)">−</button>
                        <input type="text" class="qty-input" id="quantity" value="1" readonly>
                        <button class="qty-btn" onclick="updateQty(1)">+</button>
                    </div>
                    <button class="add-to-cart-btn" onclick="addToCart()">Savatga qo'shish</button>
                </div>

                <!-- 5. Tavsif va Xususiyatlar -->
                <div class="product-description">
                    <h3>Mahsulot Haqida</h3>
                    <p>Tabiiy charmdan tayyorlangan va har qanday sharoitga mos tushadigan minimalist "Nomad" ryukzagi. Kundalik sayohatlar, ish uchrashuvlari va noutbukni xavfsiz tashish uchun mukammal yechim.</p>
                    
                    <ul class="specs-list">
                        <li>
                            <span>Material</span>
                            <span>100% Premium Teri</span>
                        </li>
                        <li>
                            <span>Noutbuk bo'limi</span>
                            <span>Bor (15.6 dyuymgacha)</span>
                        </li>
                        <li>
                            <span>Suv o'tkazmasligi</span>
                            <span>Yengil namlikka chidamli</span>
                        </li>
                        <li>
                            <span>Kafolat</span>
                            <span>1 yil</span>
                        </li>
                    </ul>
                </div>

            </div>
        </div>
    </div>

    <!-- JavaScript Interaktivlik -->
    <script>
        // 1. Rasm almashtirish funksiyasi
        function changeImage(element, imageUrl) {
            const mainImg = document.getElementById('main-product-img');
            mainImg.src = imageUrl;
            
            // Faol (active) klassni yangilash
            document.querySelectorAll('.thumbnail').forEach(thumb => {
                thumb.classList.remove('active');
            });
            element.classList.add('active');
        }

        // 3. Rang tanlash
        function selectColor(element) {
            document.querySelectorAll('.color-bubble').forEach(bubble => {
                bubble.classList.remove('active');
            });
            element.classList.add('active');
        }

        // 3. O'lcham tanlash
        function selectSize(element) {
            document.querySelectorAll('.size-box').forEach(box => {
                box.classList.remove('active');
            });
            element.classList.add('active');
        }

        // 4. Miqdor tanlash (+/-)
        function updateQty(change) {
            const qtyInput = document.getElementById('quantity');
            let currentVal = parseInt(qtyInput.value);
            
            currentVal += change;
            if (currentVal < 1) {
                currentVal = 1; // Miqdor 1 dan kamayib ketmasligi kerak
            }
            qtyInput.value = currentVal;
        }

        // Savatga qo'shish tugmasi hodisasi
        function addToCart() {
            const qty = document.getElementById('quantity').value;
            const size = document.querySelector('.size-box.active').innerText;
            alert(`Mahsulot savatga qo'shildi!\nSoni: ${qty} ta\nO'lchami: ${size}`);
        }
    </script>
</body>
</html>
