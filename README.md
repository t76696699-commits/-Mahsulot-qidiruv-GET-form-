// Oddiy HashMap implementatsiyasi (chaining usuli bilan)

class HashMap {
  constructor(size = 16) {
    this.size = size;
    this.buckets = new Array(size).fill(null).map(() => []);
  }

  // Oddiy hash funksiyasi - kalitni raqamli indeksga aylantiradi
  _hash(key) {
    let hash = 0;
    const stringKey = String(key);
    for (let i = 0; i < stringKey.length; i++) {
      hash = (hash + stringKey.charCodeAt(i) * (i + 1)) % this.size;
    }
    return hash;
  }

  // Kalit-qiymat juftligini qo'shish yoki yangilash - O(1) o'rtacha
  set(key, value) {
    const index = this._hash(key);
    const bucket = this.buckets[index];

    const existing = bucket.find((entry) => entry[0] === key);
    if (existing) {
      existing[1] = value; // Mavjud kalitni yangilash
    } else {
      bucket.push([key, value]); // Yangi juftlik qo'shish
    }
  }

  // Kalit bo'yicha qiymatni olish - O(1) o'rtacha
  get(key) {
    const index = this._hash(key);
    const bucket = this.buckets[index];
    const entry = bucket.find((entry) => entry[0] === key);
    return entry ? entry[1] : undefined;
  }

  // Kalitni o'chirish - O(1) o'rtacha
  delete(key) {
    const index = this._hash(key);
    const bucket = this.buckets[index];
    const entryIndex = bucket.findIndex((entry) => entry[0] === key);

    if (entryIndex !== -1) {
      bucket.splice(entryIndex, 1);
      return true;
    }
    return false;
  }

  has(key) {
    return this.get(key) !== undefined;
  }
}

// Amaliyot
const map = new HashMap();
map.set("name", "Ali");
map.set("age", 25);
map.set("email", "ali@example.com");

console.log("name:", map.get("name")); // Ali
console.log("age:", map.get("age")); // 25
console.log("has phone:", map.has("phone")); // false

map.delete("age");
console.log("age o'chirilgandan keyin:", map.get("age")); // undefined

// JavaScript'ning o'rnatilgan Map obyekti bilan solishtirish
const builtinMap = new Map();
builtinMap.set("name", "Ali");
console.log("Built-in Map:", builtinMap.get("name"));
