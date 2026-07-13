<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>A11y Forma Standartlari</title>
  <style>
    body { font-family: system-ui, sans-serif; padding: 20px; max-width: 500px; margin: 0 auto; color: #212529; }
    .form-group { margin-bottom: 1.5rem; display: flex; flex-direction: column; }
    label { font-weight: bold; margin-bottom: 0.5rem; }
    input[type="text"], input[type="email"] { padding: 0.5rem; font-size: 16px; border: 1px solid #ccc; border-radius: 4px; }
    input:focus-visible, fieldset:focus-visible { outline: 3px solid #0d6efd; outline-offset: 2px; }
    
    /* Xatolik holati uslubi */
    .error-message { color: #dc3545; font-size: 14px; margin-top: 0.25rem; font-weight: 500; }
    input[aria-invalid="true"] { border-color: #dc3545; }

    /* Guruhlar uchun uslub */
    fieldset { border: 1px solid #ccc; border-radius: 4px; padding: 1rem; margin-bottom: 1.5rem; }
    legend { font-weight: bold; padding: 0 0.5rem; }
    .radio-group { display: flex; gap: 15px; margin-top: 0.5rem; }

    button { padding: 0.75rem; background: #0d6efd; color: white; border: none; border-radius: 4px; font-size: 16px; cursor: pointer; font-weight: bold; }
    
    /* aria-live hududi vizual yashirin, lekin ekran o'quvchiga eshitiladi */
    .sr-only { position: absolute; width: 1px; height: 1px; padding: 0; margin: -1px; overflow: hidden; clip: rect(0, 0, 0, 0); border: 0; }
    .success-alert { background: #d1e7dd; color: #0f5132; padding: 1rem; border-radius: 4px; margin-bottom: 1rem; border: 1px solid #badbcc; }
  </style>
</head>
<body>

  <h1>Ro'yxatdan o'tish</h1>
  
  <!-- Muvaffaqiyat xabari uchun dinamik hudud (4-qoida) -->
  <div id="statusStatus" aria-live="polite"></div>

  <form id="registrationForm" novalidate>
    
    <!-- 1 va 2-QOIDA: for/id bog'liqligi va aria-required -->
    <div class="form-group">
      <label綁 for="fullName">To'liq ismingiz <span aria-hidden="true">*</span></label>
      <input 
        type="text" 
        id="fullName" 
        name="fullName"
        autocomplete="name" 
        aria-required="true"
      >
      <!-- 6-QOIDA: autocomplete="name" to'g'ri qo'yilgan -->
    </div>

    <!-- 3-QOIDA: aria-describedby orqali xatoni bog'lash -->
    <div class="form-group">
      <label for="userEmail">E-mail manzilingiz <span aria-hidden="true">*</span></label>
      <input 
        type="email" 
        id="userEmail" 
        name="userEmail"
        autocomplete="email" 
        aria-required="true"
      >
      <!-- Xato xabari ID'si shu yerga dinamik ulanadi -->
      <div id="emailError" class="error-message" style="display: none;"></div>
    </div>

    <!-- 5-QOIDA: fieldset/legend radio guruh uchun -->
    <fieldset>
      <legend>Bog'lanish turini tanlang</legend>
      <div class="radio-group">
        <label>
          <input type="radio" name="contactMethod" value="email" checked> E-mail
        </label>
        <label>
          <input type="radio" name="contactMethod" value="phone"> Telefon
        </label>
      </div>
    </fieldset>

    <button type="submit">Yuborish</button>
  </form>

  <script>
    const form = document.getElementById('registrationForm');
    const emailInput = document.getElementById('userEmail');
    const emailError = document.getElementById('emailError');
    const statusStatus = document.getElementById('statusStatus');

    form.addEventListener('submit', function(e) {
      e.preventDefault();
      
      // Oddiy validatsiya (E-mail bo'sh bo'lmasligi kerak)
      if (!emailInput.value.trim()) {
        // 3-QOIDA: Xatolikni bog'lash va e'lon qilish
        emailInput.setAttribute('aria-invalid', 'true');
        emailInput.setAttribute('aria-describedby', 'emailError');
        emailError.textContent = '⚠ E-mail manzilini kiritish majburiy!';
        emailError.style.display = 'block';
        emailInput.focus(); // Fokusni xato maydonga olib boramiz
        return;
      } else {
        // Xatoni tozalash
        emailInput.removeAttribute('aria-invalid');
        emailInput.removeAttribute('aria-describedby');
        emailError.style.display = 'none';
      }

      // 4-QOIDA: Muvaffaqiyatli yuborilganda aria-live orqali bildirishnoma
      statusStatus.innerHTML = `
        <div class="success-alert" role="status">
          <strong>Muvaffaqiyatli!</strong> Formangiz qabul qilindi. Tez orada bog'lanamiz.
        </div>
      `;
      
      form.reset();
    });
  </script>

</body>
</html>
