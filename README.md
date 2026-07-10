<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>CSS Animatsiya — Ilg'or</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: 'Segoe UI', sans-serif; background: #030712; color: white; min-height: 100vh; }

    /* ── 1. Stagger (ketma-ket) animatsiya ── */
    .stagger-section { padding: 50px 40px; }
    h2 { font-size: 16px; color: #6366f1; letter-spacing: 3px; text-transform: uppercase; margin-bottom: 24px; }

    @keyframes slideUp {
      from { opacity: 0; transform: translateY(40px); }
      to   { opacity: 1; transform: translateY(0); }
    }
    .stagger-list { display: flex; gap: 16px; flex-wrap: wrap; }
    .stagger-item {
      background: #111827;
      border: 1px solid #1f2937;
      border-radius: 12px;
      padding: 20px;
      width: 160px;
      text-align: center;
      opacity: 0;
      animation: slideUp 0.6s cubic-bezier(0.25, 0.46, 0.45, 0.94) forwards;
      animation-delay: var(--delay, 0s);
    }
    .stagger-item .icon { font-size: 32px; margin-bottom: 8px; }
    .stagger-item .label { font-size: 13px; color: #9ca3af; }

    /* ── 2. Loading animatsiya ── */
    .loading-section { padding: 30px 40px; border-top: 1px solid #111827; }
    .dots { display: flex; gap: 10px; align-items: center; }
    @keyframes dotBounce {
      0%, 80%, 100% { transform: scale(0); opacity: 0.3; }
      40%            { transform: scale(1); opacity: 1; }
    }
    .dot {
      width: 14px; height: 14px;
      background: #6366f1;
      border-radius: 50%;
      animation: dotBounce 1.4s ease-in-out infinite;
      animation-delay: var(--delay, 0s);
    }

    /* ── 3. 3D Flip karta ── */
    .flip-section { padding: 30px 40px; border-top: 1px solid #111827; }
    .flip-wrap {
      perspective: 600px;
      width: 200px; height: 130px;
      cursor: pointer;
    }
    .flip-card {
      width: 100%; height: 100%;
      transform-style: preserve-3d;
      transition: transform 0.7s cubic-bezier(0.4, 0, 0.2, 1);
      position: relative;
    }
    .flip-wrap:hover .flip-card { transform: rotateY(180deg); }
    .flip-front, .flip-back {
      position: absolute;
      inset: 0;
      border-radius: 14px;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      gap: 6px;
      backface-visibility: hidden;
      font-weight: 700;
    }
    .flip-front { background: linear-gradient(135deg, #6366f1, #8b5cf6); font-size: 36px; }
    .flip-back {
      background: linear-gradient(135deg, #f59e0b, #ef4444);
      transform: rotateY(180deg);
      font-size: 15px;
      text-align: center;
      padding: 16px;
    }

    /* ── 4. Neon pulsatsiya ── */
    .neon-section { padding: 30px 40px; border-top: 1px solid #111827; }
    @keyframes neonPulse {
      0%, 100% {
        text-shadow: 0 0 10px #6366f1, 0 0 20px #6366f1, 0 0 40px #6366f1;
        opacity: 1;
      }
      50% {
        text-shadow: 0 0 5px #6366f1, 0 0 10px #6366f1;
        opacity: 0.8;
      }
    }
    .neon-text {
      font-size: 48px;
      font-weight: 900;
      color: #a5b4fc;
      letter-spacing: 4px;
      animation: neonPulse 2s ease-in-out infinite;
    }

    /* ── 5. Progress bar animatsiya ── */
    .progress-section { padding: 30px 40px; border-top: 1px solid #111827; }
    .skill { margin-bottom: 18px; }
    .skill-header { display: flex; justify-content: space-between; margin-bottom: 6px; font-size: 13px; color: #9ca3af; }
    .bar-bg { background: #1f2937; border-radius: 20px; height: 8px; overflow: hidden; }
    @keyframes fillBar { from { width: 0; } }
    .bar-fill {
      height: 100%;
      border-radius: 20px;
      background: linear-gradient(90deg, #6366f1, #8b5cf6);
      animation: fillBar 1.5s cubic-bezier(0.25, 0.46, 0.45, 0.94) forwards;
      animation-delay: var(--delay, 0s);
    }
  </style>
</head>
<body>

  <div class="stagger-section">
    <h2>Ketma-ket paydo bo'lish</h2>
    <div class="stagger-list">
      <div class="stagger-item" style="--delay:0.1s"><div class="icon">🌐</div><div class="label">HTML</div></div>
      <div class="stagger-item" style="--delay:0.25s"><div class="icon">🎨</div><div class="label">CSS</div></div>
      <div class="stagger-item" style="--delay:0.4s"><div class="icon">⚡</div><div class="label">JavaScript</div></div>
      <div class="stagger-item" style="--delay:0.55s"><div class="icon">🐍</div><div class="label">Python</div></div>
      <div class="stagger-item" style="--delay:0.7s"><div class="icon">⚛️</div><div class="label">React</div></div>
    </div>
  </div>

  <div class="loading-section">
    <h2>Yuklash animatsiyasi</h2>
    <div class="dots">
      <div class="dot" style="--delay:0s"></div>
      <div class="dot" style="--delay:0.2s"></div>
      <div class="dot" style="--delay:0.4s"></div>
      <span style="color:#6b7280;margin-left:8px">Yuklanmoqda...</span>
    </div>
  </div>

  <div class="flip-section">
    <h2>3D Flip Karta (hover qiling)</h2>
    <div class="flip-wrap">
      <div class="flip-card">
        <div class="flip-front">🎯</div>
        <div class="flip-back">
          <div>CSS bilan</div>
          <div>3D effekt!</div>
        </div>
      </div>
    </div>
  </div>

  <div class="neon-section">
    <h2>Neon Pulsatsiya</h2>
    <div class="neon-text">NEON</div>
  </div>

  <div class="progress-section">
    <h2>Ko'nikmalar</h2>
    <div class="skill">
      <div class="skill-header"><span>HTML/CSS</span><span>90%</span></div>
      <div class="bar-bg"><div class="bar-fill" style="width:90%;--delay:0.2s"></div></div>
    </div>
    <div class="skill">
      <div class="skill-header"><span>JavaScript</span><span>70%</span></div>
      <div class="bar-bg"><div class="bar-fill" style="width:70%;--delay:0.4s"></div></div>
    </div>
    <div class="skill">
      <div class="skill-header"><span>Python</span><span>60%</span></div>
      <div class="bar-bg"><div class="bar-fill" style="width:60%;--delay:0.6s"></div></div>
    </div>
  </div>

</body>
</html>
