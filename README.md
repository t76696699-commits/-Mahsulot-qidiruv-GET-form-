<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Bootstrap 5 Admin Dashboard</title>
  <!-- Bootstrap 5 CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
  <!-- Bootstrap Icons -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css" rel="stylesheet">
  <style>
    /* Faqat sidebarning desktop va mobil moslashuvchanligini ta'minlash uchun kichik o'lchamlar */
    @media (min-width: 992px) {
      .main-content { margin-left: 240px; }
      .sidebar { width: 240px; }
    }
  </style>
</head>
<body class="bg-light">

  <!-- 1. SIDEBAR (Mobilda offcanvas bo'lib ochiladi, desktopda doimiy) -->
  <div class="sidebar offcanvas-lg offcanvas-start bg-dark text-white position-fixed top-0 bottom-0 start-0 z-3" id="sidebarMenu">
    <div class="d-flex flex-column h-100 p-3">
      <a href="#" class="d-flex align-items-center mb-4 text-white text-decoration-none h5 fw-bold">
        <i class="bi bi-speedometer2 me-2 text-primary"></i> AdminPanel
      </a>
      <hr class="text-secondary">
      <ul class="nav nav-pills flex-column mb-auto gap-1">
        <li class="nav-item">
          <a href="#" class="nav-link active d-flex align-items-center gap-2"><i class="bi bi-house-door"></i> Bosh sahifa</a>
        </li>
        <li>
          <a href="#" class="nav-link text-white d-flex align-items-center gap-2"><i class="bi bi-people"></i> Foydalanuvchilar</a>
        </li>
        <li>
          <a href="#" class="nav-link text-white d-flex align-items-center gap-2"><i class="bi bi-graph-up"></i> Analitika</a>
        </li>
        <li>
          <a href="#" class="nav-link text-white d-flex align-items-center gap-2"><i class="bi bi-gear"></i> Sozlamalar</a>
        </li>
      </ul>
    </div>
  </div>

  <!-- ASOSIY KONTENT HUDUDI (Sidebar kengligini hisobga oladi) -->
  <div class="main-content">
    
    <!-- 2. TOP NAVBAR -->
    <nav class="navbar navbar-expand bg-white border-bottom sticky-top px-4 py-2">
      <div class="container-fluid p-0 d-flex justify-content-between align-items-center">
        <!-- Mobil qurilmalar uchun sidebar burger tugmasi -->
        <button class="btn btn-light d-lg-none me-2" type="button" data-bs-toggle="offcanvas" data-bs-target="#sidebarMenu">
          <i class="bi bi-list fs-4"></i>
        </button>
        
        <!-- Qidiruv paneli -->
        <div class="input-group d-none d-sm-flex" style="max-width: 300px;">
          <input type="text" class="form-control form-control-sm border-end-0 bg-light" placeholder="Qidiruv...">
          <span class="input-group-text bg-light border-start-0 text-muted"><i class="bi bi-search"></i></span>
        </div>

        <!-- Profil qismi -->
        <div class="d-flex align-items-center gap-3">
          <i class="bi bi-bell fs-5 text-secondary cursor-pointer"></i>
          <div class="d-flex align-items-center gap-2">
            <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?q=80&w=100&h=100&fit=crop" 
                 alt="Admin" class="rounded-circle object-fit-cover" style="width: 35px; height: 35px;">
            <span class="d-none d-md-inline small fw-semibold text-dark">Alisa W.</span>
          </div>
        </div>
      </div>
    </nav>

    <!-- SAHIFA TARKIBI -->
    <div class="p-4">
      
      <!-- 3. 4 TA STATISTIKA KARTALARI (Turli xil ranglarda) -->
      <div class="row g-3 mb-4">
        <!-- Karta 1: Primary (Ko'k) -->
        <div class="col-12 col-sm-6 col-xl-3">
          <div class="card border-0 shadow-sm bg-primary text-white p-3 d-flex flex-row align-items-center justify-content-between">
            <div>
              <h6 class="text-white-50 small text-uppercase mb-1">Jami Savdolar</h6>
              <h3 class="fw-bold mb-0">$24,500</h3>
            </div>
            <i class="bi bi-cart-check fs-1 text-white-50"></i>
          </div>
        </div>
        <!-- Karta 2: Success (Yashil) -->
        <div class="col-12 col-sm-6 col-xl-3">
          <div class="card border-0 shadow-sm bg-success text-white p-3 d-flex flex-row align-items-center justify-content-between">
            <div>
              <h6 class="text-white-50 small text-uppercase mb-1">Faol Mijozlar</h6>
              <h3 class="fw-bold mb-0">1,240</h3>
            </div>
            <i class="bi bi-people fs-1 text-white-50"></i>
          </div>
        </div>
        <!-- Karta 3: Warning (Sariq) -->
        <div class="col-12 col-sm-6 col-xl-3">
          <div class="card border-0 shadow-sm bg-warning text-dark p-3 d-flex flex-row align-items-center justify-content-between">
            <div>
              <h6 class="text-dark-55 small text-uppercase mb-1">Yangi Buyurtmalar</h6>
              <h3 class="fw-bold mb-0">45 ta</h3>
            </div>
            <i class="bi bi-bag-plus fs-1 text-dark-50"></i>
          </div>
        </div>
        <!-- Karta 4: Danger (Qizil) -->
        <div class="col-12 col-sm-6 col-xl-3">
          <div class="card border-0 shadow-sm bg-danger text-white p-3 d-flex flex-row align-items-center justify-content-between">
            <div>
              <h6 class="text-white-50 small text-uppercase mb-1">Tizim Xatoliklari</h6>
              <h3 class="fw-bold mb-0">2 ta</h3>
            </div>
            <i class="bi bi-exclamation-triangle fs-1 text-white-50"></i>
          </div>
        </div>
      </div>

      <!-- JADVAL VA ACTION BLOCK -->
      <div class="card border-0 shadow-sm p-4">
        <div class="d-flex justify-content-between align-items-center mb-3 flex-wrap gap-2">
          <h5 class="fw-bold text-dark m-0">So'nggi a'zolar ro'yxati</h5>
          <!-- Modalni ochuvchi trigger tugma -->
          <button class="btn btn-primary btn-sm d-flex align-items-center gap-1" data-bs-toggle="modal" data-bs-target="#addUserModal">
            <i class="bi bi-plus-lg"></i> Yangi foydalanuvchi
          </button>
        </div>

        <!-- 4. RESPONSIVE JADVAL (5+ Qator va Badgelar) -->
        <div class="table-responsive">
          <table class="table table-hover align-middle mb-0">
            <thead class="table-light text-secondary small text-uppercase">
              <tr>
                <th>Foydalanuvchi</th>
                <th>Email</th>
                <th>Rol</th>
                <th>Status</th>
                <th>Sana</th>
              </tr>
            </thead>
            <tbody class="text-dark small">
              <tr>
                <td class="fw-semibold">Farruh Aliyev</td>
                <td>farruh@example.com</td>
                <td>Administrator</td>
                <td><span class="badge bg-success-subtle text-success rounded-pill px-2 py-1">Faol</span></td>
                <td>2026-05-12</td>
              </tr>
              <tr>
                <td class="fw-semibold">Madina Karimova</td>
                <td>madina@example.com</td>
                <td>Moderatorda</td>
                <td><span class="badge bg-success-subtle text-success rounded-pill px-2 py-1">Faol</span></td>
                <td>2026-06-01</td>
              </tr>
              <tr>
                <td class="fw-semibold">Jasur Toshpulatov</td>
                <td>jasur@example.com</td>
                <td>Foydalanuvchi</td>
                <td><span class="badge bg-warning-subtle text-warning rounded-pill px-2 py-1">Kutilmoqda</span></td>
                <td>2026-07-10</td>
              </tr>
              <tr>
                <td class="fw-semibold">Shahzoda Umarova</td>
                <td>shahzoda@example.com</td>
                <td>Foydalanuvchi</td>
                <td><span class="badge bg-danger-subtle text-danger rounded-pill px-2 py-1">Bloklangan</span></td>
                <td>2026-04-15</td>
              </tr>
              <tr>
                <td class="fw-semibold">Olimjon Nabiyev</td>
                <td>olim@example.com</td>
                <td>Foydalanuvchi</td>
                <td><span class="badge bg-success-subtle text-success rounded-pill px-2 py-1">Faol</span></td>
                <td>2026-07-11</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

    </div>
  </div>

  <!-- 5. INTAGRATSION MODAL FORM -->
  <div class="modal fade" id="addUserModal" tabindex="-1" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content border-0 shadow">
        <div class="modal-header border-bottom bg-light">
          <h5 class="modal-title fw-bold text-dark">Yangi profil yaratish</h5>
          <button type="button" class="btn-close" data-bs-close="modal" aria-label="Close"></button>
        </div>
        <form onsubmit="alert('Foydalanuvchi muvaffaqiyatli qo\'shildi!');">
          <div class="modal-body d-flex flex-column gap-3">
            <div>
              <label class="form-label small text-secondary fw-semibold">Ism va Familiya</label>
              <input type="text" class="form-control" placeholder="Masalan: Asadbek Ivanov" required>
            </div>
            <div>
              <label class="form-label small text-secondary fw-semibold">Elektron pochta</label>
              <input type="email" class="form-control" placeholder="name@example.com" required>
            </div>
            <div>
              <label class="form-label small text-secondary fw-semibold">Tizimdagi roli</label>
              <select class="form-select" required>
                <option value="">Tanlang...</option>
                <option value="user">Foydalanuvchi</option>
                <option value="moderator">Moderator</option>
              </select>
            </div>
          </div>
          <div class="modal-footer border-top bg-light">
            <button type="button" class="btn btn-sm btn-secondary" data-bs-close="modal">Yopish</button>
            <button type="submit" class="btn btn-sm btn-primary">Saqlash</button>
          </div>
        </form>
      </div>
    </div>
  </div>

  <!-- Bootstrap 5 JavaScript Bundle JS CDN (Modal va Offcanvas ishlashi uchun zarur) -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
