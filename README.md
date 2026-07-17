// ─── ES6+ class'lar chuqurroq — sweep ────────────────────────────────────

// 1) Private maydonlar va metodlar
class BankHisobi {
    #balans;
    #tarix = [];
    static #ochilgan_hisoblar = 0;       // private static

    static MAX_BALANS = 1_000_000_000;

    constructor(boshlangich = 0) {
        if (boshlangich < 0) throw new Error("Manfiy boshlangich");
        this.#balans = boshlangich;
        BankHisobi.#ochilgan_hisoblar++;
        this.raqami = BankHisobi.#ochilgan_hisoblar;
    }

    static jamiOchilgan() {
        return BankHisobi.#ochilgan_hisoblar;
    }

    #yozish(amal, n) {                    // private metod
        this.#tarix.push({ amal, n, vaqt: Date.now() });
    }

    qoshish(n) {
        if (n <= 0) throw new Error("Manfiy yoki nol");
        if (this.#balans + n > BankHisobi.MAX_BALANS) {
            throw new Error("Limit oshib ketdi");
        }
        this.#balans += n;
        this.#yozish("+", n);
        return this.#balans;
    }

    ayirish(n) {
        if (n > this.#balans) throw new Error("Yetarli mablag' yo'q");
        this.#balans -= n;
        this.#yozish("-", n);
        return this.#balans;
    }

    get balans() { return this.#balans; }
    get tarix()  { return [...this.#tarix]; }

    toString() {
        return `BankHisobi #${this.raqami} balans=${this.#balans}`;
    }
}

const h1 = new BankHisobi(1000);
const h2 = new BankHisobi(500);
h1.qoshish(500);
h2.qoshish(200);
console.log(h1.toString());
console.log(h2.toString());
console.log(`Jami ochilgan: ${BankHisobi.jamiOchilgan()}`);

// 2) Getter/setter bilan validatsiya
class Doira {
    #radius;

    constructor(radius) {
        this.radius = radius;        // setter ishlatadi
    }

    get radius() { return this.#radius; }

    set radius(val) {
        if (typeof val !== "number" || val < 0) {
            throw new TypeError("Radius musbat son bo'lishi kerak");
        }
        this.#radius = val;
    }

    get maydon() { return Math.PI * this.#radius ** 2; }
    get aylana() { return 2 * Math.PI * this.#radius; }

    toString() { return `Doira r=${this.#radius} S=${this.maydon.toFixed(2)}`; }
}

const d = new Doira(5);
console.log(`${d}`);
d.radius = 10;
console.log(`Yangi: ${d}`);
try { d.radius = -1; } catch (e) { console.log("Tutib oldim:", e.message); }

// 3) Inheritance + super
class Hayvon {
    constructor(nom, yoshi) {
        this.nom = nom;
        this.yoshi = yoshi;
    }

    gapir() {
        return `${this.nom}: ...`;
    }

    toString() {
        return `${this.constructor.name}(${this.nom}, ${this.yoshi} yosh)`;
    }
}

class It extends Hayvon {
    constructor(nom, yoshi, zot) {
        super(nom, yoshi);
        this.zot = zot;
    }

    gapir() {
        return `${this.nom} (${this.zot}): vov-vov!`;
    }
}

class A_lochiIt extends It {
    constructor(nom, yoshi) {
        super(nom, yoshi, "labrador");
    }

    gapirVaYugur() {
        const base = super.gapir();      // ota metodi
        return `${base} *yugurmoqda*`;
    }
}

const rex = new A_lochiIt("Rex", 3);
console.log(rex.gapir());
console.log(rex.gapirVaYugur());
console.log(rex.toString());
console.log(rex instanceof A_lochiIt, rex instanceof It, rex instanceof Hayvon);

// 4) Polimorfizm
class Mushuk extends Hayvon {
    gapir() { return `${this.nom}: myau!`; }
}

const hayvonlar = [
    new It("Rex", 3, "lab"),
    new Mushuk("Mursik", 5),
    new A_lochiIt("Rocky", 2),
];

console.log("\n=== Polimorfizm ===");
hayvonlar.forEach((h) => console.log(h.gapir()));

// 5) Static factory pattern
class Foydalanuvchi {
    constructor(ism, yosh, rol) {
        this.ism = ism;
        this.yosh = yosh;
        this.rol = rol;
    }

    static admin(ism) {
        return new Foydalanuvchi(ism, 25, "admin");
    }

    static guest() {
        return new Foydalanuvchi("Mehmon", 0, "guest");
    }
}

const a = Foydalanuvchi.admin("Ali");
const g = Foydalanuvchi.guest();
console.log(a, g);

// 6) Iterable class
class Sinf {
    #talabalar = [];

    constructor(nom) { this.nom = nom; }

    qosh(t) { this.#talabalar.push(t); }

    get talabalar_soni() { return this.#talabalar.length; }

    *[Symbol.iterator]() {                // generator metodi
        for (const t of this.#talabalar) yield t;
    }
}

const s = new Sinf("10-A");
["Ali", "Vali", "Gulya"].forEach((n) => s.qosh(n));

console.log(`\n${s.nom} (${s.talabalar_soni} ta):`);
for (const t of s) console.log(`  - ${t}`);
console.log([...s]);    // ['Ali', 'Vali', 'Gulya']
