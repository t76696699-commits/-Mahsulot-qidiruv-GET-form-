// ─── Closures — to'liq sweep ─────────────────────────────────────────────

// 1) Eng oddiy counter
function counterYarat() {
    let count = 0;
    return () => ++count;
}

const c1 = counterYarat();
const c2 = counterYarat();
console.log(c1(), c1(), c1());    // 1 2 3
console.log(c2());                 // 1 — alohida closure

// 2) Private state — class'siz encapsulation
function hisobYarat(boshlangich = 0) {
    let pul = boshlangich;
    let tarix = [];

    return {
        qoshish(n) {
            pul += n;
            tarix.push({ amal: "+", n, qoldiq: pul });
            return pul;
        },
        ayirish(n) {
            if (n > pul) throw new Error("Yetarli pul yo'q");
            pul -= n;
            tarix.push({ amal: "-", n, qoldiq: pul });
            return pul;
        },
        qoldiq()    { return pul; },
        tarixOl()   { return [...tarix]; },    // nusxa qaytaradi
    };
}

const hisob = hisobYarat(1000);
hisob.qoshish(500);
hisob.ayirish(200);
console.log(`Qoldiq: ${hisob.qoldiq()}`);
console.log("Tarix:", hisob.tarixOl());

// pul va tarix tashqaridan ko'rinmaydi — to'liq private
console.log("Maydon ko'rinadimi:", hisob.pul);   // undefined

// 3) Funksiya fabrikasi
const ko_paytiruvchi = (k) => (x) => x * k;

const [ikki, besh, o_n] = [2, 5, 10].map(ko_paytiruvchi);
console.log(ikki(7), besh(7), o_n(7));    // 14 35 70

// 4) Debounce
function debounce(funk, kutish) {
    let timeoutId;
    return function (...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => funk.apply(this, args), kutish);
    };
}

const log_d = debounce((s) => console.log(`Qidirilmoqda: ${s}`), 300);
log_d("p"); log_d("py"); log_d("pyt"); log_d("pyth");
// Faqat 300ms dan keyin oxirgisi chiqadi

// 5) Memoize
function memoize(funk) {
    const cache = new Map();
    return function (...args) {
        const k = JSON.stringify(args);
        if (cache.has(k)) {
            console.log(`(cache hit: ${k})`);
            return cache.get(k);
        }
        const natija = funk(...args);
        cache.set(k, natija);
        return natija;
    };
}

const sekinFib = (n) => (n < 2 ? n : sekinFib(n - 1) + sekinFib(n - 2));
const tezFib = memoize((n) => (n < 2 ? n : tezFib(n - 1) + tezFib(n - 2)));
console.log("Tez fib(30):", tezFib(30));

// 6) Klassik for-var bug
console.log("\nvar bilan (bug):");
const fns_var = [];
for (var i = 0; i < 3; i++) {
    fns_var.push(() => i);
}
console.log(fns_var.map((f) => f()));    // [3, 3, 3]

console.log("let bilan (fix):");
const fns_let = [];
for (let j = 0; j < 3; j++) {
    fns_let.push(() => j);
}
console.log(fns_let.map((f) => f()));    // [0, 1, 2]

// 7) once — funksiyani faqat bir marta chaqirish
function once(funk) {
    let bajarildi = false;
    let natija;
    return function (...args) {
        if (!bajarildi) {
            bajarildi = true;
            natija = funk.apply(this, args);
        }
        return natija;
    };
}

const bittaSalom = once(() => {
    console.log("Salom! (faqat 1 marta)");
    return Math.random();
});
bittaSalom();
bittaSalom();
bittaSalom();    // birinchi marta natija qaytadi, faqat birinchisi log qiladi

// 8) Closure + setTimeout — timer
function timerYarat(soniya) {
    let qolgan = soniya;
    return new Promise((resolve) => {
        const interval = setInterval(() => {
            console.log(`Qolgan: ${qolgan}s`);
            qolgan--;
            if (qolgan < 0) {
                clearInterval(interval);
                resolve("Tugadi!");
            }
        }, 1000);
    });
}
// timerYarat(3).then(console.log);
