CSS Grid asoslari
CSS Grid — bu ikki o'lchamli (2D) tartib tizimi bo'lib, veb-sahifalarni qator va ustunlar yordamida qulay tarzda joylashtirish imkonini beradi. Flexbox bitta o'qda ishlasa, Grid bir vaqtning o'zida gorizontal va vertikal o'q bo'ylab element joylashishini boshqaradi.

display: grid

.container {
  display: grid;
}
grid-template-columns va grid-template-rows
grid-template-columns ustunlar sonini va har bir ustun kengligini belgilaydi. fr (fraction) birligi mavjud bo'sh joyni mutanosib taqsimlaydi.


.container {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;
  grid-template-rows: 80px auto 60px;
}
repeat() funksiyasi

.container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}

.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
}
gap, row-gap, column-gap

.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
}
grid-column va grid-row (span)

.wide-card {
  grid-column: span 2;
}

.header {
  grid-column: 1 / -1;
}
grid-template-areas — Nomli maydonlar

.page {
  display: grid;
  grid-template-columns: 220px 1fr 180px;
  grid-template-rows: 64px 1fr 48px;
  grid-template-areas:
    "header  header  header"
    "sidebar content aside"
    "footer  footer  footer";
  min-height: 100vh;
  gap: 1rem;
}

.page-header  { grid-area: header; }
.page-sidebar { grid-area: sidebar; }
.page-content { grid-area: content; }
.page-aside   { grid-area: aside; }
.page-footer  { grid-area: footer; }
Amaliy misol: 3 ustunli Blog Layouti

.blog-layout {
  display: grid;
  grid-template-columns: 220px 1fr 200px;
  grid-template-rows: 64px 1fr 48px;
  grid-template-areas:
    "header  header  header"
    "sidebar content aside"
    "footer  footer  footer";
  min-height: 100vh;
  gap: 1rem;
  padding: 1rem;
  background: #f5f5f5;
}
Mini-Loyiha: CSS Grid bilan Portfolio Sahifasi
Quyidagi vazifalarni bajaring:

CSS Grid bilan 3 ustunli portfolio to'ri yarating.
Birinchi loyihani ikki ustun kengligida (featured) qiling (grid-column: span 2).
Sarlavha va footer ni to'liq kenglikda (grid-column: 1 / -1) joylashtiring.
/* Misol: Grid tuzilmasi */
.portfolio {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
.portfolio__featured { grid-column: span 2; }
Maqsad: Grid bilan murakkab sahifa tartibini qurish.
