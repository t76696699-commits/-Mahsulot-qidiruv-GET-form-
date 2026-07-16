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
