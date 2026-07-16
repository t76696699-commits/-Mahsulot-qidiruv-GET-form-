async/await — Promise'ni sinxron kabi yozish
qiyin o'qish

await x

try/catch

.then ketma-ket

chain noise

async function

xayoldai sinxron

natija — Promise'ning value'si

xato

sodda xato boshqaruvi

Promise'lar zo'r, lekin .then chain'lari uzun bo'lib ketadi. async/await — bu chain'larni xuddi sinxron koddek yozish imkonini beradi. Aslida — sintaktik shakar. await ostidagi Promise — to'xtatib turadi, natijani qaytaradi.

🏆 5 daqiqada g'alaba
BLOKA 1 — .then dan async/await ga
// Eski stil — .then chain
function eskiUsul() {
    return fetch("/api/user")
        .then(r => r.json())
        .then(user => fetch(`/api/posts/${user.id}`))
        .then(r => r.json())
        .then(posts => ({ user_id: user.id, posts }));    // ⚠️ user yo'q!
}

// Modern — async/await
async function yangiUsul() {
    const user = await fetch("/api/user").then(r => r.json());
    const posts = await fetch(`/api/posts/${user.id}`).then(r => r.json());
    return { user_id: user.id, posts };          // ✅ user mavjud
}
BLOKA 2 — try/catch — xatoni ushlash
async function olish(url) {
    try {
        const r = await fetch(url);
        if (!r.ok) throw new Error(`HTTP ${r.status}`);
        return await r.json();
    } catch (e) {
        console.error("Xato:", e.message);
        return null;
    } finally {
        console.log("Tugadi");
    }
}

const data = await olish("/api/users");
BLOKA 3 — Parallel — Promise.all bilan
// SEKVENSIAL — ketma-ket, sekinroq
async function sekin() {
    const a = await fetch("/api/a").then(r => r.json());
    const b = await fetch("/api/b").then(r => r.json());
    const c = await fetch("/api/c").then(r => r.json());
    return { a, b, c };
    // Vaqt: a + b + c
}

// PARALLEL — birgalikda, tezroq
async function tez() {
    const [a, b, c] = await Promise.all([
        fetch("/api/a").then(r => r.json()),
        fetch("/api/b").then(r => r.json()),
        fetch("/api/c").then(r => r.json()),
    ]);
    return { a, b, c };
    // Vaqt: max(a, b, c)
}
🐛 Ataylab xato
const ismlar = ["ali", "vali", "gulya"];

ismlar.forEach(async (ism) => {
    const data = await fetch(`/api/users/${ism}`).then(r => r.json());
    console.log(data);
});

console.log("Hammasi tugadi!");    // ⚠️ Yolg'on
Muammo: forEach Promise'ni KUTMAYDI. async callback shunchaki Promise qaytaradi va forEach uni e'tiborsiz qoldiradi. "Hammasi tugadi" — async ishlar tugashidan oldin. Yechim:

// 1) for...of — kutadi
for (const ism of ismlar) {
    const data = await fetch(`/api/users/${ism}`).then(r => r.json());
    console.log(data);
}

// 2) Promise.all + map — parallel
const datalar = await Promise.all(
    ismlar.map(ism => fetch(`/api/users/${ism}`).then(r => r.json())),
);
Endi tushuntiramiz
1. async funksiya nima qaytaradi?
async function f() { return 5; }

const x = f();
console.log(x);           // Promise { 5 }
console.log(await x);     // 5

// async funksiya — DOIM Promise qaytaradi
// `return 5` — Promise.resolve(5) ga ekvivalent
// `throw err` — Promise.reject(err) ga ekvivalent
2. await qoidalari
await — FAQAT async funksiya ichida (yoki top-level modulda)
Promise resolved bo'lsa — qiymat qaytaradi
Promise rejected bo'lsa — throw kabi exception
Promise BO'LMAGAN qiymatga await — darhol shu qiymat
3. try/catch va Promise xato boshqaruvi
// async/await — sinxron kodga o'xshash try/catch
async function v1() {
    try {
        const data = await fetch(url);
        return await data.json();
    } catch (e) {
        console.error(e);
    }
}

// .then chain bilan ekvivalent
function v2() {
    return fetch(url)
        .then((r) => r.json())
        .catch((e) => console.error(e));
}
4. Sekvensial vs Parallel — diqqat
// ⚠️ Sekin (sekvensial) — har biri avvalgisini kutadi
async function sekin() {
    const a = await fetchA();    // 1s
    const b = await fetchB();    // 1s
    const c = await fetchC();    // 1s
    return [a, b, c];            // jami 3s
}

// ✅ Tez (parallel) — birga boshlanadi
async function tez() {
    const aP = fetchA();          // 1s (boshlandi)
    const bP = fetchB();          // 1s (boshlandi)
    const cP = fetchC();          // 1s (boshlandi)
    return [await aP, await bP, await cP];    // jami 1s
}

// Idiomatic — Promise.all
async function eng_yaxshi() {
    return Promise.all([fetchA(), fetchB(), fetchC()]);    // jami 1s
}
5. Top-level await — modullarda
// ES2022+ va ESM modullar
// app.js
import { fetchConfig } from "./config.js";

const config = await fetchConfig();   // OK — top-level
console.log(config);

// Funksiya tashqarisida await — faqat modul'da ishlaydi
6. Async/await va loop ichida
Tanlov	Behavior
for...of + await	Sekvensial — biror tartib kerak bo'lganda
Promise.all + map	Parallel — tartib muhim emas
forEach + async	❌ Kutmaydi — xato
for (const p of promises) await p	Sekvensial wait
📌 Bu darsdan keyin siz bilasizki
async function — doim Promise qaytaradi
await — faqat async ichida; Promise tugashini kutadi
Sinxron stilda try/catch bilan xato boshqarish
Parallel uchun Promise.all; sekvensial uchun for...of
forEach + async — KUTMAYDI, xato
💻
Код
Код
#2
javascript
 Копировать
// ─── async/await — to'liq sweep ──────────────────────────────────────────

// 1) Eng oddiy async funksiya
async function salom() {
    return "Salom!";
}

console.log(salom());           // Promise { 'Salom!' }
salom().then(console.log);      // "Salom!"

// IIFE — async ni top-level'da chaqirish
(async () => {
    const xabar = await salom();
    console.log("await bilan:", xabar);
})();

// 2) Promise yaratish va await
function kutish(ms) {
    return new Promise((resolve) => setTimeout(resolve, ms));
}

(async () => {
    console.log("Boshlandi");
    await kutish(500);
    console.log("500ms o'tdi");
    await kutish(500);
    console.log("Yana 500ms");
})();

// 3) try/catch — xato boshqaruvi
async function olish(url) {
    try {
        // fetch yo'q test muhitida — simulyatsiya
        if (url.includes("bad")) throw new Error("404 Not Found");
        await kutish(100);
        return { ok: true, url };
    } catch (e) {
        console.log("Xato ushlandi:", e.message);
        return null;
    } finally {
        console.log(`finally: ${url}`);
    }
}

(async () => {
    console.log(await olish("/api/users"));
    console.log(await olish("/api/bad"));
})();

// 4) Sekvensial vs Parallel
async function sekvensial() {
    const t0 = Date.now();
    await kutish(100);
    await kutish(100);
    await kutish(100);
    console.log(`Sekvensial: ${Date.now() - t0}ms`);    // ~300
}

async function parallel() {
    const t0 = Date.now();
    await Promise.all([kutish(100), kutish(100), kutish(100)]);
    console.log(`Parallel: ${Date.now() - t0}ms`);      // ~100
}

(async () => {
    await sekvensial();
    await parallel();
})();

// 5) Loop bilan — to'g'ri va noto'g'ri
const idlar = [1, 2, 3];

async function fakeOlish(id) {
    await kutish(50);
    return { id, nom: `User ${id}` };
}

// ✅ for...of — sekvensial wait
(async () => {
    console.log("\nfor...of:");
    for (const id of idlar) {
        const u = await fakeOlish(id);
        console.log(u);
    }
})();

// ✅ Promise.all + map — parallel
(async () => {
    console.log("\nPromise.all:");
    const users = await Promise.all(idlar.map(fakeOlish));
    console.log(users);
})();

// ❌ forEach + async — kutmaydi
(async () => {
    console.log("\nforEach (noto'g'ri):");
    idlar.forEach(async (id) => {
        const u = await fakeOlish(id);
        console.log("forEach:", u);
    });
    console.log("forEach tugadi (lekin user'lar hali kutmoqda!)");
})();

// 6) Real foydalanish — JSON API
async function repoOlish(nomi) {
    try {
        const r = await fetch(`https://api.github.com/repos/${nomi}`);
        if (!r.ok) throw new Error(`HTTP ${r.status}`);
        const data = await r.json();
        return {
            nom: data.full_name,
            stars: data.stargazers_count,
            til: data.language,
        };
    } catch (e) {
        console.error(`${nomi}: ${e.message}`);
        return null;
    }
}

(async () => {
    console.log("\n=== GitHub repos (parallel) ===");
    const repos = ["python/cpython", "facebook/react", "vuejs/vue"];
    const natijalar = await Promise.all(repos.map(repoOlish));
    natijalar.filter(Boolean).forEach((r) =>
        console.log(`⭐ ${r.stars.toLocaleString().padStart(8)}  ${r.nom}  [${r.til}]`),
    );
})();
