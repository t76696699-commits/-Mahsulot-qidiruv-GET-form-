# -Mahsulot-qidiruv-GET-form-
Loyiha yechimining asosiy jihatlari:
Bog‘liqlik (Relationship): Note modelida db.ForeignKey('user.id') va User modelida db.relationship('Note', backref='author', cascade='all, delete-orphan') ishlatilgan. Bu foydalanuvchi o‘chirilganda uning barcha notalari avtomatik o‘chirilishini ta'minlaydi.

Xavfsizlik (403 Forbidden): Edit va Delete funksiyalarida:

Python
if note.user_id != session.get('user_id'):
    abort(403)
Bu kod foydalanuvchiga boshqa birovning notasini o‘zgartirish yoki o‘chirishga mutlaqo yo‘l qo‘ymaydi.

Filtrlash: /notes sahifasida faqat filter_by(user_id=session['user_id']) orqali login bo'lgan foydalanuvchining shaxsiy notalari ko'rsatiladi.

PRG Pattern: Barcha POST so‘rovlardan (yaratish, yangilash, o‘chirish) keyin redirect() ishlatilib, flash() xabarlari bilan foydalanuvchiga holat haqida ma'lumot beriladi.

Ishga tushirish:
Loyihani yuklab oling.

python orqali ishga tushiring.

Avval /login ga o'tib, foydalanuvchi nomini kiriting (yangi user bo'lsa, bazaga qo'shiladi).

So'ngra /notes orqali CRUD operatsiyalarini sinab ko'ring.

Yana biron-bir qismni tushuntirish kerakmi yoki loyiha tayyormi?
