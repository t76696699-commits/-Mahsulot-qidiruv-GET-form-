// ─── Template literallar va string metodlari ─────────────────────────────

// 1) Template literal asoslari
const ism = "Ali";
const yosh = 21;
const kasb = "frontend dev";

console.log(`Salom, ${ism}! ${yosh} yoshdagi ${kasb}.`);

// 2) Ichida ifoda — hisob va metod chaqirish
const ball = 87;
console.log(`Ball: ${ball}, daraja: ${ball >= 90 ? "A'lo" : ball >= 70 ? "Yaxshi" : "F"}`);
console.log(`Katta: ${ism.toUpperCase()} (${ism.length} ta harf)`);

// 3) Ko'p qatorli xat
const xat = `Salom, ${ism}!

Bu — sizning yutuqlaringizning oylik hisoboti:
  • Ball:   ${ball}
  • Kasb:   ${kasb}
  • Maqsad: 95+ ball

Hurmat bilan,
Jamoa`;
console.log(xat);

// 4) String metodlari — sweep
const matn = "   Modern JavaScript juda kuchli!   ";

console.log(matn.trim());
console.log(matn.trim().toLowerCase());
console.log(matn.includes("Java"));
console.log(matn.startsWith("   Modern"));
console.log(matn.endsWith("!   "));
console.log(matn.indexOf("Java"));
console.log(matn.slice(3, 9));                    // "Modern"
console.log(matn.replaceAll(" ", "_"));

// 5) split + join — matnni qayta tartiblash
const teglar = "html, css, js, react, vue";
const massiv = teglar.split(",").map(t => t.trim());
console.log(massiv);
console.log(massiv.join(" | "));

// 6) padStart, padEnd — jadval kabi formatlash
console.log("Ism".padEnd(15, ".") + "Ball");
console.log("".padEnd(20, "─"));

const talabalar = [
    { ism: "Ali",     ball: 87 },
    { ism: "Vali",    ball: 54 },
    { ism: "Gulya",   ball: 92 },
    { ism: "Doniyor", ball: 68 },
];

talabalar.forEach(t => {
    console.log(t.ism.padEnd(15, ".") + String(t.ball).padStart(3, "0"));
});

// 7) toLocaleString — sonlarni chiroyli
const summa = 1_500_000;
console.log(summa.toLocaleString("uz-UZ"));
console.log(summa.toLocaleString("uz-UZ", { style: "currency", currency: "UZS" }));
console.log((0.875).toLocaleString("uz-UZ", { style: "percent" }));

// 8) Tagged template — kichik HTML highlighter
function escape_html(s) {
    return String(s)
        .replaceAll("<", "&lt;")
        .replaceAll(">", "&gt;");
}

function html(qismlar, ...qiymatlar) {
    return qismlar.reduce((acc, q, i) =>
        acc + q + (i < qiymatlar.length ? escape_html(qiymatlar[i]) : ""),
    "");
}

const xavfli = "<script>alert(1)</script>";
console.log(html`<p>Foydalanuvchi: ${xavfli}</p>`);
// <p>Foydalanuvchi: &lt;script&gt;alert(1)&lt;/script&gt;</p>

// 9) repeat va string yaratish
console.log("─".repeat(40));
console.log(" ".repeat(10) + "Yakuniy hisobot");
console.log("─".repeat(40));
