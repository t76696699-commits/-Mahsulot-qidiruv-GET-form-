<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Zamonaviy Aloqa Formasi</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <div class="form-container">
        <h2>Aloqa Formasi</h2>
        <form>
            <!-- Ism maydoni -->
            <div class="form-group">
                <label for="name">Ismingiz</label>
                <input type="text" id="name" placeholder="Masalan: Sardor" required minlength="3">
                <span class="validation-msg">Kamida 3 ta harf kiriting</span>
            </div>

            <!-- Email maydoni -->
            <div class="form-group">
                <label for="email">Email manzilingiz</label>
                <input type="email" id="email" placeholder="example@gmail.com" required>
                <span class="validation-msg">To'g'ri email manzilini kiriting</span>
            </div>

            <!-- Xabar maydoni -->
            <div class="form-group">
                <label for="message">Xabaringiz</label>
                <textarea id="message" rows="5" placeholder="Xabaringizni shu yerga yozing..." required></textarea>
                <span class="validation-msg">Xabar maydoni bo'sh bo'lmasligi kerak</span>
            </div>

            <!-- Custom Checkbox (Shartlarga rozilik) -->
            <div class="checkbox-group">
                <label class="custom-checkbox">
                    <input type="checkbox" required>
                    <span class="checkmark"></span>
                    Shartlarga roziman
                </label>
            </div>

            <button type="submit" class="submit-btn">Yuborish</button>
        </form>
    </div>

</body>
</html>
