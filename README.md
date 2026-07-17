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
