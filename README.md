// TDD misoli: findMax funksiyasini bosqichma-bosqich yaratish

// 1-BOSQICH (RED): Avval testlarni yozamiz — kod hali yo'q
// findMax.test.js
const findMax = require('./findMax');

describe('findMax — TDD yondashuvi', () => {
  test('bitta elementli massiv', () => {
    expect(findMax([5])).toBe(5);
  });

  test('bir nechta elementli massiv', () => {
    expect(findMax([3, 7, 2, 9, 4])).toBe(9);
  });

  test('barchasi manfiy sonlar', () => {
    expect(findMax([-5, -1, -10])).toBe(-1);
  });

  test('bo\'sh massiv uchun xato tashlaydi', () => {
    expect(() => findMax([])).toThrow('Massiv bo\'sh bo\'lmasligi kerak');
  });

  test('massiv bo\'lmagan qiymat uchun xato tashlaydi', () => {
    expect(() => findMax(null)).toThrow();
  });
});

// 2-BOSQICH (GREEN): Testlarni o'tkazish uchun minimal kod
// findMax.js — birinchi versiya
function findMax(arr) {
  if (!Array.isArray(arr) || arr.length === 0) {
    throw new Error("Massiv bo'sh bo'lmasligi kerak");
  }
  return Math.max(...arr);
}

module.exports = findMax;

// 3-BOSQICH (REFACTOR): Kodni tozalash va optimallashtirish
// findMax.js — yakuniy, refaktor qilingan versiya
function findMaxRefactored(numbers) {
  validateInput(numbers);
  return numbers.reduce((max, current) => (current > max ? current : max));
}

function validateInput(numbers) {
  if (!Array.isArray(numbers) || numbers.length === 0) {
    throw new Error("Massiv bo'sh bo'lmasligi kerak");
  }
}

module.exports = findMaxRefactored;

// Testlar refaktordan keyin ham PASS bo'lishi kerak — bu TDD ning
// asosiy afzalligi: kod ichki tuzilishi o'zgaradi, tashqi xatti-harakat
// (kontrakt) bir xil qoladi va testlar buni kafolatlaydi.

// Ishga tushirish: npx jest findMax.test.js --verbose
