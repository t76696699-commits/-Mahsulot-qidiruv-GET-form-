Yagona Fayl: app.py
Python
import os
import re
from datetime import datetime
from urllib.parse import urlparse
from functools import wraps
from flask import Flask, Blueprint, render_template_string, request, redirect, url_for, flash, abort, current_app
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate
from flask_login import LoginManager, UserMixin, login_user, logout_user, login_required, current_user
from werkzeug.security import generate_password_hash, check_password_hash

# ==========================================
# 1. KENGAYTMALAR VA INITIALIZATSIYA
# ==========================================
db = SQLAlchemy()
migrate = Migrate()
login_manager = LoginManager()

login_manager.login_view = 'auth.login'
login_manager.login_message_category = 'warning'
login_manager.login_message = "Ushbu sahifani ko'rish uchun tizimga kirishingiz shart!"

@login_manager.user_loader
def load_user(user_id):
    return User.query.get(int(user_id))

# ==========================================
# 2. MODELLAR VA MANY-TO-MANY JADVALI
# ==========================================
item_tags = db.Table('item_tags',
    db.Column('item_id', db.Integer, db.ForeignKey('items.id', ondelete='CASCADE'), primary_key=True),
    db.Column('tag_id', db.Integer, db.ForeignKey('tags.id', ondelete='CASCADE'), primary_key=True)
)

class User(db.Model, UserMixin):
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(64), unique=True, nullable=False, index=True)
    email = db.Column(db.String(120), unique=True, nullable=False, index=True)
    password_hash = db.Column(db.String(255), nullable=False)
    role = db.Column(db.String(20), nullable=False, default='user')

    items = db.relationship('Item', backref='author', lazy='dynamic', cascade='all, delete-orphan')

    def set_password(self, password):
        self.password_hash = generate_password_hash(password)

    def check_password(self, password):
        return check_password_hash(self.password_hash, password)

    def is_admin(self):
        return self.role == 'admin'

class Item(db.Model):
    __tablename__ = 'items'
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100), nullable=False)
    description = db.Column(db.Text, nullable=False)
    created_at = db.Column(db.DateTime, default=datetime.utcnow)
    user_id = db.Column(db.Integer, db.ForeignKey('users.id'), nullable=False)
    
    tags = db.relationship('Tag', secondary=item_tags, backref=db.backref('items', lazy='dynamic'))

class Tag(db.Model):
    __tablename__ = 'tags'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50), unique=True, nullable=False)

# ==========================================
# 3. CUSTOM DEKORATOR (RBAC)
# ==========================================
def admin_required(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        if not current_user.is_authenticated or current_user.role != 'admin':
            abort(403)
        return f(*args, **kwargs)
    return decorated_function

# ==========================================
# 4. BLUEPRINTLAR VA MARSHRUTLAR (ROUTES)
# ==========================================
main_bp = Blueprint('main', __name__)
auth_bp = Blueprint('auth', __name__, url_prefix='/auth')
items_bp = Blueprint('items', __name__, url_prefix='/items')
admin_bp = Blueprint('admin', __name__, url_prefix='/admin')

# --- MAIN BLUEPRINT ---
@main_bp.route('/')
def index():
    page = request.args.get('page', 1, type=int)
    search_query = request.args.get('q', '', type=str)
    
    query = Item.query
    if search_query:
        query = query.filter(Item.title.contains(search_query) | Item.description.contains(search_query))
    
    pagination = query.order_by(Item.created_at.desc()).paginate(page=page, per_page=10, error_out=False)
    return render_template_string(HTML_INDEX, items=pagination.items, pagination=pagination, q=search_query)

@main_bp.route('/tag/<string:name>')
def tag_items(name):
    page = request.args.get('page', 1, type=int)
    tag = Tag.query.filter_by(name=name).first_or_404()
    pagination = tag.items.order_by(Item.created_at.desc()).paginate(page=page, per_page=10, error_out=False)
    return render_template_string(HTML_INDEX, items=pagination.items, pagination=pagination, q='', tag_name=name)

@main_bp.app_errorhandler(403)
def forbidden_error(error):
    return render_template_string(HTML_ERROR, title="403 Taqiqlangan", message="Sizda ushbu sahifaga kirish huquqi yo'q!"), 403

@main_bp.app_errorhandler(401)
def unauthorized_error(error):
    return render_template_string(HTML_ERROR, title="401 Avtorizatsiya", message="Iltimos, avval tizimga kiring!"), 401

# --- AUTH BLUEPRINT ---
@auth_bp.route('/register', methods=['GET', 'POST'])
def register():
    if request.method == 'POST':
        username = request.form.get('username', '').strip()
        email = request.form.get('email', '').strip()
        password = request.form.get('password', '')

        if not username or not email or not password:
            flash("Barcha maydonlarni to'ldiring!", "danger")
            return redirect(url_for('auth.register'))
        if len(password) < 8:
            flash("Parol kamida 8 belgidan iborat bo'lishi kerak!", "danger")
            return redirect(url_for('auth.register'))
        if User.query.filter_by(username=username).first() or User.query.filter_by(email=email).first():
            flash("Username yoki Email allaqachon band!", "danger")
            return redirect(url_for('auth.register'))

        new_user = User(username=username, email=email)
        new_user.set_password(password)
        db.session.add(new_user)
        db.session.commit()
        flash("Ro'yxatdan o'tdingiz, endi tizimga kiring.", "success")
        return redirect(url_for('auth.login'))
    return render_template_string(HTML_REGISTER)

@auth_bp.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        login_input = request.form.get('login_input', '').strip()
        password = request.form.get('password', '')
        remember = True if request.form.get('remember') else False

        user = User.query.filter((User.username == login_input) | (User.email == login_input)).first()
        if user is None or not user.check_password(password):
            flash("Login yoki parol noto'g'ri!", "danger")
            return redirect(url_for('auth.login'))

        login_user(user, remember=remember)
        next_page = request.args.get('next')
        if not next_page or urlparse(next_page).netloc != '':
            next_page = url_for('main.index')
        return redirect(next_page)
    return render_template_string(HTML_LOGIN)

@auth_bp.route('/logout')
@login_required
def logout():
    logout_user()
    flash("Siz tizimdan chiqdingiz.", "info")
    return redirect(url_for('main.index'))

# --- ITEMS BLUEPRINT ---
@items_bp.route('/create', methods=['GET', 'POST'])
@login_required
def create():
    if request.method == 'POST':
        title = request.form.get('title')
        description = request.form.get('description')
        tags_string = request.form.get('tags', '')

        new_item = Item(title=title, description=description, author=current_user)
        if tags_string:
            for name in [t.strip().lower() for t in tags_string.split(',') if t.strip()]:
                tag = Tag.query.filter_by(name=name).first() or Tag(name=name)
                new_item.tags.append(tag)

        db.session.add(new_item)
        db.session.commit()
        flash("E'lon qo'shildi!", "success")
        return redirect(url_for('main.index'))
    return render_template_string(HTML_CREATE_ITEM)

@items_bp.route('/<int:id>/edit', methods=['GET', 'POST'])
@login_required
def edit(id):
    item = Item.query.get_or_404(id)
    if current_user.id != item.user_id and current_user.role != 'admin':
        abort(403)
    if request.method == 'POST':
        item.title = request.form.get('title')
        item.description = request.form.get('description')
        db.session.commit()
        return redirect(url_for('main.index'))
    return render_template_string(HTML_EDIT_ITEM, item=item)

@items_bp.route('/<int:id>/delete', methods=['POST'])
@login_required
def delete(id):
    item = Item.query.get_or_404(id)
    if current_user.id != item.user_id and current_user.role != 'admin':
        abort(403)
    db.session.delete(item)
    db.session.commit()
    return redirect(url_for('main.index'))

# --- ADMIN BLUEPRINT ---
@admin_bp.route('/users', methods=['GET', 'POST'])
@admin_required
def manage_users():
    if request.method == 'POST':
        user = User.query.get_or_404(request.form.get('user_id'))
        user.role = request.form.get('role')
        db.session.commit()
        flash("Rol yangilandi!", "success")
    return render_template_string(HTML_ADMIN_USERS, users=User.query.all())

# ==========================================
# 5. APPLICATION FACTORY
# ==========================================
def create_app():
    app = Flask(__name__)
    app.config['SECRET_KEY'] = 'one-file-secret-key'
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db'
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

    db.init_app(app)
    migrate.init_app(app, db)
    login_manager.init_app(app)

    app.register_blueprint(main_bp)
    app.register_blueprint(auth_bp)
    app.register_blueprint(items_bp)
    app.register_blueprint(admin_bp)

    return app

# ==========================================
# 6. INLINE HTML SHABLONLARI (TEMPLATES)
# ==========================================
BASE_TEMPLATE = """
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8"><title>Flask App</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark mb-4">
        <div class="container">
            <a class="navbar-brand" href="{{ url_for('main.index') }}">Yagona Faylli App</a>
            <div class="navbar-nav ms-auto">
                <a class="nav-link text-white" href="{{ url_for('items.create') }}">+ Yangi E'lon</a>
                {% if current_user.is_authenticated %}
                    {% if current_user.role == 'admin' %}
                        <a class="nav-link text-warning" href="{{ url_for('admin.manage_users') }}">Admin Panel</a>
                    {% endif %}
                    <span class="navbar-text text-light ms-2">({{ current_user.username }})</span>
                    <a class="btn btn-sm btn-outline-danger ms-2" href="{{ url_for('auth.logout') }}">Chiqish</a>
                {% else %}
                    <a class="nav-link" href="{{ url_for('auth.login') }}">Kirish</a>
                    <a class="nav-link" href="{{ url_for('auth.register') }}">Ro'yxatdan o'tish</a>
                {% endif %}
            </div>
        </div>
    </nav>
    <div class="container">
        {% with messages = get_flashed_messages(with_categories=true) %}
            {% if messages %}{% for c, m in messages %}<div class="alert alert-{{ c }}">{{ m }}</div>{% endfor %}{% endif %}
        {% endwith %}
        {% block content %}{% endblock %}
    </div>
</body>
</html>
"""

HTML_INDEX = BASE_TEMPLATE.replace("{% block content %}{% endblock %}", """
<div class="row mb-3">
    <div class="col-md-6"><h2>{% if tag_name %}Tag: {{ tag_name }}{% else %}Barcha e'lonlar{% endif %}</h2></div>
    <div class="col-md-6"><form method="GET" action="{{ url_for('main.index') }}" class="d-flex">
        <input type="text" name="q" class="form-control me-2" placeholder="Qidiruv..." value="{{ q }}">
        <button type="submit" class="btn btn-primary">Qidirish</button>
    </form></div>
</div>
{% for item in items %}
    <div class="card mb-3"><div class="card-body">
        <h5>{{ item.title }}</h5><p>{{ item.description }}</p>
        <div>{% for tag in item.tags %}<a href="{{ url_for('main.tag_items', name=tag.name) }}" class="badge bg-secondary text-decoration-none me-1">{{ tag.name }}</a>{% endfor %}</div>
        {% if current_user.is_authenticated and (current_user.id == item.user_id or current_user.role == 'admin') %}
            <div class="mt-2">
                <a href="{{ url_for('items.edit', id=item.id) }}" class="btn btn-sm btn-warning">Tahrirlash</a>
                <form action="{{ url_for('items.delete', id=item.id) }}" method="POST" class="d-inline">
                    <button type="submit" class="btn btn-sm btn-danger" onclick="return confirm('O`chirilsinmi?')">O'chirish</button>
                </form>
            </div>
        {% endif %}
    </div></div>
{% endfor %}
<nav><ul class="pagination">
    {% for page_num in pagination.iter_pages() %}{% if page_num %}
        <li class="page-item {% if page_num == pagination.page %}active{% endif %}"><a class="page-link" href="{{ url_for('main.index', page=page_num, q=q) }}">{{ page_num }}</a></li>
    {% endif %}{% endfor %}
</ul></nav>
""")

HTML_REGISTER = BASE_TEMPLATE.replace("{% block content %}{% endblock %}", """
<div class="card mx-auto" style="max-width: 400px;"><div class="card-body">
    <h4>Ro'yxatdan o'tish</h4>
    <form method="POST">
        <div class="mb-3"><label>Username</label><input type="text" name="username" class="form-control" required></div>
        <div class="mb-3"><label>Email</label><input type="email" name="email" class="form-control" required></div>
        <div class="mb-3"><label>Parol (kamida 8 ta)</label><input type="password" name="password" class="form-control" required></div>
        <button type="submit" class="btn btn-primary w-100">Yaratish</button>
    </form>
</div></div>
""")

HTML_LOGIN = BASE_TEMPLATE.replace("{% block content %}{% endblock %}", """
<div class="card mx-auto" style="max-width: 400px;"><div class="card-body">
    <h4>Kirish</h4>
    <form method="POST">
        <div class="mb-3"><label>Username yoki Email</label><input type="text" name="login_input" class="form-control" required></div>
        <div class="mb-3"><label>Parol</label><input type="password" name="password" class="form-control" required></div>
        <div class="mb-3 form-check"><input type="checkbox" name="remember" class="form-check-input" id="r"><label class="form-check-label" for="r">Eslab qol</label></div>
        <button type="submit" class="btn btn-success w-100">Kirish</button>
    </form>
</div></div>
""")

HTML_CREATE_ITEM = BASE_TEMPLATE.replace("{% block content %}{% endblock %}", """
<h2>Yangi e'lon</h2>
<form method="POST">
    <div class="mb-3"><label>Sarlavha</label><input type="text" name="title" class="form-control" required></div>
    <div class="mb-3"><label>Tavsif</label><textarea name="description" class="form-control" rows="4" required></textarea></div>
    <div class="mb-3"><label>Taglar (vergul bilan ajrating)</label><input type="text" name="tags" class="form-control" placeholder="python, flask"></div>
    <button type="submit" class="btn btn-primary">Yaratish</button>
</form>
""")

HTML_EDIT_ITEM = BASE_TEMPLATE.replace("{% block content %}{% endblock %}", """
<h2>Tahrirlash</h2>
<form method="POST">
    <div class="mb-3"><label>Sarlavha</label><input type="text" name="title" value="{{ item.title }}" class="form-control" required></div>
    <div class="mb-3"><label>Tavsif</label><textarea name="description" class="form-control" rows="4" required>{{ item.description }}</textarea></div>
    <button type="submit" class="btn btn-warning">Yangilash</button>
</form>
""")

HTML_ADMIN_USERS = BASE_TEMPLATE.replace("{% block content %}{% endblock %}", """
<h2>Foydalanuvchilar (Admin panel)</h2>
<table class="table table-bordered">
    <thead><tr><th>ID</th><th>Username</th><th>Email</th><th>Rol</th><th>Amal</th></tr></thead>
    <tbody>
        {% for u in users %}
        <tr>
            <td>{{ u.id }}</td><td>{{ u.username }}</td><td>{{ u.email }}</td>
            <td><span class="badge {% if u.role=='admin' %}bg-danger{% else %}bg-secondary{% endif %}">{{ u.role }}</span></td>
            <td><form method="POST" class="d-flex">
                <input type="hidden" name="user_id" value="{{ u.id }}">
                <select name="role" class="form-select form-select-sm me-2" style="width:auto;">
                    <option value="user" {% if u.role=='user' %}selected{% endif %}>User</option>
                    <option value="admin" {% if u.role=='admin' %}selected{% endif %}>Admin</option>
                </select>
                <button type="submit" class="btn btn-sm btn-primary">Saqlash</button>
            </form></td>
        </tr>
        {% endfor %}
    </tbody>
</table>
""")

HTML_ERROR = BASE_TEMPLATE.replace("{% block content %}{% endblock %}", """
<div class="text-center mt-5">
    <h1 class="display-3 text-danger">{{ title }}</h1>
    <p class="lead">{{ message }}</p>
    <a href="{{ url_for('main.index') }}" class="btn btn-primary">Bosh sahifaga qaytish</a>
</div>
""")

# ==========================================
# 7. LOYIHANI ISHGA TUSHIRISH
# ==========================================
app = create_app()

if __name__ == '__main__':
    app.run(debug=True)
Ishga tushirish yo'riqnomasi:
Kutubxonalarni o'rnating:

Bash
pip install flask flask-sqlalchemy flask-migrate flask-login
Migratsiyani bajaring (Ma'lumotlar bazasini yaratish):

Bash
flask db init
flask db migrate -m "initial migration"
flask db upgrade
Dasturni yoqing:

Bash
python app.py
