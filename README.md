1. Divni Markazlash (Centering a Div) — 5 xil usul
Ichki div elementini (.child) tashqi konteyner (.parent) ichida ham gorizontal, ham vertikal markazlashtirishning eng ommabop usullari:

Usul 1: Flexbox (Zamonaviy va eng oson yo'li)
Eng ko'p ishlatiladigan va moslashuvchan usul.

CSS
.parent {
    display: flex;
    justify-content: center; /* Gorizontal markazlash */
    align-items: center;     /* Vertikal markazlash */
    height: 300px;           /* Balandlik shart */
}
Usul 2: CSS Grid (Eng qisqa kod)
Atigi ikki qator kod bilan mukammal markazlash.

CSS
.parent {
    display: grid;
    place-items: center;     /* Ham gorizontal, ham vertikal */
    height: 300px;
}
Usul 3: Absolute Position + Transform (Dinamik o'lchamlar uchun)
Agar .child elementining eni va bo'yi aniq bo'lmasa ham mukammal ishlaydi.

CSS
.parent {
    position: relative;
    height: 300px;
}
.child {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%); /* O'z eni va bo'yining yarmi miqdorida orqaga suradi */
}
Usul 4: Absolute Position + Margin Auto (O'lchamlari aniq elementlar uchun)
Agar .child elementining eni va bo'yi aniq belgilangan bo'lsa, ushbu klassik usul ajoyib ishlaydi.

CSS
.parent {
    position: relative;
    height: 300px;
}
.child {
    position: absolute;
    top: 0; bottom: 0; left: 0; right: 0;
    margin: auto;
    width: 100px;
    height: 100px;
}
Usul 5: Flexbox + Auto Margin (Ajoyib trik)
Flex-konteyner ichidagi elementga margin: auto berish uni har tomondan o'rtaga suradi.

CSS
.parent {
    display: flex;
    height: 300px;
}
.child {
    margin: auto; /* Avtomatik ravishda markazlashadi */
}
2. Sticky Footer (Yopishqoq Footer)
Muammo: Agar sahifada kontent kam bo'lsa, footer sahifaning o'rtasiga chiqib qoladi.
Yechim: Flexbox yordamida sahifa balandligi kamida 100vh bo'lishini ta'minlab, kontent kam bo'lsa ham foterning har doim pastda turishiga erishamiz.

HTML
<body class="site-wrapper">
    <header>Header</header>
    <main class="main-content">Asosiy kontent (bu yerda matn kam bo'lsa ham footer pastda qoladi)</main>
    <footer>Footer</footer>
</body>
CSS
.site-wrapper {
    display: flex;
    flex-direction: column;
    min-height: 100vh; /* Ekran balandligini to'liq egallaydi */
    margin: 0;
}

.main-content {
    flex-grow: 1; /* Barcha bo'sh joyni egallab, foterini pastga itaradi */
}
3. Holy Grail Layout (Muqaddas Maket)
Header, chap va o'ng yon panellar (sidebar), asosiy kontent va footerga ega bo'lgan klassik 3 ustunli maket. CSS Grid yordamida buni juda oson va semantik tarzda yaratish mumkin.

HTML
<div class="holy-grail">
    <header>Header</header>
    <aside class="left-sidebar">Chap Sidebar</aside>
    <main class="content">Asosiy Kontent</main>
    <aside class="right-sidebar">O'ng Sidebar</aside>
    <footer>Footer</footer>
</div>
CSS
.holy-grail {
    display: grid;
    grid-template-rows: auto 1fr auto; /* Header, Kontent, Footer balandliklari */
    grid-template-columns: 200px 1fr 200px; /* Sidebar, Asosiy kontent, Sidebar eni */
    min-height: 100vh;
}

header {
    grid-column: 1 / -1; /* To'liq eni bo'ylab cho'ziladi */
}

footer {
    grid-column: 1 / -1; /* To'liq eni bo'ylab cho'ziladi */
}

/* Mobil qurilmalarga moslashtirish (Responsive) */
@media (max-width: 768px) {
    .holy-grail {
        grid-template-columns: 1fr;
        grid-template-rows: auto auto auto auto auto;
    }
    header, footer, .left-sidebar, .right-sidebar, .content {
        grid-column: auto;
    }
}
4. Teng Balandlikdagi Ustunlar (Equal Height Columns)
Muammo: Ustunlar ichidagi matn miqdori turlicha bo'lganda, ularning balandligi har xil bo'lib qoladi va vizual dizayn buziladi.
Yechim: Flexbox yoki Grid ishlatilganda, uning ichidagi barcha "choild" elementlar tabiiy ravishda eng uzun element balandligiga moslashadi (align-items: stretch xossasi sukut bo'yicha faol bo'ladi).

HTML
<div class="columns-container">
    <div class="column">Kamroq matnli ustun.</div>
    <div class="column">Juda ko'p matnli ustun. Bu ustunning balandligi eng katta bo'ladi va boshqa barcha ustunlar bunga tenglashadi.</div>
    <div class="column">O'rtacha matnli ustun.</div>
</div>
CSS
.columns-container {
    display: flex; /* Yoki display: grid; grid-template-columns: repeat(3, 1fr); */
    gap: 20px;
}

.column {
    background-color: #e2e8f0;
    padding: 20px;
    flex: 1; /* Ustunlar eni teng bo'lishi uchun */
}
5. Fixed Header bilan To'liq Sahifa Layout
Header sahifaning tepasida qotib turadi (skrol qilinganda ham), qolgan kontent esa uning ostidan boshlanadi. Bu yerda asosiy e'tibor kontent headerning orqasida yashirinib qolmasligiga qaratiladi.

HTML
<header class="fixed-header">Logo & Navigatsiya</header>
<main class="page-content">
    <h1>Asosiy sarlavha</h1>
    <p>Sahifaning uzun kontenti shu yerda davom etadi...</p>
</main>
CSS
:root {
    --header-height: 70px; /* Header balandligini o'zgaruvchida saqlaymiz */
}

.fixed-header {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: var(--header-height);
    background-color: #0f172a;
    color: white;
    z-index: 1000; /* Boshqa elementlardan ustun turishi uchun */
}

.page-content {
    /* Eng muhim qism: Kontent fixed header ostida qolib ketmasligi uchun 
       header balandligicha yuqoridan bo'shliq (margin yoki padding) beramiz */
    margin-top: var(--header-height);
    padding: 30px;
    min-height: calc(100vh - var(--header-height));
}
