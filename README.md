<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CSS Animatsiyalar va Effektlar</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

  <main class="container">
    <h1>CSS Vizual Effektlar Namunasi</h1>

    <!-- 1. Transition bilan hover effektli tugma -->
    <section class="example-section">
      <h2>1. Silliq Hover Effektli Tugma (Transition)</h2>
      <button class="animated-btn">O'rganishni boshlash</button>
    </section>

    <!-- 2. @keyframes bilan cheksiz aylanadigan spinner -->
    <section class="example-section">
      <h2>2. Cheksiz Aylanadigan Spinner (@keyframes)</h2>
      <div class="spinner-container">
        <div class="loading-spinner" aria-label="Yuklanmoqda..."></div>
      </div>
    </section>

    <!-- 3. Kartaga hover ko'tarilish effekti -->
    <section class="example-section">
      <h2>3. Hover Ko'tarilish Effekti (Card Lift)</h2>
      <div class="card-grid">
        <div class="interactive-card">
          <div class="card-image-placeholder"></div>
          <h3>Interaktiv Karta</h3>
          <p>Sichqonchani ushbu karta ustiga olib kelib, yuqoriga silliq ko'tarilishi va soya effekti chuqurlashishini ko'ring.</p>
        </div>
      </div>
    </section>
  </main>

</body>
</html>
