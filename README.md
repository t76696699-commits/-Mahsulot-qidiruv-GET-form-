<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Kartochkalar</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #f1f5f9;
      padding: 40px;
    }
    h1 { text-align: center; color: #1e293b; margin-bottom: 32px; font-size: 28px; }

    .cards-grid {
      display: flex;
      gap: 24px;
      justify-content: center;
      flex-wrap: wrap;
    }
    .card {
      background: white;
      border-radius: 16px;
      overflow: hidden;
      width: 280px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.08);
      transition: transform 0.3s, box-shadow 0.3s;
    }
    .card:hover {
      transform: translateY(-8px);
      box-shadow: 0 12px 40px rgba(0,0,0,0.15);
    }
    .card-img {
      width: 100%;
      height: 180px;
      object-fit: cover;
      background: linear-gradient(135deg, #667eea, #764ba2);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 64px;
    }
    .card-body { padding: 20px; }
    .card-badge {
      display: inline-block;
      background: #ede9fe;
      color: #7c3aed;
      font-size: 11px;
      font-weight: 700;
      padding: 3px 10px;
      border-radius: 20px;
      margin-bottom: 10px;
      text-transform: uppercase;
    }
    .card-title { font-size: 18px; font-weight: 700; color: #1e293b; margin-bottom: 8px; }
    .card-desc { font-size: 13px; color: #64748b; line-height: 1.6; margin-bottom: 16px; }
    .card-footer {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .card-price { font-size: 20px; font-weight: 800; color: #7c3aed; }
    .card-btn {
      background: #7c3aed;
      color: white;
      border: none;
      padding: 8px 18px;
      border-radius: 8px;
      font-size: 13px;
      font-weight: 600;
      cursor: pointer;
      transition: opacity 0.2s;
    }
    .card-btn:hover { opacity: 0.85; }
  </style>
</head>
<body>
  <h1>🛍️ Mahsulotlar</h1>
  <div class="cards-grid">
    <div class="card">
      <div class="card-img">💻</div>
      <div class="card-body">
        <span class="card-badge">Yangi</span>
        <div class="card-title">HTML/CSS Kursi</div>
        <div class="card-desc">Veb-dizayn asoslarini o'rganib, chiroyli sahifalar yarating.</div>
        <div class="card-footer">
          <span class="card-price">Bepul</span>
          <button class="card-btn">Boshlash</button>
        </div>
      </div>
    </div>
    <div class="card">
      <div class="card-img">🐍</div>
      <div class="card-body">
        <span class="card-badge">Mashhur</span>
        <div class="card-title">Python Kursi</div>
        <div class="card-desc">Dasturlash asoslarini Python tili orqali o'rganing.</div>
        <div class="card-footer">
          <span class="card-price">Bepul</span>
          <button class="card-btn">Boshlash</button>
        </div>
      </div>
    </div>
    <div class="card">
      <div class="card-img">⚡</div>
      <div class="card-body">
        <span class="card-badge">Ilg'or</span>
        <div class="card-title">JavaScript Kursi</div>
        <div class="card-desc">Saytingizga interaktivlik qo'shing.</div>
        <div class="card-footer">
          <span class="card-price">Bepul</span>
          <button class="card-btn">Boshlash</button>
        </div>
      </div>
    </div>
  </div>
</body>
</html>
