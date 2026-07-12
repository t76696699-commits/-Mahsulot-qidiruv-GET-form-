<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tailwind CSS Profil Kartasi</title>
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 flex justify-content-center items-center min-h-screen m-0 p-4">

  <!-- 6. Karta: oq fon (bg-white), mukammal soya (shadow-lg) va yumaloq burchaklar (rounded-2xl) -->
  <div class="bg-white shadow-lg rounded-2xl p-6 max-w-sm w-full text-center border border-gray-100 transition-all duration-300 hover:shadow-2xl">
    
    <!-- 1. Avatar rasm: to'liq yumaloq (rounded-full), aniq 96px o'lcham (w-24 h-24) -->
    <div class="flex justify-center mb-4">
      <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?q=80&w=200&h=200&fit=crop" 
           alt="Profil rasmi" 
           class="w-24 h-24 rounded-full object-cover ring-4 ring-indigo-50 border-2 border-white shadow-sm transition-transform duration-300 hover:scale-105 cursor-pointer">
    </div>

    <!-- 2. Ism va lavozim: to'g'ri shrift o'lchami va og'irligi (text-xl, font-bold, text-sm, font-medium) -->
    <h2 class="text-xl font-bold text-gray-800 tracking-tight transition-colors duration-200 hover:text-indigo-600 cursor-pointer">
      Zuhra Usmonova
    </h2>
    <p class="text-sm font-medium text-indigo-600 mt-1 uppercase tracking-wider">
      Senior UX/UI Designer
    </p>

    <!-- 3. Qisqa bio matni: chiroyli va o'qishga qulay kulrang (text-gray-500) -->
    <p class="text-gray-500 text-sm mt-3 mb-6 leading-relaxed">
      Foydalanuvchilarga qulay va chiroyli raqamli mahsulotlar yaratishga ixtisoslashganman. Murakkab g'oyalarni oddiy dizaynlarga aylantirishni yaxshi ko'raman.
    </p>

    <!-- 4. Kamida 2 ta tugma va 5. Hover effektlar -->
    <div class="flex flex-col sm:flex-row gap-3 justify-center">
      
      <!-- Tugma 1: Primary Stil + Hover Effekti -->
      <button class="bg-indigo-600 hover:bg-indigo-700 text-white font-semibold text-sm py-2.5 px-5 rounded-xl shadow-md shadow-indigo-200 active:scale-95 transition-all duration-200 w-full sm:w-auto">
        Kuzatish
      </button>

      <!-- Tugma 2: Outline Stil + Hover Effekti -->
      <button class="bg-transparent hover:bg-gray-50 text-gray-700 font-semibold text-sm py-2.5 px-5 border border-gray-300 hover:border-gray-400 rounded-xl active:scale-95 transition-all duration-200 w-full sm:w-auto">
        Xabar yuborish
      </button>

    </div>

  </div>

</body>
</html>
