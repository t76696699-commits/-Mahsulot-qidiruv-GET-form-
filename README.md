// ===================================================
// structuredClone va Immutability - Amaliy misollar
// ===================================================

// 1. structuredClone asosiy ishlatish
const original = {
  name: 'Zulfiya',
  scores: [85, 92, 78],
  meta: { date: new Date('2024-01-15'), active: true },
  tags: new Set(['js', 'python'])
};

const clone = structuredClone(original);

// Chuqur nusxa — asl va nusxa mustaqil
clone.scores.push(99);
clone.name = 'Malika';

console.log(original.scores); // [85, 92, 78] — O'ZGARMAGAN!
console.log(clone.scores);     // [85, 92, 78, 99]
console.log(original.name);    // 'Zulfiya'
console.log(clone.name);       // 'Malika'

// Date to'g'ri nusxalanadi
console.log(clone.meta.date instanceof Date); // true!
console.log(clone.meta.date.getFullYear());   // 2024

// 2. JSON.stringify vs structuredClone
const data = {
  date: new Date(),
  map: new Map([['a', 1]]),
  set: new Set([1, 2, 3]),
  undef: undefined,
  fn: () => {} // Funksiyalar nusxalanmaydi!
};

// JSON yo'li — ko'p narsa yo'qoladi!
const jsonClone = JSON.parse(JSON.stringify(data));
console.log(jsonClone.date);  // string! (Date emas)
console.log(jsonClone.map);   // {} (Map emas)
console.log(jsonClone.undef); // yo'q (undefined o'chdi)

// structuredClone — to'g'ri nusxalash
// (funksiya va prototype o'tkazilmaydi — bu cheklash)
const structClone = structuredClone({ date: new Date(), set: new Set([1,2]) });
console.log(structClone.date instanceof Date); // true
console.log(structClone.set instanceof Set);   // true

// 3. Immutability — Noto'g'ri va to'g'ri yondashuv

// ❌ NOTO'G'RI: Asl ob'ektni o'zgartirish (mutation)
function updateUserBad(user, newName) {
  user.name = newName; // Asl ob'ektni o'zgartiradi!
  return user;
}

// ✅ TO'G'RI: Yangi ob'ekt yaratish (immutable)
function updateUserGood(user, newName) {
  return { ...user, name: newName }; // Yangi ob'ekt!
}

// ✅ TO'G'RI: structuredClone bilan chuqur nusxa
function updateUserDeep(user, updates) {
  const copy = structuredClone(user);
  return Object.assign(copy, updates);
}

// 4. Object.freeze — sayoz muzlatish
const config = Object.freeze({
  API_URL: 'https://api.example.com',
  MAX_RETRIES: 3,
  settings: { timeout: 5000 } // Bu qism muzlatilmagan!
});

// config.API_URL = 'other'; // Strict modeda xatolik!
config.settings.timeout = 99999; // Bu ishlaydi! (sayoz freeze)

// 5. Chuqur freeze funksiyasi
function deepFreeze(obj) {
  Object.getOwnPropertyNames(obj).forEach(name => {
    const value = obj[name];
    if (typeof value === 'object' && value !== null) {
      deepFreeze(value);
    }
  });
  return Object.freeze(obj);
}

const frozen = deepFreeze({ a: { b: { c: 42 } } });
// frozen.a.b.c = 100; // Strict modeda xatolik!
console.log(frozen.a.b.c); // 42 — o'zgarmagan!
