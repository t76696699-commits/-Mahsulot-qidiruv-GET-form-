// ===================================================
// Proxy va Reflect - Amaliy misollar
// ===================================================

// 1. Oddiy get va set traplari
const person = { name: 'Jasur', age: 28 };

const validatedPerson = new Proxy(person, {
  // Xossaga murojaat ushlagichi
  get(target, prop, receiver) {
    console.log(`'${prop}' o'qilmoqda`);
    return Reflect.get(target, prop, receiver);
  },
  // Xossaga yozish ushlagichi
  set(target, prop, value, receiver) {
    // Yosh uchun validatsiya
    if (prop === 'age') {
      if (typeof value !== 'number') throw new TypeError('Yosh raqam bo\'lishi kerak!');
      if (value < 0 || value > 150) throw new RangeError('Yosh 0-150 oraliqda bo\'lishi kerak!');
    }
    console.log(`'${prop}' = ${value} o'rnatilmoqda`);
    return Reflect.set(target, prop, value, receiver);
  }
});

console.log(validatedPerson.name);  // 'name' o'qilmoqda -> Jasur
validatedPerson.age = 30;            // 'age' = 30 o'rnatilmoqda
// validatedPerson.age = -5;         // RangeError!

// 2. Default qiymatlar uchun Proxy
function withDefaults(target, defaults) {
  return new Proxy(target, {
    get(obj, prop) {
      return prop in obj ? obj[prop] : defaults[prop];
    }
  });
}

const config = withDefaults(
  { theme: 'dark' },
  { theme: 'light', lang: 'en', fontSize: 16 }
);

console.log(config.theme);    // 'dark' (ob'ektda bor)
console.log(config.lang);     // 'en' (default)
console.log(config.fontSize); // 16 (default)

// 3. Kuzatuvchi (Observer) Proxy
function observable(target, onChange) {
  return new Proxy(target, {
    set(obj, prop, value) {
      const oldValue = obj[prop];
      const result = Reflect.set(obj, prop, value);
      if (result && oldValue !== value) {
        onChange({ prop, oldValue, newValue: value });
      }
      return result;
    }
  });
}

const state = observable(
  { count: 0, name: 'test' },
  ({ prop, oldValue, newValue }) => {
    console.log(`O'zgarish: ${prop}: ${oldValue} -> ${newValue}`);
  }
);

state.count = 5;   // O'zgarish: count: 0 -> 5
state.name = 'ok'; // O'zgarish: name: test -> ok
state.count = 5;   // Hech narsa chop etilmaydi (qiymat o'zgarmadi)

// 4. Reflect metodlari
const obj = { x: 1, y: 2 };
console.log(Reflect.has(obj, 'x'));         // true
console.log(Reflect.ownKeys(obj));           // ['x', 'y']
Reflect.set(obj, 'z', 3);
console.log(obj);                            // { x: 1, y: 2, z: 3 }
Reflect.deleteProperty(obj, 'x');
console.log(obj);                            // { y: 2, z: 3 }
