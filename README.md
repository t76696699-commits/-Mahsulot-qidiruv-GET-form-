# -Mahsulot-qidiruv-GET-form-
Ushbu talablarga to‘liq javob beradigan va xavfsizlik qoidalariga rioya qilingan (secret_key himoyalangan) Flask loyihasini tayyorladim.

[file-tag: code-generated-file-0-1782977465355605929]

Kodning asosiy jihatlari:
Secret Key himoyasi: secret_key kod ichida yozilmagan, u muhit o'zgaruvchisidan (Environment Variable) o'qiladi. Bu GitHub'ga kodni yuklaganda xavfsizligini ta'minlaydi.

Session boshqaruvi: Login qilgan foydalanuvchi session['username'] ga saqlanadi.

Decorator (@login_required): Maxsus funksiya yordamida /dashboard sahifasini login qilmaganlar uchun yopdik. Agar foydalanuvchi session'siz kirsa, avtomatik /login ga yo'naltiriladi.

Logout: session.pop('username', None) orqali sessiya tozalanadi va foydalanuvchi tizimdan chiqib ketadi.

Qanday ishlatiladi?
Faylni yuklab oling va oching.

Terminalda export SECRET_KEY='sizning_maxfiy_kalitingiz' deb yozing (Windows'da set SECRET_KEY=...).

python app.py ni ishga tushiring.

Bu yondashuv professional darajadagi xavfsizlik standartlariga javob beradi. Yana biror o'zgartirish kerakmi?
