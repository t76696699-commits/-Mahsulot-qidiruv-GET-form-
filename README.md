1. config.py - Konfiguratsiya
Python
import os

class Config:
    SECRET_KEY = os.environ.get('SECRET_KEY') or 'juda_maxfiy_kalit_12345'
    SQLALCHEMY_DATABASE_URI = os.environ.get('DATABASE_URL') or 'sqlite:///app.db'
    SQLALCHEMY_TRACK_MODIFICATIONS = False
2. app/__init__.py - Appni ishga tushirish va sozlash
Python
from flask import Flask, render_template
from flask_sqlalchemy import SQLAlchemy
from flask_login import LoginManager
from flask_migrate import Migrate
from config import Config

db = SQLAlchemy()
login_manager = LoginManager()
migrate = Migrate()

def create_app():
    app = Flask(__name__)
    app.config.from_object(Config)

    db.init_app(app)
    login_manager.init_app(app)
    migrate.init_app(app, db)

    login_manager.login_view = 'auth.login'
    login_manager.login_message_category = 'info'

    # Blueprintlarni ro'yxatdan o'tkazish
    from app.routes.auth import auth_bp
    from app.routes.posts import posts_bp
    from app.routes.admin import admin_bp

    app.register_blueprint(auth_bp, url_prefix='/auth')
    app.register_blueprint(posts_bp, url_prefix='/posts')
    app.register_blueprint(admin_bp, url_prefix='/admin')

    # Xatolik sahifalari (Error Handlers)
    @app.errorhandler(401)
    def unauthorized_error(error):
        return render_template('401.html'), 401

    @app.errorhandler(403)
    def forbidden_error(error):
        return render_template('403.html'), 403

    return app
3. app/models.py - Modellar
Python
from app import db, login_manager
from flask_login import UserMixin
from werkzeug.security import generate_password_hash, check_password_hash

@login_manager.user_loader
def load_user(user_id):
    return User.query.get(int(user_id))

class User(db.Model, UserMixin):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(50), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    password_hash = db.Column(db.String(128), nullable=False)
    role = db.Column(db.String(20), default='user')  # 'user' yoki 'admin'

    posts = db.relationship('Post', backref='author', lazy=True)

    def set_password(self, password):
        self.password_hash = generate_password_hash(password)

    def check_password(self, password):
        return check_password_hash(self.password_hash, password)

    @property
    def is_admin(self):
        return self.role == 'admin'

class Post(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100), nullable=False)
    content = db.Column(db.Text, nullable=False)
    user_id = db.Column(db.Integer, db.ForeignKey('user.id'), nullable=False)
4. app/decorators.py - Huquqlarni tekshiruvchi dekoratorlar
Python
from functools import wraps
from flask import abort
from flask_login import current_user

def admin_required(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        if not current_user.is_authenticated or not current_user.is_admin:
            abort(403)
        return f(*args, **kwargs)
    return decorated_function
5. app/routes/auth.py - Autentifikatsiya Blueprinti
Python
from flask import Blueprint, render_template, redirect, url_prefix, url_for, request, flash
from flask_login import login_user, logout_user, login_required, current_user
from app.models import User
from app import db

auth_bp = Blueprint('auth', __name__)

@auth_bp.route('/register', methods=['GET', 'POST'])
def register():
    if current_user.is_authenticated:
        return redirect(url_for('posts.index')) # yoki bosh sahifa
    
    if request.method == 'POST':
        username = request.form.get('username')
        email = request.form.get('email')
        password = request.form.get('password')

        if len(password) < 8:
            flash("Parol kamida 8 ta belgidan iborat bo'lishi kerak!", "danger")
            return render_template('register.html')

        if User.query.filter((User.username == username) | (User.email == email)).first():
            flash("Username yoki Email allaqachon mavjud!", "danger")
            return render_template('register.html')

        user = User(username=username, email=email)
        user.set_password(password)
        db.session.add(user)
        db.session.commit()

        flash("Ro'yxatdan muvaffaqiyatli o'tdingiz. Tizimga kiring.", "success")
        return redirect(url_for('auth.login'))

    return render_template('register.html')

@auth_bp.route('/login', methods=['GET', 'POST'])
def login():
    if current_user.is_authenticated:
        return redirect(url_for('posts.index'))

    if request.method == 'POST':
        login_input = request.form.get('login_input') # username yoki email
        password = request.form.get('password')
        remember = True if request.form.get('remember') else False

        user = User.query.filter((User.email == login_input) | (User.username == login_input)).first()

        if user and user.check_password(password):
            login_user(user, remember=remember)
            
            # ?next=... orqali kelgan sahifani aniqlash va qaytarish
            next_page = request.args.get('next')
            return redirect(next_page) if next_page else redirect(url_for('posts.index'))
        else:
            flash("Login yoki parol xato!", "danger")

    return render_template('login.html')

@auth_bp.route('/logout')
@login_required
def logout():
    logout_user()
    return redirect(url_for('auth.login'))
6. app/routes/posts.py - Postlar Blueprinti
Python
from flask import Blueprint, render_template, redirect, url_for, request, abort, flash
from flask_login import login_required, current_user
from app.models import Post
from app import db

posts_bp = Blueprint('posts', __name__)

@posts_bp.route('/')
def index():
    posts = Post.query.all()
    return render_template('base.html', posts=posts) # Bosh sahifada postlarni chiqarish

@posts_bp.route('/new', methods=['GET', 'POST'])
@login_required
def new_post():
    if request.method == 'POST':
        title = request.form.get('title')
        content = request.form.get('content')
        
        post = Post(title=title, content=content, author=current_user)
        db.session.add(post)
        db.session.commit()
        return redirect(url_for('posts.index'))
    return render_template('post_form.html', legend="Yangi Post yaratish")

@posts_bp.route('/<int:id>/edit', methods=['GET', 'POST'])
@login_required
def edit_post(id):
    post = Post.query.get_or_404(id)
    
    # Server-side tekshiruv: faqat muallif yoki admin o'zgartira oladi
    if post.author != current_user and not current_user.is_admin:
        abort(403)
        
    if request.method == 'POST':
        post.title = request.form.get('title')
        post.content = request.form.get('content')
        db.session.commit()
        return redirect(url_for('posts.index'))
        
    return render_template('post_form.html', legend="Postni tahrirlash", post=post)

@posts_bp.route('/<int:id>/delete', methods=['POST'])
@login_required
def delete_post(id):
    post = Post.query.get_or_404(id)
    
    # Server-side tekshiruv
    if post.author != current_user and not current_user.is_admin:
        abort(403)
        
    db.session.delete(post)
    db.session.commit()
    flash("Post o'chirildi!", "success")
    return redirect(url_for('posts.index'))
7. app/routes/admin.py - Admin panel Blueprinti
Python
from flask import Blueprint, render_template, redirect, url_for, request, flash
from app.models import User
from app.decorators import admin_required
from app import db

admin_bp = Blueprint('admin', __name__)

@admin_bp.route('/users', methods=['GET', 'POST'])
@admin_required
def manage_users():
    if request.method == 'POST':
        user_id = request.form.get('user_id')
        new_role = request.form.get('role')
        
        user = User.query.get(user_id)
        if user:
            user.role = new_role
            db.session.commit()
            flash(f"{user.username} roli '{new_role}'ga o'zgartirildi.", "success")
            
    users = User.query.all()
    return render_template('admin_users.html', users=users)
8. manage.py - Loyihani ishga tushirish fayli
Python
from app import create_app

app = create_app()

if __name__ == '__main__':
    app.run(debug=True)
🎨 HTML Shablonlar (Templates)
app/templates/base.html (Asosiy shablon va current_user tekshiruvi)
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>Flask RBAC</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">
</head>
<body class="container mt-4">
    <nav class="navbar navbar-expand-lg navbar-light bg-light mb-4 p-3 rounded">
        <a class="navbar-brand" href="{{ url_for('posts.index') }}">My Blog</a>
        <div class="navbar-nav ms-auto">
            {% if current_user.is_authenticated %}
                <span class="navbar-text me-3">Salom, <b>{{ current_user.username }}</b> ({{ current_user.role }})</span>
                {% if current_user.is_admin %}
                    <a class="nav-link text-danger" href="{{ url_for('admin.manage_users') }}">Admin Panel</a>
                {% endif %}
                <a class="nav-link" href="{{ url_for('posts.new_post') }}">Yangi Post</a>
                <a class="nav-link" href="{{ url_for('auth.logout') }}">Chiqish</a>
            {% else %}
                <a class="nav-link" href="{{ url_for('auth.login') }}">Kirish</a>
                <a class="nav-link" href="{{ url_for('auth.register') }}">Ro'yxatdan o'tish</a>
            {% endif %}
        </div>
    </nav>

    {% with messages = get_flashed_messages(with_categories=true) %}
        {% if messages %}
            {% for category, message in messages %}
                <div class="alert alert-{{ category }}">{{ message }}</div>
            {% endfor %}
        {% endif %}
    {% endwith %}

    {% block content %}
    <h2>Barcha Postlar</h2>
    {% for post in posts %}
        <div class="card mb-3">
            <div class="card-body">
                <h3>{{ post.title }}</h3>
                <p>{{ post.content }}</p>
                <small>Muallif: {{ post.author.username }}</small>
                
                {% if current_user.is_authenticated and (post.author == current_user or current_user.is_admin) %}
                    <div class="mt-2">
                        <a href="{{ url_for('posts.edit_post', id=post.id) }}" class="btn btn-sm btn-warning">Tahrirlash</a>
                        <form action="{{ url_for('posts.delete_post', id=post.id) }}" method="POST" style="display:inline;">
                            <button type="submit" class="btn btn-sm btn-danger">O'chirish</button>
                        </form>
                    </div>
                {% endif %}
            </div>
        </div>
    {% endfor %}
    {% endblock %}
</body>
</html>
app/templates/login.html
HTML
{% extends "base.html" %}
{% block content %}
<div class="row justify-content-center">
    <div class="col-md-6">
        <h2>Tizimga kirish</h2>
        <form method="POST">
            <div class="mb-3">
                <label>Username yoki Email</label>
                <input type="text" name="login_input" class="form-control" required>
            </div>
            <div class="mb-3">
                <label>Parol</label>
                <input type="password" name="password" class="form-control" required>
            </div>
            <div class="mb-3 form-check">
                <input type="checkbox" name="remember" class="form-check-input" id="remember">
                <label class="form-check-label" for="remember">Meni eslab qol</label>
            </div>
            <button type="submit" class="btn btn-primary">Kirish</button>
        </form>
    </div>
</div>
{% endblock %}
app/templates/admin_users.html (Rol o'zgartirish formasi)
HTML
{% extends "base.html" %}
{% block content %}
<h2>Foydalanuvchilarni boshqarish (Admin)</h2>
<table class="table">
    <thead>
        <tr>
            <th>ID</th>
            <th>Username</th>
            <th>Email</th>
            <th>Rol</th>
            <th>Roli o'zgartirish</th>
        </tr>
    </thead>
    <tbody>
        {% for user in users %}
        <tr>
            <td>{{ user.id }}</td>
            <td>{{ user.username }}</td>
            <td>{{ user.email }}</td>
            <td><span class="badge bg-secondary">{{ user.role }}</span></td>
            <td>
                <form method="POST" class="d-inline-flex">
                    <input type="hidden" name="user_id" value="{{ user.id }}">
                    <select name="role" class="form-select form-select-sm me-2">
                        <option value="user" {% if user.role == 'user' %}selected{% endif %}>User</option>
                        <option value="admin" {% if user.role == 'admin' %}selected{% endif %}>Admin</option>
                    </select>
                    <button type="submit" class="btn btn-sm btn-success">Saqlash</button>
                </form>
            </td>
        </tr>
        {% endfor %}
    </tbody>
</table>
{% endblock %}
Xato sahifalari (401 va 403)
app/templates/401.html:

HTML
{% extends "base.html" %}
{% block content %}
<div class="text-center mt-5">
    <h1 class="display-1 text-warning">401</h1>
    <h2>Avtorizatsiyadan o'tilmagan</h2>
    <p>Ushbu sahifani ko'rish uchun avval tizimga kiring.</p>
    <a href="{{ url_for('auth.login') }}" class="btn btn-primary">Kirish sahifasi</a>
</div>
{% endblock %}
app/templates/403.html:

HTML
{% extends "base.html" %}
{% block content %}
<div class="text-center mt-5">
    <h1 class="display-1 text-danger">403</h1>
    <h2>Ruxsat berilmagan</h2>
    <p>Sizda ushbu sahifaga kirish huquqi yo'q!</p>
    <a href="{{ url_for('posts.index') }}" class="btn btn-secondary">Bosh sahifaga qaytish</a>
</div>
{% endblock %}
