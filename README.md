<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Flexbox Layout</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: 'Segoe UI', sans-serif; background: #0f172a; color: white; }

    /* Header */
    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 16px 40px;
      background: #1e293b;
      border-bottom: 1px solid #334155;
    }
    .logo { font-size: 20px; font-weight: 700; color: #38bdf8; }
    nav { display: flex; gap: 24px; }
    nav a { color: #94a3b8; text-decoration: none; font-size: 14px; }
    nav a:hover { color: #38bdf8; }

    /* Hero */
    .hero {
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      gap: 16px;
      padding: 80px 40px;
      text-align: center;
    }
    .hero h1 { font-size: 48px; color: white; }
    .hero p { font-size: 18px; color: #94a3b8; max-width: 500px; }
    .hero-btns { display: flex; gap: 12px; margin-top: 8px; }
    .btn-primary {
      background: #38bdf8; color: #0f172a;
      padding: 12px 28px; border-radius: 8px;
      font-weight: 700; text-decoration: none;
    }
    .btn-outline {
      border: 2px solid #38bdf8; color: #38bdf8;
      padding: 12px 28px; border-radius: 8px;
      font-weight: 700; text-decoration: none;
    }

    /* Stats */
    .stats {
      display: flex;
      justify-content: space-evenly;
      padding: 40px;
      background: #1e293b;
      gap: 20px;
      flex-wrap: wrap;
    }
    .stat-item {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 4px;
    }
    .stat-num { font-size: 36px; font-weight: 800; color: #38bdf8; }
    .stat-label { font-size: 13px; color: #94a3b8; }

    /* Cards row */
    .features {
      display: flex;
      gap: 20px;
      padding: 40px;
      flex-wrap: wrap;
    }
    .feature-card {
      flex: 1;
      min-width: 200px;
      background: #1e293b;
      border-radius: 12px;
      padding: 24px;
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
    .feature-icon { font-size: 32px; }
    .feature-title { font-size: 16px; font-weight: 700; color: #f8fafc; }
    .feature-desc { font-size: 13px; color: #94a3b8; line-height: 1.6; }
  </style>
</head>
<body>
  <header>
    <div class="logo">TechGennis</div>
    <nav>
      <a href="#">Kurslar</a>
      <a href="#">Loyihalar</a>
      <a href="#">Haqida</a>
    </nav>
  </header>

  <div class="hero">
    <h1>Dasturlashni O'rgan 🚀</h1>
    <p>Kelajakdagi texnologiyalarni bugun o'rganishni boshlang</p>
    <div class="hero-btns">
      <a href="#" class="btn-primary">Hozir Boshlash</a>
      <a href="#" class="btn-outline">Ko'proq bilish</a>
    </div>
  </div>

  <div class="stats">
    <div class="stat-item"><span class="stat-num">500+</span><span class="stat-label">O'quvchilar</span></div>
    <div class="stat-item"><span class="stat-num">12</span><span class="stat-label">Kurslar</span></div>
    <div class="stat-item"><span class="stat-num">100%</span><span class="stat-label">Bepul</span></div>
    <div class="stat-item"><span class="stat-num">24/7</span><span class="stat-label">Qo'llab-quvvatlash</span></div>
  </div>

  <div class="features">
    <div class="feature-card">
      <span class="feature-icon">💡</span>
      <div class="feature-title">Amaliy Loyihalar</div>
      <div class="feature-desc">Har bir darsda real hayot loyihalari bilan ishlaysiz.</div>
    </div>
    <div class="feature-card">
      <span class="feature-icon">🎯</span>
      <div class="feature-title">Maqsadli Ta'lim</div>
      <div class="feature-desc">Tezkor va samarali o'rganish metodikasi.</div>
    </div>
    <div class="feature-card">
      <span class="feature-icon">🏆</span>
      <div class="feature-title">Sertifikat</div>
      <div class="feature-desc">Kursni tugatib sertifikat oling.</div>
    </div>
  </div>
</body>
</html>
