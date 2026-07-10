<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Shaxsiy Kartochka</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #667eea, #764ba2);
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 40px;
    }

    .profile-card {
      background: white;
      border-radius: 20px;
      width: 340px;
      padding-bottom: 28px;
      box-shadow: 0 20px 60px rgba(0,0,0,0.3);
      position: relative; /* absolute bola uchun havola */
      overflow: visible;
      text-align: center;
    }

    /* Fon rasmi */
    .card-cover {
      height: 120px;
      background: linear-gradient(135deg, #667eea, #764ba2);
      border-radius: 20px 20px 0 0;
      position: relative;
    }

    /* Avatar — absolute bilan markazga chiqarilgan */
    .avatar {
      width: 90px;
      height: 90px;
      border-radius: 50%;
      background: linear-gradient(135deg, #f093fb, #f5576c);
      border: 4px solid white;
      position: absolute;
      bottom: -45px;
      left: 50%;
      transform: translateX(-50%);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 36px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.2);
    }

    /* Online badge — absolute bilan joylashtirilgan */
    .online-badge {
      width: 18px;
      height: 18px;
      background: #22c55e;
      border: 3px solid white;
      border-radius: 50%;
      position: absolute;
      bottom: -45px + 8px;
      right: calc(50% - 52px);
      z-index: 1;
    }

    .card-body { padding: 56px 24px 0; }
    .name { font-size: 22px; font-weight: 700; color: #1e293b; }
    .role { font-size: 13px; color: #7c3aed; font-weight: 600; margin: 4px 0 12px; }
    .bio { font-size: 13px; color: #64748b; line-height: 1.6; margin-bottom: 20px; }

    .stats {
      display: flex;
      justify-content: space-around;
      padding: 16px 0;
      border-top: 1px solid #f1f5f9;
      border-bottom: 1px solid #f1f5f9;
      margin-bottom: 20px;
    }
    .stat { display: flex; flex-direction: column; align-items: center; gap: 2px; }
    .stat-n { font-size: 18px; font-weight: 800; color: #1e293b; }
    .stat-l { font-size: 11px; color: #94a3b8; }

    .follow-btn {
      background: #7c3aed;
      color: white;
      border: none;
      padding: 10px 32px;
      border-radius: 24px;
      font-size: 14px;
      font-weight: 600;
      cursor: pointer;
      transition: opacity 0.2s;
    }
    .follow-btn:hover { opacity: 0.85; }

    /* Fixed badge — ekranning pastida */
    .fixed-badge {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background: #7c3aed;
      color: white;
      padding: 10px 16px;
      border-radius: 24px;
      font-size: 12px;
      font-weight: 600;
      box-shadow: 0 4px 15px rgba(124,58,237,0.4);
    }
  </style>
</head>
<body>
  <div class="profile-card">
    <div class="card-cover">
      <div class="avatar">👨‍💻</div>
      <div class="online-badge"></div>
    </div>
    <div class="card-body">
      <div class="name">Alisher Toshmatov</div>
      <div class="role">Frontend Developer</div>
      <div class="bio">HTML, CSS va JavaScript o'rganayapman. Kelajakda Full-Stack developer bo'lishni xohlayman.</div>
      <div class="stats">
        <div class="stat"><span class="stat-n">128</span><span class="stat-l">Loyihalar</span></div>
        <div class="stat"><span class="stat-n">2.4k</span><span class="stat-l">Izlovchilar</span></div>
        <div class="stat"><span class="stat-n">342</span><span class="stat-l">Izlash</span></div>
      </div>
      <button class="follow-btn">Izlash</button>
    </div>
  </div>

  <div class="fixed-badge">📌 position: fixed</div>
</body>
</html>
