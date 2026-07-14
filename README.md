Buni loyihangizning ildizidagi README.md fayliga to'liqligicha joylashtiring:Markdown# CSS Box Model va Kaskad (Cascade) Asoslari

Ushbu hujjatda CSS-ning eng muhim fundamental tushunchalari — Box Model, Kaskad tizimi, Specificity (ustunlik darajasi) va elementlarni yashirish usullari nazariy hamda amaliy kod misollari yordamida batafsil yoritilgan.

---

## 1. CSS Box Model (Quti Modeli) va uning Qatlamlari

CSS-da har bir HTML elementi to'rtburchak quti (box) sifatida qaraladi. Bu quti ichkaridan tashqariga qarab 4 ta asosiy qatlamdan iborat:

### Vizual Box Model Diagrammasi
┌───────────────────────────────────────────────┐│                   MARGIN                      │  <- 4. Tashqi bo'shliq│   ┌───────────────────────────────────────┐   ││   │               BORDER                  │   │  <- 3. Chegara│   │   ┌───────────────────────────────┐   │   ││   │   │           PADDING             │   │   │  <- 2. Ichki bo'shliq│   │   │   ┌───────────────────────┐   │   │   ││   │   │   │                       │   │   │   ││   │   │   │        CONTENT        │   │   │   │  <- 1. Haqiqiy kontent│   │   │   │                       │   │   │   ││   │   │   └───────────────────────┘   │   │   ││   │   └───────────────────────────────┘   │   ││   └───────────────────────────────────────┘   │└───────────────────────────────────────────────┘
### Qatlamlarning Tushuntirishi va CSS Misoli:
1. **Content (Tarkib):** Elementning haqiqiy matni, rasmi yoki boshqa kontenti joylashadigan markaziy maydon.
2. **Padding (Ichki bo'shliq):** Kontent va uning atrofidagi chegara (Border) orasidagi masofa. Element foni unga ham ta'sir qiladi.
3. **Border (Chegara):** Padding va Margin o'rtasidagi chiziq. U qalinlik, rang va shaklga ega bo'ladi.
4. **Margin (Tashqi bo'shliq):** Element chegarasidan tashqaridagi to'liq shaffof maydon. U qo'shni elementlarni bir-biridan itarish uchun xizmat qiladi.

```css
.box-example {
    width: 300px;         /* Content eni */
    height: 150px;        /* Content bo'yi */
    padding: 20px;        /* To'rt tomondan 20px ichki bo'shliq */
    border: 5px solid;    /* 5px qalinlikdagi chegara */
    margin: 30px;         /* Boshqa elementlardan 30px uzoqlashish */
}
2. box-sizing: content-box va border-box FarqiBu xossa elementning umumiy o'lchamlari (eni va bo'yi) brauzerda qanday hisoblanishini belgilaydi.A. content-box (Standart qiymat)Brauzer siz bergan width qiymatiga padding va border qalinligini qo'shimcha ravishda qo'shadi.Formula: $\text{Haqiqiy eni} = \text{width} + \text{chap-o'ng padding} + \text{chap-o'ng border}$CSS.item-content-box {
    box-sizing: content-box;
    width: 300px;
    padding: 20px; /* Chap + o'ng = 40px */
    border: 5px solid black; /* Chap + o'ng = 10px */
    /* Ekrandagi haqiqiy eni: 300 + 40 + 10 = 350px bo'lib, maketni buzishi mumkin */
}
B. border-box (Tavsiya etilgan amaliyot)Brauzer padding va border-ni siz bergan width o'lchamining ichiga joylashtiradi. Kontent maydoni shunga mos ravishda siqiladi.Formula: $\text{Haqiqiy eni} = \text{Faqat siz bergan width (300px)}$CSS.item-border-box {
    box-sizing: border-box;
    width: 300px;
    padding: 20px;
    border: 5px solid black;
    /* Ekrandagi haqiqiy eni: aniq 300px qoladi. Kontent maydoni 250px-ga tenglashadi */
}
3. Specificity (CSS Ustunligi) KalkulyatsiyasiKaskad qoidasiga ko'ra, bitta elementga bir nechta har xil selektor stillari yozilsa, g'olibni Specificity ball tizimi aniqlaydi.Ball tizimi iyerarxiyasi: (Inline, ID, Class/Attribute, Element) yoki shartli ravishda [0, 0, 0, 0] ko'rinishida hisoblanadi.5 ta Amaliy Misol Tahlili:div pTarkibi: 2 ta Element (div, p)Ball: [0, 0, 0, 2].card .titleTarkibi: 2 ta Klass (.card, .title)Ball: [0, 0, 2, 0] (Yuqoridagidan ustun)#main-content .text pTarkibi: 1 ta ID (#main-content), 1 ta Klass (.text), 1 ta Element (p)Ball: [0, 1, 1, 1]ul.nav-list li:hoverTarkibi: 2 ta Element (ul, li), 1 ta Klass (.nav-list), 1 ta Pseudo-klass (:hover)Ball: [0, 0, 2, 2]#sidebar div.active a[href]Tarkibi: 1 ta ID (#sidebar), 1 ta Klass (.active), 1 ta Atribut ([href]), 2 ta Element (div, a)Ball: [0, 1, 2, 2] (Ushbu ro'yxatdagi eng yuqori ustunlikka ega selektor)4. display: none, visibility: hidden va opacity: 0 farqlariElementni ekrandan yashirishning ushbu uchta usuli o'zining mexanizmi bilan keskin farq qiladi:CSS/* 1. display: none */
.hide-completely {
    display: none;
    /* Element DOMda qoladi, lekin ekranda mutloq joy egallamaydi (g'oyib bo'ladi). 
       Interaktiv emas (kliklab bo'lmaydi), animatsiya qilib bo'lmaydi. */
}

/* 2. visibility: hidden */
.hide-invisible {
    visibility: hidden;
    /* Element ekranda ko'rinmaydi, lekin o'z joyini saqlab qoladi (bo'shliq ko'rinadi).
       Foydalanuvchi u bilan ishlay olmaydi (kliklab bo'lmaydi). */
}

/* 3. opacity: 0 */
.hide-transparent {
    opacity: 0;
    /* Element to'liq shaffof (ko'rinmas) holatga keladi va o'z joyini saqlab qoladi.
       ENG MUHIMI: U hamon interaktiv — ko'rinmas bo'lsa ham uni kliklash mumkin!
       Transition orqali silliq animatsiyalar (fade-in/fade-out) yaratishga mos keladi. */
}
5. !important Qoidasi: Qachon ishlatish kerak va kerak emas?!important kalit so'zi CSS-dagi barcha o'lchov va Specificity qoidalarini chetlab o'tib, stillni mutloq g'olib qiladi.❌ Ishlatish tavsiya etilmaydi (Yomon amaliyot):Oddiy kod yozish jarayonida: Agar klassingiz elementga ta'sir qilmayotgan bo'lsa, unga !important berib majburlash arxitekturani buzadi. Keyinchalik o'sha elementni o'zgartirish juda qiyinlashadi.Global komponentlar yozganda: .btn { color: white !important; } deb yozilsa, keyinchalik loyihada qora rangli tugma yaratish imkonsiz bo'lib qoladi.Ishlatish tavsiya etiladi (Yaxshi amaliyot):Yordamchi (Utility) klasslarda: Global va faqat bitta vazifani o'taydigan klasslar har doim ishonchli ishlashi uchun:CSS.text-center { text-align: center !important; }
.d-none { display: none !important; }
Tashqi kutubxonalar (Bootstrap, Tailwind v.h) stillarini majburiy o'zgartirishda: Agar tayyor plugin yoki kutubxonaning chuqur yozilgan kodini o'zgartirishga selektoringizning kuchi yetmayotgan bo'lsa, yagona to'g'ri yo'l sifatida qo'llaniladi.
