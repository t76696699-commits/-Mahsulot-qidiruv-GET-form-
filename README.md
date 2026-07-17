// ===================================================
// Iterator va Generator - Amaliy misollar
// ===================================================

// 1. Custom Iterator yaratish
function createRangeIterator(start, end, step = 1) {
  let current = start;
  return {
    next() {
      if (current <= end) {
        const value = current;
        current += step;
        return { value, done: false };
      }
      return { value: undefined, done: true };
    },
    // Iterable qilish uchun
    [Symbol.iterator]() { return this; }
  };
}

const range = createRangeIterator(1, 10, 2);
for (const num of range) {
  console.log(num); // 1, 3, 5, 7, 9
}

// 2. Generator funksiya
function* fibonacci() {
  let a = 0, b = 1;
  while (true) {
    yield a;           // Qiymat chiqarib to'xtaydi
    [a, b] = [b, a + b]; // Keyingi chaqiruvda davom etadi
  }
}

// Cheksiz Fibonacci ketmasidan birinchi 8 ta olish
const fib = fibonacci();
const first8 = Array.from({ length: 8 }, () => fib.next().value);
console.log(first8); // [0, 1, 1, 2, 3, 5, 8, 13]

// 3. Generator bilan diapason yaratish
function* range(start, end, step = 1) {
  for (let i = start; i <= end; i += step) {
    yield i;
  }
}

console.log([...range(0, 20, 5)]); // [0, 5, 10, 15, 20]

// 4. yield* delegatsiya
function* gen1() {
  yield 'A';
  yield 'B';
}

function* gen2() {
  yield 1;
  yield* gen1(); // gen1 ni delegatsiya qilish
  yield 2;
}

console.log([...gen2()]); // [1, 'A', 'B', 2]

// 5. Generator bilan asinxron simulyatsiya
function* taskRunner() {
  console.log('1-vazifa boshlandi');
  const result1 = yield fetchData('users');
  console.log('Foydalanuvchilar olindi:', result1);

  console.log('2-vazifa boshlandi');
  const result2 = yield fetchData('posts');
  console.log('Postlar olindi:', result2);
}

// Asinxron generator'ni ishga tushiruvchi
function run(generatorFn) {
  const gen = generatorFn();
  function step(value) {
    const { value: promise, done } = gen.next(value);
    if (!done) promise.then(step);
  }
  step();
}

// Oddiy kesh bilan ishlash
function* memoize(iterable) {
  const cache = [];
  for (const item of iterable) {
    cache.push(item);
    yield item;
  }
  console.log('Kesh:', cache);
}

for (const v of memoize([10, 20, 30])) {
  console.log(v); // 10, 20, 30
}
// Kesh: [10, 20, 30]
