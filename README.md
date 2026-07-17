fetch va REST API
Урок 11 из 14
· 4 раздела
📝
Текст
Текст
#1
fetch va REST API'lar
GET

POST + body

.json

Authorization header

AbortController

fetch url

server

Response: status, headers, body

JS obyekt

auth

bekor qilish

fetch — brauzerga "borib shu URL'ni o'qib kel" deyish. Browser API, hozir Node.js da ham bor. Promise qaytaradi. async/await bilan birga — kuchli kombo.

🏆 5 daqiqada g'alaba
BLOKA 1 — Eng oddiy GET
const r = await fetch("https://api.github.com/users/torvalds");
console.log(r.status);             // 200
console.log(r.ok);                  // true (2xx bo'lsa)

const user = await r.json();
console.log(user.name);             // "Linus Torvalds"
console.log(user.bio);

// Query params — URLSearchParams bilan
const params = new URLSearchParams({ q: "python", page: 2 });
const search = await fetch(`https://api.github.com/search/users?${params}`);
BLOKA 2 — POST + JSON body
const r = await fetch("https://api.example.com/users", {
    method: "POST",
    headers: {
        "Content-Type": "application/json",
        "Authorization": "Bearer XXX",
    },
    body: JSON.stringify({ ism: "Ali", yosh: 21 }),
});

if (!r.ok) {
    throw new Error(`HTTP ${r.status}`);
}

const created = await r.json();
console.log(created.id);
BLOKA 3 — Xato boshqaruvi va bekor qilish
// AbortController — so'rovni to'xtatish
const controller = new AbortController();

setTimeout(() => controller.abort(), 3000);    // 3 sekunddan keyin to'xtat

try {
    const r = await fetch(url, { signal: controller.signal });
    const data = await r.json();
} catch (e) {
    if (e.name === "AbortError") {
        console.log("So'rov bekor qilindi");
    } else {
        console.log("Xato:", e.message);
    }
}
🐛 Ataylab xato
const r = await fetch("https://api.example.com/data");
console.log(r);              // Response obyekt, lekin data emas
console.log(r.json());       // ⚠️ Promise — to'g'ri ishlatish kerak

// To'g'ri
const data = await r.json();
console.log(data);
Sabab: r.json() Promise qaytaradi (body ni asynchronously parse qiladi). await qo'shilmasa — Promise obyektni ko'rasiz, kerakli data emas. Esda saqlang: fetch 2 ta await oladi:

await fetch(...) — response keladi
await r.json() — body parse bo'ladi
Endi tushuntiramiz
1. fetch ⚠️ — yopiq holatga ham reject qilmaydi
// HTTP 404 — fetch reject EMAS qiladi
const r = await fetch("/api/yo'q");
console.log(r.ok);          // false
console.log(r.status);       // 404

// Faqat tarmoq xatosi (DNS, offline) Promise'ni reject qiladi
// 4xx/5xx — Promise resolved! r.ok ni qo'lda tekshirish kerak

if (!r.ok) {
    throw new Error(`HTTP ${r.status}: ${r.statusText}`);
}
2. Response obyekt nimasi bor
Maydon	Qaytaradi
r.status	200, 404, 500 ...
r.statusText	"OK", "Not Found"
r.ok	true agar 200-299
r.headers.get("Content-Type")	Header qiymati
r.url	Final URL (redirect'lardan keyin)
await r.json()	Body — JSON parse
await r.text()	Body — string
await r.blob()	Body — bayt (rasm, fayl)
await r.formData()	Body — FormData
3. HTTP metodlar
Metod	Maqsadi
GET	O'qish — body yo'q
POST	Yangi yaratish — body kerak
PUT	To'liq almashtirish
PATCH	Qisman yangilash
DELETE	O'chirish
4. Authorization patterns
// Bearer token
headers: { "Authorization": "Bearer XXXX" }

// API key (header'da)
headers: { "X-Api-Key": "XXXX" }

// API key (query param'da)
const url = `https://api.../endpoint?api_key=XXXX`;

// Basic auth (eski stil)
headers: { "Authorization": "Basic " + btoa("user:pass") }

// Cookie (avtomatik) — credentials sozlamasi bilan
fetch(url, { credentials: "include" })
5. Universal xavfsiz wrapper
async function api(url, options = {}) {
    const timeout = options.timeout || 10000;
    const controller = new AbortController();
    const tid = setTimeout(() => controller.abort(), timeout);

    try {
        const r = await fetch(url, {
            ...options,
            signal: controller.signal,
            headers: {
                "Content-Type": "application/json",
                ...options.headers,
            },
            body: options.body ? JSON.stringify(options.body) : undefined,
        });

        if (!r.ok) {
            const xato = await r.text();
            throw new Error(`HTTP ${r.status}: ${xato}`);
        }

        return await r.json();
    } finally {
        clearTimeout(tid);
    }
}

// Foydalanish
const user = await api("/api/user");
const created = await api("/api/posts", {
    method: "POST",
    body: { title: "Salom", content: "..." },
});
6. Qachon fetch, qachon axios / library
fetch	axios va o'xshashlari
Native — qo'shimcha kutubxonasiz	Tashqi paket
4xx/5xx — manual tekshirish	Avtomatik throw
JSON parse — qo'lda .json()	Avtomatik
Timeout — AbortController	Sozlama bilan
Interceptors yo'q	Bor (har so'rovga qo'shimcha logika)
Brauzer + modern Node	Hammasi
Default: fetch + kichik wrapper. Katta loyiha + ko'p o'ziga xos so'rovlar — axios qulayroq.

📌 Bu darsdan keyin siz bilasizki
fetch 4xx/5xx ni rejection qilmaydi — r.ok ni tekshiring
fetch 2 ta await oladi: response uchun, keyin .json() uchun
POST/PUT body — JSON.stringify(...) bilan
AbortController — timeout va manual bekor qilish
Productionda — wrapper yozish; timeout va xato boshqaruvi har joyda
💻
Код
Код
#2
javascript
 Копировать
// ─── fetch va REST API'lar — sweep ───────────────────────────────────────

// 1) Eng oddiy GET (Node 18+ / brauzer)
async function getUser(username) {
    const r = await fetch(`https://api.github.com/users/${username}`);
    if (!r.ok) throw new Error(`HTTP ${r.status}`);
    return await r.json();
}

// 2) Query params bilan
async function search(qatori, page = 1) {
    const params = new URLSearchParams({ q: qatori, page });
    const r = await fetch(`https://api.github.com/search/users?${params}`);
    if (!r.ok) throw new Error(`HTTP ${r.status}`);
    return await r.json();
}

// 3) POST + JSON body
async function createPost(token, post) {
    const r = await fetch("https://jsonplaceholder.typicode.com/posts", {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
            "Authorization": `Bearer ${token}`,
        },
        body: JSON.stringify(post),
    });
    if (!r.ok) throw new Error(`HTTP ${r.status}`);
    return await r.json();
}

// 4) PUT — yangilash
async function updatePost(id, data) {
    const r = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data),
    });
    return await r.json();
}

// 5) DELETE
async function deletePost(id) {
    const r = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
        method: "DELETE",
    });
    return r.ok;
}

// 6) Xato boshqaruvi va timeout — to'liq wrapper
async function api(url, options = {}) {
    const { timeout = 10000, body, ...rest } = options;
    const controller = new AbortController();
    const tid = setTimeout(() => controller.abort(), timeout);

    try {
        const r = await fetch(url, {
            ...rest,
            signal: controller.signal,
            headers: {
                "Content-Type": "application/json",
                ...rest.headers,
            },
            body: body !== undefined ? JSON.stringify(body) : undefined,
        });

        if (!r.ok) {
            const xato_matn = await r.text();
            throw new Error(`HTTP ${r.status}: ${xato_matn || r.statusText}`);
        }

        const tip = r.headers.get("Content-Type") || "";
        if (tip.includes("application/json")) {
            return await r.json();
        }
        return await r.text();
    } catch (e) {
        if (e.name === "AbortError") {
            throw new Error(`Timeout ${timeout}ms: ${url}`);
        }
        throw e;
    } finally {
        clearTimeout(tid);
    }
}

// 7) Real foydalanish
(async () => {
    try {
        const user = await api("https://api.github.com/users/torvalds");
        console.log(`${user.name} (${user.login}) — ${user.public_repos} repo`);
    } catch (e) {
        console.error("Xato:", e.message);
    }

    // POST misol
    try {
        const post = await api("https://jsonplaceholder.typicode.com/posts", {
            method: "POST",
            body: { title: "Test", body: "Mazmun", userId: 1 },
        });
        console.log("Yaratildi:", post);
    } catch (e) {
        console.error("Xato:", e.message);
    }

    // Parallel olish
    const repos = ["python/cpython", "facebook/react", "vuejs/vue"];
    const natijalar = await Promise.allSettled(
        repos.map((r) => api(`https://api.github.com/repos/${r}`)),
    );

    console.log("\n=== Reposlar ===");
    natijalar.forEach((res, i) => {
        if (res.status === "fulfilled") {
            const r = res.value;
            console.log(`✅ ${r.full_name.padEnd(25)} ⭐ ${r.stargazers_count.toLocaleString()}`);
        } else {
            console.log(`❌ ${repos[i]}: ${res.reason.message}`);
        }
    });
})();

// 8) Pagination — sahifa-sahifa
async function* repoSahifalari(user) {
    let page = 1;
    while (true) {
        const repos = await api(
            `https://api.github.com/users/${user}/repos?page=${page}&per_page=30`,
        );
        if (!repos.length) break;
        yield repos;
        page++;
    }
}

(async () => {
    let jami = 0;
    for await (const sahifa of repoSahifalari("torvalds")) {
        jami += sahifa.length;
        console.log(`Sahifa olindi (${sahifa.length} ta). Jami: ${jami}`);
        if (jami >= 50) break;    // misol uchun chegara
    }
})();
