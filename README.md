1. Loyiha Strukturasi
Emaillar to'g'ri render qilinishi uchun loyiha papkasini quyidagicha tashkil qiling:

Plaintext
project/
│
├── templates/
│   └── email/
│       ├── welcome.html
│       ├── welcome.txt
│       ├── reset.html
│       └── reset.txt
│
├── .env
└── app.py
2. Email Shablonlari (templates/email/)
welcome.txt
Plaintext
Salom {{ username }},

Kompaniyamizga xush kelibsiz! Ro'yxatdan o'tganingiz uchun rahmat.
welcome.html
HTML
<!DOCTYPE html>
<html>
<body>
    <h2>Salom, {{ username }}!</h2>
    <p>Kompaniyamizga xush kelibsiz! Ro'yxatdan o'tganingiz uchun rahmat.</p>
</body>
</html>
reset.txt
Plaintext
Salom,

Parolni tiklash uchun quyidagi havolaga o'ting (Havola 1 soat davomida faol bo'ladi):
{{ url }}

Agar bu so'rovni siz yubormagan bo'lsangiz, ushbu xatni e'tiborsiz qoldiring.
reset.html
HTML
<!DOCTYPE html>
<html>
<body>
    <h2>Parolni tiklash so'rovi</h2>
    <p>Parolni tiklash uchun quyidagi tugmani bosing (Havola 1 soat davomida faol bo'ladi):</p>
    <p>
        <a href="{{ url }}" style="background-color: #4CAF50; color: white; padding: 10px 20px; text-decoration: none; display: inline-block;">
            Parolni yangilash
        </a>
    </p>
    <p>Yoki quyidagi havoladan foydalaning:</p>
    <p>{{ url }}</p>
</body>
</html>
3. Asosiy Kod (app.py)
Python
import os
from threading import Thread
from dotenv import load_dotenv
from flask import Flask, request, jsonify, render_template, url_for, flash, redirect
from flask_sqlalchemy import SQLAlchemy
from flask_mail import Mail, Message
from itsdangerous import URLSafeTimedSerializer, SignatureExpired, BadTimeSignature
from werkzeug.security import generate_password_hash

# .env yuklash
load_dotenv()

app = Flask(__name__)
app.config['SECRET_KEY'] = os.getenv('SECRET_KEY', 'default-secret-key-123')

# Database konfiguratsiyasi
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///auth.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
db = SQLAlchemy(app)

# Flask-Mail konfiguratsiyasi (.env dan)
app.config['MAIL_SERVER'] = os.getenv('MAIL_SERVER', 'smtp.gmail.com')
app.config['MAIL_PORT'] = int(os.getenv('MAIL_PORT', 587))
app.config['MAIL_USE_TLS'] = os.getenv('MAIL_USE_TLS', 'True').lower() in ['true', 'on', '1']
app.config['MAIL_USERNAME'] = os.getenv('MAIL_USERNAME')
app.config['MAIL_PASSWORD'] = os.getenv('MAIL_PASSWORD')
app.config['MAIL_DEFAULT_SENDER'] = os.getenv('MAIL_DEFAULT_SENDER')
app.config['MAIL_SUPPRESS_SEND'] = os.getenv('MAIL_SUPPRESS_SEND', 'False').lower() in ['true', 'on', '1']

mail = Mail(app)

# itsdangerous serializer (Token generatsiya va validatsiya uchun)
serializer = URLSafeTimedSerializer(app.config['SECRET_KEY'])

# Model
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(50), nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    password = db.Column(db.String(200), nullable=False)

# Orqa fonda (Thread) email yuborish funksiyasi
def send_async_email(app_ctx, msg):
    with app_ctx:
        mail.send(msg)

def send_email(subject, recipient, template_path, **kwargs):
    msg = Message(subject, recipients=[recipient])
    
    # HTML va Plain Text versiyalarni render qilish
    msg.body = render_template(f"{template_path}.txt", **kwargs)
    msg.html = render_template(f"{template_path}.html", **kwargs)
    
    # Contextni uzatgan holda Thread ochish
    thr = Thread(target=send_async_email, args=[app.app_context(), msg])
    thr.start()
    return thr

# 1. Ro'yxatdan o'tish (Register) va Welcome Email
@app.route('/auth/register', methods=['POST'])
def register():
    data = request.get_json() or {}
    username = data.get('username')
    email = data.get('email')
    password = data.get('password')

    if User.query.filter_by(email=email).first():
        return jsonify({'error': 'Email allaqachon mavjud'}), 400

    hashed_pw = generate_password_hash(password)
    user = User(username=username, email=email, password=hashed_pw)
    db.session.add(user)
    db.session.commit()

    # Welcome email yuborish
    send_email(
        subject="Xush kelibsiz!",
        recipient=user.email,
        template_path="email/welcome",
        username=user.username
    )

    return jsonify({'message': 'Muvaffaqiyatli ro\'yxatdan o\'tdingiz. Welcome email yuborildi.'}), 201

# 2. Parol Unutilganda (Forgot Password) — Email Enumeration Himoyasi bilan
@app.route('/auth/forgot', methods=['POST'])
def forgot_password():
    data = request.get_json() or {}
    email = data.get('email')
    
    user = User.query.filter_by(email=email).first()

    # Email enumeration himoyasi: Foydalanuvchi bor yoki yo'qligidan qat'iy nazar bir xil javob qaytadi
    if user:
        # Token yaratish (salt orqali xavfsizlik kuchaytiriladi)
        token = serializer.dumps(user.email, salt='password-reset-salt')
        reset_url = url_for('reset_password', token=token, _external=True)
        
        # Reset email yuborish
        send_email(
            subject="Parolni tiklash",
            recipient=user.email,
            template_path="email/reset",
            url=reset_url
        )

    return jsonify({'message': 'Agar ushbu email tizimda mavjud bo\'lsa, parolni tiklash havolasi yuborildi.'}), 200

# 3. Tokenni tekshirish va Yangi Parol o'rnatish
@app.route('/auth/reset/<token>', methods=['GET', 'POST'])
def reset_password(token):
    try:
        # max_age=3600 soniya (1 soat) amal qilish muddati
        email = serializer.loads(token, salt='password-reset-salt', max_age=3600)
    except SignatureExpired:
        flash('Parolni tiklash havolasining muddati tugagan (1 soat).', 'danger')
        return jsonify({'error': 'Token muddati tugagan'}), 400
    except BadTimeSignature:
        flash('Yaroqsiz yoki buzilgan havola.', 'danger')
        return jsonify({'error': 'Yaroqsiz token'}), 400

    if request.method == 'POST':
        data = request.get_json() or {}
        new_password = data.get('password')
        
        user = User.query.filter_by(email=email).first()
        if not user:
            return jsonify({'error': 'Foydalanuvchi topilmadi'}), 404

        user.password = generate_password_hash(new_password)
        db.session.commit()
        
        flash('Parolingiz muvaffaqiyatli yangilandi.', 'success')
        return jsonify({'message': 'Parol muvaffaqiyatli yangilandi.'}), 200

    # GET so'rovi uchun (Token haqiqiyligini tekshirish)
    return jsonify({'message': 'Token faol, yangi parolni POST so\'rovi orqali yuboring.', 'email': email}), 200

if __name__ == '__main__':
    with app.app_context():
        db.create_all()
    app.run(debug=True)
4. .env Fayli Namunasi
Фрагмент кода
SECRET_KEY=super-maxfiy-kalit-soz
MAIL_SERVER=smtp.gmail.com
MAIL_PORT=587
MAIL_USE_TLS=True
MAIL_USERNAME=sizning_emailingiz@gmail.com
MAIL_PASSWORD=app_parolingiz_gmaildan
MAIL_DEFAULT_SENDER=sizning_emailingiz@gmail.com
MAIL_SUPPRESS_SEND=False
5. Testlash Konfiguratsiyasi (pytest yoki unittest uchun)
Test paytida haqiqiy email ketib qolmasligi va yuborilgan xatlarni tekshirish (assert) uchun MAIL_SUPPRESS_SEND=True va mail.record_messages ishlatiladi:

Python
import pytest
from app import app, db, User, serializer

@pytest.fixture
def client():
    app.config['TESTING'] = True
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///:memory:'
    app.config['MAIL_SUPPRESS_SEND'] = True  # Emaillarni bloklash
    
    with app.test_client() as client:
        with app.app_context():
            db.create_all()
            yield client
            db.drop_all()

def test_forgot_password_sends_email(client):
    # 1. Foydalanuvchi yaratish
    with app.app_context():
        user = User(username="testuser", email="test@example.com", password="hash_password")
        db.session.add(user)
        db.session.commit()

    # 2. Mail konteksini yozib olish (record_messages)
    with app.extensions['mail'].record_messages() as outbox:
        response = client.post('/auth/forgot', json={'email': 'test@example.com'})
        
        assert response.status_code == 200
        # Thread bir oz vaqt olishi mumkin, asinxron testlarda kutish yoki o'sha zaxoti tekshirish:
        assert len(outbox) == 1
        assert outbox[0].subject == "Parolni tiklash"
        assert "test@example.com" in outbox[0].recipients
        assert "password-reset-salt" in outbox[0].body
