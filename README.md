1. Navbar
<nav class="fixed top-0 left-0 right-0 bg-white/80 backdrop-blur-md
  border-b border-gray-200 z-50">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="flex items-center justify-between h-16">
      <a href="#" class="text-2xl font-black text-blue-600">Nexus</a>

      <ul class="hidden md:flex items-center gap-8">
        <li><a href="#features" class="text-gray-600 hover:text-gray-900 font-medium transition-colors">Xususiyatlar</a></li>
        <li><a href="#pricing" class="text-gray-600 hover:text-gray-900 font-medium transition-colors">Narxlar</a></li>
        <li><a href="#testimonials" class="text-gray-600 hover:text-gray-900 font-medium transition-colors">Mijozlar</a></li>
      </ul>

      <div class="flex items-center gap-4">
        <a href="#" class="hidden md:block text-gray-700 font-medium hover:text-blue-600 transition-colors">Kirish</a>
        <a href="#" class="bg-blue-600 text-white px-5 py-2 rounded-lg font-semibold
          hover:bg-blue-700 transition-colors shadow-sm">
          Bepul boshlash
        </a>
      </div>
    </div>
  </div>
</nav>
2. Hero Section
<section class="pt-32 pb-20 bg-gradient-to-br from-blue-50 via-white to-indigo-50">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
    <span class="inline-flex items-center gap-2 bg-blue-50 text-blue-700 px-4 py-1.5
      rounded-full text-sm font-semibold mb-8 border border-blue-200">
      🚀 Yangi: AI funksiyalari chiqarildi
    </span>

    <h1 class="text-5xl sm:text-6xl lg:text-7xl font-black text-gray-900
      leading-tight mb-6 max-w-4xl mx-auto">
      Biznesingizni <span class="text-transparent bg-clip-text
        bg-gradient-to-r from-blue-600 to-indigo-600">
        avtomatlаshtiring
      </span>
    </h1>

    <p class="text-xl text-gray-600 max-w-2xl mx-auto mb-10 leading-relaxed">
      Nexus yordamida jamoangiz samaradorligini 3x oshiring. Hech qanday kod bilimi shart emas.
    </p>

    <div class="flex flex-col sm:flex-row gap-4 justify-center mb-16">
      <a href="#" class="bg-blue-600 text-white px-8 py-4 rounded-xl font-bold text-lg
        hover:bg-blue-700 transition-all shadow-lg hover:shadow-blue-200 hover:-translate-y-0.5">
        14 kun bepul sinab ko'ring
      </a>
      <a href="#" class="bg-white text-gray-700 px-8 py-4 rounded-xl font-bold text-lg
        border-2 border-gray-200 hover:border-gray-300 transition-all hover:-translate-y-0.5">
        ▶ Demo ko'rish
      </a>
    </div>

    <!-- Statistika -->
    <div class="flex flex-wrap justify-center gap-12">
      <div class="text-center">
        <div class="text-4xl font-black text-gray-900">50K+</div>
        <div class="text-gray-500 text-sm mt-1">Faol foydalanuvchilar</div>
      </div>
      <div class="text-center">
        <div class="text-4xl font-black text-gray-900">99.9%</div>
        <div class="text-gray-500 text-sm mt-1">Uptime kafolati</div>
      </div>
      <div class="text-center">
        <div class="text-4xl font-black text-gray-900">4.9★</div>
        <div class="text-gray-500 text-sm mt-1">Mijoz bahosi</div>
      </div>
    </div>
  </div>
</section>
3. Pricing Section
<section id="pricing" class="py-24 bg-gray-50">
  <div class="max-w-7xl mx-auto px-4">
    <h2 class="text-4xl font-black text-center mb-4">Oddiy va aniq narxlar</h2>
    <p class="text-gray-500 text-center mb-16">Kredit kartasiz boshlang</p>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-8 max-w-5xl mx-auto">
      <!-- Basic -->
      <div class="bg-white rounded-2xl p-8 border border-gray-200">
        <h3 class="font-bold text-lg mb-2">Basic</h3>
        <div class="text-4xl font-black mb-6">$0<span class="text-lg font-normal text-gray-400">/oy</span></div>
        <ul class="space-y-3 mb-8 text-gray-600">
          <li class="flex items-center gap-2"><span class="text-green-500">✓</span> 3 ta loyiha</li>
          <li class="flex items-center gap-2"><span class="text-green-500">✓</span> 5 GB xotira</li>
          <li class="flex items-center gap-2 text-gray-300"><span>✗</span> AI yordamchi</li>
        </ul>
        <button class="w-full py-3 rounded-xl border-2 border-blue-600 text-blue-600
          font-semibold hover:bg-blue-50 transition-colors">Boshlash</button>
      </div>

      <!-- Pro (aktiv) -->
      <div class="bg-blue-600 rounded-2xl p-8 border border-blue-600 relative">
        <span class="absolute -top-4 left-1/2 -translate-x-1/2 bg-yellow-400 text-yellow-900
          px-4 py-1 rounded-full text-xs font-bold">MASHHUR</span>
        <h3 class="font-bold text-lg mb-2 text-white">Pro</h3>
        <div class="text-4xl font-black mb-6 text-white">$29<span class="text-lg font-normal text-blue-200">/oy</span></div>
        <ul class="space-y-3 mb-8 text-blue-100">
          <li class="flex items-center gap-2"><span class="text-yellow-300">✓</span> Cheksiz loyihalar</li>
          <li class="flex items-center gap-2"><span class="text-yellow-300">✓</span> 100 GB xotira</li>
          <li class="flex items-center gap-2"><span class="text-yellow-300">✓</span> AI yordamchi</li>
        </ul>
        <button class="w-full py-3 rounded-xl bg-white text-blue-600
          font-semibold hover:bg-blue-50 transition-colors">Boshlash</button>
      </div>

      <!-- Enterprise -->
      <div class="bg-white rounded-2xl p-8 border border-gray-200">
        <h3 class="font-bold text-lg mb-2">Enterprise</h3>
        <div class="text-4xl font-black mb-6">$99<span class="text-lg font-normal text-gray-400">/oy</span></div>
        <ul class="space-y-3 mb-8 text-gray-600">
          <li class="flex items-center gap-2"><span class="text-green-500">✓</span> Cheksiz hamma narsa</li>
          <li class="flex items-center gap-2"><span class="text-green-500">✓</span> Maxsus qo'llab-quvvatlash</li>
          <li class="flex items-center gap-2"><span class="text-green-500">✓</span> SLA kafolat</li>
        </ul>
        <button class="w-full py-3 rounded-xl border-2 border-blue-600 text-blue-600
          font-semibold hover:bg-blue-50 transition-colors">Bog'lanish</button>
      </div>
    </div>
  </div>
</section>
Mini-Loyiha: O'z SaaS Mahsulotingiz
Xayoliy SaaS mahsulot uchun to'liq landing page yarating. Barcha bo'limlar (navbar, hero, features, pricing, footer) bo'lsin. Tailwind ning faqat utility klasslaridan foydalaning — hech qanday custom CSS yozmang.

<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://cdn.tailwindcss.com"></script>
  <title>Mening SaaS Mahsulotim</title>
</head>
<body class="font-sans">
  <!-- Bu yerga barcha bo'limlar -->
</body>
</html>
