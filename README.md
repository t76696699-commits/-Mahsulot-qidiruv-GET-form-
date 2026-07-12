<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Bootstrap Utility Card</title>
  
  <!-- Bootstrap 5 CSS CDN -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  
  <!-- Bootstrap Icons CDN -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css" rel="stylesheet">
</head>
<body class="bg-light d-flex justify-content-center align-items-center vh-100 m-0">

  <!-- Profil Kartasi Containeri -->
  <div class="card shadow border-0 p-4 text-center" style="width: 22rem; border-radius: 16px;">
    
    <!-- Avatar Qismi (Flex va Rounded-Circle) -->
    <div class="d-flex justify-content-center mb-3">
      <div class="position-relative p-1 border border-2 border-secondary-subtle rounded-circle">
        <img src="https://images.unsplash.com/photo-1494790108377-be9c29b29330?auto=format&fit=crop&q=80&w=200&h=200" 
             alt="Avatar" 
             class="rounded-circle img-fluid" 
             style="width: 120px; height: 120px; object-fit: cover;">
      </div>
    </div>

    <!-- Badge (Rol ko'rsatilgan) -->
    <div class="d-flex justify-content-center mb-2">
      <span class="badge bg-info text-dark px-3 py-2 rounded-pill fw-semibold text-uppercase tracking-wide fs-7">
        Senior Developer
      </span>
    </div>

    <!-- Foydalanuvchi ismi -->
    <h3 class="fw-bold text-dark mb-3">Sarah Jenkins</h3>

    <!-- Aloqa va Ma'lumotlar (Flex utilitlari bilan) -->
    <div class="d-flex flex-column gap-2 text-start px-2 mb-4">
      
      <!-- Email -->
      <div class="d-flex align-items-center gap-2 text-primary">
        <i class="bi bi-envelope-fill fs-5"></i>
        <a href="mailto:s.jenkins@email.com" class="text-decoration-none fw-medium">s.jenkins@email.com</a>
      </div>
      
      <!-- Status -->
      <div class="d-flex align-items-center gap-2 text-success">
        <i class="bi bi-chat-fill fs-5"></i>
        <span class="fw-medium">Online</span>
      </div>
      
      <!-- Sozlamalar -->
      <div class="d-flex align-items-center gap-2 text-secondary">
        <i class="bi bi-gear-fill fs-5"></i>
        <span class="fw-medium">Settings</span>
      </div>
      
    </div>

  </div>

  <!-- Bootstrap 5 JS (Agar kerak bo'lsa) -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
