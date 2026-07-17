// ===================================================
// Review 1 — Barcha mavzularni birlashtirgan misol
// ===================================================

// Symbol bilan noyob kalit
const _cache = Symbol('cache');
const _privateData = new WeakMap();

// Generator bilan lazy ma'lumot yuklash
function* dataLoader(urls) {
  for (const url of urls) {
    yield fetch(url).then(r => r.json());
  }
}

// Proxy bilan validatsiya va logging
class DataStore {
  constructor() {
    _privateData.set(this, { items: [], [_cache]: new Map() });

    return new Proxy(this, {
      get(target, prop, receiver) {
        if (prop === 'size') {
          return _privateData.get(target).items.length;
        }
        return Reflect.get(target, prop, receiver);
      }
    });
  }

  add(item) {
    const data = _privateData.get(this);
    data.items.push(structuredClone(item));
  }

  // Generator bilan iterable qilish
  *[Symbol.iterator]() {
    const items = _privateData.get(this).items;
    for (const item of items) {
      yield structuredClone(item); // Immutable chiqarish
    }
  }
}

const store = new DataStore();
store.add({ id: 1, name: 'Ali', date: new Date() });
store.add({ id: 2, name: 'Vali', date: new Date() });

console.log(store.size); // 2 (Proxy get trap)

for (const item of store) {
  console.log(item.name); // 'Ali', 'Vali'
}

// Event Loop bilan asinxron yaratish
async function loadAllData(store) {
  console.log('Yuklash boshlandi (sinxron)');

  await Promise.resolve();
  console.log('Microtask: birinchi yield');

  for (const item of store) {
    // Har bir element uchun microtask
    await Promise.resolve();
    console.log('Qayta ishlandi:', item.id);
  }
}

loadAllData(store);
console.log('Bu microtask lardan oldin chiqadi!');
