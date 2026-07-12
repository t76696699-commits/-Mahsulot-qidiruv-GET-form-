<!DOCTYPE html>
<html lang="uz" class=""> <!-- Dark mode ishlashi uchun shu yerga 'dark' klassi qo'shiladi -->
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tailwind CSS Zamonaviy UI Komponentlari</title>
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    // Tailwind uchun dark mode konfiguratsiyasi (class-based)
    tailwind.config = {
      darkMode: 'class',
    }
  </script>
</head>
<body class="bg-gray-50 text-gray-900 dark:bg-gray-900 dark:text-gray-100 min-h-screen p-8 transition-colors duration-300">

  <!-- 3. DARK MODE TOGGLE TUGMASI -->
  <div class="max-w-5xl mx-auto flex justify-end mb-8">
    <button onclick="toggleDarkMode()" class="flex items-center gap-2 bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 px-4 py-2 rounded-xl shadow-sm text-sm font-medium hover:bg-gray-50 dark:hover:bg-gray-750 transition-all duration-200 active:scale-95">
      <span class="dark:hidden">🌙 Tungi rejim</span>
      <span class="hidden dark:inline">☀️ Kunduzgi rejim</span>
    </button>
  </div>

  <main class="max-w-5xl mx-auto">
    
    <!-- 1. TUGMALAR BLOCKI (Barcha holatlari bilan: hover, focus, active, disabled) -->
    <section class="bg-white dark:bg-gray-800 p-6 rounded-2xl shadow-sm border border-gray-100 dark:border-gray-700 mb-8 transition-all duration-300">
      <h2 class="text-lg font-bold mb-4">1. Tugmalar To'plami (States)</h2>
      <div class="flex flex-wrap gap-4">
        
        <!-- Tugma 1: Primary -->
        <button class="bg-blue-600 text-white font-medium text-sm py-2.5 px-5 rounded-xl transition-all duration-200 hover:bg-blue-700 focus:ring-4 focus:ring-blue-200 active:scale-95 disabled:opacity-50 disabled:pointer-events-none">
          Primary
        </button>

        <!-- Tugma 2: Success -->
        <button class="bg-green-600 text-white font-medium text-sm py-2.5 px-5 rounded-xl transition-all duration-200 hover:bg-green-700 focus:ring-4 focus:ring-green-200 active:scale-95 disabled:opacity-50 disabled:pointer-events-none">
          Success
        </button>

        <!-- Tugma 3: Outline -->
        <button class="bg-transparent border border-gray-300 dark:border-gray-600 text-gray-700 dark:text-gray-300 font-medium text-sm py-2.5 px-5 rounded-xl transition-all duration-200 hover:bg-gray-50 dark:hover:bg-gray-700 focus:ring-4 focus:ring-gray-100 dark:focus:ring-gray-700 active:scale-95 disabled:opacity-50 disabled:pointer-events-none">
          Outline
        </button>

        <!-- Tugma 4: Danger -->
        <button class="bg-red-600 text-white font-medium text-sm py-2.5 px-5 rounded-xl transition-all duration-200 hover:bg-red-700 focus:ring-4 focus:ring-red-200 active:scale-95 disabled:opacity-50 disabled:pointer-events-none">
          Danger
        </button>

        <!-- Tugma 5: Disabled Holati -->
        <button disabled class="bg-blue-600 text-white font-medium text-sm py-2.5 px-5 rounded-xl transition-all duration-200 hover:bg-blue-700 focus:ring-4 focus:ring-blue-200 active:scale-95 disabled:opacity-50 disabled:pointer-events-none">
          Disabled (Qulflangan)
        </button>

      </div>
    </section>

    <!-- 5. PRICING KARTA & 4. DARK: PREFIKS -->
    <section class="grid grid-cols-1 md:grid-cols-2 gap-8">
      
      <!-- 2. GROUP MODIFIKATORLI KARTA (Karta ustiga kelganda ichki element o'zgaradi) -->
      <div class="group bg-white dark:bg-gray-800 rounded-3xl p-8 shadow-md border border-gray-100 dark:border-gray-700 relative overflow-hidden transition-all duration-300 hover:shadow-xl hover:-translate-y-1">
        <div class="absolute top-0 right-0 bg-blue-600 text-white text-xs font-bold px-4 py-1.5 rounded-bl-xl uppercase tracking-wider transition-all duration-300 group-hover:bg-indigo-600">
          Ommabop
        </div>
        
        <h3 class="text-xl font-bold text-gray-800 dark:text-white">Pro Reja</h3>
        <p class="text-gray-400 dark:text-gray-400 text-sm mt-1">Kichik jamoalar va professionallar uchun</p>
        
        <div class="my-6">
          <span class="text-4xl font-extrabold text-gray-950 dark:text-white">$29</span>
          <span class="text-gray-400 dark:text-gray-400 text-sm">/oyiga</span>
        </div>

        <ul class="space-y-3 text-sm text-gray-600 dark:text-gray-300 mb-8">
          <li class="flex items-center gap-2">
            <!-- group-hover bilan ikonka rangini o'zgartirish -->
            <span class="text-blue-600 dark:text-blue-400 transition-colors duration-300 group-hover:text-indigo-500">✓</span> Cheksiz loyihalar
          </li>
          <li class="flex items-center gap-2">
            <span class="text-blue-600 dark:text-blue-400 transition-colors duration-300 group-hover:text-indigo-500">✓</span> 50GB Bulutli xotira
          </li>
          <li class="flex items-center gap-2">
            <span class="text-blue-600 dark:text-blue-400 transition-colors duration-300 group-hover:text-indigo-500">✓</span> 24/7 Premium qo'llab-quvvatlash
          </li>
        </ul>

        <!-- group-hover bilan tugma fonini butunlay boshqa rangga o'tkazish -->
        <button class="w-full bg-blue-600 text-white font-semibold py-3 px-4 rounded-xl shadow-md transition-all duration-300 group-hover:bg-indigo-600 focus:ring-4 focus:ring-blue-100 dark:focus:ring-gray-700 active:scale-98">
          Hozir boshlang
        </button>
      </div>

      <!-- Standart Premium Pricing Karta -->
      <div class="bg-white dark:bg-gray-800 rounded-3xl p-8 shadow-md border border-gray-100 dark:border-gray-700 transition-all duration-300 hover:shadow-xl hover:-translate-y-1">
        <h3 class="text-xl font-bold text-gray-800 dark:text-white">Biznes Reja</h3>
        <p class="text-gray-400 dark:text-gray-400 text-sm mt-1">Yirik korxonalar va tashkilotlar uchun</p>
        
        <div class="my-6">
          <span class="text-4xl font-extrabold text-gray-950 dark:text-white">$99</span>
          <span class="text-gray-400 dark:text-gray-400 text-sm">/oyiga</span>
        </div>

        <ul class="space-y-3 text-sm text-gray-600 dark:text-gray-300 mb-8">
          <li class="flex items-center gap-2"><span class="text-green-600 dark:text-green-400">✓</span> Barcha Pro funksiyalar</li>
          <li class="flex items-center gap-2"><span class="text-green-600 dark:text-green-400">✓</span> 1TB Bulutli xotira</li>
          <li class="flex items-center gap-2"><span class="text-green-600 dark:text-green-400">✓</span> Maxsus menejer ajratiladi</li>
        </ul>

        <button class="w-full bg-gray-900 dark:bg-gray-700 text-white dark:text-gray-100 font-semibold py-3 px-4 rounded-xl shadow-md transition-all duration-300 hover:bg-gray-800 dark:hover:bg-gray-600 focus:ring-4 focus:ring-gray-200 dark:focus:ring-gray-600 active:scale-98">
          Kompaniya uchun olish
        </button>
      </div>

    </section>
  </main>

  <!-- Dark Mode Kontrolleri uchun Logika -->
  <script>
    function toggleDarkMode() {
      const htmlElement = document.documentElement;
      if (htmlElement.classList.contains('dark')) {
        htmlElement.classList.remove('dark');
        localStorage.setItem('theme', 'light');
      } else {
        htmlElement.classList.add('dark');
        localStorage.setItem('theme', 'dark');
      }
    }

    // Sahifa yuklanganda foydalanuvchining oldingi tanlovini tekshirish
    if (localStorage.getItem('theme') === 'dark') {
      document.documentElement.classList.add('dark');
    }
  </script>
</body>
</html>
