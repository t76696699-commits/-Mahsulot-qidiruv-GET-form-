<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>A11y Standartlari Namunasi</title>
  <style>
    /* ---------------------------------------------------- */
    /* 4-QOIDA: Skip Link (Sahifa boshida yashirin havola)   */
    /* ---------------------------------------------------- */
    .skip-link {
      position: absolute;
      top: -100px;
      left: 10px;
      background: #3b82f6;
      color: white;
      padding: 10px 20px;
      z-index: 100;
      text-decoration: none;
      font-weight: bold;
      border-radius: 4px;
      transition: top 0.2s ease;
    }
    /* Fokus tushgandagina ekranda ko'rinadi */
    .skip-link:focus-visible {
      top: 10px;
    }

    /* ---------------------------------------------------- */
    /* 3-QOIDA: :focus-visible (Aniq ko'rinadigan fokus)    */
    /* ---------------------------------------------------- */
    button:focus-visible, 
    a:focus-visible, 
    [tabindex="0"]:focus-visible {
      outline: 3px solid #ff5722; /* Aniq ko'rinadigan to'q sariq chiziq */
      outline-offset: 3px;
    }
    /* Sichqoncha bilan bosilganda noqulay ko'rinmasligi uchun eski outline'ni o'chiramiz */
    button:focus, a:focus {
      outline: none;
    }

    /* Umumiy uslublar */
    body { font-family: sans-serif; margin: 0; padding: 20px; background: #f4f4f9; color: #333; }
    header { background: white; padding: 15px; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); margin-bottom: 20px; }
    
    /* Menyu uslubi */
    .accessible-menu { display: flex; gap: 10px; list-style: none; padding: 0; margin: 10px 0 0 0; }
    .menu-item { background: #e0e0e0; padding: 8px 16px; border-radius: 4px; cursor: pointer; }

    /* Modal uslubi */
    .modal-overlay {
      position: fixed; top: 0; left: 0; width: 100%; height: 100%;
      background: rgba(0,0,0,0.5); display: flex; justify-content: center; align-items: center;
      opacity: 0; pointer-events: none; transition: opacity 0.3s ease;
    }
    .modal-overlay.open { opacity: 1; pointer-events: auto; }
    .modal-box {
      background: white; padding: 30px; border-radius: 8px; width: 400px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.2); position: relative;
    }
    .close-btn { position: absolute; top: 10px; right: 10px; padding: 5px 10px; }
    .modal-actions { display: flex; justify-content: flex-end; gap: 10px; margin-top: 20px; }
  </style>
</head>
<body>

  <!-- 4. Skip Link element -->
  <a href="#main-content" class="skip-link">Asosiy mazmunga o'tish</a>

  <header>
    <nav aria-label="Asosiy navigatsiya">
      <h2>Menyu (Strelkalar orqali boshqariladi)</h2>
      <!-- 2. Strelkalar bilan boshqariladigan menyu -->
      <ul class="accessible-menu" role="menubar">
        <li class="menu-item" role="menuitem" tabindex="0">Bosh sahifa</li>
        <li class="menu-item" role="menuitem" tabindex="-1">Xizmatlar</li>
        <li class="menu-item" role="menuitem" tabindex="-1">Biz haqimizda</li>
        <li class="menu-item" role="menuitem" tabindex="-1">Aloqa</li>
      </ul>
    </nav>
  </header>

  <!-- Skip link aynan shu yerga olib keladi -->
  <main id="main-content">
    <h1>Veb sahifa mazmuni</h1>
    <p>Klaviaturadagi <b>Tab</b> tugmasini bosib elementlar bo'ylab harakatlanib ko'ring.</p>
    
    <!-- 1. Modalni ochuvchi tugma -->
    <button id="openModalBtn">Modal oynani ochish</button>
  </main>

  <!-- 5. Modal Oyna (Fokus qopqoni shu ichida ishlaydi) -->
  <div class="modal-overlay" id="modalOverlay" role="dialog" aria-modal="true" aria-labelledby="modalTitle" hidden>
    <div class="modal-box" id="modalBox">
      <h2 id="modalTitle">Oyna ochildi</h2>
      <p>Bu yerda fokus qamalgan (Focus Trap). Tab bosilganda fokus tashqariga chiqib ketmaydi.</p>
      <p>Yopish uchun <b>Escape</b> tugmasini bosing.</p>
      
      <input type="text" placeholder="Ismingizni kiriting...">
      
      <div class="modal-actions">
        <button class="close-btn" id="closeModalBtn" aria-label="Yopish">X</button>
        <button id="saveBtn">Saqlash</button>
      </div>
    </div>
  </div>

  <script>
    // ====================================================
    // 2-QOIDA: Menyu ichida strelkalar bilan harakatlanish
    // ====================================================
    const menuItems = document.querySelectorAll('.menu-item');
    let currentMenuIndex = 0;

    menuItems.forEach((item, index) => {
      item.addEventListener('keydown', (e) => {
        if (e.key === 'ArrowRight' || e.key === 'ArrowDown') {
          e.preventDefault();
          currentMenuIndex = (index + 1) % menuItems.length;
          menuItems[currentMenuIndex].focus();
        } else if (e.key === 'ArrowLeft' || e.key === 'ArrowUp') {
          e.preventDefault();
          currentMenuIndex = (index - 1 + menuItems.length) % menuItems.length;
          menuItems[currentMenuIndex].focus();
        }
        // Faqat fokuslangan element klaviatura navigatsiyasida faol bo'ladi
        menuItems.forEach((mi, i) => mi.setAttribute('tabindex', i === currentMenuIndex ? '0' : '-1'));
      });
      
      // Sichqoncha bilan bosilganda ham indeks yangilanishi uchun
      item.addEventListener('click', () => {
        currentMenuIndex = index;
        menuItems.forEach((mi, i) => mi.setAttribute('tabindex', i === currentMenuIndex ? '0' : '-1'));
      });
    });

    // ====================================================
    // 1 & 5-QOIDA: Modal, Escape va Fokus Qopqoni (Focus Trap)
    // ====================================================
    const openModalBtn = document.getElementById('openModalBtn');
    const closeModalBtn = document.getElementById('closeModalBtn');
    const saveBtn = document.getElementById('saveBtn');
    const modalOverlay = document.getElementById('modalOverlay');
    let lastFocusedElement;

    function openModal() {
      lastFocusedElement = document.activeElement; // Modal ochilishidan oldingi fokusni saqlab qolamiz
      modalOverlay.removeAttribute('hidden');
      modalOverlay.classList.add('open');
      
      // Fokusni modal ichidagi birinchi elementga yoki yopish tugmasiga beramiz
      closeModalBtn.focus(); 
      document.addEventListener('keydown', handleModalKeydown);
    }

    function closeModal() {
      modalOverlay.setAttribute('hidden', 'true');
      modalOverlay.classList.remove('open');
      document.remove('keydown', handleModalKeydown);
      
      // Foydalanuvchi chalg'imasligi uchun fokusni eski joyiga qaytaramiz
      if (lastFocusedElement) lastFocusedElement.focus();
    }

    function handleModalKeydown(e) {
      // 1. Escape orqali yopish
      if (e.key === 'Escape') {
        closeModal();
        return;
      }

      // 5. Fokus qopqoni (Focus Trap) logikasi
      if (e.key === 'Tab') {
        // Modal ichidagi fokuslana oladigan barcha elementlarni yig'amiz
        const focusableElements = modalOverlay.querySelectorAll('button, input, [tabindex="0"]');
        const firstElement = focusableElements[0];
        const lastElement = focusableElements[focusableElements.length - 1];

        if (e.shiftKey) { // Shift + Tab orqaga harakat
          if (document.activeElement === firstElement) {
            e.preventDefault();
            lastElement.focus(); // Birinchidan orqaga o'tsa, oxirgisiga sakraydi
          }
        } else { // Shunchaki Tab oldinga harakat
          if (document.activeElement === lastElement) {
            e.preventDefault();
            firstElement.focus(); // Oxirgisidan o'tsa, yana birinchisiga qaytadi
          }
        }
      }
    }

    openModalBtn.addEventListener('click', openModal);
    closeModalBtn.addEventListener('click', closeModal);
    saveBtn.addEventListener('click', closeModal);
    
    // Oyna tashqarisiga bosilganda ham yopilishi uchun
    modalOverlay.addEventListener('click', (e) => {
      if (e.target === modalOverlay) closeModal();
    });
  </script>
</body>
</html>
