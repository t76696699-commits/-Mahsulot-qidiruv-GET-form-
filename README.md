R2-State'li hisoblovchi (closure + this)
Урок 8 из 14
· 4 раздела
📝
Текст
Текст
#1
🔁 R2 — State'li hisoblovchi (closure + this + modul)
export

closure

bind/arrow

counter.js modul

create, increment, ...

private state

DOM click

handler bilan this

Modul 2 ning 3 ta texnikasi birga: closures, this+bind va ES6 modullar. Vazifa: bir nechta nazoratlanuvchi hisoblovchini yaratuvchi modul. Har biri o'z private state'iga ega, DOM tugmasiga bog'lash mumkin.

🏆 5 daqiqada g'alaba — bitta katta misol
// ═══════════ counter.js ═══════════
// Public API — factory pattern (closure orqali private state)
export function createCounter({ boshlangich = 0, qadam = 1, max = Infinity } = {}) {
    let value = boshlangich;
    let tarix = [];

    const o_zgartirish = (yangi) => {
        tarix.push({ eski: value, yangi, vaqt: Date.now() });
        value = yangi;
    };

    return {
        oshirish() {
            if (value + qadam > max) throw new Error("Max dan oshib ketdi");
            o_zgartirish(value + qadam);
            return value;
        },
        kamaytirish() {
            o_zgartirish(value - qadam);
            return value;
        },
        nollash() { o_zgartirish(boshlangich); return value; },
        get joriy() { return value; },
        get tarix() { return [...tarix]; },
    };
}

// ═══════════ app.js ═══════════
import { createCounter } from "./counter.js";

const limitli = createCounter({ boshlangich: 0, qadam: 5, max: 20 });
console.log(limitli.oshirish());    // 5
console.log(limitli.oshirish());    // 10
console.log(limitli.oshirish());    // 15
console.log(limitli.oshirish());    // 20
// limitli.oshirish();              // Error: Max dan oshib ketdi

console.log(limitli.tarix);          // 4 ta o'zgarish
3 ta texnikani birga ko'rib chiqamiz
Closures
value, tarix, qadam, max — privatе state
Har createCounter chaqirig'i alohida closure
Tashqaridan kirish yo'q — limitli.value undefined
this / bind
Metod sifatida — { oshirish() { ... } } qisqartmasi
DOM event handler'ga uzatishda this yo'qoladi — arrow yoki bind
Getter get joriy — atribut sifatida ko'rinadi (qavssiz)
ES6 modullar
export function createCounter — named export
import { createCounter } from "./counter.js"
Modul faqat bir marta yuklanadi (singleton), lekin har createCounter chaqirig'i yangi closure
📌 Module 2 ni siz endi bilasiz
Private state — class kerak emas, closure yetadi
this mantig'ini biling — event handler uchun arrow yoki bind
Har .js fayl — alohida modul; sof named export
3 ta birga ishlatilganda — siz "modular, encapsulated JS" yozyapsiz
💻
Код
Код
#2
javascript
 Копировать
// ─── R2: State'li hisoblovchi to'plami (closure + this + modul) ──────────
//
// Tasavvur qiling: counter.js modul bor. Bu yerda bir-fayllik to'liq misol.

// ═══════════ counter.js (factory pattern) ═══════════
function createCounter({ boshlangich = 0, qadam = 1, max = Infinity, min = -Infinity } = {}) {
    let value = boshlangich;
    let tarix = [];

    const o_zgartirish = (yangi) => {
        tarix.push({ eski: value, yangi, vaqt: Date.now() });
        value = yangi;
    };

    return {
        oshirish() {
            if (value + qadam > max) {
                throw new Error(`Max chegarasi (${max}) dan oshib ketdi`);
            }
            o_zgartirish(value + qadam);
            return value;
        },
        kamaytirish() {
            if (value - qadam < min) {
                throw new Error(`Min chegarasi (${min}) dan kichik`);
            }
            o_zgartirish(value - qadam);
            return value;
        },
        nollash() {
            o_zgartirish(boshlangich);
            return value;
        },
        get joriy() { return value; },
        get tarix() { return [...tarix]; },
        get o_zgarish_soni() { return tarix.length; },
    };
}

// ═══════════ app.js ═══════════
// 1) Oddiy hisoblovchi
const sodda = createCounter();
console.log(sodda.oshirish());        // 1
console.log(sodda.oshirish());        // 2
console.log(sodda.oshirish());        // 3
console.log("Sodda joriy:", sodda.joriy);

// 2) Limitli hisoblovchi
const limitli = createCounter({ qadam: 5, max: 20 });
limitli.oshirish();
limitli.oshirish();
limitli.oshirish();
console.log("Limitli joriy:", limitli.joriy);
try {
    limitli.oshirish();
    limitli.oshirish();    // 4-marta — 20, ok
    limitli.oshirish();    // 5-marta — 25, max ortib ketadi
} catch (e) {
    console.log("Tutib oldim:", e.message);
}

// 3) Alohida instance — alohida closure
const a = createCounter();
const b = createCounter();
a.oshirish();
a.oshirish();
b.oshirish();
console.log(`a=${a.joriy}, b=${b.joriy}`);    // a=2, b=1 — alohida state

// 4) Private state isboti
console.log("Private value ko'rinmaydi:", a.value);    // undefined

// 5) Tarix
console.log("Tarix:", a.tarix);

// 6) DOM bog'lash (brauzer kontekstida)
// const tugma = document.querySelector("#qoshish");
// tugma.addEventListener("click", () => {
//     const yangi = limitli.oshirish();
//     document.querySelector("#display").textContent = yangi;
// });

// 7) Sinov bilan inputdan o'qish — debounced
function debounce(funk, kutish) {
    let id;
    return (...args) => {
        clearTimeout(id);
        id = setTimeout(() => funk.apply(this, args), kutish);
    };
}

const yangilash = debounce((q) => console.log(`Joriy: ${q}`), 200);
const search = createCounter();
yangilash(search.oshirish());
yangilash(search.oshirish());
yangilash(search.oshirish());
// Faqat 200ms keyin oxirgisi

// 8) Bir nechtasini boshqarish — registry pattern
function createRegistry() {
    const counters = new Map();

    return {
        qosh(nom, opts) {
            counters.set(nom, createCounter(opts));
        },
        olish(nom) { return counters.get(nom); },
        joriy(nom) { return counters.get(nom)?.joriy ?? null; },
        hammasi() {
            const natija = {};
            for (const [nom, c] of counters) natija[nom] = c.joriy;
            return natija;
        },
    };
}

const registry = createRegistry();
registry.qosh("oshxona", { qadam: 1 });
registry.qosh("vannaxona", { qadam: 2 });
registry.olish("oshxona").oshirish();
registry.olish("oshxona").oshirish();
registry.olish("vannaxona").oshirish();

console.log("Registry:", registry.hammasi());
// { oshxona: 2, vannaxona: 2 }
