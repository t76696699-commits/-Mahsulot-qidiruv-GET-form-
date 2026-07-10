<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Login</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .login-card {
      background: white;
      border-radius: 20px;
      padding: 40px 36px;
      width: 380px;
      box-shadow: 0 20px 60px rgba(0,0,0,0.25);
    }

    .login-logo {
      text-align: center;
      font-size: 40px;
      margin-bottom: 8px;
    }
    .login-title {
      text-align: center;
      font-size: 22px;
      font-weight: 700;
      color: #1e293b;
      margin-bottom: 4px;
    }
    .login-sub {
      text-align: center;
      font-size: 13px;
      color: #94a3b8;
      margin-bottom: 28px;
    }

    .form-group { margin-bottom: 18px; }
    label {
      display: block;
      font-size: 13px;
      font-weight: 600;
      color: #374151;
      margin-bottom: 6px;
    }
    input[type="email"],
    input[type="password"],
    input[type="text"] {
      width: 100%;
      padding: 12px 14px;
      border: 1.5px solid #e2e8f0;
      border-radius: 10px;
      font-size: 14px;
      outline: none;
      transition: border-color 0.2s, box-shadow 0.2s;
      color: #1e293b;
    }
    input:focus {
      border-color: #7c3aed;
      box-shadow: 0 0 0 3px rgba(124,58,237,0.1);
    }
    input::placeholder { color: #cbd5e1; }

    .remember-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 24px;
    }
    .remember-row label {
      display: flex;
      align-items: center;
      gap: 6px;
      font-size: 13px;
      color: #64748b;
      font-weight: 400;
      cursor: pointer;
    }
    input[type="checkbox"] { width: auto; accent-color: #7c3aed; }
    .forgot { font-size: 13px; color: #7c3aed; text-decoration: none; }
    .forgot:hover { text-decoration: underline; }

    .login-btn {
      width: 100%;
      background: linear-gradient(135deg, #667eea, #764ba2);
      color: white;
      border: none;
      padding: 13px;
      border-radius: 10px;
      font-size: 15px;
      font-weight: 700;
      cursor: pointer;
      transition: opacity 0.2s, transform 0.1s;
    }
    .login-btn:hover { opacity: 0.9; transform: translateY(-1px); }
    .login-btn:active { transform: translateY(0); }

    .divider {
      text-align: center;
      margin: 20px 0;
      color: #94a3b8;
      font-size: 13px;
      position: relative;
    }
    .divider::before, .divider::after {
      content: '';
      position: absolute;
      top: 50%;
      width: 42%;
      height: 1px;
      background: #e2e8f0;
    }
    .divider::before { left: 0; }
    .divider::after { right: 0; }

    .register-link {
      text-align: center;
      font-size: 13px;
      color: #64748b;
      margin-top: 16px;
    }
    .register-link a { color: #7c3aed; font-weight: 600; text-decoration: none; }
  </style>
</head>
<body>
  <div class="login-card">
    <div class="login-logo">🔐</div>
    <div class="login-title">Xush kelibsiz!</div>
    <div class="login-sub">Akkauntingizga kiring</div>

    <form action="#" method="post">
      <div class="form-group">
        <label for="email">Email manzil</label>
        <input type="email" id="email" name="email" placeholder="email@example.com" required>
      </div>
      <div class="form-group">
        <label for="password">Parol</label>
        <input type="password" id="password" name="password" placeholder="Parolingizni kiriting" required>
      </div>
      <div class="remember-row">
        <label>
          <input type="checkbox" name="remember"> Eslab qolish
        </label>
        <a href="#" class="forgot">Parolni unutdingizmi?</a>
      </div>
      <button type="submit" class="login-btn">Kirish</button>
    </form>

    <div class="divider">yoki</div>
    <div class="register-link">
      Akkauntingiz yo'qmi? <a href="#">Ro'yxatdan o'ting</a>
    </div>
  </div>
</body>
</html>
