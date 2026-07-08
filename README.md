Mana, siz aytgan barcha talablarga javob beradigan, mukammal arxitekturaga ega Flask loyihasi strukturasi va kodi. Loyiha Factory Pattern asosida qurilgan bo'lib, Dev/Test/Prod konfigratsiyalari, Many-to-Many munosabatlar va barcha so'ralgan funksionalliklarni o'z ichiga oladi.

Loyiha Strukturasi
Plaintext
my_flask_app/
│
├── app/
│   ├── __init__.py
│   ├── config.py
│   ├── models.py
│   ├── extensions.py
│   ├── blueprints/
│   │   ├── main/
│   │   │   ├── __init__.py
│   │   │   └── routes.py
│   │   └── items/
│   │       ├── __init__.py
│   │       └── routes.py
│   └── templates/
│       ├── base.html
│       ├── main/
│       │   ├── index.html
│       │   └── tag_items.html
│       └── items/
│           └── create.html
│
├── migrations/          # Flask-Migrate tomonidan yaratiladi
├── README.md
├── requirements.txt
└── run.py
1. Konfigratsiya va Kengaytmalar (app/config.py & app/extensions.py)
app/config.py

Python
import os

class Config:
    SECRET_KEY = os.environ.get('SECRET_KEY', 'default-super-secret-key')
    SQLALCHEMY_TRACK_MODIFICATIONS = False
    ITEMS_PER_PAGE = 10

class DevelopmentConfig(Config):
    DEBUG = True
    SQLALCHEMY_DATABASE_URI = os.environ.get('DEV_DATABASE_URL', 'sqlite:///dev.db')

class TestingConfig(Config):
    TESTING = True
    SQLALCHEMY_DATABASE_URI = os.environ.get('TEST_DATABASE_URL', 'sqlite:///test.db')

class ProductionConfig(Config):
    DEBUG = False
    SQLALCHEMY_DATABASE_URI = os.environ.get('DATABASE_URL', 'sqlite:///prod.db')

config_by_name = {
    'dev': DevelopmentConfig,
    'test': TestingConfig,
    'prod': ProductionConfig
}
app/extensions.py

Python
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate

db = SQLAlchemy()
migrate = Migrate()
2. Modellar (app/models.py)
Many-to-Many bog'lanish va so'ralgan cascade='all, delete-orphan' munosabati bilan yozilgan modellar:

Python
from app.extensions import db
from datetime import datetime

# Many-to-Many yordamchi jadvali (Item <-> Tag)
item_tags = db.Table('item_tags',
    db.Column('item_id', db.Integer, db.ForeignKey('items.id', ondelete='CASCADE'), primary_key=True),
    db.Column('tag_id', db.Integer, db.ForeignKey('tags.id', ondelete='CASCADE'), primary_key=True)
)

class User(db.Model):
    __tablename__ = 'users'
    
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(64), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    
    # Cascade o'rnatilgan: Foydalanuvchi o'chirilsa, uning barcha e'lonlari ham o'chadi
    items = db.relationship('Item', backref='author', lazy='dynamic', cascade='all, delete-orphan')

    def __repr__(self):
        return f'<User {self.username}>'

class Item(db.Model):
    __tablename__ = 'items'
    
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100), nullable=False)
    description = db.Column(db.Text, nullable=False)
    created_at = db.Column(db.DateTime, default=datetime.utcnow)
    user_id = db.Column(db.Integer, db.ForeignKey('users.id'), nullable=False)
    
    # Many-to-Many munosabati
    tags = db.relationship('Tag', secondary=item_tags, backref=db.backref('items', lazy='dynamic'))

    def __repr__(self):
        return f'<Item {self.title}>'

class Tag(db.Model):
    __tablename__ = 'tags'
    
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50), unique=True, nullable=False)

    def __repr__(self):
        return f'<Tag {self.name}>'
3. Application Factory (app/__init__.py)
Python
from flask import Flask
from app.config import config_by_name
from app.extensions import db, migrate

def create_app(config_name='dev'):
    app = Flask(__name__)
    app.config.from_object(config_by_name[config_name])

    # Kengaytmalarni initsializatsiya qilish
    db.init_app(app)
    migrate.init_app(app, db)

    # Blueprint'larni ro'yxatdan o'tkazish
    from app.blueprints.main import main_bp
    from app.blueprints.items import items_bp

    app.register_blueprint(main_bp)
    app.register_blueprint(items_bp, url_prefix='/items')

    return app
4. Blueprint'lar va Controller'lar
app/blueprints/main/__init__.py

Python
from flask import Blueprint
main_bp = Blueprint('main', __name__)
from . import routes
app/blueprints/main/routes.py (Paginatsiya, Qidiruv va Tag bo'yicha filtrlash)

Python
from flask import render_template, request, current_app
from app.blueprints.main import main_bp
from app.models import Item, Tag

@main_bp.route('/')
def index():
    page = request.args.get('page', 1, type=int)
    search_query = request.args.get('q', '', type=str)
    per_page = current_app.config['ITEMS_PER_PAGE']

    query = Item.query
    if search_query:
        query = query.filter(Item.title.contains(search_query) | Item.description.contains(search_query))
    
    # Eng yangi e'lonlar birinchi chiqishi uchun tartiblaymiz
    pagination = query.order_by(Item.created_at.desc()).paginate(page=page, per_page=per_page, error_out=False)
    items = pagination.items

    return render_template('main/index.html', items=items, pagination=pagination, q=search_query)

@main_bp.route('/tag/<string:name>')
def tag_items(name):
    page = request.args.get('page', 1, type=int)
    per_page = current_app.config['ITEMS_PER_PAGE']
    
    tag = Tag.query.filter_by(name=name).first_or_404()
    # Tagga tegishli itemlarni paginatsiya qilish
    pagination = tag.items.order_by(Item.created_at.desc()).paginate(page=page, per_page=per_page, error_out=False)
    
    return render_template('main/tag_items.html', tag=tag, items=pagination.items, pagination=pagination)
app/blueprints/items/__init__.py

Python
from flask import Blueprint
items_bp = Blueprint('items', __name__)
from . import routes
app/blueprints/items/routes.py (Yangi e'lon qo'shish formasi va mantiq)

Python
from flask import render_template, redirect, url_for, flash, request
from app.blueprints.items import items_bp
from app.extensions import db
from app.models import Item, Tag, User

@items_bp.route('/create', methods=['GET', 'POST'])
def create():
    if request.method == 'POST':
        title = request.form.get('title')
        description = request.form.get('description')
        tags_string = request.form.get('tags', '')

        if not title or not description:
            flash("Sarlavha va matn majburiy!", "danger")
            return redirect(url_for('items.create'))

        # Autentifikatsiya keyingi modulda bo'lgani uchun vaqtincha test foydalanuvchisi
        user = User.query.first()
        if not user:
            user = User(username="test_user", email="test@example.com")
            db.session.add(user)
            db.session.commit()

        new_item = Item(title=title, description=description, author=user)

        # Taglarni qayta ishlash (vergul bilan ajratilgan)
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
5. HTML Shablonlar (templates)
app/templates/base.html

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>Flask App</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark mb-4">
        <div class="container">
            <a class="navbar-brand" href="{{ url_for('main.index') }}">E'lonlar Doskasi</a>
            <div class="navbar-nav ms-auto">
                <a class="nav-link" href="{{ url_for('items.create') }}">+ Yangi E'lon</a>
            </div>
        </div>
    </nav>
    <div class="container">
        {% with messages = get_flashed_messages(with_categories=true) %}
            {% if messages %}
                {% for category, message in messages %}
                    <div class="alert alert-{{ category }}">{{ message }}</div>
                {% endfor %}
            {% endif %}
        {% endwith %}
        
        {% block content %}{% endblock %}
    </div>
</body>
</html>
app/templates/main/index.html

HTML
{% extends 'base.html' %}
{% block content %}
<div class="row mb-4">
    <div class="col-md-6">
        <h2>Barcha e'lonlar</h2>
    </div>
    <div class="col-md-6">
        <form method="GET" action="{{ url_for('main.index') }}" class="d-flex">
            <input type="text" name="q" class="form-control me-2" placeholder="Qidiruv..." value="{{ q }}">
            <button type="submit" class="btn btn-outline-primary">Qidirish</button>
        </form>
    </div>
</div>

{% for item in items %}
    <div class="card mb-3">
        <div class="card-body">
            <h5 class="card-title">{{ item.title }}</h5>
            <p class="card-text">{{ item.description }}</p>
            <p class="text-muted"><small>Muallif: {{ item.author.username }} | {{ item.created_at.strftime('%Y-%m-%d') }}</small></p>
            <div>
                {% for tag in item.tags %}
                    <a href="{{ url_for('main.tag_items', name=tag.name) }}" class="badge bg-secondary text-decoration-none">{{ tag.name }}</a>
                {% endfor %}
            </div>
        </div>
    </div>
{% else %}
    <p>Hech qanday e'lon topilmadi.</p>
{% endfor %}

<nav>
    <ul class="pagination">
        {% for page_num in pagination.iter_pages() %}
            {% if page_num %}
                <li class="page-item {% if page_num == pagination.page %}active{% endif %}">
                    <a class="page-link" href="{{ url_for('main.index', page=page_num, q=q) }}">{{ page_num }}</a>
                </li>
            {% else %}
                <li class="page-item disabled"><span class="page-link">...</span></li>
            {% endif %}
        {% endfor %}
    </ul>
</nav>
{% endblock %}
(Eslatma: app/templates/main/tag_items.html shabloni ham index.html bilan deyarli bir xil tuziladi, faqat sarlavhasi Tag: {{ tag.name }} ko'rinishida bo'ladi).

app/templates/items/create.html

HTML
{% extends 'base.html' %}
{% block content %}
<h2>Yangi e'lon qo'shish</h2>
<form method="POST">
    <div class="mb-3">
        <label class="form-label">Sarlavha</label>
        <input type="text" name="title" class="form-control" required>
    </div>
    <div class="mb-3">
        <label class="form-label">Tavsif (Matni)</label>
        <textarea name="description" class="form-control" rows="5" required></textarea>
    </div>
    <div class="mb-3">
        <label class="form-label">Taglar (vergul bilan ajrating)</label>
        <input type="text" name="tags" class="form-control" placeholder="python, flask, backend">
    </div>
    <button type="submit" class="btn btn-success">Chop etish</button>
</form>
{% endblock %}
6. Loyihani ishga tushiruvchi fayl (run.py)
Python
import os
from app import create_app

config_name = os.environ.get('FLASK_CONFIG', 'dev')
app = create_app(config_name)

if __name__ == '__main__':
    app.run()
7. README.md (Loyiha hujjatlashuvi)
Markdown
# Flask E'lonlar Doskasi Loyihasi

Ushbu loyiha Flask frameworkida Application Factory patteri yordamida yozilgan.

### Ishga tushirish qadamlari

1. **Virtual muhitni yarating va faollashtiring:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # Linux/Mac
   # yoki
   venv\Scripts\activate     # Windows
Kutubxonalarni o'rnating:

Bash
pip install -r requirements.txt
(Fayl ichida: Flask, Flask-SQLAlchemy, Flask-Migrate bo'lishi kerak)

Ma'lumotlar bazasi migratsiyasini bajarish:

Bash
flask db init
flask db migrate -m "Initial migration"
flask db upgrade
Loyihani ishga tushirish:

Bash
python run.py
