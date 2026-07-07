1. Modellar (models.py)
Modellar oldingi holatda qoladi, faqat bazani bog'lashda biroz aniqlik kiritamiz (joinedload(Note.author) o'rniga talab bo'yicha Note.user qilingan, shuning uchun munosabat nomini user deb o'zgartiramiz):

Python
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

note_tags = db.Table('note_tags',
    db.Column('note_id', db.Integer, db.ForeignKey('note.id', ondelete='CASCADE'), primary_key=True),
    db.Column('tag_id', db.Integer, db.ForeignKey('tag.id', ondelete='CASCADE'), primary_key=True)
)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    notes = db.relationship('Note', back_populates='user', cascade='all, delete-orphan')

class Note(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(200), nullable=False)
    content = db.Column(db.Text, nullable=False)
    user_id = db.Column(db.Integer, db.ForeignKey('user.id'), nullable=False)
    
    # Talabga binoan joinedload(Note.user) ishlashi uchun back_populates='user' qildik
    user = db.relationship('User', back_populates='notes')
    tags = db.relationship('Tag', secondary=note_tags, backref=db.backref('notes', lazy='dynamic'))

class Tag(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50), unique=True, nullable=False)
2. Flask Marshrutlari (app.py)
Bu yerda joinedload(Note.user), or_ va ilike orqali qidiruv hamda paginate() mexanizmi to'liq amalga oshirilgan.

Python
from flask import Flask, render_template, request
from models import db, User, Note, Tag
from sqlalchemy import or_

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///notes.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db.init_app(app)

@app.route('/')
def index():
    page = request.args.get('page', 1, type=int)
    search_query = request.args.get('q', '', type=str)
    
    # N+1 muammosini oldini olish uchun joinedload qo'llaymiz
    query = Note.query.options(db.joinedload(Note.user), db.joinedload(Note.tags))
    
    # Qidiruv: title VA content (body) bo'yicha ilike (case-insensitive)
    if search_query:
        query = query.filter(
            or_(
                Note.title.ilike(f'%{search_query}%'),
                Note.content.ilike(f'%{search_query}%')
            )
        )
    
    # Paginatsiya: har sahifada 10 tadan nota
    pagination = query.paginate(page=page, per_page=10, error_out=False)
    return render_template('index.html', pagination=pagination, search_query=search_query, endpoint='index')


@app.route('/tag/<name>')
def filter_by_tag(name):
    page = request.args.get('page', 1, type=int)
    tag = Tag.query.filter_by(name=name).first_or_404()
    
    # Tagga tegishli notalarni paginatsiya qilish
    pagination = Note.query.options(db.joinedload(Note.user), db.joinedload(Note.tags))\
                           .filter(Note.tags.contains(tag))\
                           .paginate(page=page, per_page=10, error_out=False)
                           
    return render_template('index.html', pagination=pagination, tag_name=name, endpoint='filter_by_tag')


@app.route('/user/<username>')
def filter_by_user(username):
    page = request.args.get('page', 1, type=int)
    user = User.query.filter_by(username=username).first_or_404()
    
    # Userga tegishli notalarni paginatsiya qilish
    pagination = Note.query.options(db.joinedload(Note.user), db.joinedload(Note.tags))\
                           .filter_by(user_id=user.id)\
                           .paginate(page=page, per_page=10, error_out=False)
                           
    return render_template('index.html', pagination=pagination, username=username, endpoint='filter_by_user')


# CLI Command: db yaratish uchun
@app.cli.command("create-db")
def create_db():
    db.create_all()
    print("Baza yaratildi.")
3. Seed Skripti (seed.py)
3 ta user, 4 ta tag va testlash uchun 35 ta nota yaratib beradigan skript.

Python
# Terminalda: flask shell ichida yoki alohida skript sifatida ishlating
from app import app, db
from models import User, Note, Tag
import random

with app.app_context():
    db.drop_all()
    db.create_all()

    # Userlar
    u1 = User(username="ali")
    u2 = User(username="vali")
    u3 = User(username="guli")
    db.session.add_all([u1, u2, u3])

    # Taglar
    t1 = Tag(name="Python")
    t2 = Tag(name="Flask")
    t3 = Tag(name="Shaxsiy")
    t4 = Tag(name="Dasturlash")
    db.session.add_all([t1, t2, t3, t4])
    db.session.commit()

    users = [u1, u2, u3]
    tags = [t1, t2, t3, t4]

    # 35 ta Nota yaratish
    for i in range(1, 36):
        note = Note(
            title=f"Mavzu #{i}: Muqaddas dasturlash sirlari",
            content=f"Bu {i}-notaning matni hisoblanadi. Bu yerda SQLAlchemy ORM va uning qulayliklari haqida gap boradi.",
            user=random.choice(users)
        )
        # Har bir notaga tasodifiy 1 tadan 3 tagacha tag biriktiramiz
        note.tags.extend(random.sample(tags, k=random.randint(1, 3)))
        db.session.add(note)

    db.session.commit()
    print("35 ta nota muvaffaqiyatli seed qilindi!")
4. Bosh Sahifa Shaboni (templates/index.html)
Paginatsiya linklari (Oldingi / Sahifalar / Keyingi) va qidiruv tizimi integratsiya qilingan universal shablon.

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>Notalar Tizimi</title>
    <style>
        body { font-family: 'Segoe UI', Arial, sans-serif; margin: 40px; background-color: #f8f9fa; color: #333; }
        .search-box { margin-bottom: 25px; }
        .search-box input[type="text"] { padding: 8px; width: 300px; border: 1px solid #ccc; border-radius: 4px; }
        .search-box button { padding: 8px 15px; background: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer; }
        .note-card { background: white; padding: 20px; margin-bottom: 15px; border-radius: 6px; box-shadow: 0 1px 3px rgba(0,0,0,0.1); }
        .meta { font-size: 0.85em; color: #6c757d; margin-bottom: 10px; }
        .meta a { color: #007bff; text-decoration: none; }
        .meta a:hover { text-decoration: underline; }
        .tag { background: #e2e3e5; color: #383d41; padding: 3px 8px; border-radius: 4px; font-size: 0.8em; margin-right: 5px; text-decoration: none; }
        .tag:hover { background: #d6d8db; }
        
        /* Paginatsiya Dizayni */
        .pagination { margin-top: 30px; display: flex; gap: 10px; align-items: center; }
        .pagination a, .pagination span { padding: 8px 12px; border: 1px solid #dee2e6; border-radius: 4px; text-decoration: none; color: #007bff; }
        .pagination .current { background-color: #007bff; color: white; border-color: #007bff; }
        .pagination .disabled { color: #6c757d; pointer-events: none; background-color: #fff; border-color: #dee2e6; }
    </style>
</head>
<body>

    <h1>
        {% if tag_name %} #{{ tag_name }} tegiga oid notalar
        {% elif username %} @{{ username }} sahifasi
        {% else %} Barcha Notalar
        {% endif %}
    </h1>

    {% if endpoint == 'index' %}
    <div class="search-box">
        <form action="{{ url_for('index') }}" method="GET">
            <input type="text" name="q" value="{{ search_query }}" placeholder="Sarlavha yoki matn bo'yicha qidiruv...">
            <button type="submit">Qidirish</button>
        </form>
    </div>
    {% endif %}

    {% for note in pagination.items %}
        <div class="note-card">
            <h2>{{ note.title }}</h2>
            <div class="meta">
                Muallif: <a href="{{ url_for('filter_by_user', username=note.user.username) }}">@{{ note.user.username }}</a>
            </div>
            <p>{{ note.content }}</p>
            <div>
                {% for tag in note.tags %}
                    <a href="{{ url_for('filter_by_tag', name=tag.name) }}" class="tag">#{{ tag.name }}</a>
                {% endfor %}
            </div>
        </div>
    {% else %}
        <p>Hech qanday nota topilmadi.</p>
    {% endfor %}

    {% if pagination.pages > 1 %}
    <div class="pagination">
        
        {% if pagination.has_prev %}
            <a href="{{ url_for(endpoint, page=pagination.prev_num, q=search_query, name=tag_name, username=username) }}">Oldingi</a>
        {% else %}
            <span class="disabled">Oldingi</span>
        {% endif %}

        {% for page_num in pagination.iter_pages(left_edge=1, right_edge=1, left_current=2, right_current=2) %}
            {% if page_num %}
                {% if page_num == pagination.page %}
                    <span class="current">{{ page_num }}</span>
                {% else %}
                    <a href="{{ url_for(endpoint, page=page_num, q=search_query, name=tag_name, username=username) }}">{{ page_num }}</a>
                {% endif %}
            {% else %}
                <span>...</span>
            {% endif %}
        {% endfor %}

        {% if pagination.has_next %}
            <a href="{{ url_for(endpoint, page=pagination.next_num, q=search_query, name=tag_name, username=username) }}">Keyingi</a>
        {% else %}
            <span class="disabled">Keyingi</span>
        {% endif %}

    </div>
    {% endif %}

</body>
</html>
