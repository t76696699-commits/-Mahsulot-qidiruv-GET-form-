<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Parallax Effekti</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: 'Georgia', serif; }

    /* Parallax bo'lim */
    .parallax {
      min-height: 100vh;
      background-attachment: fixed;
      background-size: cover;
      background-position: center;
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
      color: white;
    }

    /* Har bir parallax uchun o'z foni */
    .parallax-1 {
      background-image: linear-gradient(rgba(0,0,0,0.55), rgba(0,0,0,0.55)),
        url("https://images.unsplash.com/photo-1451187580459-43490279c0fa?w=1600");
    }
    .parallax-2 {
      background-image: linear-gradient(rgba(10,20,80,0.7), rgba(10,20,80,0.7)),
        url("https://images.unsplash.com/photo-1534796636912-3b95b3ab5986?w=1600");
    }
    .parallax-3 {
      background-image: linear-gradient(rgba(0,60,0,0.65), rgba(0,60,0,0.65)),
        url("https://images.unsplash.com/photo-1510915361894-db8b60106cb1?w=1600");
    }

    .parallax-content { max-width: 700px; padding: 40px; }
    .parallax-label {
      font-size: 13px;
      letter-spacing: 6px;
      text-transform: uppercase;
      color: rgba(255,255,255,0.7);
      margin-bottom: 16px;
    }
    .parallax-title { font-size: 56px; font-weight: 900; line-height: 1.1; margin-bottom: 16px; }
    .parallax-desc { font-size: 18px; color: rgba(255,255,255,0.85); line-height: 1.7; margin-bottom: 28px; }
    .parallax-btn {
      border: 2px solid white;
      color: white;
      padding: 14px 36px;
      text-decoration: none;
      font-size: 14px;
      letter-spacing: 3px;
      text-transform: uppercase;
      transition: background 0.3s, color 0.3s;
    }
    .parallax-btn:hover { background: white; color: #0a0a0a; }

    /* Oddiy kontent bo'limlari */
    .content-section {
      padding: 80px 40px;
      text-align: center;
      background: white;
    }
    .content-section.dark {
      background: #0f172a;
      color: white;
    }
    .content-section h2 { font-size: 32px; margin-bottom: 16px; color: #1e293b; }
    .content-section.dark h2 { color: white; }
    .content-section p { font-size: 16px; color: #64748b; max-width: 600px; margin: 0 auto; line-height: 1.8; }
    .content-section.dark p { color: #94a3b8; }

    .features-row {
      display: flex;
      gap: 32px;
      justify-content: center;
      margin-top: 40px;
      flex-wrap: wrap;
    }
    .feature {
      background: #f8fafc;
      border-radius: 16px;
      padding: 28px;
      width: 220px;
      text-align: center;
      border: 1px solid #e2e8f0;
    }
    .content-section.dark .feature { background: #1e293b; border-color: #334155; }
    .feature .fi { font-size: 36px; margin-bottom: 12px; }
    .feature h3 { font-size: 15px; font-weight: 700; color: #1e293b; margin-bottom: 6px; }
    .content-section.dark .feature h3 { color: white; }
    .feature p { font-size: 12px; color: #64748b; }
    .content-section.dark .feature p { color: #94a3b8; }
  </style>
</head>
<body>

  <!-- 1-parallax: Salom -->
  <div class="parallax parallax-1">
    <div class="parallax-content">
      <div class="parallax-label">Parallax effekti</div>
      <div class="parallax-title">Kelajak Boshlanadi</div>
      <div class="parallax-desc">Scroll qilib ko'ring — fon va kontent turli tezlikda harakatlanadi</div>
      <a href="#" class="parallax-btn">Kashf Etish</a>
    </div>
  </div>

  <!-- Kontent bo'limi -->
  <div class="content-section">
    <h2>Nima o'rganasiz?</h2>
    <p>Bizning kursimizda HTML, CSS va JavaScript orqali professional veb-sahifalar yaratishni o'rganasiz.</p>
    <div class="features-row">
      <div class="feature"><div class="fi">🌐</div><h3>HTML</h3><p>Sahifa tuzilmasi</p></div>
      <div class="feature"><div class="fi">🎨</div><h3>CSS</h3><p>Sahifa ko'rinishi</p></div>
      <div class="feature"><div class="fi">⚡</div><h3>JS</h3><p>Interaktivlik</p></div>
    </div>
  </div>

  <!-- 2-parallax: Kurslar -->
  <div class="parallax parallax-2">
    <div class="parallax-content">
      <div class="parallax-label">Bizning kurslar</div>
      <div class="parallax-title">Professional Darajada O'rgan</div>
      <div class="parallax-desc">Har bir dars amaliy loyiha bilan mustahkamlanadi</div>
      <a href="#" class="parallax-btn">Kurslarni Ko'rish</a>
    </div>
  </div>

  <!-- Kontent bo'limi -->
  <div class="content-section dark">
    <h2>Statistika</h2>
    <p>Biz bilan ming minglab o'quvchilar muvaffaqiyatga erishgan</p>
    <div class="features-row">
      <div class="feature"><div class="fi">👥</div><h3>500+ O'quvchi</h3><p>Faol o'quvchilar</p></div>
      <div class="feature"><div class="fi">📚</div><h3>12 Kurs</h3><p>Turli yo'nalishlar</p></div>
      <div class="feature"><div class="fi">⭐</div><h3>4.9/5</h3><p>O'quvchilar bahosi</p></div>
    </div>
  </div>

  <!-- 3-parallax: Yakuniy -->
  <div class="parallax parallax-3">
    <div class="parallax-content">
      <div class="parallax-label">Hoziroq boshlang</div>
      <div class="parallax-title">O'z Kelajagingizni Quring</div>
      <div class="parallax-desc">Dasturlash — kelajakning tili. Bugun o'rganishni boshlang!</div>
      <a href="#" class="parallax-btn">Ro'yxatdan O'tish</a>
    </div>
  </div>

</body>
</html>
