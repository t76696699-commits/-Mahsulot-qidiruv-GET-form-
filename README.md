1. INNER JOIN bilan 3 ta jadvalni bog‘lash
Ushbu so‘rov faqat baholangan talabalar, ularning fanlari va olgan baholarini chiqaradi. Agar talaba baho olmagan bo‘lsa, bu ro‘yxatga kirmaydi.

SQL
SELECT 
    t.ism, 
    f.fan_nomi, 
    b.baho
FROM talabalar t
INNER JOIN baholar b ON t.id = b.talaba_id
INNER JOIN fanlar f ON b.fan_id = f.id;
2. LEFT JOIN bilan bahosizlarni saqlash
Bahosi bor yoki yo‘qligidan qat’i nazar, barcha talabalarni va ularning baholarini ko‘rish uchun LEFT JOIN ishlatamiz. Bahosi yo‘q talabalar qarshisida NULL chiqadi.

SQL
SELECT 
    t.ism, 
    b.baho
FROM talabalar t
LEFT JOIN baholar b ON t.id = b.talaba_id;
3. Bahosizlarni topish (WHERE b.id IS NULL)
Hali biror marta baho olmagan (yoki imtihondan qarzdor) talabalarni aniqlash uchun LEFT JOIN qilib, so‘ng NULL qiymatlarni filtrlaymiz.

SQL
SELECT 
    t.ism
FROM talabalar t
LEFT JOIN baholar b ON t.id = b.talaba_id
WHERE b.id IS NULL;
4. Har talabaning o‘rtacha bahosi (LEFT JOIN + GROUP BY)
Hatto baho olmagan talabalarni ham tushirib qoldirmaslik uchun LEFT JOIN qilamiz va ROUND funksiyasi orqali o‘rtacha bahoni yaxlitlaymiz.

SQL
SELECT 
    t.ism, 
    ROUND(AVG(b.baho), 2) AS o_rtacha_baho
FROM talabalar t
LEFT JOIN baholar b ON t.id = b.talaba_id
GROUP BY t.id, t.ism;
5. Har fan bo‘yicha eng yuqori ball olgan talaba
Bu masalani PostgreSQL-ning eng qulay instrumenti bo‘lgan DISTINCT ON yoki Oyna funksiyalari (Window Functions) bilan yechish mumkin. Quyida eng samarali yo‘li keltirilgan:

SQL
SELECT DISTINCT ON (f.id)
    f.fan_nomi,
    t.ism AS talaba_ismi,
    b.baho AS eng_yuqori_baho
FROM fanlar f
INNER JOIN baholar b ON f.id = b.fan_id
INNER JOIN talabalar t ON b.talaba_id = t.id
ORDER BY f.id, b.baho DESC;
6. Self JOIN misoli (Xuddi sinfdoshlar kabi)
Bitta jadvalni o‘zini o‘ziga bog‘lash. Masalan, bir xil guruhda o‘qiydigan talabalarni (sinfdoshlarni) juftlik qilib chiqarish. t1.id < t2.id sharti bir xil juftlik qayta takrorlanmasligi (A va B, B va A bo'lib kelmasligi) uchun kerak.

SQL
SELECT 
    t1.ism AS talaba_1, 
    t2.ism AS talaba_2, 
    t1.guruh_id
FROM talabalar t1
INNER JOIN talabalar t2 ON t1.guruh_id = t2.guruh_id AND t1.id < t2.id;
7. Ataylab xato — Implicit JOIN (Cartesian Product / CROSS JOIN)
Eski uslubdagi (ANSI-89) bog‘lashda WHERE sharti unutilsa yoki noto‘g‘ri yozilsa, Cartesian Product (Dekart ko‘paytmasi) hosil bo‘ladi. Ya'ni, jadvaldagi har bir qator ikkinchi jadvaldagi har bir qatorga ko‘paytirib chiqiladi (tizimni yuklama ostida qoldiradigan xavfli xato).

SQL
-- ATAYLAB QILINGAN XATO: WHERE sharti yo'q
SELECT 
    t.ism, 
    f.fan_nomi
FROM talabalar t, fanlar f; 
-- Bu so'rov CROSS JOIN vazifasini bajarib yuboradi va keraksiz millionlab qatorlarni qa
