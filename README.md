<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>CSS Position — Ilg'or</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: 'Segoe UI', sans-serif; background: #f8fafc; }

    /* ── 1. Dropdown Navigatsiya ── */
    .nav { background: #1e293b; padding: 0 40px; display: flex; gap: 4px; }
    .nav-item { position: relative; }
    .nav-item > a {
      display: block;
      color: #cbd5e1; text-decoration: none;
      padding: 16px 18px; font-size: 14px;
      transition: background 0.2s;
    }
    .nav-item:hover > a { background: #334155; color: white; }

    /* Dropdown panel */
    .dropdown {
      position: absolute;
      top: 100%;
      left: 0;
      background: white;
      border-radius: 0 0 10px 10px;
      box-shadow: 0 8px 24px rgba(0,0,0,0.15);
      min-width: 200px;
      display: none;
      z-index: 999;
    }
    .nav-item:hover .dropdown { display: block; }
    .dropdown a {
      display: block;
      padding: 12px 20px;
      color: #374151;
      text-decoration: none;
      font-size: 13px;
      border-bottom: 1px solid #f3f4f6;
      transition: background 0.15s;
    }
    .dropdown a:hover { background: #ede9fe; color: #7c3aed; }
    .dropdown a:last-child { border-bottom: none; }

    /* ── 2. Overlay kartochka ── */
    .section { padding: 40px; }
    h2 { font-size: 20px; color: #1e293b; margin-bottom: 16px; }

    .overlay-card {
      position: relative;
      width: 280px;
      border-radius: 16px;
      overflow: hidden;
      cursor: pointer;
    }
    .overlay-bg {
      width: 100%;
      height: 200px;
      background: linear-gradient(135deg, #6366f1, #8b5cf6);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 64px;
    }
    .overlay-layer {
      position: absolute;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background: rgba(0,0,0,0.7);
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      gap: 8px;
      opacity: 0;
      transition: opacity 0.3s;
      color: white;
    }
    .overlay-card:hover .overlay-layer { opacity: 1; }
    .overlay-layer h3 { font-size: 20px; }
    .overlay-layer p { font-size: 13px; color: #ddd; }

    /* ── 3. Tooltip ── */
    .tooltip-demo { display: flex; gap: 20px; flex-wrap: wrap; margin-top: 12px; }
    .tooltip-wrap {
      position: relative;
      display: inline-block;
    }
    .tooltip-btn {
      background: #7c3aed; color: white;
      border: none; padding: 10px 20px;
      border-radius: 8px; cursor: pointer;
      font-size: 14px; font-weight: 600;
    }
    .tooltip-text {
      position: absolute;
      bottom: calc(100% + 8px);
      left: 50%;
      transform: translateX(-50%);
      background: #1e293b;
      color: white;
      padding: 6px 12px;
      border-radius: 6px;
      font-size: 12px;
      white-space: nowrap;
      display: none;
      z-index: 10;
    }
    .tooltip-text::after {
      content: '';
      position: absolute;
      top: 100%; left: 50%;
      transform: translateX(-50%);
      border: 5px solid transparent;
      border-top-color: #1e293b;
    }
    .tooltip-wrap:hover .tooltip-text { display: block; }
  </style>
</head>
<body>
  <!-- Dropdown Navigatsiya -->
  <nav class="nav">
    <div class="nav-item">
      <a href="#">Kurslar ▾</a>
      <div class="dropdown">
        <a href="#">HTML / CSS</a>
        <a href="#">JavaScript</a>
        <a href="#">Python</a>
        <a href="#">React</a>
      </div>
    </div>
    <div class="nav-item">
      <a href="#">Loyihalar ▾</a>
      <div class="dropdown">
        <a href="#">Mini-loyihalar</a>
        <a href="#">Katta loyihalar</a>
      </div>
    </div>
    <div class="nav-item"><a href="#">Haqida</a></div>
  </nav>

  <!-- Overlay -->
  <div class="section">
    <h2>🖼️ Overlay Effekti (hover qiling)</h2>
    <div class="overlay-card">
      <div class="overlay-bg">🐍</div>
      <div class="overlay-layer">
        <h3>Python Kursi</h3>
        <p>Boshlang'ich daraja</p>
      </div>
    </div>
  </div>

  <!-- Tooltip -->
  <div class="section">
    <h2>💬 Tooltip (hover qiling)</h2>
    <div class="tooltip-demo">
      <div class="tooltip-wrap">
        <button class="tooltip-btn">Saqlash</button>
        <span class="tooltip-text">Ma'lumotni saqlash</span>
      </div>
      <div class="tooltip-wrap">
        <button class="tooltip-btn">O'chirish</button>
        <span class="tooltip-text">Elementni o'chirish</span>
      </div>
      <div class="tooltip-wrap">
        <button class="tooltip-btn">Tahrirlash</button>
        <span class="tooltip-text">Elementni tahrirlash</span>
      </div>
    </div>
  </div>
</body>
</html>
