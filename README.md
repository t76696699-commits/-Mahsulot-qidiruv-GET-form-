1. Kengaytmalarni yangilash (app/extensions.py)
LoginManager ob'ektini yaratamiz va uni loyihaga qo'shamiz:

Python
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate
from flask_login import LoginManager

db = SQLAlchemy()
migrate = Migrate()
login_manager = LoginManager()

# Himoyalangan sahifaga kirmoqchi bo'lgan anonim foydalanuvchi qaysi manzildan login qilishi kerakligi
login_manager.login_view = 'auth.login'
# Flash xabarning dizayni (Bootstrap klasi)
login_manager.login_message_category = 'warning'
login_manager.login_message = "Ushbu sahifani ko'rish uchun tizimga kirishingiz shart!"
2. User modeliga UserMixin qo'shish va user_loader (app/models.py)
Flask-Login ishlashi uchun User modeli ma'lum bir metodlarga (is_authenticated, is_active va h.k.) ega bo'lishi kerak. Buni UserMixin orqali meros qilib olamiz:

Python
from app.extensions import db, login_manager
from flask_login import UserMixin
from datetime import datetime
from werkzeug.security import generate_password_hash, check_password_hash

@login_manager.user_loader
def load_user(user_id):
    return User.query.get(int(user_id))

class User(db.Model, UserMixin): # UserMixin qo'shildi
    __tablename__ = 'users'
    
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(64), unique=True, nullable=False, index=True)
    email = db.Column(db.String(120), unique=True, nullable=False, index=True)
    password_hash = db.Column(db.String(255), nullable=False)

    items = db.relationship('Item', backref='author', lazy='dynamic', cascade='all, delete-orphan')

    def set_password(self, password):
        self.password_hash = generate_password_hash(password)

    def check_password(self, password):
        return check_password_hash(self.password_hash, password)

    def __repr__(self):
        return f'<User {self.username}>'
3. Application Factory'ni yangilash (app/__init__.py)
login_managerni dasturga ro'yxatdan o'tkazamiz:

Python
from flask import Flask
from app.config import config_by_name
from app.extensions import db, migrate, login_manager # login_manager qo'shildi

def create_app(config_name='dev'):
    app = Flask(__name__)
    app.config.from_object(config_by_name[config_name])

    db.init_app(app)
    migrate.init_app(app, db)
    login_manager.init_app(app) # Initsializatsiya

    # Blueprint'lar...
    from app.blueprints.main import main_bp
    from app.blueprints.auth import auth_bp
    from app.blueprints.items import items_bp

    app.register_blueprint(main_bp)
    app.register_blueprint(auth_bp)
    app.register_blueprint(items_bp, url_prefix='/items')

    return app
4. Auth Route'larini kengaytirish (app/blueprints/auth/routes.py)
Login (Username yoki Email bilan kirish, next va remember me) hamda Logout mantiqlari:

Python
from flask import render_template, redirect, url_for, flash, request
from flask_login import login_user, logout_user, login_required, current_user
from werkzeug.urls import url_parse
from app.blueprints.auth import auth_bp
from app.extensions import db
from app.models import User

@auth_bp.route('/login', methods=['GET', 'POST'])
def login():
    if current_user.is_authenticated:
        return redirect(url_for('main.index'))

    if request.method == 'POST':
        login_input = request.form.get('login_input', '').strip() # Username yoki Email bo'lishi mumkin
        password = request.form.get('password', '')
        remember = True if request.form.get('remember') else False

        # Foydalanuvchini ham username, ham email bo'yicha qidiramiz
        user = User.query.filter((User.username == login_input) | (User.email == login_input)).first()

        if user is None or not user.check_password(password):
            flash("Login yoki parol noto'g'ri!", "danger")
            return redirect(url_for('auth.login'))

        # Tizimga kiritish
        login_user(user, remember=remember)

        # ?next=... parametrini tekshirish (Xavfsiz qaytish)
        next_page = request.args.get('next')
        if not next_page or url_parse(next_page).netloc != '':
            next_page = url_for('main.index')
            
        flash(f"Xush kelibsiz, {user.username}!", "success")
        return redirect(next_page)

    return render_template('auth/login.html')

@auth_bp.route('/logout')
@login_required # Faqat tizimga kirganlar chiqa oladi
def logout():
    logout_user()
    flash("Siz tizimdan chiqdingiz.", "info")
    return redirect(url_for('main.index'))
5. Items Route'larini himoyalash (app/blueprints/items/routes.py)
Yangi e'lon qo'shish sahifasini @login_required bilan yopamiz va avtomatik ravishda current_user.idni biriktiramiz:

Python
from flask import render_template, redirect, url_for, flash, request
from flask_login import login_required, current_user
from app.blueprints.items import items_bp
from app.extensions import db
from app.models import Item, Tag

@items_bp.route('/create', methods=['GET', 'POST'])
@login_required # Himoyalandi!
def create():
    if request.method == 'POST':
        title = request.form.get('title')
        description = request.form.get('description')
        tags_string = request.form.get('tags', '')

        if not title or not description:
            flash("Sarlavha va matn majburiy!", "danger")
            return redirect(url_for('items.create'))

        # Endi e'lon egasi sifatida to'g'ridan-to'g'ri current_user biriktiriladi
        new_item = Item(title=title, description=description, author=current_user)

        if tags_string:
            tag_names = [t.strip().lower() for t in tags_string.split(',') if t.strip()]
            for name in tag_names:
                tag = Tag.query.filter_by(name=name).first()
                if not tag:
                    tag = Tag(name=name)
                    db.session.add(tag)
                new_item.tags.append(tag)

        db.session.add(new_item)
        db.session.commit()
        flash("E'lon muvaffaqiyatli qo'shildi!", "success")
        return redirect(url_for('main.index'))

    return render_template('items/create.html')
6. HTML Shablonlari
Login Formasi (app/templates/auth/login.html)

HTML
{% extends 'base.html' %}
{% block content %}
<div class="row justify-content-center">
    <div class="col-md-5">
        <div class="card shadow-sm">
            <div class="card-header bg-success text-white">
                <h4 class="mb-0">Tizimga kirish</h4>
            </div>
            <div class="card-body">
                <form method="POST">
                    <div class="mb-3">
                        <label class="form-label">Username yoki Email</label>
                        <input type="text" name="login_input" class="form-control" required>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Parol</label>
                        <input type="password" name="password" class="form-control" required>
                    </div>
                    <div class="mb-3 form-check">
                        <input type="checkbox" name="remember" class="form-check-input" id="rememberMe">
                        <label class="form-check-label" for="rememberMe">Meni eslab qol</label>
                    </div>
                    <button type="submit" class="btn btn-success w-100">Kirish</button>
                </form>
                <div class="mt-3 text-center">
                    <small>Akkountingiz yo'qmi? <a href="{{ url_for('auth.register') }}">Ro'yxatdan o'ting</a></small>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}
Bosh Menyuni Dinamik qilish (app/templates/base.html)
current_user.is_authenticated orqali menyu holatini o'zgartiramiz:

HTML
<nav class="navbar navbar-expand-lg navbar-dark bg-dark mb-4">
    <div class="container">
        <a class="navbar-brand" href="{{ url_for('main.index') }}">E'lonlar Doskasi</a>
        <div class="navbar-nav ms-auto">
            <a class="nav-link" href="{{ url_for('items.create') }}">+ Yangi E'lon</a>
            
            {% if current_user.is_authenticated %}
                <span class="navbar-text text-white me-3">
                    Hi, <strong>{{ current_user.username }}</strong>
                </span>
                <a class="btn btn-outline-danger btn-sm text-white" href="{{ url_for('auth.logout') }}">Chiqish</a>
            {% else %}
                <a class="nav-link" href="{{ url_for('auth.login') }}">Kirish</a>
                <a class="nav-link" href="{{ url_for('auth.register') }}">Ro'yxatdan o'tish</a>
            {% endif %}
        </div>
    </div>
</nav>
