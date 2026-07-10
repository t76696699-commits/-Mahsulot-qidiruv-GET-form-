<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Bugatti — Hashamatli Avtomobillar</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: 'Georgia', serif; background: #0a0a0a; color: white; }

    /* Navigatsiya */
    .navbar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 20px 60px;
      background: rgba(0,0,0,0.9);
      position: fixed;
      width: 100%;
      top: 0;
      z-index: 100;
      border-bottom: 1px solid #b8860b;
    }
    .brand { font-size: 24px; font-weight: 900; letter-spacing: 6px; color: #b8860b; }
    .nav-menu { display: flex; gap: 32px; list-style: none; }
    .nav-menu a { color: #ccc; text-decoration: none; letter-spacing: 2px; font-size: 13px; }
    .nav-menu a:hover { color: #b8860b; }

    /* Hero */
    .hero {
      min-height: 100vh;
      background: linear-gradient(135deg, #0a0a0a 0%, #1a0a00 50%, #0a0a0a 100%);
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      text-align: center;
      padding: 80px 40px 40px;
      position: relative;
      overflow: hidden;
    }
    .hero::before {
      content: '🏎️';
      font-size: 200px;
      position: absolute;
      right: 5%;
      top: 50%;
      transform: translateY(-50%);
      opacity: 0.15;
    }
    .hero-label {
      letter-spacing: 8px;
      color: #b8860b;
      font-size: 13px;
      text-transform: uppercase;
      margin-bottom: 16px;
    }
    .hero h1 {
      font-size: 72px;
      font-weight: 900;
      letter-spacing: 4px;
      text-shadow: 0 0 40px rgba(184,134,11,0.4);
      margin-bottom: 16px;
    }
    .hero p { color: #999; font-size: 18px; max-width: 500px; line-height: 1.6; margin-bottom: 32px; }
    .hero-btn {
      border: 2px solid #b8860b;
      color: #b8860b;
      padding: 14px 40px;
      text-decoration: none;
      letter-spacing: 4px;
      font-size: 13px;
      text-transform: uppercase;
      transition: background 0.3s, color 0.3s;
    }
    .hero-btn:hover { background: #b8860b; color: black; }

    /* Specs */
    .specs {
      display: flex;
      justify-content: center;
      gap: 0;
      background: #111;
      border-top: 1px solid #b8860b;
      border-bottom: 1px solid #b8860b;
    }
    .spec-item {
      flex: 1;
      padding: 32px;
      text-align: center;
      border-right: 1px solid #222;
    }
    .spec-item:last-child { border-right: none; }
    .spec-num { font-size: 40px; font-weight: 900; color: #b8860b; }
    .spec-unit { font-size: 16px; color: #b8860b; }
    .spec-label { font-size: 12px; color: #666; letter-spacing: 2px; margin-top: 4px; text-transform: uppercase; }

    /* Footer */
    footer {
      text-align: center;
      padding: 30px;
      background: #050505;
      color: #444;
      letter-spacing: 2px;
      font-size: 12px;
      border-top: 1px solid #b8860b;
    }
  </style>
</head>
<body>
  <nav class="navbar">
    <div class="brand">BUGATTI</div>
    <ul class="nav-menu">
      <li><a href="#">Modellar</a></li>
      <li><a href="#">Texnik</a></li>
      <li><a href="#">Tarix</a></li>
      <li><a href="#">Aloqa</a></li>
    </ul>
  </nav>

  <div class="hero">
    <div class="hero-label">Hashamat va tezlik</div>
    <h1>CHIRON</h1>
    <p>Dunyoning eng tez va eng hashamatli avtomobili. Muhandislikning cho'qqisi.</p>
    <a href="#" class="hero-btn">Kashf Etish</a>
  </div>

  <div class="specs">
    <div class="spec-item">
      <div class="spec-num">1500<span class="spec-unit">л.к.</span></div>
      <div class="spec-label">Quvvat</div>
    </div>
    <div class="spec-item">
      <div class="spec-num">2.4<span class="spec-unit">s</span></div>
      <div class="spec-label">0-100 km/h</div>
    </div>
    <div class="spec-item">
      <div class="spec-num">420<span class="spec-unit">км/с</span></div>
      <div class="spec-label">Max tezlik</div>
    </div>
    <div class="spec-item">
      <div class="spec-num">3M<span class="spec-unit">€</span></div>
      <div class="spec-label">Narxi</div>
    </div>
  </div>

  <footer>© 2024 BUGATTI AUTOMOBILES S.A.S.</footer>
</body>
</html>
