1. tailwind.config.js (Konfiguratsiya fayli)
Bu yerda siz so'ragan maxsus brand ranglari hamda .display va .caption uchun custom shrift o'lchamlari kiritilgan.

JavaScript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{html,js}"],
  theme: {
    extend: {
      // 1. Brand ranglari (Primary, Secondary, Accent)
      colors: {
        brand: {
          primary: {
            DEFAULT: '#4f46e5', // Indigo-600
            hover: '#4338ca',   // Indigo-700
          },
          secondary: {
            DEFAULT: '#10b981', // Emerald-500
            hover: '#059669',   // Emerald-600
          },
          accent: {
            DEFAULT: '#f59e0b', // Amber-500
            hover: '#d97706',   // Amber-600
          }
        }
      },
      // 2. Custom shrift o'lchamlari (Display va Caption)
      fontSize: {
        'display': ['3.5rem', { lineHeight: '1.2', fontWeight: '800' }], // Yirik sarlavhalar uchun
        'caption': ['0.75rem', { lineHeight: '1.4', fontWeight: '400', letterSpacing: '0.05em' }], // Kichik izohlar uchun
      }
    },
  },
  plugins: [],
}
2. src/input.css (Komponentlarni yaratish)
Ushbu qismda @apply direktivasi yordamida tugmalar, karta va inputlar yaratilgan. 5- va 6-shartlarga ko'ra, barcha komponentlarda hover:, focus: holatlari hamda silliq o'tish (transition-all duration-200) animatsiyalari sozlangan.

CSS
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
  
  // 3. TUGMALAR KOMPONENTLARI (.btn-primary, .btn-secondary, .btn-outline)
  .btn-primary {
    @apply px-5 py-2.5 rounded-xl font-semibold text-sm text-white bg-brand-primary transition-all duration-200 
           hover:bg-brand-primary-hover 
           focus:outline-none focus:ring-4 focus:ring-indigo-100 
           active:scale-95;
  }

  .btn-secondary {
    @apply px-5 py-2.5 rounded-xl font-semibold text-sm text-white bg-brand-secondary transition-all duration-200 
           hover:bg-brand-secondary-hover 
           focus:outline-none focus:ring-4 focus:ring-emerald-100 
           active:scale-95;
  }

  .btn-outline {
    @apply px-5 py-2.5 rounded-xl font-semibold text-sm text-gray-700 bg-transparent border border-gray-300 transition-all duration-200 
           hover:bg-gray-50 hover:border-gray-400 
           focus:outline-none focus:ring-4 focus:ring-gray-100 
           active:scale-95;
  }

  // 4. KARTA KOMPONENTI (.card)
  .card {
    @apply bg-white p-6 rounded-2xl border border-gray-100 shadow-sm transition-all duration-200 
           hover:shadow-md hover:-translate-y-0.5;
  }

  // 4. INPUT KOMPONENTI (.input)
  .input {
    @apply w-full px-4 py-2.5 rounded-xl border border-gray-300 bg-white text-gray-900 text-sm outline-none transition-all duration-200 
           placeholder:text-gray-400
           hover:border-gray-400 
           focus:border-brand-primary focus:ring-4 focus:ring-indigo-50;
  }

}
3. HTML-da qo'llanilishi (index.html)
Yuqorida yaratilgan klasslar va konfiguratsiyalarni HTML-da ishlatish bo'yicha amaliy misol:

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tailwind Design System</title>
  <link href="./dist/output.css" rel="stylesheet">
</head>
<body class="bg-gray-50 p-8 min-h-screen flex flex-col justify-center items-center gap-6">

  <!-- Custom Shrift O'lchamlari (Display) -->
  <h1 class="text-display text-gray-900 mb-2">Salom Dunyo!</h1>
  <!-- Custom Shrift O'lchamlari (Caption) -->
  <p class="text-caption text-gray-500 uppercase mb-4">Loyihaning maxsus kichik matni</p>

  <!-- Karta komponenti ichida input va tugmalar -->
  <div class="card max-w-sm w-full flex flex-col gap-4">
    
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">Email manzilingiz</label>
      <!-- Input komponenti -->
      <input type="email" class="input" placeholder="example@mail.com">
    </div>

    <div class="flex flex-col gap-2 mt-2">
      <!-- Tugmalar komponentlari -->
      <button class="btn-primary">Asosiy amallar</button>
      <button class="btn-secondary">Muvaffaqiyatli saqlash</button>
      <button class="btn-outline">Bekor qilish</button>
    </div>

  </div>

</body>
</html>
