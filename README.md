<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Ro'yxat Dizayni</title>
  <style>
    body { font-family: 'Segoe UI', sans-serif; padding: 30px; background: #1a1a2e; color: #eee; }
    h2 { color: #e94560; margin-bottom: 20px; }

    /* Oddiy tartibsiz ro'yxat */
    .simple-list {
      list-style: none;
      padding: 0;
    }
    .simple-list li {
      padding: 12px 16px;
      margin: 6px 0;
      background: #16213e;
      border-left: 4px solid #e94560;
      border-radius: 4px;
      transition: transform 0.2s;
    }
    .simple-list li:hover { transform: translateX(8px); }

    /* Sonli ro'yxat */
    .numbered-list {
      counter-reset: step;
      list-style: none;
      padding: 0;
      margin-top: 20px;
    }
    .numbered-list li {
      counter-increment: step;
      display: flex;
      align-items: center;
      gap: 14px;
      padding: 12px 16px;
      margin: 8px 0;
      background: #16213e;
      border-radius: 8px;
    }
    .numbered-list li::before {
      content: counter(step);
      background: #e94560;
      color: white;
      width: 30px;
      height: 30px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: bold;
      flex-shrink: 0;
    }
  </style>
</head>
<body>
  <h2>🛠️ Texnologiyalar</h2>
  <ul class="simple-list">
    <li>HTML — Sahifa tuzilmasi</li>
    <li>CSS — Sahifa ko'rinishi</li>
    <li>JavaScript — Interaktivlik</li>
    <li>Python — Backend</li>
  </ul>

  <h2>📚 O'rganish bosqichlari</h2>
  <ol class="numbered-list">
    <li>HTML asoslarini o'rgan</li>
    <li>CSS bilan bezashni o'rgan</li>
    <li>JavaScript qo'sh</li>
    <li>Framework o'rgan</li>
    <li>Loyiha qur</li>
  </ol>
</body>
</html>
