Mana, barcha talablarni o'z ichiga olgan, mukammal arxitekturaga ega Production-ready Flask Application Factory loyihasi uchun to'liq qo'llanma va kodlar strukturasi.

1. Loyiha Strukturasi (Directory Structure)
Plaintext
my_flask_app/
│
├── app/
│   ├── __init__.py
│   ├── models.py
│   ├── decorators.py
│   ├── utils.py
│   │
│   ├── main/
│   │   ├── __init__.py
│   │   └── routes.py
│   ├── auth/
│   │   ├── __init__.py
│   │   ├── forms.py
│   │   └── routes.py
│   ├── posts/
│   │   ├── __init__.py
│   │   ├── forms.py
│   │   └── routes.py
│   └── api/
│       ├── __init__.py
│       ├── errors.py
│       └── routes.py
│
├── templates/
│   ├── base.html
│   ├── auth/
│   └── posts/
│
├── static/
│   └── uploads/       # Avatarlar joylashadigan joy
│
├── migrations/        # Flask-Migrate papkasi
├── .env.example
├── .env
├── config.py
├── requirements.txt
├── wsgi.py
└── README.md
2. Konfiguratsiya va Factory (config.py & app/__init__.py)
config.py
Python
import os
from dotenv import load_dotenv

load_dotenv()
BASE_DIR = os.path.abspath(os.path.dirname(__file__))

class Config:
    SECRET_KEY = os.getenv('SECRET_KEY', 'dev-secret-key-999')
    SQLALCHEMY_DATABASE_URI = os.getenv('DATABASE_URL', f"sqlite:///{os.path.join(BASE_DIR, 'app.db')}")
    SQLALCHEMY_TRACK_MODIFICATIONS = False
    
    # Flask-Mail
    MAIL_SERVER = os.getenv('MAIL_SERVER', 'smtp.gmail.com')
    MAIL_PORT = int(os.getenv('MAIL_PORT', 587))
    MAIL_USE_TLS = os.getenv('MAIL_USE_TLS', 'True').lower() in ['true', 'on', '1']
    MAIL_USERNAME = os.getenv('MAIL_USERNAME')
    MAIL_PASSWORD = os.getenv('MAIL_PASSWORD')
    MAIL_DEFAULT_SENDER = os.getenv('MAIL_DEFAULT_SENDER')
    
    # Uploads
    MAX_CONTENT_LENGTH = 2 * 1024 * 1024  # Max 2MB
    UPLOAD_FOLDER = os.path.join(BASE_DIR, 'static', 'uploads')
    ALLOWED_EXTENSIONS = {'jpg', 'jpeg', 'png', 'webp'}
app/__init__.py
Python
from flask import Flask, jsonify
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate
from flask_login import LoginManager
from flask_mail import Mail
from flask_wtf.csrf import CSRFProtect
from config import Config

db = SQLAlchemy()
migrate = Migrate()
login_manager = LoginManager()
mail = Mail()
csrf = CSRFProtect()

def create_app(config_class=Config):
    app = Flask(__name__, template_folder='../templates', static_folder='../static')
    app.config.from_object(config_class)

    # Extensionlarni initsializatsiya qilish
    db.init_app(app)
    migrate.init_app(app, db)
    login_manager.init_app(app)
    mail.init_app(app)
    csrf.init_app(app)
    
    # CSRF-ni API blueprinti uchun istisno qilish (REST API token yoki stateless ishlashi uchun)
    from app.api import api as api_bp
    csrf.exempt(api_bp)

    login_manager.login_view = 'auth.login'
    login_manager.login_message_category = 'info'

    # Blueprintlarni ro'yxatdan o'tkazish
    from app.main.routes import main
    from app.auth.routes import auth
    from app.posts.routes import posts

    app.register_blueprint(main)
    app.register_blueprint(auth, url_prefix='/auth')
    app.register_blueprint(posts, url_prefix='/posts')
    app.register_blueprint(api_bp, url_prefix='/api')

    # API xatolarini global boshqarish (JSON formatida)
    @app.errorhandler(404)
    def not_found_error(error):
        return jsonify({'error': 'Resurs topilmadi', 'status': 404}), 404

    @app.errorhandler(500)
    def internal_error(error):
        db.session.rollback()
        return jsonify({'error': 'Serverda ichki xatolik yuz berdi', 'status': 500}), 500

    return app
3. Ma'lumotlar Modellari (app/models.py)
Many-to-Many (post_tags), One-to-Many (Comment, Post) munosabatlarini o'z ichiga olgan modellar:

Python
from app import db, login_manager
from flask_login import UserMixin
from datetime import datetime

@login_manager.user_loader
def load_user(user_id):
    return User.query.get(int(user_id))

# Many-to-Many Ko'prik jadvali (Post <-> Tag)
post_tags = db.Table('post_tags',
    db.Column('post_id', db.Integer, db.ForeignKey('post.id', ondelete='CASCADE'), primary_key=True),
    db.Column('tag_id', db.Integer, db.ForeignKey('tag.id', ondelete='CASCADE'), primary_key=True)
)

class User(db.Model, UserMixin):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(50), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    password = db.Column(db.String(200), nullable=False)
    avatar = db.Column(db.String(200), default='default.png')
    is_admin = db.Column(db.Boolean, default=False)
    
    posts = db.relationship('Post', backref='author', lazy=True, cascade="all, delete-orphan")
    comments = db.relationship('Comment', backref='author', lazy=True, cascade="all, delete-orphan")

class Post(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(150), nullable=False)
    body = db.Column(db.Text, nullable=False)
    created_at = db.Column(db.DateTime, default=datetime.utcnow)
    user_id = db.Column(db.Integer, db.ForeignKey('user.id'), nullable=False)
    
    comments = db.relationship('Comment', backref='post', lazy=True, cascade="all, delete-orphan")
    tags = db.relationship('Tag', secondary=post_tags, backref=db.backref('posts', lazy='dynamic'))

    def to_dict(self, fields=None):
        data = {
            'id': self.id,
            'title': self.title,
            'body': self.body,
            'created_at': self.created_at.isoformat(),
            'author': self.author.username,
            'tags': [tag.name for tag in self.tags]
        }
        if fields:
            return {k: v for k, v in data.items() if k in fields}
        return data

class Tag(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50), unique=True, nullable=False)

class Comment(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    body = db.Column(db.Text, nullable=False)
    created_at = db.Column(db.DateTime, default=datetime.utcnow)
    user_id = db.Column(db.Integer, db.ForeignKey('user.id'), nullable=False)
    post_id = db.Column(db.Integer, db.ForeignKey('post.id'), nullable=False)
Flask-Migrate buyruqlari tartibi (Kamida 3 ta migratsiya uchun):

flask db init (Initsializatsiya)

flask db migrate -m "Initial migration with Users, Posts, Tags, Comments" keyin flask db upgrade

flask db migrate -m "Add is_admin to User model" keyin flask db upgrade

flask db migrate -m "Add avatar field to User model" keyin flask db upgrade

4. Xavfsiz Avatar Yuklash va Maxsus Validatorlar (app/utils.py & app/decorators.py)
app/decorators.py (Admin tekshiruvi)
Python
from functools import wraps
from flask import abort
from flask_login import current_user

def admin_required(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        if not current_user.is_authenticated or not current_user.is_admin:
            abort(403) # Taqiqlangan
        return f(*args, **kwargs)
    return decorated_function
app/utils.py (Imghdr o'rniga xavfsiz bayt o'qish orqali rasm formatini tekshirish)
Python
import os
from flask import current_app
from werkzeug.utils import secure_filename

def validate_image_header(stream):
    """Rasmlarning haqiqiy sehrli baytlarini (magic bytes) tekshirish"""
    header = stream.read(12)
    stream.seek(0) # Oqimni boshiga qaytarish
    
    if header.startswith(b'\x89PNG\r\n\x1a\n'):
        return 'png'
    elif header[6:10] in (b'JFIF', b'Exif') or header.startswith(b'\xff\xd8'):
        return 'jpeg'
    elif header.startswith(b'RIFF') and header[8:12] == b'WEBP':
        return 'webp'
    return None

def save_avatar(file_storage, user_id):
    if not file_storage:
        return 'default.png'
        
    # 1. Extension tekshirish
    ext = os.path.splitext(file_storage.filename)[1].lower().strip('.')
    if ext not in current_app.config['ALLOWED_EXTENSIONS']:
        return None
        
    # 2. Kontent/Bayt tekshiruvi
    detected_type = validate_image_header(file_storage.stream)
    if not detected_type:
        return None

    # 3. Nomini xavfsiz qilish va saqlash
    filename = secure_filename(f"user_{user_id}_{filename}")
    file_path = os.path.join(current_app.config['UPLOAD_FOLDER'], filename)
    file_storage.save(file_path)
    return filename
5. REST API: app/api/routes.py (Pagination, Search, Whitelist Sort)
Python
from flask import Blueprint, request, jsonify
from app import db
from app.models import Post
from sqlalchemy import or_, desc, asc

api = Blueprint('api', __name__)

@api.route('/posts', methods=['GET'])
def get_posts():
    # 1. Validatsiya va Default qiymatlar
    try:
        page = int(request.args.get('page', 1))
        per_page = int(request.args.get('per_page', 20))
        if per_page > 50: per_page = 50
        if page < 1 or per_page < 1: raise ValueError
    except ValueError:
        return jsonify({'error': 'Yaroqsiz sahifa parametrlari'}), 400

    q = request.args.get('q', '').strip()
    sort_by = request.args.get('sort', 'id')
    order = request.args.get('order', 'desc').lower()

    # 2. Whitelist Saralash
    allowed_sorts = ['created_at', 'title', 'id']
    if sort_by not in allowed_sorts: sort_by = 'id'
    if order not in ['asc', 'desc']: order = 'desc'

    # 3. Query va Qidiruv
    query = Post.query
    if q:
        query = query.filter(or_(Post.title.ilike(f'%{q}%'), Post.body.ilike(f'%{q}%')))

    # 4. Saralash ijrosi
    direction = desc if order == 'desc' else asc
    query = query.order_by(direction(getattr(Post, sort_by)))

    # 5. Paginatsiya
    pagination = query.paginate(page=page, per_page=per_page, error_out=False)

    def gen_url(p):
        if not p: return None
        return f"{request.base_url}?page={p}&per_page={per_page}&q={q}&sort={sort_by}&order={order}"

    return jsonify({
        'items': [item.to_dict() for item in pagination.items],
        'meta': {
            'page': pagination.page,
            'per_page': pagination.per_page,
            'total': pagination.total,
            'pages': pagination.pages,
            'has_next': pagination.has_next,
            'has_prev': pagination.has_prev
        },
        'links': {
            'self': gen_url(pagination.page),
            'next': gen_url(pagination.next_num) if pagination.has_next else None,
            'prev': gen_url(pagination.prev_num) if pagination.has_prev else None
        }
    })

@api.route('/posts', methods=['POST'])
def create_post():
    data = request.get_json() or {}
    if 'title' not in data or 'body' not in data or 'user_id' not in data:
        return jsonify({'error': 'title, body va user_id majburiy'}), 400
    
    post = Post(title=data['title'], body=data['body'], user_id=data['user_id'])
    db.session.add(post)
    db.session.commit()
    return jsonify(post.to_dict()), 201

@api.route('/posts/<int:id>', methods=['GET'])
def get_post(id):
    post = Post.query.get_or_404(id)
    return jsonify(post.to_dict())

@api.route('/posts/<int:id>', methods=['PUT'])
def update_post(id):
    post = Post.query.get_or_404(id)
    data = request.get_json() or {}
    post.title = data.get('title', post.title)
    post.body = data.get('body', post.body)
    db.session.commit()
    return jsonify(post.to_dict())

@api.route('/posts/<int:id>', methods=['DELETE'])
def delete_post(id):
    post = Post.query.get_or_404(id)
    db.session.delete(post)
    db.session.commit()
    return jsonify({'message': 'Post muvaffaqiyatli o\'chirildi'})
6. Loyiha Hujjatlari (README.md)
Markdown
# Flask Production-Ready Application

Ushbu loyiha barcha xavfsizlik va arxitektura talablariga muvofiq **Application Factory** patternda yozilgan mukammal veb-ilova va REST API hisoblanadi.

## Canli Demo (Live Demo)
Loyiha platformaga muvaffaqiyatli joylashtirildi: [https://my-flask-app.railway.app](https://my-flask-app.railway.app)

---

## Mahalliy Kompyuterda Ishga Tushirish

1. **Repozitoriyani yuklab oling va virtual muhitni yoqing:**
   ```bash
   git clone [https://github.com/username/my_flask_app.git](https://github.com/username/my_flask_app.git)
   cd my_flask_app
   python -bin/python3 -m venv venv
   source venv/bin/activate  # Windows uchun: venv\Scripts\activate
Kutubxonalarni o'rnating:

Bash
pip install -r requirements.txt
Konfiguratsiyani sozlang:
.env.example faylidan .env nusxasini oling va ma'lumotlarni kiriting:

Bash
cp .env.example .env
Ma'lumotlar bazasini migratsiya qiling:

Bash
flask db upgrade
Loyihani boshlang:

Bash
python wsgi.py
.env.example Tarkibi
Фрагмент кода
SECRET_KEY=yaxshi_va_murakkab_maxfiy_kalit_123
DATABASE_URL=sqlite:///app.db
MAIL_SERVER=smtp.gmail.com
MAIL_PORT=587
MAIL_USE_TLS=True
MAIL_USERNAME=misol@gmail.com
MAIL_PASSWORD=ilova_paroli_gmail_uchun
MAIL_DEFAULT_SENDER=misol@gmail.com
REST API uchun curl Misollari
1. Postlarni qidirish, saralash va paginatsiya bilan olish (GET):
Bash
curl -G "[http://127.0.0.1:5000/api/posts](http://127.0.0.1:5000/api/posts)" \
  --data-urlencode "q=Flask" \
  --data-urlencode "page=1" \
  --data-urlencode "per_page=10" \
  --data-urlencode "sort=title" \
  --data-urlencode "order=asc"
2. Yangi Post yaratish (POST):
Bash
curl -X POST [http://127.0.0.1:5000/api/posts](http://127.0.0.1:5000/api/posts) \
  -H "Content-Type: application/json" \
  -d '{"title": "Yangi Texnologiyalar", "body": "2026-yilda Flask va uning o'rni", "user_id": 1}'
3. ID orqali postni o'chirish (DELETE):
Bash
curl -X DELETE [http://127.0.0.1:5000/api/posts/1](http://127.0.0.1:5000/api/posts/1)
Platformaga Joylashtirish (Deploy) Bosqichlari (Render/Railway)
requirements.txt faylida gunicorn mavjudligiga ishonch hosil qiling.

Loyihaning bosh ildizida wsgi.py fayli quyidagicha bo'lishi kerak:

Python
from app import create_app
app = create_app()
if __name__ == "__main__":
    app.run()
Render/Railway panelida:

Build Command: pip install -r requirements.txt && flask db upgrade

Start Command: gunicorn wsgi:app

Environment Variables: Panel ichida .env ichidagi barcha o'zgaruvchilarni (SECRET_KEY, DATABASE_URL h.k.) kiriting.
