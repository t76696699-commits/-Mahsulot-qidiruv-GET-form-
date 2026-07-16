// ─── R1: Sotuvchi statistikasi to'liq misoli ──────────────────────────────

const savdolar = [
    { sotuvchi: "Ali",     mahsulot: "noutbuk",    narx: 12_000_000, miqdor: 1 },
    { sotuvchi: "Vali",    mahsulot: "telefon",    narx: 5_500_000,  miqdor: 2 },
    { sotuvchi: "Ali",     mahsulot: "klaviatura", narx: 450_000,    miqdor: 3 },
    { sotuvchi: "Gulya",   mahsulot: "monitor",    narx: 3_200_000,  miqdor: 1 },
    { sotuvchi: "Vali",    mahsulot: "noutbuk",    narx: 11_800_000, miqdor: 1 },
    { sotuvchi: "Ali",     mahsulot: "telefon",    narx: 6_200_000,  miqdor: 1 },
    { sotuvchi: "Doniyor", mahsulot: "monitor",    narx: 3_500_000,  miqdor: 2 },
];

// 1) Jami savdo summasi
const jami = savdolar.reduce((acc, { narx, miqdor }) => acc + narx * miqdor, 0);
console.log(`Jami savdo: ${jami.toLocaleString("uz-UZ")} so'm`);

// 2) Takrorlanmas sotuvchilar — Set
const sotuvchilar = [...new Set(savdolar.map(s => s.sotuvchi))].sort();
console.log("Sotuvchilar:", sotuvchilar);

// 3) Sotuvchi -> jami summa
const jami_per_sot = savdolar.reduce((acc, { sotuvchi, narx, miqdor }) => {
    acc[sotuvchi] = (acc[sotuvchi] || 0) + narx * miqdor;
    return acc;
}, {});
console.log("Per sotuvchi:", jami_per_sot);

// 4) TOP 3 sotuvchi
const top3 = Object.entries(jami_per_sot)
    .sort(([, a], [, b]) => b - a)
    .slice(0, 3);

console.log("\n=== TOP 3 SOTUVCHI ===");
top3.forEach(([ism, summa], i) => {
    console.log(`${i + 1}. ${ism.padEnd(10)} ${summa.toLocaleString("uz-UZ").padStart(15)} so'm`);
});

// 5) Eng katta bitta savdo
const eng_qimmat = savdolar.reduce((a, b) => (a.narx > b.narx ? a : b));
console.log(`\nEng qimmat: ${eng_qimmat.sotuvchi} — ${eng_qimmat.mahsulot} (${eng_qimmat.narx.toLocaleString("uz-UZ")} so'm)`);

// 6) Mahsulot bo'yicha guruhlash
const per_mahsulot = savdolar.reduce((acc, { mahsulot, narx, miqdor }) => {
    acc[mahsulot] = (acc[mahsulot] || 0) + narx * miqdor;
    return acc;
}, {});

console.log("\nMahsulot bo'yicha:");
Object.entries(per_mahsulot)
    .sort(([, a], [, b]) => b - a)
    .forEach(([nom, summa]) => {
        console.log(`  ${nom.padEnd(12)} ${summa.toLocaleString("uz-UZ").padStart(15)} so'm`);
    });

// 7) Pipeline — kategoriya filterlash
const noutbuk_savdolar = savdolar
    .filter(s => s.mahsulot === "noutbuk")
    .map(({ sotuvchi, narx }) => ({ sotuvchi, narx_mln: narx / 1_000_000 }));

console.log("\nNoutbuk savdolari (mln):", noutbuk_savdolar);

// 8) every / some
console.log("Hammasi 100k+:", savdolar.every(s => s.narx >= 100_000));
console.log("Bor 10M+:    ", savdolar.some(s => s.narx >= 10_000_000));

// 9) Hisobot — bitta katta template literal
const hisobot = `
═══════════════════════════════════════
       SAVDO HISOBOTI
═══════════════════════════════════════
Jami savdo:    ${jami.toLocaleString("uz-UZ")} so'm
Savdo soni:    ${savdolar.length} ta
O'rtacha:      ${Math.round(jami / savdolar.length).toLocaleString("uz-UZ")} so'm

TOP sotuvchi:  ${top3[0][0]} (${top3[0][1].toLocaleString("uz-UZ")} so'm)
═══════════════════════════════════════
`;
console.log(hisobot);
