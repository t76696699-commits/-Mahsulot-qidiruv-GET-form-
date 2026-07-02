Ushbu PostgreSQL so‘rovlari ma’lumotlar bazasi bilan ishlashda eng ko‘p uchraydigan va muhim operatsiyalarni qamrab oladi.

1. INSERT (Ma’lumot qo‘shish usullari)
SQL
-- 1. Oddiy INSERT
INSERT INTO users (name, age) VALUES ('Ali', 25);

-- 2. Ko‘p qatorli (vergulli) INSERT
INSERT INTO users (name, age) VALUES ('Vali', 30), ('Guli', 22), ('Soli', 28);

-- 3. RETURNING bilan (qo‘shilgan qator ID sini olish)
INSERT INTO users (name, age) VALUES ('Karim', 35) RETURNING id;

-- 4. Boshqa jadvaldan SELECT bilan INSERT
INSERT INTO archive_users (name, age) 
SELECT name, age FROM users WHERE age > 30;

-- 5. ON CONFLICT (Upsert) - Agar kalit takrorlansa, yangilaydi
INSERT INTO users (id, name, age) VALUES (1, 'Ali', 26) 
ON CONFLICT (id) DO UPDATE SET age = EXCLUDED.age;
2. UPDATE (Ma’lumotni yangilash)
SQL
-- 1. Oddiy UPDATE
UPDATE users SET age = 27 WHERE name = 'Ali';

-- 2. Expression (ifoda) bilan UPDATE
UPDATE products SET price = price * 1.1 WHERE category = 'electronics';

-- 3. Korelatsion (Subquery) bilan UPDATE
UPDATE orders SET total_price = (SELECT SUM(amount) FROM order_items WHERE order_items.order_id = orders.id);
3. DELETE (O‘chirish)
SQL
-- 1. Oddiy DELETE
DELETE FROM users WHERE age < 18;

-- 2. RETURNING bilan (o‘chirilgan qatorlarni ko‘rish)
DELETE FROM users WHERE name = 'Guli' RETURNING *;
4. Tranzaksiyalar (BEGIN, COMMIT, ROLLBACK)
SQL
-- Tranzaksiyani boshlash
BEGIN;

INSERT INTO accounts (user_id, balance) VALUES (1, 100);
UPDATE accounts SET balance = balance - 100 WHERE user_id = 2;

-- Xatolik bo‘lsa bekor qilish (ROLLBACK)
ROLLBACK; 

-- Barchasi muvaffaqiyatli bo‘lsa saqlash (COMMIT)
COMMIT;
5. SAVEPOINT (Qisman bekor qilish)
SQL
BEGIN;
INSERT INTO logs (action) VALUES ('Start');
SAVEPOINT sp1; -- Qayta tiklash nuqtasi

INSERT INTO logs (action) VALUES ('Risky operation');
ROLLBACK TO SAVEPOINT sp1; -- 'Risky operation' bekor qilindi, 'Start' saqlandi

COMMIT;
6. Xavf: WHERE'siz UPDATE/DELETE
DIQQAT: WHERE shartisiz bajarilgan UPDATE yoki DELETE buyruqlari jadvaldagi barcha qatorlarni o‘zgartiradi yoki o‘chirib tashlaydi.

Xavfli UPDATE: UPDATE users SET status = 'active'; (Barcha foydalanuvchilar aktiv bo‘lib qoladi).

Xavfli DELETE: DELETE FROM users; (Jadval bo‘shatiladi).

Himoya: Ishlab chiqarish (production) muhitida DELETE yoki UPDATE yozishdan oldin, avval SELECT bilan qaysi qatorlar tanlanayotganini tekshirib ko‘ring. Tranzaksiya (BEGIN) ichida bajarish esa xatolikni ROLLBACK qilish imkonini beradi.
