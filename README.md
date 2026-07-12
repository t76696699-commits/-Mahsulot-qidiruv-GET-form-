Tailwind bilan Flexbox
Tailwind flexbox klasslarini ishlatish an'anaviy CSS ga qaraganda ancha qisqa va qulay. Quyida asosiy flex klasslar:

<div class="flex items-center justify-between gap-4">
  <div>Chap element</div>
  <div>O'ng element</div>
</div>

<!-- Vertikal markazlash -->
<div class="flex flex-col items-center justify-center h-screen">
  <h1 class="text-4xl font-bold">Markazlashgan kontent</h1>
</div>

<!-- Wrap -->
<div class="flex flex-wrap gap-3">
  <span class="bg-blue-100 text-blue-700 px-3 py-1 rounded-full text-sm">HTML</span>
  <span class="bg-green-100 text-green-700 px-3 py-1 rounded-full text-sm">CSS</span>
  <span class="bg-yellow-100 text-yellow-700 px-3 py-1 rounded-full text-sm">JavaScript</span>
</div>
Tailwind bilan CSS Grid
<!-- 3 ustunli grid -->
<div class="grid grid-cols-3 gap-6">
  <div class="bg-white rounded-xl p-4 shadow">Karta 1</div>
  <div class="bg-white rounded-xl p-4 shadow">Karta 2</div>
  <div class="bg-white rounded-xl p-4 shadow">Karta 3</div>
</div>

<!-- col-span -->
<div class="grid grid-cols-4 gap-4">
  <div class="col-span-3 bg-blue-50 p-4 rounded-xl">Keng element (3 ustun)</div>
  <div class="col-span-1 bg-gray-50 p-4 rounded-xl">Tor element</div>
</div>
Responsive Prefixlar
<!-- sm: 640px, md: 768px, lg: 1024px, xl: 1280px -->
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
  <div class="bg-white rounded-xl p-6 shadow-sm">
    <img src="img.jpg" class="w-full h-48 object-cover rounded-lg mb-4">
    <h3 class="text-lg font-semibold mb-2">Karta sarlavhasi</h3>
    <p class="text-gray-500 text-sm">Qisqa tavsif matni bu yerda.</p>
  </div>
</div>

<!-- Mobilda yashirin, desktopda ko'rinadigan -->
<div class="hidden lg:block">Faqat desktopda</div>
<div class="block lg:hidden">Faqat mobilda</div>
Amaliy misol — Nav bari
<nav class="bg-white shadow-sm px-6 py-4">
  <div class="max-w-6xl mx-auto flex items-center justify-between">
    <a href="/" class="text-2xl font-bold text-blue-600">Logo</a>

    <ul class="hidden md:flex items-center gap-8">
      <li><a href="#" class="text-gray-600 hover:text-blue-600 transition-colors font-medium">Bosh sahifa</a></li>
      <li><a href="#" class="text-gray-600 hover:text-blue-600 transition-colors font-medium">Haqida</a></li>
      <li><a href="#" class="text-gray-600 hover:text-blue-600 transition-colors font-medium">Aloqa</a></li>
    </ul>

    <button class="bg-blue-600 text-white px-5 py-2 rounded-lg font-medium
      hover:bg-blue-700 transition-colors hidden md:block">
      Boshlash
    </button>

    <!-- Mobil burger -->
    <button class="md:hidden p-2 rounded-lg hover:bg-gray-100">☰</button>
  </div>
</nav>
Mini-Loyiha: Mahsulotlar Sahifasi
Onlayn do'kon mahsulotlar ro'yxati sahifasini yarating. Nav bar, sarlavha va kamida 6 ta mahsulot kartasi. Kartalar grid bilan responsive joylashsin (mobil: 1, tablet: 2, desktop: 3).

<section class="py-12 px-4">
  <div class="max-w-6xl mx-auto">
    <h2 class="text-3xl font-bold mb-8">Mahsulotlar</h2>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <!-- Mahsulot kartalari bu yerga -->
    </div>
  </div>
</section>
Maqsad: Tailwind grid, flex va responsive prefixlarni birgalikda ishlatish.
