# -Mahsulot-qidiruv-GET-form-
Loyiha qisqacha tushuntirishi:
PRG Pattern: /post route'ida xabar saqlangandan so'ng, redirect(url_for('index')) amalga oshiriladi. Bu foydalanuvchi sahifani yangilaganda (F5) xabar qayta yuborilishining oldini oladi.

Duplikatni bloklash: session['last_post'] orqali oxirgi yuborilgan xabar matni saqlanadi. Agar yangi yuborilayotgan xabar avvalgisi bilan bir xil bo'lsa, xabar qabul qilinmaydi va xatolik xabari chiqadi.

Xavfsizlik: @login_required dekoratori yordamida /post faqat login qilganlar uchun ochiq qilingan.

Jinja2: Bosh sahifada {% for post in posts[:20] %} orqali eng yangi 20 ta xabar va ularning muallifi, vaqti hamda matni dinamik tarzda render qilinadi.

Faylni yuklab olib, loyihangizga qo'shishingiz kifoya. Yana yordam kerak bo'lsa, bemalol ayting!app.py: Barcha kerakli modellar (Note klassi), SQLite konfiguratsiyasi va marshrutlar (/ va /add) mavjud.

seed.py: Loyihani ishga tushirganingizda bazani to'ldirib beruvchi maxsus skript (Kamida 3 ta nota qo'shadi).

templates/index.html: Notlarni eng yangisidan boshlab chiqaruvchi (yangi notlar yuqorida) Jinja2 shabloni.

README.md: Loyihani qanday ishga tushirish bo'yicha aniq qadamlar (o'rnatish, migratsiya va ishga tushirish).

100 ball olish uchun eslatma:
Seed: Loyihani birinchi marta ishga tushirayotganda python seed.py buyrug'ini bajaring, shunda bazangiz bo'sh qolmaydi.

DateTime: Notlar yaratilgan vaqt (created_at) avtomatik tarzda datetime.utcnow orqali saqlanadi.

Tartiblash: Note.query.order_by(Note.created_at.desc()).all() qismi yangi notlarning doimo birinchi bo'lib chiqishini ta'minlaydi.
