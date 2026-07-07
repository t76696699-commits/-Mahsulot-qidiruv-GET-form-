🏗️ Flask-Migrate Ishlash Mexanizmi
Flask-Migrate orqasida Alembic dvigateli turadi. U ma'lumotlar bazasi sxemasidagi o'zgarishlarni ketma-ketlik (tarix) ko'rinishida saqlaydi. Har bir migratsiya fayli o'zidan oldingi migratsiyaga (down_revision) va o'zining unikal identifikatoriga (revision) ega bo'ladi.

1. Migratsiya Fayllari Ierarxiyasi
Sizning holatingizda migrations/versions/ papkasida ikkita asosiy fayl zanjir hosil qiladi:

1-Migratsiya (Initial Tables):

Revision ID: a1b2c3d4e5f6 (ixtiyoriy)

Down Revision: None (boshlang'ich nuqta)

Tarkib: User, Note, Tag, va bog'lovchi note_tags jadvallarini yaratish (op.create_table).

2-Migratsiya (Add is_pinned to notes):

Revision ID: f6e5d4c3b2a1

Down Revision: a1b2c3d4e5f6 (1-migratsiyaga bog'langan)

Tarkib: notes jadvaliga is_pinned ustunini qo'shish (op.add_column).

💻 CLI Komandalari va Diagnostika
README faylida aks etishi kerak bo'lgan va terminalda holatni tekshirish uchun ishlatiladigan buyruqlar:

Joriy Holatni Tekshirish
Bash
flask db current
Kutilayotgan natija: Agar bazangiz eng oxirgi o'zgarishlar bilan yangilangan bo'lsa, terminalda 2-migratsiyaning Revision IDsi va uning yonida (head) yozuvi chiqadi.

Migratsiyalar Tarixi
Bash
flask db history
Kutilayotgan natija:

Plaintext
a1b2c3d4e5f6 -> f6e5d4c3b2a1 (head), add is_pinned to notes
None -> a1b2c3d4e5f6, initial tables
Baza Bilan Ishlash Komandalari
flask db init: Loyihada birinchi marta migratsiya muhitini (papkani) yaratish.

flask db migrate -m "xabar": Modellardagi o'zgarishlarni aniqlab, yangi migratsiya faylini yaratish.

flask db upgrade: Yaratilgan migratsiyalarni ma'lumotlar bazasiga qo'llash (Sxemani yangilash).

flask db downgrade: Oxirgi qo'llangan migratsiyani orqaga qaytarish (Masalan, is_pinned ustunini bazadan o'chirish).

📌 Biznes Mantiq: "is_pinned" Bo'yicha Saralash
Bosh sahifada qatirilgan (pinned) qaydlarni yuqorida, qolganlarini esa yaratilgan vaqti bo'yicha ko'rsatish uchun Flask-SQLAlchemy orqali so'rovni (query) quyidagicha yozish kerak:

Python
from models import Note

@app.route('/')
def index():
    # Avval is_pinned=True bo'lganlar, keyin esa id yoki yaratilgan vaqti bo'yicha saralash
    notes = Note.query.order_by(Note.is_pinned.desc(), Note.id.desc()).all()
    return render_template('index.html', notes=notes)
