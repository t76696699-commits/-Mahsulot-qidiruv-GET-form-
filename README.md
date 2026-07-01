# -Mahsulot-qidiruv-GET-form-
Loyiha yechimining asosiy jihatlari:
Factory Pattern: app/__init__.py ichidagi create_app() funksiyasi ilovaning barcha sozlamalarini, ma'lumotlar bazasini va Blueprint'larni ro'yxatdan o'tkazadi.

Blueprint'lar:

main_bp: Foydalanuvchi autentifikatsiyasi (/login, /logout) va bosh sahifa uchun.

notes_bp: Notalar uchun barcha CRUD operatsiyalari (/notes/new, /notes/<id>/edit va h.k.).

Toza app.py: app.py fayli loyihani ishga tushirish uchun minimal darajaga keltirildi.

O'zaro bog'liqlik: db obyektini app/__init__.py da yaratib, uni modellarda ishlatish orqali "Circular Import" xatolarining oldi olindi.

Ishga tushirish:
Faylni yuklab oling va arxivni oching.

app.py joylashgan papkada terminalni oching.

python app.py buyrug'ini bering.

Ushbu arxitektura "8-dars" talablariga to'liq javob beradi va 100 ball olish uchun zarur bo'lgan barcha modulli tizimni o'z ichiga oladi. Yana biron bir qism bo'yicha yordam kerakmi?
