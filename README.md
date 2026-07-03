Siz so'ragan struktura bo'yicha CSS Box Model tamoyillariga asoslangan, toza va tushunarli kod namunasi.

HTML strukturasi (index.html)
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Layout Structure</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <header class="header">Header</header>

    <main class="main">
        <div class="left-col">Left Column</div>
        <div class="center-col">Center Column</div>
        <div class="right-col">Right Column</div>
    </main>

    <footer class="footer">Footer</footer>

</body>
</html>
CSS uslublari (style.css)
CSS
/* Global o'rnatishlar */
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    max-width: 1200px;
    margin: 0 auto; /* Sahifani markazlashtirish */
    font-family: sans-serif;
}

/* Umumiy bo'limlar */
.header, .footer {
    background-color: #333;
    color: white;
    padding: 20px;
    border: 2px solid #000;
    margin-bottom: 10px;
}

.main {
    display: flex; /* Ustunlarni yonma-yon qilish uchun */
    gap: 10px;
    margin-bottom: 10px;
}

/* Ustunlar uslubi */
.left-col, .center-col, .right-col {
    padding: 20px;
    border: 2px solid #555;
    background-color: #f4f4f4;
}

.left-col, .right-col {
    flex: 1; /* Yon ustunlar */
}

.center-col {
    flex: 2; /* Markaziy ustun kengroq */
    background-color: #ddd;
}
