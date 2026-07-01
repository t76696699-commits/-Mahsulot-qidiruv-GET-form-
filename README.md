# -Mahsulot-qidiruv-GET-form-
Loyiha qisqacha tushuntirishi:
PRG Pattern: /post route'ida xabar saqlangandan so'ng, redirect(url_for('index')) amalga oshiriladi. Bu foydalanuvchi sahifani yangilaganda (F5) xabar qayta yuborilishining oldini oladi.

Duplikatni bloklash: session['last_post'] orqali oxirgi yuborilgan xabar matni saqlanadi. Agar yangi yuborilayotgan xabar avvalgisi bilan bir xil bo'lsa, xabar qabul qilinmaydi va xatolik xabari chiqadi.

Xavfsizlik: @login_required dekoratori yordamida /post faqat login qilganlar uchun ochiq qilingan.

Jinja2: Bosh sahifada {% for post in posts[:20] %} orqali eng yangi 20 ta xabar va ularning muallifi, vaqti hamda matni dinamik tarzda render qilinadi.

Faylni yuklab olib, loyihangizga qo'shishingiz kifoya. Yana yordam kerak bo'lsa, bemalol ayting!
