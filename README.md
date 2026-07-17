 // ===================================================
// Symbol - Amaliy misollar
// ===================================================

// 1. Symbol noyobligini tekshirish
const s1 = Symbol('id');
const s2 = Symbol('id');
console.log(s1 === s2);        // false — har doim noyob!
console.log(typeof s1);         // 'symbol'
console.log(s1.toString());     // 'Symbol(id)'
console.log(s1.description);    // 'id'

// 2. Ob'ekt xossasi kaliti sifatida
const USER_ID = Symbol('userId');
const ROLE = Symbol('role');

const user = {
  name: 'Nodira',
  [USER_ID]: 12345,    // Symbol kalit
  [ROLE]: 'admin'
};

console.log(user[USER_ID]);       // 12345
console.log(user[ROLE]);           // 'admin'
console.log(Object.keys(user));    // ['name'] — Symbol ko'rinmaydi!
console.log(JSON.stringify(user)); // {"name":"Nodira"} — Symbol yo'q

// Symbollarni ko'rish
const symbols = Object.getOwnPropertySymbols(user);
console.log(symbols); // [Symbol(userId), Symbol(role)]

// 3. Symbol.for — Global registry
const globalSym1 = Symbol.for('sharedKey');
const globalSym2 = Symbol.for('sharedKey');
console.log(globalSym1 === globalSym2); // true — bir xil!
console.log(Symbol.keyFor(globalSym1)); // 'sharedKey'

// 4. Well-known Symbol: Symbol.iterator
class Range {
  constructor(start, end) {
    this.start = start;
    this.end = end;
  }

  // Maxsus iterator yaratish
  [Symbol.iterator]() {
    let current = this.start;
    const end = this.end;
    return {
      next() {
        return current <= end
          ? { value: current++, done: false }
          : { value: undefined, done: true };
      }
    };
  }
}

const r = new Range(1, 5);
console.log([...r]);              // [1, 2, 3, 4, 5]
for (const n of r) console.log(n); // 1 2 3 4 5

// 5. Symbol.toPrimitive
const temperature = {
  celsius: 25,
  [Symbol.toPrimitive](hint) {
    if (hint === 'number') return this.celsius;
    if (hint === 'string') return `${this.celsius}°C`;
    return this.celsius;  // default
  }
};

console.log(+temperature);        // 25 (number hint)
console.log(`Harorat: ${temperature}`); // 'Harorat: 25°C' (string hint)
console.log(temperature + 5);     // 30 (default hint)
