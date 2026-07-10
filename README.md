<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Ro'yxatdan O'tish</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 40px 20px;
    }
    .card {
      background: white;
      border-radius: 20px;
      padding: 36px;
      width: 100%;
      max-width: 480px;
      box-shadow: 0 20px 60px rgba(0,0,0,0.3);
    }
    h2 { font-size: 22px; font-weight: 800; color: #1e293b; margin-bottom: 4px; }
    .sub { font-size: 13px; color: #94a3b8; margin-bottom: 24px; }

    .row { display: flex; gap: 14px; }
    .row .form-group { flex: 1; }

    .form-group { margin-bottom: 16px; }
    label {
      display: block;
      font-size: 12px;
      font-weight: 700;
      color: #374151;
      margin-bottom: 5px;
      text-transform: uppercase;
      letter-spacing: 0.5px;
    }
    input, select, textarea {
      width: 100%;
      padding: 11px 13px;
      border: 1.5px solid #e2e8f0;
      border-radius: 9px;
      font-size: 13px;
      outline: none;
      transition: border-color 0.2s;
      color: #1e293b;
      font-family: inherit;
    }
    input:focus, select:focus, textarea:focus {
      border-color: #0ea5e9;
      box-shadow: 0 0 0 3px rgba(14,165,233,0.1);
    }
    input::placeholder, textarea::placeholder { color: #cbd5e1; }
    textarea { resize: vertical; min-height: 80px; }

    .radio-group { display: flex; gap: 16px; margin-top: 4px; }
    .radio-opt { display: flex; align-items: center; gap: 6px; cursor: pointer; font-size: 13px; color: #374151; }
    input[type="radio"] { width: auto; accent-color: #0ea5e9; }

    .agree-row { display: flex; align-items: flex-start; gap: 8px; margin-bottom: 20px; }
    .agree-row input { width: auto; margin-top: 2px; accent-color: #0ea5e9; }
    .agree-row label { font-size: 12px; color: #64748b; text-transform: none; font-weight: 400; letter-spacing: 0; }
    .agree-row a { color: #0ea5e9; }

    .submit-btn {
      width: 100%;
      background: linear-gradient(135deg, #0ea5e9, #0284c7);
      color: white;
      border: none;
      padding: 13px;
      border-radius: 10px;
      font-size: 15px;
      font-weight: 700;
      cursor: pointer;
      transition: opacity 0.2s;
    }
    .submit-btn:hover { opacity: 0.88; }

    .login-link { text-align: center; font-size: 13px; color: #64748b; margin-top: 14px; }
    .login-link a { color: #0ea5e9; font-weight: 600; text-decoration: none; }
  </style>
</head>
<body>
  <div class="card">
    <h2>Ro'yxatdan O'ting 🚀</h2>
    <p class="sub">Akkaunt yaratib, o'rganishni boshlang</p>

    <form action="#" method="post">
      <div class="row">
        <div class="form-group">
          <label for="fname">Ism</label>
          <input type="text" id="fname" name="fname" placeholder="Alijon" required minlength="2">
        </div>
        <div class="form-group">
          <label for="lname">Familiya</label>
          <input type="text" id="lname" name="lname" placeholder="Toshmatov" required minlength="2">
        </div>
      </div>

      <div class="form-group">
        <label for="reg-email">Email</label>
        <input type="email" id="reg-email" name="email" placeholder="email@example.com" required>
      </div>

      <div class="form-group">
        <label for="phone">Telefon raqam</label>
        <input type="tel" id="phone" name="phone" placeholder="+998 90 123 45 67">
      </div>

      <div class="form-group">
        <label for="reg-pass">Parol</label>
        <input type="password" id="reg-pass" name="password" placeholder="Kamida 8 ta belgi" required minlength="8">
      </div>

      <div class="form-group">
        <label for="city">Shahar</label>
        <select id="city" name="city">
          <option value="">Tanlang...</option>
          <option value="tashkent">Toshkent</option>
          <option value="samarkand">Samarqand</option>
          <option value="bukhara">Buxoro</option>
          <option value="namangan">Namangan</option>
          <option value="andijan">Andijon</option>
        </select>
      </div>

      <div class="form-group">
        <label>Jinsi</label>
        <div class="radio-group">
          <label class="radio-opt"><input type="radio" name="gender" value="male"> Erkak</label>
          <label class="radio-opt"><input type="radio" name="gender" value="female"> Ayol</label>
        </div>
      </div>

      <div class="form-group">
        <label for="about">O'zingiz haqingizda</label>
        <textarea id="about" name="about" placeholder="Qisqacha ma'lumot..."></textarea>
      </div>

      <div class="agree-row">
        <input type="checkbox" id="agree" name="agree" required>
        <label for="agree">
          <a href="#">Foydalanish shartlari</a> va
          <a href="#">Maxfiylik siyosati</a> ga roziman
        </label>
      </div>

      <button type="submit" class="submit-btn">Akkaunt Yaratish</button>
    </form>

    <div class="login-link">Akkauntingiz bormi? <a href="#">Kirish</a></div>
  </div>
</body>
</html>
