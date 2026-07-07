1. Loyiha fayllari strukturasi
Kodni quyidagi tartibda joylashtirishingiz mumkin:

Plaintext
app/
├── app.py          # Asosiy Flask ilovasi va marshrutlar
├── models.py       # SQLAlchemy modellari va bog'lanishlar
└── templates/
    └── index.html  # Bosh sahifa (HTML)
2. Modellar (models.py)
Bu yerda User, Note, Tag modellari va ular o'rtasidagi munosabatlar (relationship, cascade, secondary) joylashgan.

Python
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

# Tag va Note o'rtasidagi oraliq (association) jadvali
note_tags = db.Table('note_tags',
    db.Column('note_id', db.Integer, db.ForeignKey('note.id', ondelete='CASCADE'), primary_key=True),
    db.Column('tag_id', db.Integer, db.ForeignKey('tag.id', ondelete='CASCADE'), primary_key=True)
)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    
    # Cascade 'all, delete-orphan' -> User o'chganda uning barcha notalari ham o'chadi
    notes = db.relationship('Note', back_populates='author', cascade='all, delete-orphan')

    def __repr__(self):
        return f'<User {self.username}>'

class Note(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(200), nullable=False)
    content = db.Column(db.Text, nullable=False)
    
    user_id = db.Column(db.Integer, db.ForeignKey('user.id'), nullable=False)
    
    # User bilan teskari bog'lanish
    author = db.relationship('User', back_populates='notes')
    
    # Tag bilan Many-to-Many bog'lanish
    tags = db.relationship('Tag', secondary=note_tags, backref=db.backref('notes', lazy='dynamic'))

    def __repr__(self):
        return f'<Note {self.title}>'

class Tag(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50), unique=True, nullable=False)

    def __repr__(self):
        return f'<Tag {self.name}>'
3. Flask Ilova va Marshrutlar (app.py)
Bosh sahifada barcha notalarni author va tags ma'lumotlari bilan birga yuklab olamiz.

Python
from flask import Flask, render_template
from models import db, User, Note, Tag

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///notes.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db.init_app(app)

@app.route('/')
def index():
    # N+1 muammosini oldini olish uchun author va tags'ni join oqrqali birdan yuklaymiz
    notes = Note.query.options(db.joinedload(Note.author), db.joinedload(Note.tags)).all()
    return render_template('index.html', notes=notes)

# Bazani yaratish uchun CLI komanda (flask create-db)
@app.cli.command("create-db")
def create_db():
    db.create_all()
    print("Ma'lumotlar bazasi muvaffaqiyatli yaratildi!")

if __name__ == '__main__':
    app.run(debug=True)
4. Flask Shell Skripti
So'ralganidek 3 ta user, 6 ta note va 4 ta tag yaratib, ularni o'zaro bog'laydigan skript. Buni terminalda flask shell ichida ishga tushirishingiz yoki alohida Python skript qilib ishlatsangiz ham bo'ladi.

Python
# Terminalda: flask shell
# Keyin quyidagi kodni nusxalab tashlang:

from app import app, db
from models import User, Note, Tag

with app.app_context():
    # 1. Bazani tozalash va qayta yaratish
    db.drop_all()
    db.create_all()

    # 2. 3 ta User yaratish
    u1 = User(username="ali")
    u2 = User(username="vali")
    u3 = User(username="guli")
    db.session.add_all([u1, u2, u3])

    # 3. 4 ta Tag yaratish
    t1 = Tag(name="Dasturlash")
    t2 = Tag(name="Shaxsiy")
    t3 = Tag(name="Eslatma")
    t4 = Tag(name="Motivatsiya")
    db.session.add_all([t1, t2, t3, t4])
    
    db.session.commit() # ID'larni olish uchun kommit qilamiz

    # 4. 6 ta Note yaratish va ularni Author hamda Taglar bilan bog'lash
    n1 = Note(title="Python o'rganish", content="Bugun Flask munosabatlarini o'rgandim.", author=u1)
    n1.tags.extend([t1, t3])

    n2 = Note(title="Bozorlik ro'yxati", content="Sut, qatiq va non olish kerak.", author=u1)
    n2.tags.append(t2)

    n3 = Note(title="Ertangi rejalar", content="Ertalab soat 6:00 da turish.", author=u2)
    n3.tags.extend([t2, t3])

    n4 = Note(title="Yangi g'oya", content="Sun'iy intellekt yordamida startap qilish.", author=u2)
    n4.tags.extend([t1, t4])

    n5 = Note(title="Kitob mutolaasi", content="Har kuni 20 sahifa kitob o'qish.", author=u3)
    n5.tags.extend([t3, t4])

    n6 = Note(title="Sport bilan shug'ullanish", content="Yugurish va turnik.", author=u3)
    n6.tags.append(t2)

    db.session.add_all([n1, n2, n3, n4, n5, n6])
    db.session.commit()

    print("3 ta user, 6 ta note va 4 ta tag muvaffaqiyatli yaratildi va bog'landi!")
5. Frontend (templates/index.html)
Bosh sahifada har bir notaning yonida uning muallifi (author.username) va unga tegishli barcha taglar chiroyli ko'rinishda chiqariladi.

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>Notelar va Mualliflar</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 40px; background-color: #f4f4f9; }
        .note-card { background: white; padding: 20px; margin-bottom: 15px; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        .meta-info { color: #666; font-size: 0.9em; margin-bottom: 10px; }
        .author { font-weight: bold; color: #333; }
        .tag { background: #e0f7fa; color: #006064; padding: 3px 8px; border-radius: 4px; font-size: 0.8em; margin-right: 5px; }
    </style>
</head>
<body>

    <h1>Barcha Notalar Ro'yxati</h1>
    
    {% for note in notes %}
        <div class="note-card">
            <h2>{{ note.title }}</h2>
            <div class="meta-info">
                Muallif: <span class="author">@{{ note.author.username }}</span>
            </div>
            <p>{{ note.content }}</p>
            <div>
                {% for tag in note.tags %}
                    <span class="tag">#{{ tag.name }}</span>
                {% endfor %}
            </div>
        </div>
    {% else %}
        <p>Hozircha hech qanday nota yo'q.</p>
    {% endfor %}

</body>
</html>
