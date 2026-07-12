Bootstrap 5 nima?
Bootstrap — dunyodagi eng mashhur CSS framework. U tayyor UI komponentlar, responsive grid system va yuzlab utility klasslarni taqdim etadi. Bootstrap 5 jQuery-dan voz kechib, sof JavaScript bilan ishlaydi.

CDN orqali ulash
<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Bootstrap Loyiha</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
  <!-- Kontent shu yerda -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
Grid System — 12 ustunli tizim
Bootstrap grid 12 ustundan iborat. Har bir kontent bloki 1 dan 12 gacha ustun egallashi mumkin. container → row → col ierarxiyasi qat'iy.

Asosiy Grid misoli
<div class="container">
  <div class="row">
    <div class="col-md-4 bg-primary text-white p-3">4 ustun</div>
    <div class="col-md-4 bg-secondary text-white p-3">4 ustun</div>
    <div class="col-md-4 bg-success text-white p-3">4 ustun</div>
  </div>
</div>
Responsive Breakpointlar
<!-- xs: 576px gacha — col (prefikssiz) -->
<!-- sm: 576px+ — col-sm-* -->
<!-- md: 768px+ — col-md-* -->
<!-- lg: 992px+ — col-lg-* -->
<!-- xl: 1200px+ — col-xl-* -->
<!-- xxl: 1400px+ — col-xxl-* -->

<div class="container">
  <div class="row g-3">
    <!-- Mobilda to'liq, planshetda yarim, desktopda 1/3 -->
    <div class="col-12 col-sm-6 col-lg-4">Karta 1</div>
    <div class="col-12 col-sm-6 col-lg-4">Karta 2</div>
    <div class="col-12 col-sm-6 col-lg-4">Karta 3</div>
  </div>
</div>
Offset va Order
<div class="row">
  <div class="col-md-4 offset-md-4">Markazda (offset bilan)</div>
</div>

<div class="row">
  <div class="col order-3">Uchinchi ko'rinadi</div>
  <div class="col order-1">Birinchi ko'rinadi</div>
  <div class="col order-2">Ikkinchi ko'rinadi</div>
</div>
Container turlari
<div class="container">Har bir breakpointda max-width bor</div>
<div class="container-fluid">Har doim 100% kenglik</div>
<div class="container-md">md dan katta bo'lsa max-width, kichikda fluid</div>
Mini-Loyiha: Responsive Portfolio Grid
Maqsad: Bootstrap Grid yordamida 6 ta portfolio ishini responsive sathka joylashtiring.

<div class="container py-5">
  <h2 class="text-center mb-4">Portfolio</h2>
  <div class="row g-4">
    <div class="col-12 col-sm-6 col-lg-4">
      <div class="card">
        <div class="card-body">Loyiha 1</div>
      </div>
    </div>
    <!-- Qolgan 5 ta karta -->
  </div>
</div>
1. 6 ta karta yarating, har birida rasm o'rniga rangli placeholder div qo'ying
2. Mobilda 1 ta, sm da 2 ta, lg da 3 ta ustunda ko'rsating
3. g-4 gap va har bir kartaga shadow-sm qo'shing
