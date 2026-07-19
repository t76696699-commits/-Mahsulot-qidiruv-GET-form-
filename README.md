1-qism: HashMap va Chaining (LinkedList) Implementatsiyasi
Bu versiyada set, get, delete, keys, values va entries metodlari mavjud. collision (to'qnashuv) holatlari uchun LinkedList (zanjirlash) ishlatilgan.

JavaScript
class HashMap {
  constructor(size = 53) {
    this.keyMap = new Array(size);
    this.size = size;
  }

  _hash(key) {
    let total = 0;
    const PRIME = 31;
    for (let i = 0; i < Math.min(key.length, 100); i++) {
      let char = key[i];
      let value = char.charCodeAt(0) - 96;
      total = (total * PRIME + value) % this.size;
    }
    return total;
  }

  set(key, value) {
    let index = this._hash(key);
    if (!this.keyMap[index]) {
      this.keyMap[index] = [];
    }
    // Agar kalit mavjud bo'lsa, qiymatni yangilash
    for (let pair of this.keyMap[index]) {
      if (pair[0] === key) {
        pair[1] = value;
        return;
      }
    }
    this.keyMap[index].push([key, value]);
  }

  get(key) {
    let index = this._hash(key);
    if (this.keyMap[index]) {
      for (let pair of this.keyMap[index]) {
        if (pair[0] === key) return pair[1];
      }
    }
    return undefined;
  }

  delete(key) {
    let index = this._hash(key);
    if (this.keyMap[index]) {
      for (let i = 0; i < this.keyMap[index].length; i++) {
        if (this.keyMap[index][i][0] === key) {
          this.keyMap[index].splice(i, 1);
          return true;
        }
      }
    }
    return false;
  }

  keys() {
    let arr = [];
    for (let bucket of this.keyMap) {
      if (bucket) for (let pair of bucket) arr.push(pair[0]);
    }
    return arr;
  }

  values() {
    let arr = [];
    for (let bucket of this.keyMap) {
      if (bucket) for (let pair of bucket) arr.push(pair[1]);
    }
    return arr;
  }

  entries() {
    let arr = [];
    for (let bucket of this.keyMap) {
      if (bucket) for (let pair of bucket) arr.push(pair);
    }
    return arr;
  }

  // To'qnashuvlar tahlili uchun yordamchi metod
  getCollisionStats() {
    let stats = { totalCollisions: 0, bucketsWithCollisions: 0 };
    for (let bucket of this.keyMap) {
      if (bucket && bucket.length > 1) {
        stats.totalCollisions += (bucket.length - 1);
        stats.bucketsWithCollisions++;
      }
    }
    return stats;
  }
}
2-qism: So'zlar Lug'ati (30 ta juftlik)
JavaScript
const dictionary = new HashMap(53);
const words = [
  ["apple", "olma"], ["book", "kitob"], ["cat", "mushuk"], ["dog", "it"], ["eye", "ko'z"],
  ["fish", "baliq"], ["goat", "echki"], ["hat", "shlyapa"], ["ice", "muz"], ["jump", "sakramoq"],
  ["key", "kalit"], ["lamp", "chiroq"], ["moon", "oy"], ["nest", "uyali"], ["owl", "boyo'g'li"],
  ["pen", "qalam"], ["queen", "qirolicha"], ["rain", "yomg'ir"], ["sun", "quyosh"], ["tree", "daraxt"],
  ["umbrella", "soyabon"], ["van", "furgon"], ["water", "suv"], ["xray", "rentgen"], ["yellow", "sariq"],
  ["zoo", "hayvonot bog'i"], ["bird", "qush"], ["desk", "parta"], ["food", "ovqat"], ["girl", "qiz"]
];

words.forEach(([eng, uzb]) => dictionary.set(eng, uzb));

// Tarjima funksiyasi
const translate = (word) => dictionary.get(word.toLowerCase()) || "Topilmadi";
console.log("Tarjima ('apple'):", translate("apple"));
3-qism: Collision Tahlili
JavaScript
const stats = dictionary.getCollisionStats();
console.log("--- Tahlil Natijalari ---");
console.log("Jami elementlar:", words.length);
console.log("To'qnashuvlar soni (Collisions):", stats.totalCollisions);
console.log("To'qnashuv sodir bo'lgan chelaklar (Buckets):", stats.bucketsWithCollisions);

// Bucket to'lish tartibi
dictionary.keyMap.forEach((bucket, i) => {
  if (bucket && bucket.length > 0) {
    console.log(`Bucket ${i}: ${bucket.length} ta element -`, bucket.map(p => p[0]));
  }
});
