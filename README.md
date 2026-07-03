1. 10,000+ qator generatsiya qilish
Katta hajmdagi ma'lumotlar bilan ishlash uchun jadval yaratamiz va generate_series funksiyasi yordamida ma'lumot to‘ldiramiz.

SQL
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    full_name VARCHAR(100),
    department VARCHAR(50),
    salary NUMERIC(10, 2),
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO employees (full_name, department, salary)
SELECT 
    'Employee ' || i,
    (ARRAY['IT', 'HR', 'Sales', 'Marketing'])[floor(random() * 4) + 1],
    (random() * 5000 + 1000)::NUMERIC(10, 2)
FROM generate_series(1, 20000) AS i;
2. Indekssiz EXPLAIN (Seq Scan)
Indeks bo‘lmaganda PostgreSQL butun jadvalni qidirib chiqadi (Sequential Scan).

SQL
EXPLAIN ANALYZE SELECT * FROM employees WHERE full_name = 'Employee 15000';
-- Natija: Seq Scan on employees (cost=0.00..385.00 rows=1 width=35)
3. B-tree indeks qo‘shish va taqqoslash
full_name ustuniga B-tree indeksi qo‘shamiz.

SQL
CREATE INDEX idx_employees_full_name ON employees (full_name);

-- Indeks bilan qayta tekshiramiz
EXPLAIN ANALYZE SELECT * FROM employees WHERE full_name = 'Employee 15000';
-- Natija: Index Scan using idx_employees_full_name on employees 
-- Seq Scan'dan ancha tezroq ishlaydi.
4. Boshqa indeks turlari
Multi-column indeks:
Ko'pincha department va salary bo'yicha bir vaqtda qidiruv bo'lsa, bu indeks samarali bo'ladi.

SQL
CREATE INDEX idx_dept_salary ON employees (department, salary);
Functional indeks (LOWER):
Agar so‘rovlar LOWER(full_name) ko‘rinishida bo‘lsa, oddiy indeks ishlamaydi.

SQL
CREATE INDEX idx_lower_fullname ON employees (LOWER(full_name));
Partial index (Qisman indeks):
Faqat 'IT' bo‘limidagi xodimlarni qidirish uchun bazani yengillashtirish.

SQL
CREATE INDEX idx_it_employees ON employees (full_name) WHERE department = 'IT';
FK ustuniga indeks:
PostgreSQL FOREIGN KEY ustunlariga avtomatik indeks qo'ymaydi. Agar orders jadvalida customer_id bo'lsa, uni qo'lda qo'shish kerak:

SQL
-- Agar orders jadvali bo'lsa:
CREATE INDEX idx_orders_customer_id ON orders(customer_id);
5. Indekslar narxi (Hisobot)
Indekslar qidiruvni tezlashtirsa-da, ularning "narxi" (salbiy tomonlari) mavjud:

Yozish tezligining pasayishi (Write Overhead): Har safar INSERT, UPDATE yoki DELETE qilganda, bazaning asosiy jadval bilan birga barcha indekslarni ham yangilashiga to‘g‘ri keladi. Bu operatsiyani sekinlashtiradi.

Xotira (Disk Space): Indekslar qo‘shimcha disk joyini egallaydi. Juda ko‘p indekslar bazaning umumiy hajmini keskin oshirib yuborishi mumkin.

RAM (Buffer Cache): Indekslar RAM'da joylashadi. Keraksiz indekslar operativ xotirani egallab, boshqa muhim ma'lumotlar uchun joy qoldirmasligi mumkin.

Planner murakkabligi: Juda ko‘p indeks bo‘lsa, SQL Optimizer (Planner) eng yaxshi
