<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>WCAG Vizual va Harakat Qoidalari</title>
  <style>
    /* Boshlang'ich o'zgaruvchilar (Ranglar to'g'ri kontrast uchun tanlangan) */
    :root {
      --bg-color: #ffffff;
      --text-color: #212529; /* Oddiy matn uchun kontrast 15.1:1 (4.5:1 dan ancha yuqori) */
      --text-large-color: #495057; /* Katta matn uchun kontrast 8.1:1 (3:1 dan yuqori) */
      --error-color: #dc3545;
      --success-color: #198754;
      --focus-outline: #0d6efd;
    }

    body {
      font-family: system-ui, -apple-system, sans-serif;
      background-color: var(--bg-color);
      color: var(--text-color);
      line-height: 1.6;
      padding: 2rem;
      max-width: 600px;
      margin: 0 auto;
    }

    /* ---------------------------------------------------- */
    /* 1-QOIDA: Oddiy matn kontrasti (16px, 4.5:1 dan yuqori) */
    /* ---------------------------------------------------- */
    .normal-text {
      font-size: 16px;
      color: var(--text-color);
    }

    /* ---------------------------------------------------- */
    /* 2-QOIDA: Katta matn kontrasti (24px, 3:1 dan yuqori)  */
    /* ---------------------------------------------------- */
    .large-text {
      font-size: 24px;
      font-weight: bold;
      color: var(--text-large-color);
      margin-top: 1.5rem;
    }

    /* ---------------------------------------------------- */
    /* 3-QOIDA: Rangni yagona ma'lumot manbai qilmaslik    */
    /* ---------------------------------------------------- */
    .notification {
      padding: 1rem;
      margin: 1rem 0;
      border-radius: 4px;
      display: flex;
      align-items: center;
      gap: 10px;
    }
    
    /* Xatolik statusi: Faqat qizil rang emas, ikona va qalin matn ham bor */
    .notification-error {
      background-color: #f8d7da;
      color: var(--error-color);
      border: 2px solid var(--error-color);
    }
    .status-icon {
      font-weight: bold;
      font-size: 1.2rem;
    }

    /* ---------------------------------------------------- */
    /* 4-QOIDA: Barcha interaktiv elementlarda fokus        */
    /* ---------------------------------------------------- */
    .btn, .input-field {
      padding: 0.5rem 1rem;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    
    .btn {
      background-color: #f8f9fa;
      cursor: pointer;
    }

    /* Istalgan interaktiv elementga klaviatura kelganda aniq ko'rinishi shart */
    .btn:focus-visible, 
    .input-field:focus-visible {
      outline: 3px solid var(--focus-outline);
      outline-offset: 2px;
    }

    /* ---------------------------------------------------- */
    /* 5-QOIDA: prefers-reduced-motion media query          */
    /* ---------------------------------------------------- */
    /* Standart holatda chiroyli animatsiya */
    .animated-box {
      width: 100px;
      height: 100px;
      background-color: #6f42c1;
      margin-top: 2rem;
      transition: transform 0.6s ease-in-out;
    }
    .animated-box:hover {
      transform: scale(1.2) rotate(45deg);
    }

    /* Foydalanuvchi tizimda (Windows/Mac/Android) animatsiyalarni o'chirgan bo'lsa */
    @media (prefers-reduced-motion: reduce) {
      .animated-box {
        transition: none; /* Animatsiya vaqtini butunlay o'chiramiz */
      }
      .animated-box:hover {
        transform: none; /* Keskin o'zgarishlar (kattalashish/aylanish) bo'lmaydi */
      }
      
      /* Agar saytda boshqa umumiy animatsiyalar bo'lsa, ularni ham sekinlashtiramiz */
      *, *::before, *::after {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
        scroll-behavior: auto !important;
      }
    }
  </style>
</head>
<body>

  <header>
    <h1>A11y Dizayn Standartlari</h1>
  </header>

  <main>
    <!-- 1 va 2-qoidalar: Kontrast -->
    <p class="normal-text">Bu oddiy matn (16px). Uning rangi to'q kulrang bo'lib, oq fonda kamida 4.5:1 kontrast nisbatini to'liq ta'minlaydi. Odamlar qiynalmay o'qiy olishadi.</p>
    
    <h2 class="large-text">Bu katta sarlavha matni (24px)</h2>
    <p class="normal-text">Matn o'lchami katta bo'lgani uchun uning kontrast nisbati biroz yumshoqroq (kamida 3:1) bo'lishi kifoya.</p>

    <hr>

    <!-- 3-qoida: Rangdan tashqari boshqa signal -->
    <div class="notification notification-error" role="alert">
      <!-- Diqqat: Rang ko'ra olmaydigan odam uchun [XATO] so'zi va ikona yordam beradi -->
      <span class="status-icon" aria-hidden="true">⚠</span>
      <span><strong>Xatolik:</strong> Tizimga kirishda xatolik yuz berdi. Iltimos, qaytadan urinib ko'ring.</span>
    </div>

    <hr>

    <!-- 4-qoida: Fokus indikatori -->
    <div style="margin: 1rem 0; display: flex; gap: 10px;">
      <input type="text" class="input-field" placeholder="Ismingiz...">
      <button class="btn">Yuborish</button>
    </div>

    <hr>

    <!-- 5-qoida: Animatsiyani cheklash -->
    <p class="normal-text">Sichqonchani quyidagi qutining ustiga olib keling (Agar kompyuteringizda "Reduce Motion" yoniq bo'lsa, u aylanmaydi va kattalashmaydi):</p>
    <div class="animated-box" aria-label="Animatsiyali quti"></div>
  </main>

</body>
</html>
