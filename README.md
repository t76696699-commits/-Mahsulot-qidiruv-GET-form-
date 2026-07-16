// ─── Massiv iteratorlari — to'liq sweep ──────────────────────────────────

const sonlar = [1, 5, -3, 8, -2, 11, 0, 7];

// 1) map — har elementni transformatsiya
console.log(sonlar.map(x => x * x));

// 2) filter — shartga to'g'ri keladiganlar
console.log(sonlar.filter(x => x > 0));

// 3) filter + map zanjiri
const musbat_kvadratlar = sonlar
    .filter(x => x > 0)
    .map(x => x * x);
console.log("Musbat kvadratlar:", musbat_kvadratlar);

// 4) reduce — yig'indi
const jami = sonlar.reduce((acc, x) => acc + x, 0);
console.log("Jami:", jami);

// 5) reduce — group by (eng kuchli pattern)
const matnlar = ["olma", "olma", "non", "olma", "non", "sut", "sut"];
const sanash = matnlar.reduce((acc, m) => {
    acc[m] = (acc[m] || 0) + 1;
    return acc;
}, {});
console.log("Sanash:", sanash);

// 6) reduce — eng katta sonni topish
const eng_katta = sonlar.reduce((a, b) => (a > b ? a : b));
console.log("Eng katta:", eng_katta);

// 7) Real ma'lumot — talabalar
const talabalar = [
    { ism: "Ali",     ball: 87, kasb: "dev" },
    { ism: "Vali",    ball: 54, kasb: "designer" },
    { ism: "Gulya",   ball: 92, kasb: "dev" },
    { ism: "Doniyor", ball: 68, kasb: "qa" },
    { ism: "Karim",   ball: 95, kasb: "dev" },
];

// find — birinchi yuqori ballini topish
const a_lo = talabalar.find(t => t.ball >= 90);
console.log("Birinchi A'lo:", a_lo);

// some / every
console.log("Bor a'lo:",      talabalar.some(t => t.ball >= 90));
console.log("Hammasi >50:",   talabalar.every(t => t.ball > 50));

// 8) Chain — TOP 3 dev
const top3_dev = talabalar
    .filter(t => t.kasb === "dev")
    .sort((a, b) => b.ball - a.ball)
    .slice(0, 3);
console.log("TOP 3 dev:", top3_dev);

// 9) reduce — kasb bo'yicha guruhlash
const kasb_guruhlari = talabalar.reduce((acc, t) => {
    if (!acc[t.kasb]) acc[t.kasb] = [];
    acc[t.kasb].push(t.ism);
    return acc;
}, {});
console.log("Kasb guruhlari:", kasb_guruhlari);

// 10) reduce — kasb bo'yicha o'rta ball
const o_rta_per_kasb = Object.entries(kasb_guruhlari).map(([kasb, ismlar]) => {
    const ballar = ismlar.map(ism =>
        talabalar.find(t => t.ism === ism).ball,
    );
    const o_rta = ballar.reduce((a, b) => a + b, 0) / ballar.length;
    return { kasb, o_rta };
});
console.log("O'rta per kasb:", o_rta_per_kasb);

// 11) Mahsulotlar misoli
const mahsulotlar = [
    { nom: "Olma", narx: 12000, soni: 5 },
    { nom: "Non",  narx: 4000,  soni: 12 },
    { nom: "Sut",  narx: 9000,  soni: 3 },
];

const jami_summa = mahsulotlar.reduce(
    (acc, m) => acc + m.narx * m.soni,
    0,
);
console.log("Jami:", jami_summa.toLocaleString(), "so'm");
