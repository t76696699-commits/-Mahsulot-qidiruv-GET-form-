1. Ma'lumotlar bazasi sxemasi (DDL)
SQL
CREATE TABLE categories (
    cat_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE
);

CREATE TABLE products (
    prod_id SERIAL PRIMARY KEY,
    cat_id INT REFERENCES categories(cat_id) ON DELETE RESTRICT,
    name VARCHAR(100) NOT NULL,
    price NUMERIC(12, 2) CHECK (price >= 0)
);

CREATE TABLE customers (
    cust_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL
);

CREATE TABLE orders (
    ord_id SERIAL PRIMARY KEY,
    cust_id INT REFERENCES customers(cust_id) ON DELETE CASCADE,
    ord_date TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE order_items (
    item_id SERIAL PRIMARY KEY,
    ord_id INT REFERENCES orders(ord_id) ON DELETE CASCADE,
    prod_id INT REFERENCES products(prod_id),
    quantity INT CHECK (quantity > 0),
    subtotal NUMERIC(12, 2)
);

-- Indekslar
CREATE INDEX idx_orders_cust ON orders(cust_id);
CREATE INDEX idx_items_prod ON order_items(prod_id);
CREATE INDEX idx_products_cat ON products(cat_id);
CREATE INDEX idx_orders_date ON orders(ord_date);
2. Tranzaksiya ichida test ma'lumotlar
SQL
BEGIN;
-- Ma'lumotlar to'ldiriladi (5 kategoriya, 15 mahsulot, 8 mijoz, 14 buyurtma)
-- ... [INSERT INTO... buyruqlari] ...
COMMIT;
3. Tahliliy so‘rovlar va optimallashtirish
Murakkab so'rov: Har kategoriyadagi eng qimmat mahsulot (Window Function)

SQL
SELECT name, category_name, price FROM (
    SELECT p.name, c.name as category_name, p.price,
           RANK() OVER(PARTITION BY c.cat_id ORDER BY p.price DESC) as rn
    FROM products p JOIN categories c ON p.cat_id = c.cat_id
) t WHERE rn = 1;
Sotilmagan mahsulotlar (Self-join/Anti-join):

SQL
SELECT p.name FROM products p 
LEFT JOIN order_items oi ON p.prod_id = oi.prod_id 
WHERE oi.item_id IS NULL;
CTE bilan optimallashtirilgan hisobot (Oylik daromad o'sishi):

SQL
WITH MonthlySales AS (
    SELECT DATE_TRUNC('month', o.ord_date) as m, SUM(oi.subtotal) as revenue
    FROM orders o JOIN order_items oi ON o.ord_id = oi.ord_id
    GROUP BY 1
)
SELECT m, revenue, 
       LAG(revenue) OVER(ORDER BY m) as prev_rev,
       ((revenue - LAG(revenue) OVER(ORDER BY m)) / LAG(revenue) OVER(ORDER BY m)) * 100 as growth_pct
FROM MonthlySales;
4. RFM Segmentatsiya (Bonus)
Mijozlarni Recency (oxirgi xarid), Frequency (xaridlar soni) va Monetary (sarflagan puli) bo‘yicha ajratish:

SQL
SELECT cust_id,
       NTILE(5) OVER(ORDER BY MAX(ord_date)) as recency_score,
       NTILE(5) OVER(ORDER BY COUNT(ord_id)) as frequency_score,
       NTILE(5) OVER(ORDER BY SUM(total_amount)) as monetary_score
FROM (SELECT cust_id, ord_id, MAX(ord_date) as ord_date, SUM(subtotal) as total_amount 
      FROM orders JOIN order_items USING(ord_id) GROUP BY 1, 2) t
GROUP BY cust_id;
5. EXPLAIN ANALYZE va Tekshiruv
Siz so‘rovlaringiz samaradorligini quyidagicha tekshirishingiz kerak:

SQL
EXPLAIN ANALYZE SELECT * FROM order_items WHERE prod_id = 5;
Tavsiyalar:

EXPLAIN ANALYZE natijasida Seq Scan ko‘rsatsa, indeks yetarli emas yoki ma'lumot juda kam (Postgres optimizatori indeksdan ko‘ra butun jadvalni o‘qishni tezroq deb topadi).

CTE dan foydalanganda, agar so‘rov juda sekin bo‘lsa, CTE ichida MATERIALIZED kalit so‘zini qo‘shib ko‘ring.

Screenshotlar: SQL natijalarini (masalan, pgAdmin yoki DBeaverda) va EXPLAIN ANALYZE ning grafikli (Plan Tree) ko‘rinishini saqlab qo‘ying.

Bu sxema 14 kunlik muddat ichida ishlash uchun yetarli fundamentni beradi. Har bir bosqichni ma'lumotlar bilan to'ldirib, natijalarni vizuallashtiring.
