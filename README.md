// ===================================================
// Event Loop chuqurroq - Amaliy misollar
// ===================================================

// 1. Bajarilish tartibini bashorat qiling
console.log('1 — Sinxron: boshlandi');

setTimeout(() => {
  console.log('4 — Macrotask: setTimeout 0ms');
}, 0);

Promise.resolve()
  .then(() => console.log('3 — Microtask: Promise.then'));

queueMicrotask(() => {
  console.log('3.5 — Microtask: queueMicrotask');
});

console.log('2 — Sinxron: tugadi');
// Natija: 1, 2, 3, 3.5, 4

// 2. Microtask zanjirlari
console.log('--- Microtask zanjiri ---');
Promise.resolve()
  .then(() => {
    console.log('Microtask 1');
    return Promise.resolve('2-dan qiymat');
  })
  .then((val) => {
    console.log('Microtask 2:', val);
  })
  .then(() => {
    console.log('Microtask 3');
  });

setTimeout(() => console.log('Macrotask — bu eng oxirida!'), 0);

// 3. UI muzlatish muammosi
function heavySync(n) {
  // Bu Call Stack ni uzoq ushlab turadi — UI muzlaydi!
  let result = 0;
  for (let i = 0; i < n; i++) result += i;
  return result;
}

// Yechim: katta ishni bo'laklarga bo'lish
async function heavyAsync(n, chunkSize = 100_000) {
  let result = 0;
  for (let i = 0; i < n; i += chunkSize) {
    const end = Math.min(i + chunkSize, n);
    for (let j = i; j < end; j++) result += j;
    // Har bo'lakdan keyin Event Loop ga nazorat qaytarish
    await new Promise(resolve => setTimeout(resolve, 0));
  }
  return result;
}

// 4. Promise va setTimeout tartibini tahlil qilish
async function analyzeOrder() {
  console.log('async funksiya boshlandi (sinxron qism)');

  await Promise.resolve(); // Microtask — keyingi tick ga o'tadi

  console.log('await dan keyin (microtask sifatida bajariladi)');
}

analyzeOrder();
console.log('analyzeOrder() dan keyin (sinxron davom etadi)');
// Natija:
// 'async funksiya boshlandi'
// 'analyzeOrder() dan keyin'
// 'await dan keyin'

// 5. requestAnimationFrame (Brauzerda)
// requestAnimationFrame — macrotask emas, render frame oldidan chaqiriladi
// setTimeout(fn, 0) — macrotask (keyingi event loop tick)
// Promise.then — microtask (joriy tick ichida)

function demoOrder() {
  console.log('Boshlanish');
  // requestAnimationFrame(() => console.log('rAF — render oldidan'));
  setTimeout(() => console.log('setTimeout'), 0);
  Promise.resolve().then(() => console.log('Promise'));
  console.log('Oxiri');
  // Boshlanish -> Oxiri -> Promise -> setTimeout
}
demoOrder();
