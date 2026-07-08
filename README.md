Siz so'ragan xavfsiz autentifikatsiya tizimi va User modelining kengaytirilgan ko'rinishini quyida taqdim etaman. Bu safar parollarni xavfsiz heshlash uchun werkzeug.security kutubxonasidan foydalanamiz va barcha validatsiya qoidalarini amalda qo'llaymiz.

1. Modelni Yangilash (app/models.py)
User modeliga password_hash ustuni hamda parolni heshlash va tekshirish metodlari qo'shildi:

Python
from app.extensions import db
from datetime import datetime
from werkzeug.security import generate_password_hash, check_password_hash

# Many-to-Many va boshqa modellar o'zgarishsiz qoladi...

class User(db.Model):
    __tablename__ = 'users'
    
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(64), unique=True, nullable=False, index=True)
    email = db.Column(db.String(120), unique=True, nullable=False, index=True)
    password_hash = db.Column(db.String(255), nullable=False) # Yangi ustun

    items = db.relationship('Item', backref='author', lazy='dynamic', cascade='all, delete-orphan')

    # Parolni heshlab saqlash metodi
    def set_password(self, password):
        self.password_hash = generate_password_hash(password)

    # Kiritilgan parolni tekshirish metodi
    def check_password(self, password):
        return check_password_hash(self.password_hash, password)

    def __repr__(self):
        return f'<User {self.username}>'
2. Flask-Migrate orqali ma'lumotlar bazasini yangilash
Modelga yangi ustun qo'shilganidan so'ng, terminalda quyidagi buyruqlarni ketma-ket bajaring:

Bash
# 1. Yangi ustun qo'shilganini aniqlab, migratsiya faylini yaratish
flask db migrate -m "add password_hash to users"

# 2. O'zgarishlarni bazaga qo'llash
flask db upgrade
3. Autentifikatsiya Blueprint'i (app/blueprints/auth/)
Loyiha arxitekturasini toza saqlash uchun yangi auth_bp blueprint'ini yaratamiz.

app/blueprints/auth/__init__.py

Python
from flask import Blueprint

auth_bp = Blueprint('auth', __name__, url_prefix='/auth')

from . import routes
app/blueprints/auth/routes.py (Validatsiya va Ro'yxatdan o'tish)

Python
from flask import render_template, redirect, url_for, flash, request
from app.blueprints.auth import auth_bp
from app.extensions import db
from app.models import User
import re

@auth_bp.route('/register', methods=['GET', 'POST'])
def register():
    if request.method == 'POST':
        username = request.form.get('username', '').strip()
        email = request.form.get('email', '').strip()
        password = request.form.get('password', '')

        # 1. Validatsiya: Majburiy maydonlar tekshiruvi
        if not username or not email or not password:
            flash("Barcha maydonlarni to'ldirish majburiy!", "danger")
            return render_template('auth/register.html')

        # 2. Validatsiya: Parol uzunligi kamida 8 ta belgi bo'lishi shart
        if len(password) < 8:
            flash("Parol uzunligi kamida 8 ta belgi bo'lishi kerak!", "danger")
            return render_template('auth/register.html')

        # Simple Email format check (ixtiyoriy lekin tavsiya etiladi)
        if not re.match(r"[^@]+@[^@]+\.[^@]+", email):
            flash("Noto'g'ri elektron pochta manzili!", "danger")
            return render_template('auth/register.html')

        # 3. Unique tekshiruvi: Username bandligini tekshirish
        if User.query.filter_by(username=username).first():
            flash("Bu username allaqachon band!", "danger")
            return render_template('auth/register.html')

        # 4. Unique tekshiruvi: Email bandligini tekshirish
        if User.query.filter_by(email=email).first():
            flash("Bu email bilan allaqachon ro'yxatdan o'tilgan!", "danger")
            return render_template('auth/register.html')

        # Hammasi yaxshi bo'lsa, yangi foydalanuvchini yaratamiz
        new_user = User(username=username, email=email)
        new_user.set_password(password) # Parol heshlanadi

        db.session.add(new_user)
        db.session.commit()

        flash("Ro'yxatdan muvaffaqiyatli o'tdingiz! Endi tizimga kirishingiz mumkin.", "success")
        return redirect(url_for('main.index'))

    return render_template('auth/register.html')
4. Blueprint'ni Factory'da ro'yxatdan o'tkazish (app/__init__.py)
Yaratgan auth_bp blueprint'imizni factory funksiyasi ichiga qo'shib qo'yamiz:

Python
def create_app(config_name='dev'):
    app = Flask(__name__)
    app.config.from_object(config_by_name[config_name])

    db.init_app(app)
    migrate.init_app(app, db)

    # Eski blueprintlar...
    from app.blueprints.main import main_bp
    from app.blueprints.items import items_bp
    app.register_blueprint(main_bp)
    app.register_blueprint(items_bp, url_prefix='/items')

    # YANGI: Auth blueprint'ni ulash
    from app.blueprints.auth import auth_bp
    app.register_blueprint(auth_bp)

    return app
5. Ro'yxatdan o'tish Sahifasi (app/templates/auth/register.html)
HTML
{% extends 'base.html' %}
{% block content %}
<div class="row justify-content-center">
    <div class="col-md-6">
        <div class="card shadow-sm">
            <div class="card-header bg-primary text-white">
                <h4 class="mb-0">Ro'yxatdan o'tish</h4>
            </div>
            <div class="card-body">
                <form method="POST" action="{{ url_for('auth.register') }}">
                    <div class="mb-3">
                        <label class="form-label">Foydalanuvchi nomi (Username)</label>
                        <input type="text" name="username" class="form-control" required>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Elektron pochta (Email)</label>
                        <input type="email" name="email" class="form-control" required>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Parol (kamida 8 ta belgi)</label>
                        <input type="password" name="password" class="form-control" minlength="8" required>
                    </div>
                    <button type="submit" class="btn btn-primary w-100">Ro'yxatdan o'tish</button>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
6. Flask Shell orqali Metodlarni Test Qilish
Tizim ishlashini va parollar to'g'ri heshlanayotganini terminalda flask shell orqali tekshirib ko'ramiz:

Bash
$ flask shell
Aktivlashgan shell muhitida quyidagi Python kodlarini yozib test qiling:

Python
>>> from app.extensions import db
>>> from app.models import User

# 1. Bazadagi birinchi foydalanuvchini olish
>>> user = User.query.first()
>>> print(user.username)
'test_user'

# 2. Noto'g'ri parol bilan tekshirish (False qaytishi kerak)
>>> user.check_password('notogri-parol')
False

# 3. To'g'ri parol bilan tekshirish (True qaytishi kerak)
# Eslatma: Agar foydalanuvchini oldingi modulda create_route orqali parolsiz yaratgan bo'lsangiz,
# avval unga set_password() qilib oling:
>>> user.set_password('my_secret_password_123')
>>> db.session.commit()

# Endi qayta tekshiramiz:
>>> user.check_password('my_secret_password_123')
True

# 4. Parol bazada ochiq holda emas, chalkash hesh holatida saqlanganini ko'rish:
>>> user.password_hash
'scrypt:32768:8:1$vXb8J...'
