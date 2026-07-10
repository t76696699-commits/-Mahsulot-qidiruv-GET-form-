<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Navigatsiya Paneli</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: 'Segoe UI', sans-serif; }

    nav {
      background: #0f172a;
      position: sticky;
      top: 0;
      z-index: 100;
      box-shadow: 0 2px 20px rgba(0,0,0,0.3);
    }
    .nav-container {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0 40px;
      height: 64px;
    }
    .logo {
      color: #38bdf8;
      font-size: 22px;
      font-weight: 700;
      text-decoration: none;
    }
    .nav-links {
      list-style: none;
      display: flex;
      gap: 8px;
    }
    .nav-links a {
      color: #cbd5e1;
      text-decoration: none;
      padding: 8px 16px;
      border-radius: 8px;
      font-size: 14px;
      font-weight: 500;
      transition: background 0.2s, color 0.2s;
    }
    .nav-links a:hover {
      background: #1e293b;
      color: #38bdf8;
    }
    .nav-links a.active {
      background: #38bdf8;
      color: #0f172a;
    }
    .nav-btn {
      background: #38bdf8;
      color: #0f172a;
      padding: 9px 20px;
      border-radius: 8px;
      font-weight: 700;
      text-decoration: none;
      font-size: 14px;
      transition: opacity 0.2s;
    }
    .nav-btn:hover { opacity: 0.85; }

    /* Sahifa kontenti */
    section { padding: 80px 40px; min-height: 50vh; }
    section:nth-child(even) { background: #f8fafc; }
    h1 { font-size: 36px; color: #0f172a; margin-bottom: 12px; }
    p { color: #64748b; font-size: 16px; }
  </style>
</head>
<body>
  <nav>
    <div class="nav-container">
      <a href="#" class="logo">TechGennis</a>
      <ul class="nav-links">
        <li><a href="#bosh" class="active">Bosh sahifa</a></li>
        <li><a href="#kurslar">Kurslar</a></li>
        <li><a href="#haqida">Haqimizda</a></li>
        <li><a href="#aloqa">Aloqa</a></li>
      </ul>
      <a href="#" class="nav-btn">Boshlash</a>
    </div>
  </nav>

  <section id="bosh">
    <h1>Xush kelibsiz! 👋</h1>
    <p>Bu navigatsiya paneli mini-loyihasidir. Menyu bo'ylab harakatlaning.</p>
  </section>
  <section id="kurslar">
    <h1>Kurslar 📚</h1>
    <p>HTML, CSS, JavaScript va Python kurslarimiz mavjud.</p>
  </section>
</body>
</html>
