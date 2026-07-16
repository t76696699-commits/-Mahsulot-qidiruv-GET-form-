// ─── this, call, apply, bind — sweep ─────────────────────────────────────

"use strict";

// 1) Obyekt metodida this
const it = {
    nom: "Rex",
    gapir() { console.log(`${this.nom}: vov!`); },
};
it.gapir();

// 2) Metodni boshqaga uzatganda this yo'qoladi
const gap = it.gapir;
try {
    gap();
} catch (e) {
    console.log("Tutib oldim:", e.message);
}

// 3) call — this'ni aniq berish
function tasvirla(yosh, kasb) {
    return `${this.nom}, ${yosh} yosh, ${kasb}`;
}

const ali = { nom: "Ali" };
const vali = { nom: "Vali" };
console.log(tasvirla.call(ali, 21, "frontend"));
console.log(tasvirla.call(vali, 19, "backend"));

// 4) apply — argumentlar massivda
console.log(tasvirla.apply(ali, [25, "fullstack"]));

// 5) bind — this'ni doimiy biriktirish
const aliTasvir = tasvirla.bind(ali);
console.log(aliTasvir(21, "dev"));
console.log(aliTasvir(22, "lead dev"));

// 6) Partial application — bind bilan argumentni ham biriktirish
const yoshKattaDev = tasvirla.bind(ali, 30);
console.log(yoshKattaDev("senior dev"));

// 7) setInterval bug — strict mode'da
const counter = {
    son: 0,
    boshlash() {
        // YOMON:
        // setInterval(function () { this.son++; }, 100);

        // YAXSHI 1: arrow (lexical this)
        const id = setInterval(() => {
            this.son++;
            console.log(`Arrow this — son: ${this.son}`);
            if (this.son >= 3) clearInterval(id);
        }, 50);
    },
};
counter.boshlash();

// 8) Class va arrow class maydon
class Servis {
    constructor(nom) {
        this.nom = nom;
        this.so_rovlar = 0;
    }

    // Oddiy metod — uzatilganda this yo'qoladi
    so_rov() {
        this.so_rovlar++;
        console.log(`${this.nom}: ${this.so_rovlar}-so'rov`);
    }

    // Arrow class maydon — doim bound
    so_rov_bound = () => {
        this.so_rovlar++;
        console.log(`${this.nom} (bound): ${this.so_rovlar}-so'rov`);
    };
}

const api = new Servis("API");

// Oddiy chaqiruvda OK
api.so_rov();

// Uzatilganda — bug
const yo_qol = api.so_rov;
try {
    yo_qol();
} catch (e) {
    console.log("Bound emas — tutildi");
}

// bound — har joyda ishlaydi
const ishla = api.so_rov_bound;
ishla();
ishla();

// 9) Real misol — array metodlari ichidagi this
const utils = {
    prefix: "user_",
    tegla(ismlar) {
        // Arrow — tashqi this'ga kirish mumkin
        return ismlar.map((ism) => this.prefix + ism);
    },
};
console.log(utils.tegla(["ali", "vali", "gulya"]));
// ["user_ali", "user_vali", "user_gulya"]

// Agar oddiy function ishlatsa:
const utils2 = {
    prefix: "user_",
    tegla(ismlar) {
        // function ichida this — undefined, .prefix ko'rinmaydi
        return ismlar.map(function (ism) {
            // return this.prefix + ism;    // TypeError
            return ism;
        });
    },
};
