1. Semantik HTML nima va u nega muhim?HTML yozishda faqat <div> va <span> teglaridan foydalanish eng katta xatolardan biridir. Brauzerlar va qidiruv tizimlari (SEO botlari) sahifani tushunishi uchun Semantik (ma'noga ega) teglardan foydalanish shart.Semantik TegVazifasiMuqobili (Ishlatmaslik tavsiya etiladi)<header>Sahifaning yoki bo'limning eng yuqori qismi (navigatsiya, logotip)<div class="top-site"><nav>Menyu va navigatsiya havolalari uchun joy<div class="menu"><main>Sahifaning yagona va asosiy mazmuni joylashadigan qism<div class="content"><section>Umumiy mavzuga doir blok yoki bo'lim<div class="block"><article>Mustaqil, alohida o'qilishi mumkin bo'lgan maqola yoki yangilik<div class="post"><footer>Sahifaning eng pastki qismi (mualliflik huquqlari, aloqa)<div class="footer">Nega muhim?SEO (Qidiruv tizimi): Google kabi qidiruv tizimlari <main> va <h1> ichidagi matnlarni sahifaning eng muhim qismi deb biladi va qidiruvda yuqoriroq ko'rsatadi.Accessibility (Maxsus imkoniyatlar): Ko'zi ojiz insonlar ekran o'quvchi dasturlar (screen readers) yordamida internetdan foydalanganda, dastur aynan semantik teglarga qarab saytni ovozli o'qib beradi.2. Block vs Inline ElementlarCSS Box Modelda gaplashganimizdek, HTML elementlari tabiatan ikki xil xulq-atvorga ega bo'ladi:Block-level (Blokli) elementlar:Har doim yangi qatordan boshlanadi va sahifaning butun enini (100%) egallaydi. Ularga width, height, margin va padding to'liq ta'sir qiladi.Misollar: <div>, <p>, <h1>–<h6>, <ul>, <li>, <section>.Inline (Satrli) elementlar:Yangi qatordan boshlanmaydi, faqat o'ziga kerakli bo'lgan kontent miqdoricha joy egallaydi. Ularga width va height xossalari ta'sir qilmaydi. margin va padding faqat chap va o'ng tomondan ishlaydi.Misollar: <span>, <a>, <strong>, <em>, <img>.3. Atributlar va eng muhim qoidalarAtributlar HTML teglariga qo'shimcha ma'lumot yoki xususiyat berish uchun ishlatiladi. Ular har doim ochiluvchi teg ichida yoziladi (masalan: name="value").href — <a> tegi uchun manzilni belgilaydi.src va alt — <img> rasm tegi uchun. alt atributini yozish juda muhim! Agar rasm yuklanmay qolsa, uning o'rniga shu matn ko'rinadi va bu SEO uchun ham juda foydali.target="_blank" — Havola (link) bosilganda uni yangi oynada ochish uchun ishlatiladi.4. To'g'ri HTML Strukturasi (Andoza)Har bir standart HTML5 hujjati quyidagi asosiy skeletga ega bo'lishi kerak:HTML<!DOCTYPE html> <!-- Brauzerga bu HTML5 hujjati ekanligini bildiradi -->
<html lang="uz"> <!-- Sayt tili -->
<head>
    <meta charset="UTF-8"> <!-- Belgilar kodirovkasi (barcha tillarni qo'llab-quvvatlash uchun) -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0"> <!-- Mobil moslashuvchanlik uchun -->
    <title>Mening ajoyib sahifam</title>
</head>
<body>

    <header>
        <nav>
            <a href="#home">Bosh sahifa</a>
            <a href="#about">Biz haqimizda</a>
        </nav>
    </header>

    <main>
        <article>
            <h1>HTML asoslari</h1>
            <p>HTML — bu dasturlash tili emas, balki belgilash tilidir.</p>
        </article>
    </main>

    <footer>
        <p>&copy; 2026 Barcha huquqlar himoyalangan.</p>
    </footer>

</body>
</html>
