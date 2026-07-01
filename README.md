# -Mahsulot-qidiruv-GET-form-
Ushbu loyiha uchun CRUD (Create, Read, Update, Delete) operatsiyalarini to'liq qamrab olgan, PRG (Post/Redirect/Get) patterniga amal qiluvchi va xavfsiz Flask dasturini tayyorladim.

[file-tag: code-generated-file-0-1783073356801980846]

Loyiha qisqacha tushuntirishi:
CRUD funksionalligi:

Create: /notes/new orqali ma'lumot kiritiladi.

Read: Bosh sahifada ro'yxat va /notes/<id> da alohida nota ko'rinishi.

Update: /notes/<id>/edit orqali mavjud ma'lumot tahrirlanadi.

Delete: /notes/<id>/delete orqali o'chiriladi.

404 Xatolik: get_or_404() funksiyasi yordamida mavjud bo'lmagan ID uchun avtomatik 404 sahifasi qaytariladi.

PRG Pattern va Flash: Har bir POST so'rovdan so'ng redirect() ishlatilgan va flash() orqali foydalanuvchiga muvaffaqiyatli amal bajarilgani haqida xabar beriladi.

Jinja2 Templates: HTML shablonlari bazadagi ma'lumotlarni dinamik ko'rsatishga moslangan.
