<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WAI-ARIA Standartlari bilan HTML Namunasi</title>
    <style>
        body { font-family: sans-serif; padding: 20px; line-height: 1.6; }
        .accordion-content { display: none; padding: 10px; border: 1px solid #ddd; }
        .modal { display: none; position: fixed; top: 30%; left: 50%; transform: translate(-50%, -50%); border: 1px solid #000; padding: 20px; background: #fff; z-index: 1000; }
        .active { display: block; }
        .notification { color: green; font-weight: bold; margin-top: 10px; }
    </style>
</head>
<body>

    <h1>WAI-ARIA Amaliy Namunasi</h1>
    <hr>

    <!-- 1. ACCORDION (Akkordeon) -->
    <h2>1. Akkordeon Qismi</h2>
    <!-- aria-controls va id bir-biriga mos kelishi shart -->
    <button id="acc-button" aria-expanded="false" aria-controls="acc-content">
        Batafsil ma'lumotni ochish
    </button>
    <!-- Yopiq turganda aria-hidden="true" bo'ladi -->
    <div id="acc-content" class="accordion-content" aria-hidden="true">
        <p>Bu yerda ekran o'quvchi dasturlar faqat tugma ochilgandagina o'qiydigan muhim matn bor.</p>
    </div>

    <hr>

    <!-- 2 & 4. MODAL OYNA VA YOPISH TUGMASI -->
    <h2>2. Modal Oyna</h2>
    <button id="open-modal-btn">Modal oynani ochish</button>

    <!-- role="dialog" va aria-modal="true" brauzerga bu alohida oyna ekanini bildiradi -->
    <div id="my-modal" class="modal" role="dialog" aria-modal="true" aria-labelledby="modal-title" aria-describedby="modal-desc">
        <!-- 4. Yopish tugmasiga aria-label berilgan -->
        <button id="close-modal-btn" aria-label="Yopish">&times;</button>
        
        <h3 id="modal-title">Tizim bildirishnomasi</h3>
        <p id="modal-desc">Amalni davom ettirishdan oldin shartlar bilan tanishib chiqing.</p>
    </div>

    <hr>

    <!-- 3. TUGMALAR (ARIA-LABEL BILAN) -->
    <h2>3. Ikonkali Tugma</h2>
    <p>Quyidagi tugma ichida matn yo'q (faqat belgi), lekin ko'zi ojizlar uchun u "Savat" deb o'qiladi:</p>
    <button id="cart-btn" aria-label="Savatga qo'shish">🛒</button>

    <hr>

    <!-- 5. DINAMIK O'ZGARISHLAR (ARIA-LIVE) -->
    <h2>5. Dinamik Bildirishnoma</h2>
    <p>Tugma bosilganda sahifa yangilanmasdan matn o'zgaradi va u foydalanuvchiga ovozli eshittiriladi.</p>
    <button id="action-btn">Savatni yangilash</button>
    
    <!-- aria-live="polite" ichidagi o'zgarishlarni darhol ekran o'quvchiga yetkazadi -->
    <div id="live-zone" aria-live="polite" class="notification"></div>


    <!-- MATNLARNI DINAMIK BOSHQARISH UCHUN JAVASCRIPT -->
    <script>
        // 1. Akkordeon logikasi
        const accBtn = document.getElementById('acc-button');
        const accContent = document.getElementById('acc-content');
        
        accBtn.addEventListener('click', () => {
            const isExpanded = accBtn.getAttribute('aria-expanded') === 'true';
            accBtn.setAttribute('aria-expanded', !isExpanded);
            accContent.setAttribute('aria-hidden', isExpanded);
            accContent.classList.toggle('active');
        });

        // 2. Modal oynani ochish/yopish logikasi
        const modal = document.getElementById('my-modal');
        document.getElementById('open-modal-btn').addEventListener('click', () => {
            modal.classList.add('active');
            document.getElementById('close-modal-btn').focus(); // Fokusni modal ichiga olib kirish
        });
        document.getElementById('close-modal-btn').addEventListener('click', () => {
            modal.classList.remove('active');
        });

        // 5. Dinamik aria-live logikasi
        document.getElementById('action-btn').addEventListener('click', () => {
            document.getElementById('live-zone').innerText = "Savat muvaffaqiyatli yangilandi (1 ta mahsulot qo'shildi).";
        });
    </script>
</body>
</html>
