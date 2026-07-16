Promiselar (.then, .catch, Promise.all)
Урок 9 из 14
· 4 раздела
📝
Текст
Текст
#1
Promiselar — async ish'ning birinchi qatlami
pending

resolve

reject

.then

.catch

.finally

.finally

new Promise

kutilmoqda

fulfilled

rejected

natijani ishlat

xatoni ushla

tugadi

JS — single-threaded. Lekin server, fayl, timer, animatsiya — vaqt oladi. Promise — kelajakda keladigan natijaning "vakuum kartochkasi". Promise resolved, rejected yoki pending bo'ladi. Bu — async/await va fetch'ning poydevori.

🏆 5 daqiqada g'alaba
BLOKA 1 — Promise yaratish
const promise = new Promise((resolve, reject) => {
    // Kechiktirilgan ish
    setTimeout(() => {
        const ok = Math.random() > 0.5;
        if (ok) {
            resolve("Muvaffaqiyatli!");      // pending -> fulfilled
        } else {
            reject(new Error("Xatolik"));     // pending -> rejected
        }
    }, 1000);
});

console.log(promise);    // Promise { <pending> }

promise
    .then((natija) => console.log("✅", natija))
    .catch((xato) =>  console.log("❌", xato.message))
    .finally(()    =>  console.log("Tugadi"));
BLOKA 2 — Chain — .then ketma-ket
function kutish(ms) {
    return new Promise((resolve) => setTimeout(resolve, ms));
}

kutish(1000)
    .then(() => { console.log("1 sek o'tdi"); return kutish(1000); })
    .then(() => { console.log("Yana 1 sek o'tdi"); return kutish(500); })
    .then(() => console.log("3 sekund jami"));

// Yoki .then ichida qiymat qaytarish
Promise.resolve(5)
    .then((x) => x * 2)              // 10
    .then((x) => x + 1)               // 11
    .then((x) => console.log(x));     // 11
BLOKA 3 — Promise.all va Promise.race
// Hammasini parallel kutish
const url1 = "https://api.github.com/users/torvalds";
const url2 = "https://api.github.com/users/gvanrossum";

Promise.all([
    fetch(url1).then(r => r.json()),
    fetch(url2).then(r => r.json()),
])
    .then(([linus, guido]) => {
        console.log(`${linus.name} va ${guido.name}`);
    })
    .catch((e) => console.log("Bittasi ham yiqilsa — catch"));

// Eng birinchi javob keladigan
Promise.race([
    fetch("https://api1.example.com"),
    fetch("https://api2.example.com"),
])
    .then((r) => r.json())
    .then(console.log);
🐛 Ataylab xato
const p = new Promise((resolve) => resolve(42));
p.then((natija) => {
    console.log(natija);
    throw new Error("Ichida xato");
});

// Konsolda — UnhandledPromiseRejection
Muammo: .then ichida xato chiqsa va .catch qo'shilmagan bo'lsa — silently fail (lekin Node yangi versiyalarda crash). Qoida: har chain'da DOIM oxirida .catch bo'lsin:

p
    .then((natija) => {
        if (notog'ri) throw new Error("...");
        return natija;
    })
    .catch((e) => console.error("Ushladim:", e.message));
Endi tushuntiramiz
1. Promise — 3 holat
Holat	Ma'nosi
pending	Kutilmoqda — hali tugamagan
fulfilled	Muvaffaqiyatli tugadi (resolve chaqirilgan)
rejected	Xato bilan tugadi (reject chaqirilgan)
Bir marta pending'dan chiqsa — qaytmaydi (immutable). Resolve yoki reject — bitta marta.

2. Promise statik metodlar
Metod	Maqsadi
Promise.resolve(x)	Darhol fulfilled Promise
Promise.reject(e)	Darhol rejected Promise
Promise.all([p1, p2])	Hammasi tugashini kutadi. Bittasi rejected -> butun array fail
Promise.allSettled([p1, p2])	Hammasini kutadi — fail bo'lganlari ham natijada
Promise.race([p1, p2])	Birinchi tugagani (fulfilled yoki rejected)
Promise.any([p1, p2])	Birinchi fulfilled. Hammasi rejected bo'lsa — AggregateError
3. Chain'da xato'ni boshqarish
fetch(url)
    .then((r) => {
        if (!r.ok) throw new Error(`HTTP ${r.status}`);
        return r.json();
    })
    .then((data) => processData(data))
    .catch((e) => {
        if (e.message.includes("HTTP")) {
            // tarmoq xatosi
        } else {
            // boshqa
        }
    });
.catch chain'dagi HAR QANDAY xatoni ushlaydi — qaerda yuz berishidan qat'i nazar.

4. Promise.all — paralel
// 3 ta API chaqiruvi — birgalikda
const [users, posts, comments] = await Promise.all([
    fetch("/api/users").then(r => r.json()),
    fetch("/api/posts").then(r => r.json()),
    fetch("/api/comments").then(r => r.json()),
]);
// Eng sekin so'rovning vaqti — emas barcha vaqtlarning yig'indisi
5. Promise.allSettled — har biri haqida ma'lumot
const natijalar = await Promise.allSettled([
    fetch("/api/a"),
    fetch("/api/b"),     // bu fail bo'lsin
    fetch("/api/c"),
]);

natijalar.forEach((r, i) => {
    if (r.status === "fulfilled") {
        console.log(`API ${i}: OK`, r.value);
    } else {
        console.log(`API ${i}: FAIL`, r.reason);
    }
});
6. Callback hell vs Promise chain
// Callback hell — eski stil
authenticate(user, (err, token) => {
    if (err) return console.error(err);
    fetchProfile(token, (err, profile) => {
        if (err) return console.error(err);
        fetchPosts(profile.id, (err, posts) => {
            if (err) return console.error(err);
            console.log(posts);
        });
    });
});

// Promise chain — toza
authenticate(user)
    .then(fetchProfile)
    .then((profile) => fetchPosts(profile.id))
    .then(console.log)
    .catch(console.error);
📌 Bu darsdan keyin siz bilasizki
Promise — kelajakdagi natija "vakuum kartochkasi": pending -> fulfilled/rejected
.then(...) — natija; .catch(...) — xato; .finally(...) — har holatda
Chain'da xato — har qaysi .then'dan oxirgi .catch ga "tushadi"
Promise.all — parallel kutish; allSettled — har biri haqida ma'lumot
Har chain oxirida .catch qo'shing — silently fail oldini olish uchun
💻
Код
Код
#2
javascript
 Копировать
// ─── Promiselar — to'liq sweep ───────────────────────────────────────────

// 1) Eng oddiy Promise — kutish
function kutish(ms) {
    return new Promise((resolve) => setTimeout(resolve, ms));
}

console.log("Boshlandi");
kutish(500).then(() => console.log("500ms o'tdi"));
kutish(1000).then(() => console.log("1000ms o'tdi"));

// 2) Resolve / Reject — tasodifiy
function shubhali(success_rate = 0.5) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (Math.random() < success_rate) {
                resolve({ ok: true, vaqt: Date.now() });
            } else {
                reject(new Error("network down"));
            }
        }, 200);
    });
}

shubhali(0.7)
    .then((r) => console.log("✅", r))
    .catch((e) => console.log("❌", e.message));

// 3) Chain — qiymat o'zgaradi
Promise.resolve(5)
    .then((x) => x * 2)
    .then((x) => x + 10)
    .then((x) => `Natija: ${x}`)
    .then(console.log);

// 4) Chain ichida Promise qaytarish — keyinchalik kutish
function olish(id) {
    return kutish(100).then(() => ({ id, nom: `User ${id}` }));
}

olish(1)
    .then((u) => {
        console.log("1-step:", u);
        return olish(u.id + 1);
    })
    .then((u) => {
        console.log("2-step:", u);
        return olish(u.id + 1);
    })
    .then((u) => console.log("3-step:", u));

// 5) Promise.all — parallel
const idlar = [10, 20, 30];
Promise.all(idlar.map(olish))
    .then((natijalar) => {
        console.log("Hammasi parallel:", natijalar.map((u) => u.nom));
    });

// 6) Promise.allSettled — fail bo'lsa ham hammasi
Promise.allSettled([
    shubhali(0.9),
    shubhali(0.1),       // ehtimol fail
    shubhali(0.5),
]).then((natijalar) => {
    natijalar.forEach((r, i) => {
        if (r.status === "fulfilled") {
            console.log(`#${i}: OK`, r.value);
        } else {
            console.log(`#${i}: FAIL`, r.reason.message);
        }
    });
});

// 7) Promise.race — eng birinchisi
function timeout(ms) {
    return new Promise((_, reject) =>
        setTimeout(() => reject(new Error(`Timeout ${ms}ms`)), ms),
    );
}

function fetchWithTimeout(promise, ms) {
    return Promise.race([promise, timeout(ms)]);
}

fetchWithTimeout(shubhali(0.9), 500)
    .then((r) => console.log("Tez:", r))
    .catch((e) => console.log("Timeout/xato:", e.message));

// 8) Retry pattern — Promise bilan
function retry(funk, marotaba = 3, kutish_ms = 100) {
    return funk().catch((e) => {
        if (marotaba <= 1) throw e;
        return kutish(kutish_ms).then(() => retry(funk, marotaba - 1, kutish_ms));
    });
}

retry(() => shubhali(0.2), 5)
    .then((r) => console.log("Retry muvaffaqiyatli:", r))
    .catch((e) => console.log("Retry chiqdi:", e.message));

// 9) Promisify — eski callback API'ni Promise'ga aylantirish
function callback_style(arg, callback) {
    setTimeout(() => callback(null, arg * 2), 100);
}

function promisify(funk) {
    return function (...args) {
        return new Promise((resolve, reject) => {
            funk(...args, (err, natija) => {
                if (err) reject(err);
                else resolve(natija);
            });
        });
    };
}

const promised = promisify(callback_style);
promised(5).then((x) => console.log("Promisified:", x));    // 10
