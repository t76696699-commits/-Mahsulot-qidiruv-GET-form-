Web Workers
Урок 9 из 11
· 3 раздела
✓ Пройден
📝
Текст
Nazariya
#1
Web Workers nima?
JavaScript asosan bir oqimli (single-threaded) bo'lib, barcha kod bitta asosiy oqimda (main thread) bajariladi. Bu og'ir hisob-kitoblar UI ni «muzlatishiga» olib keladi. Web Workers — bu brauzer API'si bo'lib, JavaScript kodini alohida background thread'da ishga tushirish imkonini beradi. Shunday qilib, asosiy thread erkin qoladi va foydalanuvchi interfeysini boshqaradi.

Web Worker turlari
Dedicated Worker: Bitta sahifaga bog'liq, eng ko'p ishlatiladigan tur. new Worker('script.js') bilan yaratiladi.
Shared Worker: Bir nechta sahifa/tab o'rtasida umumlashtirilgan worker. new SharedWorker('script.js') bilan yaratiladi.
Service Worker: Tarmoq so'rovlarini ushlab oladi, offline rejim, push notifications. PWA asosini tashkil etadi.
Worker bilan muloqot
Asosiy thread va Worker o'rtasidagi aloqa xabar almashish (message passing) orqali amalga oshiriladi:

worker.postMessage(data) — ma'lumot yuborish
worker.onmessage = (e) => {...} — xabar qabul qilish
worker.terminate() — Worker ni to'xtatish
Worker ichida: self.postMessage(data) va self.onmessage
Nima uchun va qachon ishlatish kerak?
Holat	Worker kerakmi?
Katta JSON parsing (>1MB)	Ha
Kriptografik hisob-kitoblar	Ha
Rasm yoki video qayta ishlash	Ha
Machine learning inference	Ha
Oddiy fetch so'rovlari	Yo'q
UI animatsiyalari	Yo'q
Worker cheklovlari
Web Worker'lar ba'zi imkoniyatlarga ega emas:

DOM'ga kirish imkoni yo'q (document, window yo'q)
UI elementlarini o'zgartira olmaydi
Ma'lumotlar postMessage orqali Structured Clone algoritmi bilan uzatiladi (funksiyalar, DOM elementlari o'tkazilmaydi)
Asosiy Thread UI boshqaradi

new Worker worker.js

worker.postMessage data

Worker alohida threadda ishlaydi

Og'ir hisob-kitoblar bajariladi

self.postMessage natija

Asosiy thread onmessage natija oladi

UI yangilanadi - muzlamaydi!

Inline Worker: Alohida fayl o'rniga Worker kodini Blob yoki URL.createObjectURL orqali to'g'ridan-to'g'ri JavaScript ichida yozish ham mumkin. Bu ayniqsa bundler ishlatilmaydigan loyihalarda qulay.

💻
Код
Kod namunasi
#2
code
 Копировать
// ===================================================
// Web Workers - Amaliy misollar
// ===================================================

// === worker.js (alohida fayl) ===
// self — Worker ichidagi global ob'ekt
self.onmessage = function(event) {
  const { type, data } = event.data;

  if (type === 'COMPUTE_PRIMES') {
    const primes = findPrimes(data.limit);
    self.postMessage({ type: 'PRIMES_RESULT', primes });
  }

  if (type === 'SORT_ARRAY') {
    const sorted = [...data.array].sort((a, b) => a - b);
    self.postMessage({ type: 'SORTED', sorted });
  }
};

function findPrimes(limit) {
  // Sieve of Eratosthenes algoritmi
  const sieve = new Uint8Array(limit + 1).fill(1);
  sieve[0] = sieve[1] = 0;
  for (let i = 2; i * i <= limit; i++) {
    if (sieve[i]) {
      for (let j = i * i; j <= limit; j += i) sieve[j] = 0;
    }
  }
  return sieve.reduce((acc, val, idx) => val ? [...acc, idx] : acc, []);
}

// === main.js (asosiy fayl) ===
class WorkerManager {
  constructor(workerPath) {
    this.worker = new Worker(workerPath);
    this.pending = new Map(); // ID -> { resolve, reject }
    this.nextId = 0;

    this.worker.onmessage = (e) => {
      const { id, result, error } = e.data;
      const callbacks = this.pending.get(id);
      if (!callbacks) return;
      this.pending.delete(id);
      error ? callbacks.reject(new Error(error)) : callbacks.resolve(result);
    };

    this.worker.onerror = (err) => {
      console.error('Worker xatoligi:', err);
    };
  }

  // Promise asosida worker ga vazifa berish
  run(task, data) {
    return new Promise((resolve, reject) => {
      const id = this.nextId++;
      this.pending.set(id, { resolve, reject });
      this.worker.postMessage({ id, task, data });
    });
  }

  terminate() {
    this.worker.terminate();
  }
}

// Inline Worker — alohida fayl shart emas
function createInlineWorker(fn) {
  const blob = new Blob([`(${fn.toString()})()`], { type: 'text/javascript' });
  return new Worker(URL.createObjectURL(blob));
}

const inlineWorker = createInlineWorker(function() {
  self.onmessage = (e) => {
    // Hisob-kitob va natijani qaytarish
    const result = e.data.numbers.reduce((a, b) => a + b, 0);
    self.postMessage({ sum: result });
  };
});

inlineWorker.postMessage({ numbers: [1, 2, 3, 4, 5] });
inlineWorker.onmessage = (e) => {
  console.log('Yig\'indi:', e.data.sum); // 15
  inlineWorker.terminate();
};
