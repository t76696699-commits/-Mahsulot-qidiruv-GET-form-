<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tailwind CSS Responsive Grid Layout</title>
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 py-12">

  <!-- 6. Konteyner: maksimal kenglik (max-w-6xl), markazlashtirish (mx-auto) va ichki yon masofa (px-4) -->
  <main class="max-w-6xl mx-auto px-4">
    
    <div class="mb-8 text-center sm:text-left">
      <h1 class="text-3xl font-extrabold text-gray-900 tracking-tight">Kashf qiling</h1>
      <p class="text-gray-500 mt-2">Eng so'nggi va ommabop maqolalar hamda mahsulotlar to'plami.</p>
    </div>

    <!-- 1. Grid layout: mobilda 1 ta, planshetda 2 ta (md:grid-cols-2), desktopda 3 ta ustun (lg:grid-cols-3) -->
    <!-- 5. Gap (oraliq): kartalar orasidagi masofa gap-6 (24px) -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      
      <!-- 3. KAMIDA 6 TA KARTA (Karta 1) -->
      <!-- 4. Hover effekt: shadow-xl soya va scale-102 (kattalashish) effekti -->
      <article class="bg-white rounded-2xl overflow-hidden shadow-sm border border-gray-100 transition-all duration-300 hover:shadow-xl hover:-translate-y-1">
        <!-- 2. Rasm: balandligi h-48 va proporsiyani saqlovchi object-cover -->
        <img src="https://images.unsplash.com/photo-1506744038136-46273834b3fb?q=80&w=400&h=300&fit=crop" alt="Tabiat" class="w-full h-48 object-cover">
        <div class="p-5 flex flex-col justify-between h-56">
          <div>
            <!-- 2. Sarlavha -->
            <h3 class="text-lg font-bold text-gray-900 line-clamp-1 hover:text-indigo-600 transition-colors cursor-pointer">Sokin tog'lar go'zalligi</h3>
            <!-- 2. Tavsif -->
            <p class="text-gray-500 text-sm mt-2 line-clamp-3">Ertalabki quyosh nurlari ostida yastanib yotgan tog' cho'qqilari inson ruhiga xotirjamlik va cheksiz ilhom bag'ishlaydi.</p>
          </div>
          <!-- 2. Tugma -->
          <button class="w-full mt-4 bg-gray-900 hover:bg-indigo-600 text-white font-medium text-sm py-2 px-4 rounded-xl transition-colors duration-200">Batafsil</button>
        </div>
      </article>

      <!-- Karta 2 -->
      <article class="bg-white rounded-2xl overflow-hidden shadow-sm border border-gray-100 transition-all duration-300 hover:shadow-xl hover:-translate-y-1">
        <img src="https://images.unsplash.com/photo-1451187580459-43490279c0fa?q=80&w=400&h=300&fit=crop" alt="Texnologiya" class="w-full h-48 object-cover">
        <div class="p-5 flex flex-col justify-between h-56">
          <div>
            <h3 class="text-lg font-bold text-gray-900 line-clamp-1 hover:text-indigo-600 transition-colors cursor-pointer">Kiber-makon va Kelajak</h3>
            <p class="text-gray-500 text-sm mt-2 line-clamp-3">Sun'iy intellekt va global tarmoqlar rivojlanishi bizning yashash va ishlash tarzimizni butunlay o'zgartirib yubormoqda.</p>
          </div>
          <button class="w-full mt-4 bg-gray-900 hover:bg-indigo-600 text-white font-medium text-sm py-2 px-4 rounded-xl transition-colors duration-200">Batafsil</button>
        </div>
      </article>

      <!-- Karta 3 -->
      <article class="bg-white rounded-2xl overflow-hidden shadow-sm border border-gray-100 transition-all duration-300 hover:shadow-xl hover:-translate-y-1">
        <img src="https://images.unsplash.com/photo-1447752875215-b2761acb3c5d?q=80&w=400&h=300&fit=crop" alt="O'rmon" class="w-full h-48 object-cover">
        <div class="p-5 flex flex-col justify-between h-56">
          <div>
            <h3 class="text-lg font-bold text-gray-900 line-clamp-1 hover:text-indigo-600 transition-colors cursor-pointer">Yashil o'rmon sirlari</h3>
            <p class="text-gray-500 text-sm mt-2 line-clamp-3">Sayyoramizning o'pkalari hisoblangan yashil o'rmonlar o'z bag'rida minglab noyob flora va fauna turlarini saqlaydi.</p>
          </div>
          <button class="w-full mt-4 bg-gray-900 hover:bg-indigo-600 text-white font-medium text-sm py-2 px-4 rounded-xl transition-colors duration-200">Batafsil</button>
        </div>
      </article>

      <!-- Karta 4 -->
      <article class="bg-white rounded-2xl overflow-hidden shadow-sm border border-gray-100 transition-all duration-300 hover:shadow-xl hover:-translate-y-1">
        <img src="https://images.unsplash.com/photo-1470071459604-3b5ec3a7fe05?q=80&w=400&h=300&fit=crop" alt="Peyzaj" class="w-full h-48 object-cover">
        <div class="p-5 flex flex-col justify-between h-56">
          <div>
            <h3 class="text-lg font-bold text-gray-900 line-clamp-1 hover:text-indigo-600 transition-colors cursor-pointer">Tonggi shabnam sehri</h3>
            <p class="text-gray-500 text-sm mt-2 line-clamp-3">Tabiat uyg'onadigan eng go'zal pallada, yam-yashil vodiylar uzra yoyilgan mayin tuman ajoyib manzara hosil qiladi.</p>
          </div>
          <button class="w-full mt-4 bg-gray-900 hover:bg-indigo-600 text-white font-medium text-sm py-2 px-4 rounded-xl transition-colors duration-200">Batafsil</button>
        </div>
      </article>

      <!-- Karta 5 -->
      <article class="bg-white rounded-2xl overflow-hidden shadow-sm border border-gray-100 transition-all duration-300 hover:shadow-xl hover:-translate-y-1">
        <img src="https://images.unsplash.com/photo-1513836279014-a89f7a76ae86?q=80&w=400&h=300&fit=crop" alt="Ekotizim" class="w-full h-48 object-cover">
        <div class="p-5 flex flex-col justify-between h-56">
          <div>
            <h3 class="text-lg font-bold text-gray-900 line-clamp-1 hover:text-indigo-600 transition-colors cursor-pointer">Ekologik muvozanat</h3>
            <p class="text-gray-500 text-sm mt-2 line-clamp-3">Atrof-muhitni asrash va kelajak avlodga toza ekotizim qoldirish har birimizning fuqarolik burchimizdir.</p>
          </div>
          <button class="w-full mt-4 bg-gray-900 hover:bg-indigo-600 text-white font-medium text-sm py-2 px-4 rounded-xl transition-colors duration-200">Batafsil</button>
        </div>
      </article>

      <!-- Karta 6 -->
      <article class="bg-white rounded-2xl overflow-hidden shadow-sm border border-gray-100 transition-all duration-300 hover:shadow-xl hover:-translate-y-1">
        <img src="https://images.unsplash.com/photo-1472214222541-d510753a8707?q=80&w=400&h=300&fit=crop" alt="Ufq" class="w-full h-48 object-cover">
        <div class="p-5 flex flex-col justify-between h-56">
          <div>
            <h3 class="text-lg font-bold text-gray-900 line-clamp-1 hover:text-indigo-600 transition-colors cursor-pointer">Oltin ufq manzaralari</h3>
            <p class="text-gray-500 text-sm mt-2 line-clamp-3">Kun botish arafasida osmon bag'rini qoplagan to'q sariq va binafsha ranglar tabiatning eng mukammal san'at asaridir.</p>
          </div>
          <button class="w-full mt-4 bg-gray-900 hover:bg-indigo-600 text-white font-medium text-sm py-2 px-4 rounded-xl transition-colors duration-200">Batafsil</button>
        </div>
      </article>

    </div>
  </main>

</body>
</html>
