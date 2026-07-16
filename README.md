ES6 modullar (import/export)
Урок 7 из 14
· 4 раздела
✓ Пройден
📝
Текст
Текст
#1
ES6 modullar — import / export
export

import

export default

import default

namespace

math.js

sum, mul

app.js

user.js

class User

import * as utils

JS uzoq yillar modul tizimi'siz yashagan — barcha kod global scope'da edi. ES6 (ES2015) bunga chek qo'ydi. Endi har .js fayl — alohida modul. Named exports bir nechta narsa eksport qiladi, default export — bittasi. Bu kursdan keyin siz "spaghetti script" yozmaysiz.

🏆 5 daqiqada g'alaba
Modullar ishlashi uchun:

HTML'da <script type="module" src="app.js"></script>
Yoki Node.js'da package.json ga "type": "module"
Yoki Vite/Webpack kabi build tool
BLOKA 1 — Named exports
// math.js
export const PI = 3.14159;

export function sum(a, b) {
    return a + b;
}

export const mul = (a, b) => a * b;

// Yoki barchasini oxirida
const div = (a, b) => a / b;
const sub = (a, b) => a - b;
export { div, sub };

// app.js
import { sum, mul, PI } from "./math.js";

console.log(sum(3, 4));    // 7
console.log(mul(5, 6));    // 30
console.log(PI);            // 3.14159
BLOKA 2 — Default export
// user.js
export default class User {
    constructor(ism, yosh) {
        this.ism = ism;
        this.yosh = yosh;
    }
    salomlash() {
        return `Salom, ${this.ism}!`;
    }
}

// app.js
import User from "./user.js";           // istalgan nom (qavssiz)

const u = new User("Ali", 21);
console.log(u.salomlash());
BLOKA 3 — Aralash, alias va namespace
// utils.js
export const formatla = (s) => s.trim().toLowerCase();
export const log = (s) => console.log(`[LOG] ${s}`);
export default function init() {
    console.log("Boshlandi");
}

// app.js
import init, { formatla, log as logla } from "./utils.js";
//        ^^                       ^^^^^^^^^^
//        default                  alias

init();
logla(formatla("  SALOM  "));    // [LOG] salom

// Yoki barchasini namespace bilan
import * as utils from "./utils.js";
utils.formatla("...");
utils.log("...");
utils.default();    // default — `default` deb chaqiriladi
🐛 Ataylab xato
// counter.js
let count = 0;
export const oshirish = () => count++;
export { count };

// app.js
import { count, oshirish } from "./counter.js";

console.log(count);    // 0
oshirish();
oshirish();
console.log(count);    // ???
Natija: 0. Modullar live binding — agar eksport qiluvchi modul ichidagi qiymat o'zgarsa, importchi tomon asl referenceni ko'radi... LEKIN let count — primitive. Modulda qiymat oshadi, lekin app.js dagi ko'rinish ham yangilanadi.

Aslida: ES module live bindings — qiymat o'zgarsa importchi ham yangi qiymatni ko'radi. Demak natija aslida 2 bo'ladi. Lekin CommonJS'da (Node eski) — snapshot, primitive nusxalanadi. Esda saqlash: ES module — live, CommonJS — snapshot. Shu farqni bilmaslik — eng ko'p uchraydigan migration bug.

Endi tushuntiramiz
1. Named vs default export
Named	Default
Bir necha	Faqat bittasi
Aniq nom kerak: { sum }	Istalgan nom: import X from ...
Yordamchi funksiyalar uchun	Modulning asosiy "qahramoni" uchun
Refactoring xavfsiz (rename topiladi)	Rename qiyin — har joyda boshqa nom
Best practice: default'siz, faqat named. Hammasi bir-biriga moslashadi va katta loyihalarda boshqarish oson. Default — kichik modul + bittagina narsa eksport bo'lganda OK.

2. Reexport — bir joydan boshqaga uzatish
// index.js — "barrel" file
export { sum, mul } from "./math.js";
export { default as User } from "./user.js";
export * from "./utils.js";

// boshqa joyda
import { sum, User, formatla } from "./index.js";
3. Dynamic import — runtime'da yuklash
// Faqat kerak bo'lganda yuklab olish (code splitting)
button.addEventListener("click", async () => {
    const { katta_kutubxona } = await import("./katta-kutubxona.js");
    katta_kutubxona();
});

// Promise qaytaradi
import("./kichik.js").then((mod) => mod.salom());
4. Modul properties
Strict mode — har modul avtomatik strict
Top-level — this = undefined (window emas)
Live bindings — eksport qiymati o'zgarsa, importchi yangi qiymatni ko'radi
Singleton — modul faqat bir marta yuklanadi va cache'lanadi
Async — modullar parallel yuklanadi
5. ES modules vs CommonJS
ES modules (ESM)	CommonJS (CJS)
import / export	require() / module.exports
Statik (build-time tahlil)	Dinamik (runtime)
Live bindings	Snapshot
Top-level await	Yo'q
Async yuklash	Sync
Brauzer + modern Node	Eski Node
6. Qachon import path'iga .js qo'shish kerak?
Brauzer ESM — .js majburiy: ./math.js
Node ESM — .js majburiy (yangi qoidasi)
Webpack / Vite / TS bilan — odatda .js shart emas (resolver topadi)
📌 Bu darsdan keyin siz bilasizki
export/import — modul tizimi (ES2015+)
Named export — bir nechtasi; default export — bittagina
{ } ichida — named; qavssiz — default
import * as X — namespace, as alias — qayta nomlash
HTML'da <script type="module"> kerak; modulda strict mode default
💻
Код
Код
#2
javascript
 Копировать
// ─── ES6 modullar misoli ─────────────────────────────────────────────────
// Eslatma: bu kodlar HTML'da <script type="module"> bilan ishga tushadi.
// Bu yerda kod misol sifatida — har bir blok alohida fayl deb tasavvur qiling.

// ═══════════ math.js ═══════════
// export const PI = 3.14159;
// export function sum(a, b) { return a + b; }
// export const mul = (a, b) => a * b;
// const div = (a, b) => a / b;
// export { div };

// ═══════════ user.js ═══════════
// export default class User {
//     constructor(ism, yosh) {
//         this.ism = ism;
//         this.yosh = yosh;
//     }
//     salomlash() {
//         return `Salom, ${this.ism}!`;
//     }
// }

// ═══════════ utils.js ═══════════
// export const formatla = s => s.trim().toLowerCase();
// export const log = s => console.log(`[LOG] ${s}`);
// export default function init() { console.log("Boshlandi"); }

// ═══════════ index.js — barrel ═══════════
// export { sum, mul, PI } from "./math.js";
// export { default as User } from "./user.js";
// export * from "./utils.js";

// ═══════════ app.js ═══════════
// // 1) Named import
// import { sum, mul, PI } from "./math.js";
// console.log(sum(3, 4));
// console.log(mul(5, 6));
// console.log(PI);
//
// // 2) Default import
// import User from "./user.js";
// const ali = new User("Ali", 21);
// console.log(ali.salomlash());
//
// // 3) Aralash + alias
// import init, { formatla, log as logla } from "./utils.js";
// init();
// logla(formatla("  SALOM  "));
//
// // 4) Namespace
// import * as math from "./math.js";
// console.log(math.sum(1, 2));
// console.log(math.PI);
//
// // 5) Barrel orqali
// import { sum, User, formatla } from "./index.js";
//
// // 6) Dynamic import — code splitting
// document.querySelector("#tugma").addEventListener("click", async () => {
//     const { huge_lib } = await import("./big-library.js");
//     huge_lib();
// });

// ─── Brauzer console'da test qilish uchun bir fayllik misol ───────────────
// (modulsiz, lekin shaklini ko'rsatish uchun)

const moduleSimulator = {
    // math
    PI: 3.14159,
    sum: (a, b) => a + b,
    mul: (a, b) => a * b,

    // user
    User: class {
        constructor(ism, yosh) {
            this.ism = ism;
            this.yosh = yosh;
        }
        salomlash() { return `Salom, ${this.ism}!`; }
    },

    // utils
    formatla: (s) => s.trim().toLowerCase(),
    log: (s) => console.log(`[LOG] ${s}`),
};

// Bu kod — agar har biri alohida modul bo'lsa, qanday ko'rinishini ko'rsatuvchi
// "fake import" simulyatsiyasi:
const { sum, mul, PI, User, formatla, log } = moduleSimulator;

console.log("=== math demo ===");
console.log(`sum(3, 4) = ${sum(3, 4)}`);
console.log(`mul(5, 6) = ${mul(5, 6)}`);
console.log(`PI = ${PI}`);

console.log("\n=== user demo ===");
const u = new User("Ali", 21);
console.log(u.salomlash());

console.log("\n=== utils demo ===");
log(formatla("  Modern JS  "));

// ─── Modul shakli bo'yicha qoidalar ───────────────────────────────────────
// 1) Default export DIQQAT BILAN — refactoring qiyin
// 2) Named export'lar — IDE yaxshi qo'llaydi
// 3) Barrel (index.js) — ko'p eksport bo'lsa
// 4) Dynamic import — katta libraries uchun (code splitting)
// 5) `import.meta.url` — modul URL'i (kerak bo'lsa)

console.log("\nModul URL (Node ESM uchun):", typeof import.meta !== "undefined" ? "available" : "n/a");
