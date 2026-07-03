1. Subquery turlari
Scalar (bitta qiymat qaytaradi):

SQL
SELECT title, (SELECT COUNT(*) FROM comments) as total_comments FROM posts;
IN (ro‘yxat bilan tekshirish):

SQL
SELECT title FROM posts WHERE user_id IN (SELECT user_id FROM users WHERE username = 'Ali');
EXISTS (mavjudligini tekshirish - tezroq ishlaydi):

SQL
SELECT title FROM posts p WHERE EXISTS (SELECT 1 FROM comments c WHERE c.post_id = p.post_id);
2. CTE (Common Table Expressions)
CTE so‘rovni qismlarga bo‘lib, o‘qishni osonlashtiradi.

Bir nechta CTE:

SQL
WITH user_stats AS (
    SELECT user_id, COUNT(*) as p_count FROM posts GROUP BY user_id
),
active_users AS (
    SELECT * FROM user_stats WHERE p_count > 5
)
SELECT * FROM active_users;
Recursive CTE (1 dan 10 gacha sonlar):

SQL
WITH RECURSIVE counter(n) AS (
    VALUES (1)
    UNION ALL
    SELECT n + 1 FROM counter WHERE n < 10
)
SELECT * FROM counter;
3. Window Functions (Oyna funksiyalari)
Dastlab, test jadvalini tasavvur qilamiz: students (name, class, score).

ROW_NUMBER (har sinfdan eng yuqori natijali 1 ta):

SQL
SELECT * FROM (
    SELECT name, class, score, ROW_NUMBER() OVER(PARTITION BY class ORDER BY score DESC) as rn
    FROM students
) t WHERE rn = 1;
RANK vs DENSE_RANK (farqi):
RANK bo'shliq qoldiradi, DENSE_RANK esa yo'q.

SQL
SELECT name, score, 
       RANK() OVER(ORDER BY score DESC) as rnk,
       DENSE_RANK() OVER(ORDER BY score DESC) as drnk
FROM students;
LAG (oldingi natija bilan taqqoslash):

SQL
SELECT name, score, LAG(score) OVER(ORDER BY score) as prev_score
FROM students;
Kumulyativ SUM va Foiz hisoblash:

SQL
SELECT name, score,
       SUM(score) OVER(ORDER BY name) as running_total,
       (score * 100.0 / SUM(score) OVER()) as percentage
FROM students;
