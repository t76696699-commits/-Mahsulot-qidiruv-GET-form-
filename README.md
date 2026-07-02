Quyida PostgreSQL uchun siz so'ragan barcha SQL so'rovlari keltirilgan. Bu so'rovlar students (talabalar) va grades (baholar) jadvallari mavjud deb faraz qilingan.

1. Sinf reytingi (count, avg, max, min)
SQL
-- Sinf bo'yicha talabalar soni, o'rtacha bahosi, eng yuqori va eng past bahoni aniqlash
SELECT 
    class_id,
    COUNT(student_id) AS total_students,
    AVG(grade) AS avg_grade,
    MAX(grade) AS max_grade,
    MIN(grade) AS min_grade
FROM grades
GROUP BY class_id;
2. Fan bo'yicha statistika (HAVING bilan)
SQL
-- O'rtacha bahosi 70 dan yuqori bo'lgan fanlarni topish
SELECT 
    subject_name,
    AVG(grade) AS avg_subject_grade
FROM grades
GROUP BY subject_name
HAVING AVG(grade) > 70;
3. Har fan bo'yicha eng yaxshi talaba
SQL
-- Subquery yordamida har bir fandan eng yuqori baho olgan talabani aniqlash
SELECT g.subject_name, g.student_name, g.grade
FROM grades g
WHERE g.grade = (
    SELECT MAX(grade) 
    FROM grades 
    WHERE subject_name = g.subject_name
);
4. Bahosiz talabalar (LEFT JOIN + IS NULL)
SQL
-- Hali birorta ham baho olmagan talabalarni topish
SELECT s.student_name
FROM students s
LEFT JOIN grades g ON s.student_id = g.student_id
WHERE g.grade IS NULL;
5. "Universal a'lochi" (MIN(baho) >= 85)
SQL
-- Barcha fanlardan olgan baholari 85 dan kam bo'lmagan talabalar
SELECT student_id, student_name
FROM grades
GROUP BY student_id, student_name
HAVING MIN(grade) >= 85;
6. Hamma talabalar uchun o'rtacha (LEFT JOIN)
SQL
-- Bahosi bor yoki yo'qligidan qat'iy nazar hamma talabaning o'rtacha bahosi
SELECT s.student_name, AVG(g.grade) AS average_score
FROM students s
LEFT JOIN grades g ON s.student_id = g.student_id
GROUP BY s.student_id, s.student_name;
7. Bonus — UNION ALL bilan dashboard metrikalari
SQL
-- Dashboard uchun umumiy statistika (jami talabalar va umumiy o'rtacha)
SELECT 'Jami talabalar soni' AS metric, COUNT(*)::text AS value
FROM students
UNION ALL
SELECT 'Umumiy o''rtacha baho', AVG(grade)::text
FROM grades;
Eslatma:

Ushbu so'rovlar PostgreSQL muhitida optimal ishlashi uchun student_id va subject_name ustunlarida indekslar bo'lishi tavsiya etiladi.

Agar jadval tuzilmasi (schema) bo'yicha aniqroq o'zgarishlar kerak bo'lsa (masalan, jadval nomlari boshqacha bo'lsa), bemalol ayting, kodni moslashtirib beraman.
