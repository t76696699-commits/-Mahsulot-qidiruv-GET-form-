Sass utility klasslarimiz va oldingi komponentlarimizni yanada kengaytirib, dinamik va funksional Modal Oyna (Popup) hamda uning ichidagi validation elementlariga ega Forma strukturasini shakllantiramiz.

Ushbu kodda input guruhlari (input-groups), xatolik/muvaffaqiyat holatlari (validation states) va tugmalarning ishlash mexanizmlari to‘liq qamrab olingan.

1. Kengaytirilgan Semantik HTML (index.html ga qo'shimcha)
Ushbu qismni <body> yopilishidan oldin har qanday joyga joylashtirishingiz mumkin. Shuningdek, modalni ochish uchun sahifaga bitta trigger tugma qo'shilgan.

HTML
<!-- MODALNI OCHUVCHI TRIGGER TUGMA -->
<div class="text-center mt-5">
  <button class="btn-primary p-3" onclick="openModal()">Yangi mahsulot qo'shish</button>
</div>

<!-- O'ZIGA XOS MODAL STRUKTURASI -->
<div class="modal-backdrop" id="productModal" onclick="closeModalOnBackdrop(event)">
  <div class="modal-card bg-white p-4">
    
    <!-- Modal Header -->
    <div class="modal-header mb-4">
      <h2 class="text-dark">Yangi mahsulot anketasi</h2>
      <button class="close-btn text-secondary" onclick="closeModal()">&times;</button>
    </div>

    <!-- Modal Body (Forma 4+ field va Validation bilan) -->
    <div class="modal-body">
      <form id="productForm" onsubmit="handleFormSubmit(event)">
        
        <!-- Field 1: Input Group (Mahsulot nomi) -->
        <div class="form-group mb-3">
          <label class="text-dark d-block mb-1">Mahsulot nomi</label>
          <div class="input-group">
            <span class="input-group-text bg-light text-secondary">📦</span>
            <input type="text" id="prodName" class="form-control" placeholder="Masalan: iPhone 14 Pro">
          </div>
          <span class="error-feedback text-danger mt-1">Mahsulot nomi kamida 3 ta harfdan iborat bo'lishi kerak!</span>
        </div>

        <!-- Field 2: Input Group (Narxi) -->
        <div class="form-group mb-3">
          <label class="text-dark d-block mb-1">Narxi (UZS)</label>
          <div class="input-group">
            <input type="number" id="prodPrice" class="form-control" placeholder="0.00">
            <span class="input-group-text bg-light text-secondary">so'm</span>
          </div>
          <span class="error-feedback text-danger mt-1">Iltimos, to'g'ri narx kiriting!</span>
        </div>

        <!-- Field 3: Select Custom Field (Kategoriya) -->
        <div class="form-group mb-3">
          <label class="text-dark d-block mb-1">Kategoriya</label>
          <select id="prodCategory" class="form-control">
            <option value="">Kategoriyani tanlang...</option>
            <option value="electronics">Elektronika</option>
            <option value="appliances">Maishiy texnika</option>
            <option value="accessories">Aksessuarlar</option>
          </select>
          <span class="error-feedback text-danger mt-1">Iltimos, kategoriyalardan birini tanlang!</span>
        </div>

        <!-- Field 4: Textarea Field (Tavsif) -->
        <div class="form-group mb-4">
          <label class="text-dark d-block mb-1">Qisqacha tavsif</label>
          <textarea id="prodDesc" class="form-control" rows="3" placeholder="Mahsulot haqida batafsil..."></textarea>
          <span class="success-feedback text-secondary mt-1">Kiritish ixtiyoriy (Yaxshi tavsif savdoni oshiradi).</span>
        </div>

        <!-- Modal Footer Tugmalari -->
        <div class="modal-footer mt-4">
          <button type="button" class="btn-secondary p-2" onclick="closeModal()">Bekor qilish</button>
          <button type="submit" class="btn-primary p-2">Saqlash va Yuborish</button>
        </div>

      </form>
    </div>

  </div>
</div>
2. Modal va Forma uchun Sass Stillari (scss/components/_modal.scss)
Ushbu stillar oynaning silliq ochilishi, input guruhlarining birlashishi va validation holatlariga ko'ra dinamik ranglanishini ta'minlaydi.

SCSS
@use '../abstracts';

// Modal orqa foni (Backdrop)
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s ease;

  // Modal ochiq holati
  &.active {
    opacity: 1;
    pointer-events: auto;
    
    .modal-card {
      transform: translateY(0);
    }
  }
}

// Modal asosiy qutisi (Card)
.modal-card {
  width: 100%;
  max-width: 500px;
  border-radius: 8px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
  transform: translateY(-50px);
  transition: transform 0.3s ease;

  .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px solid abstracts.$color-light;
    padding-bottom: 10px;

    .close-btn {
      background: none;
      border: none;
      font-size: 1.8rem;
      cursor: pointer;
      line-height: 1;
    }
  }

  .modal-footer {
    display: flex;
    justify-content: flex-end;
    gap: 10px;
    border-top: 1px solid abstracts.$color-light;
    padding-top: 15px;
  }
}

// --- FORM VA INPUT GROUP UTILITIES ---
.form-group {
  .d-block { display: block; }
  .form-control {
    width: 100%;
    padding: 10px 12px;
    border: 1px solid darken(abstracts.$color-light, 15%);
    border-radius: 4px;
    font-size: 0.95rem;
    outline: none;
    transition: border-color 0.2s;

    &:focus {
      border-color: abstracts.$color-primary;
    }
  }
}

// Input Group komponenti (Belgilar bilan ishlash uchun)
.input-group {
  display: flex;
  width: 100%;

  .input-group-text {
    display: flex;
    align-items: center;
    padding: 0 15px;
    border: 1px solid darken(abstracts.$color-light, 15%);
    white-space: nowrap;
  }

  // Elementlarni bir-biriga ulash konturi
  & > :first-child {
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
  }
  & > :last-child {
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
    border-left: none;
  }
  & > :not(:first-child):not(:last-child) {
    border-radius: 0;
  }
}

// --- VALIDATION HOLATLARI (STATES) ---
.error-feedback, .success-feedback {
  display: none; // Standart holatda yashirin bo'ladi
  font-size: 0.8rem;
}

// Xatolik holati
.form-group.is-invalid {
  .form-control, .input-group-text {
    border-color: abstracts.$color-danger !important;
    background-color: rgba(abstracts.$color-danger, 0.03);
  }
  .error-feedback {
    display: block;
  }
}

// Muvaffaqiyatli holat
.form-group.is-valid {
  .form-control, .input-group-text {
    border-color: abstracts.$color-secondary !important;
    background-color: rgba(abstracts.$color-secondary, 0.03);
  }
}
3. Logika va Oynani Boshqarish (JavaScript)
Modal qutilarining ishlashi hamda formadagi 4 ta elementni dinamik tekshirish (validation) uchun quyidagi funksiyalardan foydalaniladi.

JavaScript
// 1. MODAL OYNANI OCHISH VA YOPISH
function openModal() {
  document.getElementById('productModal').classList.add('active');
}

function closeModal() {
  document.getElementById('productModal').classList.remove('active');
  resetForm();
}

// Backdrop (bo'sh joy) bosilganda modalni yopish
function closeModalOnBackdrop(event) {
  if (event.target.id === 'productModal') {
    closeModal();
  }
}

// 2. FORM VALIDATION LOGIKASI
function handleFormSubmit(event) {
  event.preventDefault(); // Sahifa yangilanishini to'xtatish
  
  const name = document.getElementById('prodName');
  const price = document.getElementById('prodPrice');
  const category = document.getElementById('prodCategory');
  
  let isValid = true;

  // Mahsulot nomi tekshiruvi (Kamida 3 ta belgi)
  if (name.value.trim().length < 3) {
    setValidationState(name, false);
    isValid = false;
  } else {
    setValidationState(name, true);
  }

  // Narx tekshiruvi (Noldan katta bo'lishi shart)
  if (!price.value || parseFloat(price.value) <= 0) {
    setValidationState(price, false);
    isValid = false;
  } else {
    setValidationState(price, true);
  }

  // Kategoriya tanlanganligini tekshirish
  if (category.value === "") {
    setValidationState(category, false);
    isValid = false;
  } else {
    setValidationState(category, true);
  }

  // Agar barchasi to'g'ri bo'lsa
  if (isValid) {
    alert("Muvaffaqiyatli! Mahsulot tizimga qo'shildi.");
    closeModal();
  }
}

// Yordamchi funksiya: Klasslarni almashtirish
function setValidationState(element, valid) {
  const group = element.closest('.form-group');
  if (valid) {
    group.classList.remove('is-invalid');
    group.classList.add('is-valid');
  } else {
    group.classList.remove('is-valid');
    group.classList.add('is-invalid');
  }
}

// Formani tozalash funksiyasi
function resetForm() {
  const form = document.getElementById('productForm');
  form.reset();
  const groups = form.querySelectorAll('.form-group');
  groups.forEach(group => {
    group.classList.remove('is-valid', 'is-invalid');
  });
}
