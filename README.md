HashMap (Hash Table) nima?
HashMap — kalit-qiymat (key-value) juftliklarini saqlaydigan va ularga o'rtacha O(1) vaqtda kirish imkonini beruvchi ma'lumotlar tuzilmasi. Bu uning eng katta afzalligi — massiv yoki LinkedList'da qidiruv O(n) vaqt olsa, HashMap'da bu deyarli bir zumda amalga oshadi.

Qanday ishlaydi
HashMap ichida hash funksiyasi har bir kalitni raqamli indeksga aylantiradi, bu indeks esa massiv (bucket'lar to'plami)dagi o'rinni bildiradi. Masalan, "email" kalitini hash funksiyasi 42-indeksga aylantirishi mumkin, va qiymat aynan shu joyga saqlanadi.

Kolliziyalarni hal qilish (Collision Resolution)
Chaining — bir xil indeksga tushgan barcha elementlar shu bucket ichida LinkedList (yoki massiv) sifatida saqlanadi.
Open Addressing — kolliziya yuz berganda, algoritm keyingi bo'sh joyni qidiradi (masalan, linear probing).
Load factor (yuklama koeffitsienti) — elementlar sonining bucket'lar soniga nisbati. Yuqori load factor ko'proq kolliziyaga olib keladi, shuning uchun HashMap odatda ma'lum chegaradan oshganda o'lchamini kattalashtiradi (resize).

Solishtirish jadvali
Amal	O'rtacha holat	Eng yomon holat
Qo'shish (set)	O(1)	O(n)
Qidirish (get)	O(1)	O(n)
O'chirish (delete)	O(1)	O(n)
Quyidagi diagramma hash table ichidagi bucket'lar tuzilishini ko'rsatadi (chaining usuli bilan):

hash('email')=2

Bucket 0: bo'sh

Bucket 1: bo'sh

Bucket 2: [email -> user@site.com]

Bucket 3: [age -> 25] -> [name -> Ali]

JavaScript'da HashMap tabiati Object va Map orqali amalga oshiriladi. Map obyekti kalit sifatida istalgan turdan foydalanish, qulay iteratsiya va o'lchamni size orqali olish imkonini beradi, shuning uchun zamonaviy kodda Objectdan ko'ra Map afzal ko'riladi.

💻
Код
Kod namunasi
#2
code
 Копировать
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
