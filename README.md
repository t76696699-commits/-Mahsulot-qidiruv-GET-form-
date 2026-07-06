1. COUNT(*) va COUNT(DISTINCT) bilan misol
COUNT(*) – jadvaldagi jami qatorlar sonini hisoblaydi (hatto qiymati NULL bo'lsa ham).

COUNT(DISTINCT ustun) – faqat takrorlanmas (unikal) va NULL bo'lmagan qiymatlarni hisoblaydi.

SQL
SELECT 
    COUNT(*) AS jami_talabalar,
    COUNT(DISTINCT guruh_id) AS jami_unikal_guruhlar
FROM talabalar;
2. SUM, AVG, MIN, MAX va ROUND bilan to‘liq statistika
Xodimlarning oylik maoshlari misolida umumiy, eng yuqori, eng past va yaxlitlangan o‘rtacha qiymatlarni hisoblash:

SQL
SELECT 
    SUM(maosh) AS jami_maosh_fondi,
    MIN(maosh) AS eng_kam_maosh,
    MAX(maosh) AS eng_ko_p_maosh,
    AVG(maosh) AS aniq_o_rtacha_maosh,
    ROUND(AVG(maosh), 2) AS yaxlitlangan_o_rtacha_maosh  -- Verguldan keyin 2 ta raqamgacha
FROM xodimlar;
3. String funksiyalar misoli (UPPER, LENGTH, ||)
Matnlar ustida amallar bajarish: ism-familiyani katta harfga o'tkazish, ularni birlashtirish (||) va matn uzunligini o'lchash.

SQL
SELECT 
    UPPER(ism) AS katta_harfda,
    ism || ' ' || familiya AS to_liq_ism,
    LENGTH(ism) AS ism_harflar_soni
FROM xodimlar;
4. Sana funksiyalari (NOW(), CURRENT_DATE, EXTRACT)
Vaqt va sana bilan ishlash, sanadan ma'lum bir qismni (yil, oy, kun) ajratib olish.

SQL
SELECT 
    NOW() AS hozirgi_vaqt_va_sana,          -- Masalan: 2026-07-06 10:47:03
    CURRENT_DATE AS bugungi_sana,           -- Masalan: 2026-07-06
    EXTRACT(YEAR FROM tugilgan_sana) AS tugilgan_yili,
    EXTRACT(MONTH FROM NOW()) AS joriy_oy  -- Hozirgi oy raqami (7)
FROM xodimlar;
5. Matematik funksiyalar (CEIL, FLOOR, MOD)
CEIL(x) – sonni tepaga qarab butun songa yaxlitlaydi.

FLOOR(x) – sonni pastga qarab butun songa yaxlitlaydi.

MOD(x, y) – x ni y ga bo‘lgandagi qoldiqni hisoblaydi.

SQL
SELECT 
    CEIL(5.1) AS tepaga_yaxlitlash,   -- Natija: 6
    FLOOR(5.9) AS pastga_yaxlitlash,  -- Natija: 5
    MOD(10, 3) AS qoldiq_hisoblash    -- Natija: 1 (10 ni 3 ga bo'lgandagi qoldiq)
FROM xodimlar LIMIT 1;
6. Ataylab xato — WHERE ichida agregat funksiya ishlatish
❌ Xato so‘rov:
SQL
-- ATAYLAB QILINGAN XATO
SELECT ism, maosh 
FROM xodimlar 
WHERE maosh > AVG(maosh);
Xatolik xabari: ERROR: aggregate functions are not allowed in WHERE

💡 Sababi va tushuntirish:
SQL so'rovlarni bajarishda ma'lum bir ketma-ketlikka amal qiladi (SQL Order of Execution). WHERE filtri jadvaldagi qatorlar guruhlanishidan va agregat funksiyalar (masalan, AVG()) hisoblanishidan oldin ishga tushadi. Ya'ni, WHERE ishlayotgan vaqtda PostgreSQL hali o‘rtacha maosh qanchaligini bilmaydi.

✅ To‘g‘ri yechim (Subquery yoki HAVING yordamida):
O'rtacha maoshdan yuqori oladiganlarni topish uchun ichma-ich so'rov (Subquery) ishlatiladi:

SQL
SELECT ism, maosh 
FROM xodimlar 
WHERE maosh > (SELECT AVG(maosh) FROM xodimlar);
