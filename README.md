Sass utility klasslari va arxitekturasini yaratgandan so'ng, ularni real amaliyotda tekshirish uchun mukammal bir maket (layout) kerak bo'ladi. Siz so'ragan barcha komponentlarni (Navbar collapse, 6 ta mahsulot kartasi, badge va tugmalar, pagination hamda alert) o'z ichiga olgan toza va semantik HTML strukturasini taqdim etaman.

Ushbu strukturada yuqorida biz yaratgan .col-1 dan .col-12 gacha bo'lgan grid tizimi hamda .bg-primary, .p-3, .mt-3 kabi utility klasslardan foydalanilgan.

Semantik HTML Maketi (index.html)
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sass Utilities Amaliy Maket</title>
  <link rel="stylesheet" href="css/main.css">
</head>
<body class="theme-adaptive bg-light">

  <!-- 1. ALERT XABAR -->
  <div class="alert bg-warning text-dark p-3 text-center">
    <strong>Diqqat!</strong> Bugun barcha mahsulotlar uchun 20% gacha chegirmalar mavjud!
  </div>

  <!-- 2. NAVBAR COLLAPSE BILAN -->
  <nav class="navbar bg-dark text-light p-3">
    <div class="navbar-container">
      <a href="#" class="navbar-brand text-primary">UzShop</a>
      
      <!-- Mobil qurilmalar uchun burger tugma -->
      <button class="navbar-toggle" onclick="toggleNavbar()">
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>

      <!-- Collapse bo'ladigan menyu qismi -->
      <div class="navbar-collapse" id="navbarMenu">
        <ul class="navbar-nav">
          <li><a href="#" class="text-light">Bosh sahifa</a></li>
          <li><a href="#" class="text-light">Mahsulotlar</a></li>
          <li><a href="#" class="text-light">Biz haqimizda</a></li>
          <li><a href="#" class="text-light">Aloqa</a></li>
        </ul>
      </div>
    </div>
  </nav>

  <!-- ASOSIY MAZMUN -->
  <main class="container p-4">
    <h1 class="text-dark mb-4">Yangi Mahsulotlar</h1>

    <!-- 3. MAHSULOTLAR PANELI (GRID-CONTAINER) -->
    <div class="row">
      
      <!-- KARTA 1 -->
      <div class="col-4 p-2">
        <div class="card bg-white p-3 mb-3">
          <div class="badge bg-danger text-light">Yangi</div>
          <img src="https://picsum.photos/300/200?random=1" alt="Mahsulot" class="img-fluid mt-2">
          <h3 class="text-dark mt-2">Smartfon X10</h3>
          <p class="text-secondary">Ajoyib kamera va tezkor protsessorga ega flagman.</p>
          <button class="btn-primary mt-3">Sotib olish</button>
        </div>
      </div>

      <!-- KARTA 2 -->
      <div class="col-4 p-2">
        <div class="card bg-white p-3 mb-3">
          <div class="badge bg-warning text-dark">Chegirma</div>
          <img src="https://picsum.photos/300/200?random=2" alt="Mahsulot" class="img-fluid mt-2">
          <h3 class="text-dark mt-2">Simsiz Quloqchin</h3>
          <p class="text-secondary">Yuqori sifatli ovoz va uzoqqa yetuvchi quvvat.</p>
          <button class="btn-primary mt-3">Sotib olish</button>
        </div>
      </div>

      <!-- KARTA 3 -->
      <div class="col-4 p-2">
        <div class="card bg-white p-3 mb-3">
          <div class="badge bg-info text-light">Top</div>
          <img src="https://picsum.photos/300/200?random=3" alt="Mahsulot" class="img-fluid mt-2">
          <h3 class="text-dark mt-2">Aqlli Soat S3</h3>
          <p class="text-secondary">Sog'ligingiz va bildirishnomalaringiz doim nazoratda.</p>
          <button class="btn-primary mt-3">Sotib olish</button>
        </div>
      </div>

      <!-- KARTA 4 -->
      <div class="col-4 p-2">
        <div class="card bg-white p-3 mb-3">
          <div class="badge bg-danger text-light">Yangi</div>
          <img src="https://picsum.photos/300/200?random=4" alt="Mahsulot" class="img-fluid mt-2">
          <h3 class="text-dark mt-2">Noutbuk Ultra</h3>
          <p class="text-secondary">Dasturlash va ofis ishlari uchun eng yengil noutbuk.</p>
          <button class="btn-primary mt-3">Sotib olish</button>
        </div>
      </div>

      <!-- KARTA 5 -->
      <div class="col-4 p-2">
        <div class="card bg-white p-3 mb-3">
          <div class="badge bg-secondary text-light">Tavsiya</div>
          <img src="https://picsum.photos/300/200?random=5" alt="Mahsulot" class="img-fluid mt-2">
          <h3 class="text-dark mt-2">Mexanik Klaviatura</h3>
          <p class="text-secondary">RGB yoritgichli va tezkor chertiluvchi tugmalar.</p>
          <button class="btn-primary mt-3">Sotib olish</button>
        </div>
      </div>

      <!-- KARTA 6 -->
      <div class="col-4 p-2">
        <div class="card bg-white p-3 mb-3">
          <div class="badge bg-warning text-dark">-15% Chegirma</div>
          <img src="https://picsum.photos/300/200?random=6" alt="Mahsulot" class="img-fluid mt-2">
          <h3 class="text-dark mt-2">Geymer Sichqonchasi</h3>
          <p class="text-secondary">Ergonomik dizayn va yuqori aniqlikdagi sensor.</p>
          <button class="btn-primary mt-3">Sotib olish</button>
        </div>
      </div>

    </div>

    <!-- 4. PAGINATION MAVJUD -->
    <div class="pagination-container mt-4 mb-5">
      <ul class="pagination">
        <li><a href="#" class="page-link disabled">&laquo;</a></li>
        <li><a href="#" class="page-link active">1</a></li>
        <li><a href="#" class="page-link">2</a></li>
        <li><a href="#" class="page-link">3</a></li>
        <li><a href="#" class="page-link">&raquo;</a></li>
      </ul>
    </div>
  </main>

  <!-- Navbar Collapse mexanizmi uchun oddiygina JavaScript eshittiruvchisi -->
  <script>
    function toggleNavbar() {
      const menu = document.getElementById('navbarMenu');
      menu.classList.toggle('show');
    }
  </script>
</body>
</html>
Ushbu komponentlarni qo'llab-quvvatlovchi qo'shimcha Sass Stillari
Utility klasslardan tashqari, ushbu komponentlarning struktura skeletini (Layout) scss/components/ yoki scss/layouts/ ichida quyidagicha yozib olishingiz tavsiya etiladi:

SCSS
// Grid strukturasini boshqarish uchun (scss/layouts/_grid.scss)
.row {
  display: flex;
  flex-wrap: wrap;
  margin-left: -0.5rem;
  margin-right: -0.5rem;
}

// Navbar collapse logikasi uchun asos (scss/components/_navbar.scss)
.navbar {
  .navbar-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    width: 100%;
  }
  
  .navbar-nav {
    display: flex;
    list-style: none;
    gap: 1.5rem;
  }
  
  .navbar-toggle {
    display: none; // Kompyuterda yashirin turadi
    flex-direction: column;
    gap: 4px;
    background: transparent;
    border: none;
    cursor: pointer;
    
    .icon-bar {
      width: 25px;
      height: 3px;
      background-color: #fff;
    }
  }

  // Mobil ekranlarda collapse ishlashi
  @media (max-width: 768px) {
    .navbar-toggle { display: flex; }
    .navbar-collapse {
      display: none;
      width: 100%;
      &.show { display: block; }
    }
    .navbar-nav {
      flex-direction: column;
      gap: 1rem;
      padding-top: 1rem;
    }
  }
}

// Karta va Badge dizayni (scss/components/_card.scss)
.card {
  position: relative;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.05);
  height: 100%;
  
  .badge {
    position: absolute;
    top: 15px;
    left: 15px;
    padding: 4px 8px;
    border-radius: 4px;
    font-size: 0.75rem;
    font-weight: bold;
  }
  
  .img-fluid {
    width: 100%;
    height: auto;
    border-radius: 4px;
  }
}

// Pagination (scss/components/_pagination.scss)
.pagination-container {
  display: flex;
  justify-content: center;
  
  .pagination {
    display: flex;
    list-style: none;
    padding: 0;
    gap: 5px;
    
    .page-link {
      display: block;
      padding: 8px 16px;
      border: 1px solid #ddd;
      color: #3498db;
      text-decoration: none;
      border-radius: 4px;
      
      &.active {
        background-color: #3498db;
        color: white;
        border-color: #3498db;
      }
      &.disabled {
        color: #ccc;
        pointer-events: none;
      }
    }
  }
}
