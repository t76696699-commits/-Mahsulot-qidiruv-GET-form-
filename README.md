<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>CSS Animatsiya — Asoslar</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #0f172a;
      color: white;
      padding: 40px;
    }
    h2 { color: #38bdf8; margin: 32px 0 16px; font-size: 18px; }
    h1 { font-size: 28px; margin-bottom: 8px; }

    /* ── 1. Fade In (paydo bo'lish) ── */
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(20px); }
      to   { opacity: 1; transform: translateY(0); }
    }
    .fade-box {
      background: #1e293b;
      padding: 20px 28px;
      border-radius: 12px;
      border-left: 4px solid #38bdf8;
      animation: fadeIn 1s ease-out forwards;
    }

    /* ── 2. Bounce (sakrash) ── */
    @keyframes bounce {
      0%, 100% { transform: translateY(0); }
      50%       { transform: translateY(-30px); }
    }
    .bounce-ball {
      width: 60px; height: 60px;
      background: radial-gradient(circle at 35% 35%, #f472b6, #7c3aed);
      border-radius: 50%;
      animation: bounce 1s ease-in-out infinite;
    }

    /* ── 3. Rotate (aylanish) ── */
    @keyframes spin {
      from { transform: rotate(0deg); }
      to   { transform: rotate(360deg); }
    }
    .spinner {
      width: 60px; height: 60px;
      border: 5px solid #1e293b;
      border-top-color: #38bdf8;
      border-radius: 50%;
      animation: spin 0.8s linear infinite;
    }

    /* ── 4. Pulse (yurak urishi) ── */
    @keyframes pulse {
      0%, 100% { transform: scale(1); opacity: 1; }
      50%       { transform: scale(1.15); opacity: 0.8; }
    }
    .pulse-btn {
      background: #e94560;
      color: white;
      border: none;
      padding: 12px 28px;
      border-radius: 24px;
      font-size: 16px;
      cursor: pointer;
      animation: pulse 1.5s ease-in-out infinite;
    }

    /* ── 5. Typing effect ── */
    @keyframes typing {
      from { width: 0; }
      to   { width: 100%; }
    }
    @keyframes blink {
      50% { border-color: transparent; }
    }
    .typing-text {
      font-family: 'Courier New', monospace;
      font-size: 20px;
      color: #22c55e;
      white-space: nowrap;
      overflow: hidden;
      border-right: 3px solid #22c55e;
      width: fit-content;
      animation: typing 3s steps(30) forwards, blink 0.7s step-end infinite;
    }

    /* ── 6. Gradient background harakat ── */
    @keyframes gradientMove {
      0%   { background-position: 0% 50%; }
      50%  { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }
    .gradient-box {
      padding: 24px 32px;
      border-radius: 14px;
      background: linear-gradient(270deg, #6366f1, #8b5cf6, #ec4899, #f43f5e);
      background-size: 400% 400%;
      animation: gradientMove 4s ease infinite;
      font-size: 18px;
      font-weight: 700;
    }

    .demo-row { display: flex; gap: 24px; flex-wrap: wrap; align-items: center; margin-bottom: 10px; }
  </style>
</head>
<body>
  <h1>🎬 CSS Animatsiya Namunalari</h1>

  <h2>1. Fade In (paydo bo'lish)</h2>
  <div class="fade-box">Bu element sekin paydo bo'ldi! ✨</div>

  <h2>2. Bounce (sakrash)</h2>
  <div class="demo-row">
    <div class="bounce-ball"></div>
  </div>

  <h2>3. Spinner (yuklash)</h2>
  <div class="demo-row">
    <div class="spinner"></div>
    <span style="color:#94a3b8">Yuklanmoqda...</span>
  </div>

  <h2>4. Pulse (nafas olish)</h2>
  <div class="demo-row">
    <button class="pulse-btn">❤️ Yoqtirish</button>
  </div>

  <h2>5. Typing Effect (yozuv)</h2>
  <div class="typing-text">Salom, men dasturchi bo'lmoqchiman!</div>

  <h2>6. Gradient Harakat</h2>
  <div class="gradient-box">🌈 Rangli gradient harakatda</div>
</body>
</html>
