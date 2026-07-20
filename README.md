Yakuniy loyiha: To'liq test qamrovi bilan modul yaratish
Bu darsda siz o'rgangan barcha bilimlarni birlashtirasiz: Jest asoslari, mock/spy, asinxron testlash, TDD va coverage. Vazifa — "Savatcha" (Shopping Cart) modulini TDD yondashuvi bilan yaratish va uni to'liq test qilish.

Loyiha talablari
Mahsulot qo'shish (addItem) — narx va miqdorni saqlaydi
Mahsulot olib tashlash (removeItem)
Umumiy summani hisoblash (getTotal) — chegirmalarni hisobga oladi
Asinxron "to'lovni tasdiqlash" funksiyasi (checkout) — tashqi to'lov API'sini mock qilish orqali test qilinadi
TDD yondashuvida ishlash tartibi
Har bir metod uchun avval testlar yoziladi (RED)
Testlarni o'tkazish uchun minimal kod yoziladi (GREEN)
Kod tozalanadi, takrorlanishlar olib tashlanadi (REFACTOR)
Coverage hisobot orqali tekshirilmagan holatlar aniqlanadi va yangi testlar qo'shiladi
Test strategiyasi
Funksiya	Test turi
addItem, removeItem	Oddiy sinxron unit test
getTotal	Turli holatlar: bo'sh savat, chegirmali, chegirmasiz
checkout	Asinxron test + to'lov API mocklash
Yakuniy loyiha oqimi
Ha

Yo'q

Talablarni tahlil qilish

TDD: RED - testlar yozish

TDD: GREEN - minimal kod

TDD: REFACTOR - kodni tozalash

Mock: to'lov API'sini soxtalashtirish

Asinxron checkout testlari

Coverage hisobotini tekshirish

Coverage >= 80%?

Loyiha yakunlandi ✔

Bu loyiha real ish joylarida qo'llaniladigan to'liq test strategiyasini aks ettiradi — bir nechta test turini birlashtirish orqali ishonchli va barqaror kod yaratish.

💻
Код
Kod namunasi
#2
code
 Копировать
// shoppingCart.js — TDD bilan yaratilgan yakuniy modul
class ShoppingCart {
  constructor() {
    this.items = [];
  }

  addItem(name, price, quantity = 1) {
    if (price < 0) throw new Error("Narx manfiy bo'lishi mumkin emas");
    this.items.push({ name, price, quantity });
  }

  removeItem(name) {
    this.items = this.items.filter((item) => item.name !== name);
  }

  getTotal(discountPercent = 0) {
    const subtotal = this.items.reduce(
      (sum, item) => sum + item.price * item.quantity,
      0
    );
    return Math.round(subtotal * (1 - discountPercent / 100));
  }

  async checkout(paymentGateway) {
    const total = this.getTotal();
    if (total === 0) {
      throw new Error("Savatcha bo'sh");
    }
    const result = await paymentGateway.charge(total);
    if (!result.success) {
      throw new Error("To'lov amalga oshmadi");
    }
    this.items = [];
    return { total, transactionId: result.transactionId };
  }
}

module.exports = ShoppingCart;

// shoppingCart.test.js — to'liq test to'plami
const ShoppingCart = require('./shoppingCart');

describe('ShoppingCart — sinxron metodlar', () => {
  let cart;

  beforeEach(() => {
    cart = new ShoppingCart();
  });

  test('mahsulot qo\'shiladi va summaga qo\'shiladi', () => {
    cart.addItem('Kitob', 50000, 2);
    expect(cart.getTotal()).toBe(100000);
  });

  test('mahsulot olib tashlanadi', () => {
    cart.addItem('Kitob', 50000, 1);
    cart.addItem('Ruchka', 5000, 1);
    cart.removeItem('Ruchka');
    expect(cart.getTotal()).toBe(50000);
  });

  test('chegirma to\'g\'ri hisoblanadi', () => {
    cart.addItem('Noutbuk', 1000000, 1);
    expect(cart.getTotal(10)).toBe(900000);
  });

  test('manfiy narxda xato tashlaydi', () => {
    expect(() => cart.addItem('Xato mahsulot', -100, 1)).toThrow();
  });
});

describe('ShoppingCart — checkout (asinxron + mock)', () => {
  let cart;

  beforeEach(() => {
    cart = new ShoppingCart();
    cart.addItem('Telefon', 500000, 1);
  });

  test('muvaffaqiyatli to\'lovda savat tozalanadi', async () => {
    const mockGateway = {
      charge: jest.fn().mockResolvedValue({
        success: true,
        transactionId: 'TXN123',
      }),
    };

    const result = await cart.checkout(mockGateway);

    expect(result.total).toBe(500000);
    expect(result.transactionId).toBe('TXN123');
    expect(cart.items.length).toBe(0);
    expect(mockGateway.charge).toHaveBeenCalledWith(500000);
  });

  test('to\'lov muvaffaqiyatsiz bo\'lsa xato tashlaydi', async () => {
    const mockGateway = {
      charge: jest.fn().mockResolvedValue({ success: false }),
    };

    await expect(cart.checkout(mockGateway)).rejects.toThrow(
      "To'lov amalga oshmadi"
    );
  });

  test('bo\'sh savatda checkout xato tashlaydi', async () => {
    const emptyCart = new ShoppingCart();
    const mockGateway = { charge: jest.fn() };

    await expect(emptyCart.checkout(mockGateway)).rejects.toThrow(
      "Savatcha bo'sh"
    );
    expect(mockGateway.charge).not.toHaveBeenCalled();
  });
});

// Ishga tushirish: npx jest shoppingCart.test.js --coverage --verbose
