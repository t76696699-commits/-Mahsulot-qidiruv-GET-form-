Quyida PostgreSQL uchun keltirilgan talablarga to‘liq javob beradigan ma’lumotlar bazasi sxemasi va misollar keltirilgan.

1. Jadval yaratish va bog‘lanishlar (DDL)
Keling, kichik bir elektron do‘kon tizimini yaratamiz: Kategoriyalar, Mahsulotlar, Mijozlar va Buyurtmalar.

SQL
-- 1. Kategoriyalar jadvali
CREATE TABLE categories (
    category_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE
);

-- 2. Mahsulotlar jadvali (CHECK va DEFAULT bilan)
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price NUMERIC(12, 2) CHECK (price > 0),
    stock_qty INT DEFAULT 0,
    category_id INT REFERENCES categories(category_id) ON DELETE RESTRICT
);

-- 3. Mijozlar jadvali (Multi-column UNIQUE misoli)
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    -- Pasport seriyasi va raqami birgalikda takrorlanmasligi kerak
    passport_series VARCHAR(2) NOT NULL,
    passport_number VARCHAR(7) NOT NULL,
    UNIQUE(passport_series, passport_number) 
);

-- 4. Buyurtmalar jadvali (TIMESTAMPTZ va ON DELETE CASCADE bilan)
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers(customer_id) ON DELETE CASCADE,
    product_id INT REFERENCES products(product_id),
    order_date TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);
2. ALTER TABLE misollari
Jadval yaratilgandan keyin unga yangi ustun qo‘shish yoki mavjud ustunni o‘zgartirish:

SQL
-- Yangi ustun qo'shish
ALTER TABLE products ADD COLUMN discount_price NUMERIC(12, 2);

-- Mavjud ustun turini o'zgartirish (yoki cheklov qo'shish)
ALTER TABLE products ALTER COLUMN name SET NOT NULL;
3. Xatolarga urinishlar (Constraint Violation)
Quyidagi buyruqlar yuqoridagi cheklovlarni (constraints) buzadi va PostgreSQL xato qaytaradi:

SQL
-- 1. CHECK xatosi: Narx 0 dan kichik bo'lishi mumkin emas
INSERT INTO products (name, price) VALUES ('Telefon', -100);
-- Xato: new row for relation "products" violates check constraint "products_price_check"

-- 2. UNIQUE xatosi: Bir xil pasport ma'lumotlarini qayta kiritish
INSERT INTO customers (first_name, last_name, email, passport_series, passport_number) 
VALUES ('Ali', 'Aliyev', 'ali@mail.com', 'AA', '1234567');

INSERT INTO customers (first_name, last_name, email, passport_series, passport_number) 
VALUES ('Vali', 'Valiyev', 'vali@mail.com', 'AA', '1234567');
-- Xato: duplicate key value violates unique constraint "customers_passport_series_passport_number_key"

-- 3. FOREIGN KEY (RESTRICT) xatosi: Kategoriyani o'chirishga urinish
-- Agar kategoriyada mahsulot bo'lsa, uni o'chirib bo'lmaydi
DELETE FROM categories WHERE category_id = 1;
-- Xato: update or delete on table "categories" violates foreign key constraint 
-- "products_category_id_fkey" on table "products"
Tushuntirishlar:
NUMERIC(12, 2): 12 ta xona, shundan 2 tasi verguldan keyin (pul birliklari uchun ideal).

TIMESTAMPTZ: Vaqt mintaqasini hisobga oluvchi vaqt formati (server vaqtini to‘g‘ri saqlaydi).

ON DELETE RESTRICT: Agar bog‘liq jadvalda (products) ma’lumot bo‘lsa, asosiy jadval (categories) qatorini o‘chirishga ruxsat bermaydi.

ON DELETE CASCADE: Agar mijoz (customer) o‘chirilsa, uning barcha buyurtmalari (orders) avtomatik ravishda bazadan o‘chiriladi.

Multi-column UNIQUE: UNIQUE(passport_series, passport_number) kombinatsiyasi bir kishi bir xil pasport ma'lumotlari bilan ikki marta ro'yxatdan o'tishini oldini oladi.
