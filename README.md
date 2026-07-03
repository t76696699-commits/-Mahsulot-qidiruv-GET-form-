1. Sxemani yaratish
SQL
-- 1. Foydalanuvchilar
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL
);

-- 2. Postlar
CREATE TABLE posts (
    post_id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(user_id) ON DELETE RESTRICT,
    title TEXT NOT NULL
);

-- 3. Izohlar (CASCADE misoli)
CREATE TABLE comments (
    comment_id SERIAL PRIMARY KEY,
    post_id INT REFERENCES posts(post_id) ON DELETE CASCADE,
    content TEXT NOT NULL
);

-- 4. Teglar
CREATE TABLE tags (
    tag_id SERIAL PRIMARY KEY,
    tag_name VARCHAR(50) UNIQUE NOT NULL
);

-- 5. Bog'lovchi jadval (M:N munosabat)
CREATE TABLE post_tags (
    post_id INT REFERENCES posts(post_id),
    tag_id INT REFERENCES tags(tag_id),
    PRIMARY KEY (post_id, tag_id)
);

-- FK ustunlariga 5 ta indeks (PostgreSQL avtomatik qilmaydi)
CREATE INDEX idx_posts_user_id ON posts(user_id);
CREATE INDEX idx_comments_post_id ON comments(post_id);
CREATE INDEX idx_post_tags_post_id ON post_tags(post_id);
CREATE INDEX idx_post_tags_tag_id ON post_tags(tag_id);
CREATE INDEX idx_users_username ON users(username);
2. Tranzaksiya ichida test ma'lumotlar
SQL
BEGIN;
INSERT INTO users (username) VALUES ('Ali'), ('Vali');
INSERT INTO posts (user_id, title) VALUES (1, 'SQL darslari'), (2, 'PostgreSQL asoslari');
INSERT INTO comments (post_id, content) VALUES (1, 'Zo''r post!'), (1, 'Tushunarli');
INSERT INTO tags (tag_name) VALUES ('dasturlash'), ('db');
INSERT INTO post_tags (post_id, tag_id) VALUES (1, 1), (1, 2), (2, 2);
COMMIT;
3. Hisobotlar (Queries)
1. Eng faol foydalanuvchi:

SQL
SELECT u.username, COUNT(p.post_id) as post_count
FROM users u JOIN posts p ON u.user_id = p.user_id
GROUP BY u.username ORDER BY post_count DESC LIMIT 1;
2. Eng mashhur teg:

SQL
SELECT t.tag_name, COUNT(pt.post_id) as usage_count
FROM tags t JOIN post_tags pt ON t.tag_id = pt.tag_id
GROUP BY t.tag_name ORDER BY usage_count DESC LIMIT 1;
3. Har postning izohlari (EXPLAIN ANALYZE bilan):

SQL
EXPLAIN ANALYZE 
SELECT p.title, c.content 
FROM posts p LEFT JOIN comments c ON p.post_id = c.post_id;
4. 0 izohli postlar:

SQL
SELECT title FROM posts p 
LEFT JOIN comments c ON p.post_id = c.post_id 
WHERE c.comment_id IS NULL;
4. ON DELETE RESTRICT vs CASCADE
ON DELETE RESTRICT (posts jadvali): Agar biror foydalanuvchining posti bo'lsa, foydalanuvchini bazadan o'chirib bo'lmaydi. Bu ma'lumotlar yaxlitligini saqlash uchun "xatolik" qaytaradi. Foydalanuvchini o'chirishdan oldin avval uning postlarini o'chirishingiz shart.

ON DELETE CASCADE (comments jadvali): Agar post o'chirilsa, unga tegishli barcha izohlar ham avtomatik ravishda bazadan o'chiriladi. Bu ma'lumotlarni tozalash jarayonini avtomatlashtiradi va "yetim" (orphan) ma'lumotlar qolishining oldini oladi.
