Sass arxitekturasida 7-1 Pattern (yoki undan ilhomlangan soddalashtirilgan tuzilmalar) bilan ishlashda zamonaviy @use va @forward qoidalaridan foydalanish kodni modulli va toza saqlashning eng yaxshi usuli hisoblanadi.

Eski @import tizimidan farqli o‘laroq, @use har bir faylni alohida modul (namespace) sifatida yuklaydi va global to‘qnashuvlarning oldini oladi.

Quyida siz so‘ragan barcha talablar asosida loyiha to‘liq va bosqichma-bosqich yig‘ilgan.

1. Loyiha tuzilmasi (Folder Structure)
Dastlab, loyihangiz papkalar va fayllar ko‘rinishi quyidagicha shakllanadi. Har bir ichki papkada o‘zining _index.scss fayli bo‘ladi, bu esa main.scss ga fayllarni import qilishni ancha osonlashtiradi:

Plaintext
scss/
│
├── abstracts/
│   ├── _variables.scss
│   ├── _mixins.scss
│   ├── _functions.scss
│   └── _index.scss
│
├── base/
│   ├── _reset.scss
│   ├── _typography.scss
│   └── _index.scss
│
├── components/
│   ├── _buttons.scss
│   ├── _card.scss
│   └── _index.scss
│
├── layouts/
│   ├── _header.scss
│   ├── _footer.scss
│   └── _index.scss
│
├── pages/
│   ├── _home.scss
│   └── _index.scss
│
└── main.scss
2. Abstracts papkasidagi asosiy fayllar
Bu fayllarda loyihaning global sozlamalari, miksinlar va funksiyalar joylashadi. E'tibor bering, ular alohida yozilganda hech qanday tashqi bog'liqlikka ega emas.

scss/abstracts/_variables.scss
SCSS
// Ranglar
$color-primary: #3498db;
$color-secondary: #2ecc71;
$color-dark: #2c3e50;
$color-light: #ecf0f1;

// Shriftlar
$font-main: 'Helvetica Neue', sans-serif;
scss/abstracts/_mixins.scss
SCSS
// Flexbox markazlashtirish miksini
@mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

// Responsive media so'rovlari uchun miksin
@mixin respond-to($breakpoint) {
  @if $breakpoint == "tablet" {
    @media (max-width: 768px) { @content; }
  } @else if $breakpoint == "mobile" {
    @media (max-width: 480px) { @content; }
  }
}
scss/abstracts/_functions.scss
SCSS
// Pixellarni Rem ga o'giruvchi funksiya (asos: 16px)
@function to-rem($pixels) {
  @return ($pixels / 16) * 1rem;
}
3. @forward yordamida _index.scss fayllarini yaratish
Har bir papkaning ichidagi _index.scss fayli o‘sha papkadagi barcha ichki fayllarni birlashtirib, tashqariga ko‘prik (@forward) vazifasini bajaradi.

scss/abstracts/_index.scss
Muhim qoida: abstracts ichidagi fayllar bir-biriga bog'liq bo'lishi mumkin (masalan, miksin ichida o'zgaruvchi ishlatilishi mumkin). Shuning uchun ularni avval @use bilan bir-biriga ulab, keyin @forward qilish eng xavfsiz yo'ldir.

SCSS
@forward 'variables';
@forward 'mixins';
@forward 'functions';
Xuddi shu tartibda qolgan papkalarning ham _index.scss fayllarini yaratib chiqamiz:

scss/base/_index.scss
SCSS
@forward 'reset';
@forward 'typography';
scss/components/_index.scss
SCSS
@forward 'buttons';
@forward 'card';
scss/layouts/_index.scss
SCSS
@forward 'header';
@forward 'footer';
scss/pages/_index.scss
SCSS
@forward 'home';
4. Namespace (Nomlar makoni) orqali abstracts'dan foydalanish
Endi boshqa har qanday komponent yoki sahifa faylida (masalan, _buttons.scss yoki _home.scss ichida) o'zgaruvchilar va miksinlarni Namespace orqali ishlatamiz.

Ushbu fayl abstracts papkasidagi _index.scss ga murojaat qiladi va uning ichidagi hamma narsani bitta abstracts nomli paket qilib oladi.

Misol: scss/components/_buttons.scss
SCSS
@use '../abstracts'; // Avtomatik ravishda abstracts/_index.scss ni yuklaydi

.btn-primary {
  // O'zgaruvchini namespace bilan ishlatish
  background-color: abstracts.$color-primary; 
  color: abstracts.$color-light;
  
  // Funksiyani namespace bilan ishlatish
  padding: abstracts.to-rem(10) abstracts.to-rem(20); 
  border: none;
  border-radius: 4px;
  cursor: pointer;
  
  // Miksinni namespace bilan ishlatish
  @include abstracts.flex-center; 
  
  &:hover {
    background-color: darken(abstracts.$color-primary, 10%);
  }

  @include abstracts.respond-to('mobile') {
    width: 100%;
  }
}
5. Asosiy yig'uvchi fayl: scss/main.scss
Loyiha arxitekturasining yakuniy nuqtasi bu main.scss faylidir. Biz barcha papkalarning _index.scss fayllarini @use orqali shu yerda yig'amiz.

Ushbu fayl CSS ga kompiyatsiya bo'lganda tartib buzilmasligi uchun komponentlarni to'g'ri ketma-ketlikda yuklash lozim:

SCSS
// 1. Sozlamalar va instrumentlar (Visual CSS render qilmaydi, faqat yuklanadi)
@use 'abstracts';

// 2. Asosiy va global stillar (Reset, Typography)
@use 'base';

// 3. Maket stillari (Header, Footer, Grid grid-tizimi)
@use 'layouts';

// 4. Kichik modullar va komponentlar (Buttons, Cards, Inputs)
@use 'components';

// 5. Maxsus sahifalar stillari (Home, About, Dashboard)
@use 'pages';
