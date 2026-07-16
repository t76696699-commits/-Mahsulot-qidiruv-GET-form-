// ─── Arrow, destructuring, spread/rest — to'liq sweep ─────────────────────

// 1) Arrow — barcha shakllar
const ikkilash = x => x * 2;
const qosh = (a, b) => a + b;
const yarat = (id, nom) => ({ id, nom, vaqt: Date.now() });
const ko_p = (a, b) => {
    const natija = a * b;
    return natija;
};

console.log(ikkilash(5));
console.log(qosh(3, 4));
console.log(yarat(1, "olma"));

// 2) Default parametrlar
const salom = (ism, salom_so_zi = "Salom") => `${salom_so_zi}, ${ism}!`;
console.log(salom("Ali"));
console.log(salom("Vali", "Assalomu alaykum"));

// 3) Obyektdan destructuring
const foydalanuvchi = {
    ism: "Ali",
    yosh: 21,
    manzil: { shahar: "Toshkent", indeks: 100000 },
    teglar: ["dev", "js"],
};

const { ism, yosh } = foydalanuvchi;
console.log(ism, yosh);

// Nested + default + nom o'zgartirish
const {
    ism: name,
    kasb = "dev",
    manzil: { shahar },
    teglar: [birinchi_teg],
} = foydalanuvchi;
console.log(name, kasb, shahar, birinchi_teg);

// 4) Funksiya argumentini destructure
const tasvir = ({ ism, yosh, kasb = "noma'lum" }) =>
    `${ism} (${yosh} yosh) — ${kasb}`;
console.log(tasvir(foydalanuvchi));

// 5) Massivdan
const [birinchi, ikkinchi, ...qolgani] = [10, 20, 30, 40, 50];
console.log(birinchi, ikkinchi, qolgani);

// O'rtasini o'tkazib yuborish
const [bosh, , uchinchi] = [1, 2, 3];
console.log(bosh, uchinchi);

// 6) Spread — array
const a = [1, 2, 3];
const b = [4, 5];
console.log([...a, ...b, 100]);
console.log(Math.max(...a, ...b));

// 7) Spread — obyekt
const defaults = { theme: "dark", lang: "uz", timeout: 30 };
const user = { lang: "en", verbose: true };
console.log({ ...defaults, ...user });

// 8) Rest — funksiya parametri
const eng_kattasi = (...sonlar) => {
    if (sonlar.length === 0) return null;
    return Math.max(...sonlar);
};
console.log(eng_kattasi(3, 7, 2, 9, 4));

// 9) Real misol — immutable update (React patterni)
const eski = { ism: "Ali", yosh: 21, sevimli: ["python"] };
const yangilangan = {
    ...eski,
    yosh: 22,
    sevimli: [...eski.sevimli, "js", "ts"],
};
console.log("Eski:", eski);
console.log("Yangi:", yangilangan);
console.log("Eski o'zgarmagan:", eski.yosh === 21);

// 10) Pipeline — talaba ballarini transformatsiya
const talabalar = [
    { ism: "Ali", ball: 87 },
    { ism: "Vali", ball: 54 },
    { ism: "Gulya", ball: 92 },
];

const tasvirlar = talabalar.map(({ ism, ball }) =>
    `${ism}: ${ball >= 70 ? "✅" : "❌"} ${ball}`,
);
console.log(tasvirlar);
