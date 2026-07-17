// ─── CAPSTONE: TODO ilova (class + localStorage + fetch) ─────────────────
//
// To'liq ishchi versiya. Brauzerda HTML fayl bilan birga ishlatish mumkin:
//
//   <!doctype html>
//   <html>
//   <body>
//     <input id="matn" placeholder="Yangi vazifa..."/>
//     <button id="qosh">Qo'shish</button>
//     <ul id="lst"></ul>
//     <pre id="stat"></pre>
//     <script type="module" src="app.js"></script>
//   </body>
//   </html>

// 1) Todo domain klassi
class Todo {
    constructor({ id, matn, bajarildi = false, kategoriya = "boshqa", muhim = false, sana }) {
        this.id = id;
        this.matn = matn;
        this.bajarildi = bajarildi;
        this.kategoriya = kategoriya;
        this.muhim = muhim;
        this.sana = sana || new Date().toISOString();
    }

    get tasvir() {
        const tick = this.bajarildi ? "✅" : "⏳";
        const star = this.muhim ? "⭐" : "  ";
        return `${tick} ${star} [${this.kategoriya}] ${this.matn}`;
    }
}

// 2) TodoStore — encapsulated state
class TodoStore {
    #todos = [];
    #counter = 0;
    #kalit = "todos-v1";
    #apiBase;
    #listeners = [];

    constructor({ apiBase = "" } = {}) {
        this.#apiBase = apiBase;
        this.#yuklash();
    }

    // Subscribe — DOM yangilanish uchun
    on_change(fn) {
        this.#listeners.push(fn);
        return () => {
            this.#listeners = this.#listeners.filter((f) => f !== fn);
        };
    }

    #notify() {
        this.#listeners.forEach((fn) => fn(this));
    }

    // ── Public API ────────────────────────────────────────────────────
    qosh(matn, opts = {}) {
        if (!matn || !matn.trim()) throw new Error("Bo'sh matn");
        const id = ++this.#counter;
        this.#todos.push(new Todo({ id, matn: matn.trim(), ...opts }));
        this.#saqlash();
        this.#notify();
        return id;
    }

    belgilab(id) {
        const t = this.#todos.find((t) => t.id === id);
        if (!t) throw new Error(`Topilmadi: ${id}`);
        t.bajarildi = !t.bajarildi;
        this.#saqlash();
        this.#notify();
    }

    o_chir(id) {
        const idx = this.#todos.findIndex((t) => t.id === id);
        if (idx === -1) return;
        this.#todos.splice(idx, 1);
        this.#saqlash();
        this.#notify();
    }

    tahrirla(id, yangi_matn) {
        const t = this.#todos.find((t) => t.id === id);
        if (!t) return;
        t.matn = yangi_matn.trim();
        this.#saqlash();
        this.#notify();
    }

    nollash() {
        this.#todos = [];
        this.#counter = 0;
        this.#saqlash();
        this.#notify();
    }

    // ── Computed atributlar ──────────────────────────────────────────
    get hammasi()      { return [...this.#todos]; }
    get bajarilmagan() { return this.#todos.filter((t) => !t.bajarildi); }
    get bajarilgan()   { return this.#todos.filter((t) =>  t.bajarildi); }
    get muhimlar()     { return this.#todos.filter((t) =>  t.muhim); }

    kategoriya_bo_yicha(kat) {
        return this.#todos.filter((t) => t.kategoriya === kat);
    }

    qidirish(qatori) {
        const q = qatori.toLowerCase();
        return this.#todos.filter((t) => t.matn.toLowerCase().includes(q));
    }

    get statistika() {
        const jami = this.#todos.length;
        const bajarildi = this.bajarilgan.length;
        return {
            jami,
            bajarildi,
            qolgan: jami - bajarildi,
            foiz: jami ? Math.round((bajarildi / jami) * 100) : 0,
            kategoriyalar: [...new Set(this.#todos.map((t) => t.kategoriya))],
        };
    }

    // ── Server bilan sinxronlash ─────────────────────────────────────
    async sync() {
        if (!this.#apiBase) return;
        try {
            const r = await fetch(`${this.#apiBase}/todos`, {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(this.#todos),
            });
            if (!r.ok) throw new Error(`HTTP ${r.status}`);
            const natija = await r.json();
            console.log("Sync OK:", natija);
            return natija;
        } catch (e) {
            console.error("Sync xato:", e.message);
            return null;
        }
    }

    // ── Private persistensiya ────────────────────────────────────────
    #saqlash() {
        try {
            localStorage.setItem(this.#kalit, JSON.stringify(this.#todos));
        } catch (e) {
            console.error("Saqlash xato:", e);
        }
    }

    #yuklash() {
        try {
            const raw = localStorage.getItem(this.#kalit);
            if (!raw) return;
            const data = JSON.parse(raw);
            this.#todos = data.map((d) => new Todo(d));
            this.#counter = Math.max(0, ...this.#todos.map((t) => t.id));
        } catch (e) {
            console.error("Yuklash xato:", e);
        }
    }
}

// 3) Demo (Node yoki test muhitida — localStorage bo'lmasa skip)
if (typeof localStorage !== "undefined") {
    const store = new TodoStore({ apiBase: "" });
    store.nollash();

    store.qosh("Python o'rganish", { kategoriya: "ta'lim", muhim: true });
    store.qosh("Sport zal", { kategoriya: "sog'liq" });
    store.qosh("Loyihani tugatish", { kategoriya: "ish", muhim: true });
    store.qosh("Kitob o'qish", { kategoriya: "ta'lim" });

    store.belgilab(1);

    console.log("\n=== HAMMASI ===");
    store.hammasi.forEach((t) => console.log(t.tasvir));

    console.log("\n=== STATISTIKA ===");
    console.log(store.statistika);

    console.log("\n=== MUHIMLAR ===");
    store.muhimlar.forEach((t) => console.log(t.tasvir));

    console.log("\n=== Bajarilmagan ish kategoriyasi ===");
    store.kategoriya_bo_yicha("ish")
        .filter((t) => !t.bajarildi)
        .forEach((t) => console.log(t.tasvir));
}

// 4) DOM bog'lash (brauzer'da) — kommentdan chiqarib ishlating
//
// const store = new TodoStore({ apiBase: "/api" });
// const $matn = document.querySelector("#matn");
// const $qosh = document.querySelector("#qosh");
// const $lst  = document.querySelector("#lst");
// const $stat = document.querySelector("#stat");
//
// function render(s) {
//     $lst.innerHTML = s.hammasi
//         .map((t) => `
//             <li data-id="${t.id}">
//                 <span style="text-decoration:${t.bajarildi ? "line-through" : "none"}">
//                     ${t.tasvir}
//                 </span>
//                 <button data-act="toggle">✓</button>
//                 <button data-act="o_chir">×</button>
//             </li>
//         `)
//         .join("");
//
//     $stat.textContent = JSON.stringify(s.statistika, null, 2);
// }
//
// // Subscribe — har o'zgarishda DOM yangilanadi
// store.on_change(render);
//
// // Initial render
// render(store);
//
// // Add tugmasi
// $qosh.addEventListener("click", () => {
//     try {
//         store.qosh($matn.value);
//         $matn.value = "";
//         $matn.focus();
//     } catch (e) {
//         alert(e.message);
//     }
// });
//
// // Enter bilan ham qo'shish
// $matn.addEventListener("keydown", (e) => {
//     if (e.key === "Enter") $qosh.click();
// });
//
// // Event delegation — har LI tugmasi uchun alohida listener emas
// $lst.addEventListener("click", (e) => {
//     const li = e.target.closest("li");
//     if (!li) return;
//     const id = Number(li.dataset.id);
//     const act = e.target.dataset.act;
//     if (act === "toggle") store.belgilab(id);
//     if (act === "o_chir") store.o_chir(id);
// });
