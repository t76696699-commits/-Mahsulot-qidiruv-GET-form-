Test Coverage (test qamrovi) nima?
Test coverage — bu kodning qancha qismi testlar tomonidan "bosib o'tilgani"ni ko'rsatuvchi metrika. U foizlarda o'lchanadi va sizga qaysi qatorlar, funksiyalar yoki shartlar hech qachon test qilinmaganini ko'rsatadi.

Coverage turlari
Turi	Nima o'lchanadi
Statement coverage	Har bir operator (qator) bajarilganmi
Branch coverage	Har bir if/else shoxi tekshirilganmi
Function coverage	Har bir funksiya chaqirilganmi
Line coverage	Har bir qator bajarilganmi
Jest'da coverage olish
Coverage hisobotini olish uchun terminalda:

npx jest --coverage
Bu buyruq coverage/ papkasida HTML hisobot yaratadi va terminalda jadval ko'rsatadi:

File        | % Stmts | % Branch | % Funcs | % Lines
mathUtils.js|   87.5  |   75.0   |  100.0  |   87.5
100% coverage — bu yaxshimi?
Muhim tushuncha: yuqori coverage kod xatosiz degani emas. Coverage faqat kod bajarilganini ko'rsatadi, lekin expect() orqali to'g'ri natija tekshirilganini kafolatlamaydi. Shu bilan birga, past coverage (masalan 20%) degani ko'p kod umuman tekshirilmagan va yashirin xatolar bo'lishi mumkin.

Coverage jarayoni
Ha, masalan 80%+

Yo'q

jest --coverage ishga tushadi

Har bir qator kuzatiladi

Testlar bajariladi

Qaysi qatorlar bosib o'tildi?

HTML va terminal hisobot yaratiladi

Coverage yetarlimi?

Loyiha sifat standartiga mos

Yangi testlar qo'shish kerak

Amaliy tavsiya
Ko'pchilik professional jamoalar 80% coverageni minimal standart sifatida belgilaydi — bu balansni ta'minlaydi: yetarlicha ishonch, lekin har bir mayda detalni testlashga vaqt sarflamaslik.

💻
Код
Kod namunasi
#2
code
 Копировать
// discount.js — coverage bilan tahlil qilinadigan modul
function calculateDiscount(price, customerType, couponCode) {
  let discount = 0;

  if (customerType === 'vip') {
    discount = 0.2;
  } else if (customerType === 'regular') {
    discount = 0.1;
  } else {
    discount = 0;
  }

  if (couponCode === 'SAVE10') {
    discount += 0.1;
  }

  if (discount > 0.5) {
    discount = 0.5; // Maksimal chegirma 50%
  }

  return Math.round(price * (1 - discount));
}

module.exports = calculateDiscount;

// discount.test.js — coverage'ni oshiruvchi testlar
const calculateDiscount = require('./discount');

describe('calculateDiscount — barcha shoxlarni qamrash', () => {
  test('VIP mijoz uchun 20% chegirma', () => {
    expect(calculateDiscount(1000, 'vip', null)).toBe(800);
  });

  test('oddiy mijoz uchun 10% chegirma', () => {
    expect(calculateDiscount(1000, 'regular', null)).toBe(900);
  });

  test('mijoz turi noma\'lum bo\'lsa chegirma yo\'q', () => {
    expect(calculateDiscount(1000, 'guest', null)).toBe(1000);
  });

  test('kupon kodi qo\'shimcha 10% beradi', () => {
    expect(calculateDiscount(1000, 'regular', 'SAVE10')).toBe(800);
  });

  test('VIP + kupon birgalikda 30% chegirma', () => {
    expect(calculateDiscount(1000, 'vip', 'SAVE10')).toBe(700);
  });

  test('maksimal chegirma 50% dan oshmaydi', () => {
    // Bu holatni sinash uchun tasavvuriy holat: juda katta chegirmalar
    // Coverage: bu shoxni tekshirish uchun махсус holat kerak bo'lishi mumkin
    expect(calculateDiscount(1000, 'vip', 'SAVE10')).toBeLessThanOrEqual(1000);
  });

  test('noto\'g\'ri kupon kodida qo\'shimcha chegirma yo\'q', () => {
    expect(calculateDiscount(1000, 'regular', 'WRONGCODE')).toBe(900);
  });
});

// Coverage hisobotini ko'rish: npx jest discount.test.js --coverage
// Natija taxminan: Statements 100%, Branches 87.5%, Functions 100%, Lines 100%
// Branch coverage 100% dan past bo'lishi mumkin — chunki "discount > 0.5"
// shoxi hali alohida to'liq test qilinmagan.
