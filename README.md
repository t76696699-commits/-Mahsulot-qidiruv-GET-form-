Mana, Flask ilovangiz uchun barcha talablar bajarilgan, to'liq va xavfsiz fayl yuklash tizimi kodi. Proyekt strukturasi static elementlarni yuklash papkasidan ajratgan holda tuzildi.

1. Papkalar strukturasi va .gitignore
Loyihangiz ildiz (root) papkasida uploads/ nomli katalog oching va uning ichidagilar Git-ga tushib ketmasligi uchun .gitignore fayliga quyidagi qatorni qo'shing:

Plaintext
# .gitignore
uploads/
2. Flask Ilovasi va Konfiguratsiya (app.py)
Kodingizda imghdr moduli o'rniga zamonaviy va xavfsizroq puremagic yoki filetype kutubxonalaridan foydalanish tavsiya etiladi (chunki imghdr Python 3.11 dan boshlab deprecated bo'lgan va Python 3.13 da butunlay olib tashlangan). Biroq, talabga binoan imghdr mantiqini quyida keltiraman.

Python
import os
import uuid
import imghdr
from flask import Flask, render_template, request, send_from_directory, abort
from flask_sqlalchemy import SQLAlchemy
from flask_wtf import FlaskForm
from flask_wtf.file import FileField, FileRequired, FileAllowed
from werkzeug.utils import secure_filename

app = Flask(__name__)
app.config['SECRET_KEY'] = 'sizning_maxfiy_kalitingiz'

# Yuklash papkasi static dan tashqarida
UPLOAD_FOLDER = os.path.join(app.root_path, 'uploads')
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER

# Fayl hajmi cheklovi: 5MB (5 * 1024 * 1024 bayt)
app.config['MAX_CONTENT_LENGTH'] = 5 * 1024 * 1024

# Ma'lumotlar bazasi konfiguratsiyasi
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db'
db = SQLAlchemy(app)

# Papka mavjudligini tekshirish
if not os.path.exists(UPLOAD_FOLDER):
    os.makedirs(UPLOAD_FOLDER)

# -------------------------------------------------------------
# MIGRATION VA MODEL
# -------------------------------------------------------------
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    avatar = db.Column(db.String(255), nullable=True)  # Avatar ustuni (String 255)

# DIQQAT: Flask-Migrate orqali realizatsiya qilish uchun terminalda:
# flask db init -> flask db migrate -m "add avatar to user" -> flask db upgrade

# -------------------------------------------------------------
# FORMA VA VALIDATORLAR
# -------------------------------------------------------------
class AvatarForm(FlaskForm):
    avatar = FileField('Profil rasmi', validators=[
        FileRequired(message="Fayl tanlanmagan!"),
        FileAllowed(['jpg', 'jpeg', 'png', 'gif'], message="Faqat rasm formatlari ruxsat etilgan (jpg, png, gif)!")
    ])

# -------------------------------------------------------------
# YORDAMCHI FUNKSIYA (Fayl turini ichki tekshirish)
# -------------------------------------------------------------
def validate_image_stream(file_stream):
    """Faylning haqiqiy kontent turini (imghdr) orqali tekshirish"""
    header = file_stream.read(512)  # Dastlabki baytlarni o'qiymiz
    file_stream.seek(0)             # Stream ko'rsatkichini boshiga qaytaramiz
    
    format_type = imghdr.what(None, header)
    if not format_type:
        return None
    # imghdr 'jpeg' qaytaradi, biz ruxsat bergan formatlarga moslaymiz
    return format_type if format_type != 'jpeg' else 'jpg'

# -------------------------------------------------------------
# MARSHRUTLAR (ROUTES)
# -------------------------------------------------------------

@app.route('/user/upload', methods=['GET', 'POST'])
def upload_avatar():
    form = AvatarForm()
    if form.validate_on_submit():
        file = form.avatar.data
        
        # 1. Imghdr yordamida ichki tur tekshiruvi (Mime-type spoofing oldini olish)
        detected_type = validate_image_stream(file.stream)
        allowed_types = ['jpg', 'jpeg', 'png', 'gif']
        
        if not detected_type or detected_type not in allowed_types:
            return "Xatolik: Fayl kengaytmasi rasm bo'lsa-da, uning ichki tuzilishi rasm emas!", 400

        # 2. secure_filename + uuid.uuid4().hex bilan xavfsiz va noyob nom yaratish
        original_filename = secure_filename(file.filename)
        extension = os.path.splitext(original_filename)[1]
        unique_filename = f"{uuid.uuid4().hex}{extension}"
        
        # Faylni saqlash
        file_path = os.path.join(app.config['UPLOAD_FOLDER'], unique_filename)
        file.save(file_path)
        
        # 3. Modelni bazaga saqlash (Misol tariqasida 1-id li userga bog'laymiz)
        user = User.query.get(1) # Haqiqiy loyihada current_user ishlatiladi
        if user:
            user.avatar = unique_filename
            db.session.commit()
            
        return f"Fayl muvaffaqiyatli yuklandi: {unique_filename}"
        
    return render_template('upload.html', form=form)

# Faylni tashqariga xizmat qildirish (send_from_directory)
@app.route('/user/uploads/<filename>')
def serve_avatar(filename):
    return send_from_directory(app.config['UPLOAD_FOLDER'], filename)

# -------------------------------------------------------------
# ERROR HANDLERS (413 Too Large)
# -------------------------------------------------------------
@app.errorhandler(413)
def request_entity_too_large(error):
    return render_template('errors/413.html'), 413

if __name__ == '__main__':
    with app.app_context():
        db.create_all()  # Test uchun bazani yaratish
    app.run(debug=True)
3. HTML Shablonlar (Templates)
templates/upload.html
Shablonda albatta enctype="multipart/form-data" atributi bo'lishi shart, aks holda fayl serverga yuborilmaydi.

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>Avatar Yuklash</title>
</head>
<body>
    <h2>Profil avatarini yuklash</h2>
    
    <form method="POST" enctype="multipart/form-data">
        {{ form.hidden_tag() }}
        
        <div>
            {{ form.avatar.label }}<br>
            {{ form.avatar() }}
            {% if form.avatar.errors %}
                {% for error in form.avatar.errors %}
                    <span style="color: red;">{{ error }}</span>
                {% endfor %}
            {% endif %}
        </div>
        
        <br>
        <button type="submit">Yuklash</button>
    </form>
</body>
</html>
templates/errors/413.html
Fayl hajmi 5MB dan oshib ketganda chiroyli xato sahifasini ko'rsatish uchun:

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>Fayl o'lchami juda katta</title>
    <style>
        body { text-align: center; font-family: sans-serif; padding-top: 100px; background-color: #f8f9fa; }
        .container { max-width: 500px; margin: 0 auto; background: white; padding: 40px; border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        h1 { color: #dc3545; }
        p { color: #6c757d; }
        a { display: inline-block; margin-top: 20px; color: #007bff; text-decoration: none; }
    </style>
</head>
<body>
    <div class="container">
        <h1>413 - Fayl hajmi juda katta!</h1>
        <p>Kechirasiz, siz yuklamoqchi bo'lgan fayl hajmi ruxsat etilgan cheklovdan (5 MB) oshib ketdi.</p>
        <a href="/user/upload">Orqaga qaytish</a>
    </div>
</body>
</html>
