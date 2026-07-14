Siz so'ragan barcha savollarga javoblar, amaliy tahlillar va o'qituvchingiz talab qiladigan mukammal CSS kod misollari bilan tayyorlandi.

1. CSS Position turlari va ularning farqlari
CSS-da position xossasi elementning sahifa koordinatalari tizimida qanday joylashishini belgilaydi.

Qiymat	Hujjat oqimidan (Document Flow) chiqadimi?	Koordinatalari kimga nisbatan olinadi (top, left...)?	Skrol (scroll) qilingandagi tabiati
static (Default)	Yo'q	Koordinata xossalari unga ta'sir qilmaydi.	Sahifa bilan birga skrol bo'ladi.
relative	Yo'q (O'z joyini saqlab qoladi)	O'zining dastlabki tabiiy o'rniga nisbatan suriladi.	Sahifa bilan birga skrol bo'ladi.
absolute	Ha (Keyingi elementlar uning o'rnini egallaydi)	Eng yaqin joylashgan position qiymati static bo'lmagan ota-onasiga nisbatan.	Sahifa bilan birga skrol bo'ladi.
fixed	Ha	To'g'ridan-to'g'ri brauzer oynasiga (Viewport) nisbatan.	Ekran oynasida qotib turadi, skrol qilinganda ham joyidan qimirlamaydi.
sticky	Yo'q (Dastlab oqimda turadi)	Belgilangan chegaraga yetguncha relative, yetgach esa fixed kabi o'zini tutadi.	Ota-ona elementi (parent container) ichida skrol bilan birga siljiydi va chegarada qotadi.
2. Stacking Context (Qatlamlar Muhiti) yaratuvchi xususiyatlar
z-index shunchaki elementlarni yuqoriga yoki pastga suradigan oddiy raqam emas. U faqat Stacking Context (qatlamlar muhiti) ichidagina ishlaydi. Quyidagi xususiyatlar elementda yangi "Stacking Context" yaratadi (boshqacha aytganda, yangi 3D qatlamlar guruhini boshlab beradi):

Hujjatning bosh ildizi (<html> elementi).

position qiymati absolute yoki relative bo'lgan va z-index qiymati autodan farqli (masalan, 1, 10) bo'lgan elementlar.

position qiymati fixed yoki sticky bo'lgan har qanday element (z-index shart emas).

Flexbox yoki Grid konteynerining ichki "child" elementi bo'lib, uning z-index qiymati autodan farqli bo'lsa.

opacity qiymati 1.0 dan kichik bo'lgan elementlar (masalan, opacity: 0.99;).

transform xossasi nonedan farqli bo'lgan elementlar (masalan, transform: scale(1);).

filter, perspective, clip-path, mask xossalari qo'llanilgan elementlar.

will-change xossasiga yuqoridagi qiymatlar yozilgan elementlar.

3. z-index nega ba'zida ishlamaydi? (Amaliy misollar)
Ko'p hollarda z-index: 99999; deb yozilsa ham element yuqoriga chiqmaydi. Buning 2 ta asosiy sababi bor:

Muammo A: Elementda position aniqlanmagan
z-index xossasi static (default) holatdagi elementlarda mutlaqo ishlamaydi.

Xato kod:

CSS
.box {
    z-index: 10; /* Ishlamaydi, chunki position: static */
}
Yechim: Kamida position: relative; qo'shish kerak.

CSS
.box {
    position: relative;
    z-index: 10; /* Endi mukammal ishlaydi! */
}
Muammo B: Ota-onaning "Stacking Context" iyerarxiyasi (Guruhlar jangi)
Agar ikkita element har xil stacking context ichida bo'lsa, ularning ota-onalarining z-indexi muhimroq rol o'ynaydi. Bola ota-onasining qatlamidan yuqoriga chiqa olmaydi.

Xato kod (Vizual muammo):

HTML
<div class="parent-A" style="position: relative; z-index: 1;">
    <div class="child-A" style="position: absolute; z-index: 9999;">Men eng tepada turmoqchiman!</div>
</div>
<div class="parent-B" style="position: relative; z-index: 2;">
    <div class="child-B" style="position: absolute; z-index: 5;">Men oddiy elementman.</div>
</div>
Natija: .child-A ning z-indexi 9999 bo'lishiga qaramay, u .child-B (z-index: 5) ning ostida qolib ketadi. Chunki uning otasi .parent-A (z-index: 1) ning kuchi .parent-B (z-index: 2) dan kuchsizroqdir.

4. overflow: hidden ning position ga ta'siri
overflow: hidden ota-onadan tashqariga chiqib ketgan elementlarni qirqib yuborish uchun ishlatiladi. Biroq, bu xossaning position elementlariga ta'siri quyidagicha:

absolute elementlarga ta'siri:
Agar ota-onada overflow: hidden bo'lsa va shu ota-onaning position qiymati relative, absolute yoki fixed bo'lsa (ya'ni u containing block bo'lsa), undan tashqariga chiqqan absolute bola element qirqiladi (yashiriladi).
Lekin, agar ota-onada position: static bo'lsa, overflow: hidden qo'llanilgan bo'lsa ham, absolute bola element qirqilmaydi va tashqariga chiqib turaveradi.

fixed elementlarga ta'siri:
fixed elementlar har doim to'g'ridan-to'g'ri brauzer oynasiga bog'langanligi sababli, ota-ona elementidagi overflow: hidden unga hech qachon ta'sir qilmaydi va uni qirqib qo'ymaydi (ota-onada g'ayrioddiy transform xossalari bo'lmasa).

5. Containing Block (Chegaralovchi Blok) nima va qanday aniqlanadi?
Containing Block — bu elementning o'lchamlari (foizda berilgan width, height, padding, margin) va joylashuv koordinatalari (top, left, right, bottom) kimga yoki qaysi maydonga qarab hisoblanishini belgilovchi mos yozuvlar ramkasidir (reference frame).

Elementning position turiga qarab Containing Block quyidagicha aniqlanadi:

static va relative elementlar uchun:
Eng yaqin joylashgan ota-ona (parent) elementning content maydoni (content-box) ularning containing block-i hisoblanadi.

absolute elementlar uchun:
Eng yaqin joylashgan, position xossasi static bo'lmagan (relative, absolute, fixed yoki sticky bo'lgan) ota-ona elementning padding maydoni (padding-box) containing block bo'ladi.

Eslatma: Agar bunday ota-ona topilmasa, eng yuqoridagi Initial Containing Block (ya'ni brauzer oynasi/hujjat maydoni) asos qilib olinadi.

fixed elementlar uchun:
Deyarli har doim brauzer oynasining o'zi (Viewport) containing block bo'ladi.

Istisno: Agar ota-ona elementlardan birortasida transform, perspective yoki filter xossalari qo'llanilgan bo'lsa, o'sha ota-ona elementi fixed element uchun containing block vazifasini o'tashni boshlaydi.
