Flexbox nima?
Flexbox (Flexible Box Layout) — CSS-ning zamonaviy joylashuv modeli bo'lib, elementlarni qator yoki ustun bo'ylab moslashuvchan tarzda joylashtirish imkonini beradi. U responsive dizayn yaratishni sezilarli darajada osonlashtiradi va murakkab layoutlarni bir necha qator kod bilan amalga oshirish imkonini beradi.

1. display: flex — Flexbox-ni yoqish
Konteyner elementiga display: flex xususiyatini qo'shish uning barcha to'g'ridan-to'g'ri bolalarini flex elementlarga aylantiradi. Bu Flexbox-dan foydalanishning birinchi va eng muhim qadamidir.

.container {
  display: flex;
  background-color: #f5f5f5;
  padding: 16px;
}
2. flex-direction — Yo'nalishni belgilash
flex-direction xususiyati elementlarning asosiy o'q bo'ylab qanday yo'nalishda joylashishini belgilaydi. To'rtta qiymat mavjud:

row — chapdan o'ngga (standart qiymat)
row-reverse — o'ngdan chapga
column — yuqoridan pastga
column-reverse — pastdan yuqoriga
.vertical-container {
  display: flex;
  flex-direction: column;
}

.horizontal-container {
  display: flex;
  flex-direction: row;
}
3. justify-content — Asosiy o'q bo'ylab tekislash
justify-content elementlarni asosiy o'q bo'ylab qanday taqsimlash kerakligini boshqaradi:

flex-start — boshiga tekislash (standart)
flex-end — oxiriga tekislash
center — markazga tekislash
space-between — birinchi va oxirgi element chekkada, qolganlar orasida teng masofa
space-around — har bir element atrofida teng masofa
space-evenly — barcha bo'shliqlar teng
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 24px;
  height: 60px;
}
4. align-items — Ko'ndalang o'q bo'ylab tekislash
align-items elementlarni ko'ndalang o'q bo'ylab tekislaydi.

stretch — konteyner balandligiga cho'zish (standart)
flex-start — tepaga tekislash
flex-end — pastga tekislash
center — vertikal markazga tekislash
baseline — matn bazasi bo'yicha tekislash
5. flex-wrap — Ko'p qatorga o'rash
Standart holda barcha flex elementlar bitta qatorda joylashadi. flex-wrap: wrap ular uchun yangi qatorga o'tish imkonini beradi.

.gallery {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}

.gallery-item {
  flex: 1 1 200px;
}
6. flex-grow, flex-shrink, flex-basis
flex-grow — bo'sh joyni qancha o'sib to'ldirish
flex-shrink — joy yetishmasa qancha kichrayish
flex-basis — elementning boshlang'ich asosiy o'lchami
7. gap — Elementlar orasidagi masofa
.container {
  display: flex;
  gap: 20px;
}
Amaliy misol: Karta Layouti
.cards-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: stretch;
  gap: 24px;
  padding: 32px;
  max-width: 1200px;
  margin: 0 auto;
}

.card {
  display: flex;
  flex-direction: column;
  flex: 1 1 280px;
  background: #ffffff;
  border-radius: 12px;
  padding: 24px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
}

.card__text {
  flex-grow: 1;
  color: #555;
  margin-bottom: 16px;
}

.card__btn {
  align-self: flex-start;
  padding: 10px 20px;
  background: #4f46e5;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}
Mini-Loyiha: Flexbox bilan Navigatsiya va Karta Qatori
Quyidagi vazifalarni bajaring:

Flexbox yordamida gorizontal navigatsiya paneli yarating — logotip chapda, menu o'ngda.
3 ta karta (rasm, sarlavha, matn, tugma) ni bir qatorda, teng kenglikda joylashtiring.
flex-wrap bilan ekran torayganida kartalar pastga tushsin.
/* Misol: Flex konteyner */
.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 32px;
}
Maqsad: Flexbox bilan real sahifa tartibini qurish.
