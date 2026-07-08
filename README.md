📝 1. app/forms.py - WTForms Klasslari
Barcha so'ralgan formalar va validatorlar shu yerda joylashgan. SearchForm uchun CSRF himoyasi o'chirib qo'yilgan, chunki u GET so'rovi orqali ishlaydi.

Python
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField, BooleanField, TextAreaField, SubmitField
from wtforms.validators import DataRequired, Email, Length, EqualTo, ValidationError
from app.models import User

class RegisterForm(FlaskForm):
    username = StringField('Username', validators=[
        DataRequired(message="Username kiritilishi shart!"),
        Length(min=3, max=50, message="Username 3 va 50 ta belgi oralig'ida bo'lishi kerak.")
    ])
    email = StringField('Email', validators=[
        DataRequired(message="Email kiritilishi shart!"),
        Email(message="To'g'ri email manzili kiriting.")
    ])
    password = PasswordField('Parol', validators=[
        DataRequired(message="Parol kiritilishi shart!"),
        Length(min=8, message="Parol kamida 8 ta belgidan iborat bo'lishi kerak.")
    ])
    confirm_password = PasswordField('Parolni tasdiqlang', validators=[
        DataRequired(message="Parolni qayta kiriting!"),
        EqualTo('password', message="Parollar bir-biriga mos kelmadi.")
    ])
    submit = SubmitField("Ro'yxatdan o'tish")

    # Maxsus validatorlar (Custom Validators)
    def validate_username(self, username):
        user = User.query.filter_by(username=username.data).first()
        if user:
            raise ValidationError("Ushbu username allaqachon band qilingan!")

    def validate_email(self, email):
        user = User.query.filter_by(email=email.data).first()
        if user:
            raise ValidationError("Ushbu email manzili allaqachon ro'yxatdan o'tgan!")


class LoginForm(FlaskForm):
    login_input = StringField('Username yoki Email', validators=[
        DataRequired(message="Ushbu maydonni to'ldirish shart!")
    ])
    password = PasswordField('Parol', validators=[
        DataRequired(message="Parolni kiriting!")
    ])
    remember = BooleanField('Meni eslab qol')
    submit = SubmitField('Kirish')


class PostForm(FlaskForm):
    title = StringField('Sarlavha', validators=[
        DataRequired(message="Post sarlavhasi bo'sh bo'lishi mumkin emas!"),
        Length(max=100)
    ])
    content = TextAreaField('Kontent', validators=[
        DataRequired(message="Post matni bo'sh bo'lishi mumkin emas!")
    ])
    submit = SubmitField('Saqlash')


class SearchForm(FlaskForm):
    # GET form bo'lgani uchun meta orqali CSRF o'chiriladi
    class Meta:
        csrf = False

    q = StringField('Qidiruv', validators=[
        DataRequired(message="Qidiruv so'zini kiriting.")
    ], render_kw={"placeholder": "Postlarni qidirish..."})
🚀 2. app/routes/ - Yangilangan Blueprintlar
Endi tekshiruvlar form.validate_on_submit() orqali amalga oshadi, qo'lda yozilgan request.form.get() kodlari olib tashlangan.

app/routes/auth.py
Python
from flask import Blueprint, render_template, redirect, url_for, request, flash
from flask_login import login_user, logout_user, login_required, current_user
from app.models import User
from app.forms import RegisterForm, LoginForm
from app import db

auth_bp = Blueprint('auth', __name__)

@auth_bp.route('/register', methods=['GET', 'POST'])
def register():
    if current_user.is_authenticated:
        return redirect(url_for('posts.index'))
    
    form = RegisterForm()
    if form.validate_on_submit(): # Avtomatik tarzda maxsus validatorlarni ham ishga tushiradi
        user = User(username=form.username.data, email=form.email.data)
        user.set_password(form.password.data)
        db.session.add(user)
        db.session.commit()
        flash("Ro'yxatdan muvaffaqiyatli o'tdingiz!", "success")
        return redirect(url_for('auth.login'))
        
    return render_template('register.html', form=form)

@auth_bp.route('/login', methods=['GET', 'POST'])
def login():
    if current_user.is_authenticated:
        return redirect(url_for('posts.index'))

    form = LoginForm()
    if form.validate_on_submit():
        user = User.query.filter(
            (User.email == form.login_input.data) | (User.username == form.login_input.data)
        ).first()

        if user and user.check_password(form.password.data):
            login_user(user, remember=form.remember.data)
            next_page = request.args.get('next')
            return redirect(next_page) if next_page else redirect(url_for('posts.index'))
        else:
            flash("Login yoki parol xato!", "danger")

    return render_template('login.html', form=form)
app/routes/posts.py (Qidiruv tizimi qo'shildi)
Python
from flask import Blueprint, render_template, redirect, url_for, request, abort, flash
from flask_login import login_required, current_user
from app.models import Post
from app.forms import PostForm, SearchForm
from app import db

posts_bp = Blueprint('posts', __name__)

# SearchForm context_processor orqali har doim navbar'ga yetib boradi
@posts_bp.context_processor
def inject_search_form():
    return dict(search_form=SearchForm(request.args))

@posts_bp.route('/')
def index():
    q = request.args.get('q')
    if q:
        # Qidiruv natijalarini filterlash
        posts = Post.query.filter(Post.title.contains(q) | Post.content.contains(q)).all()
    else:
        posts = Post.query.all()
    return render_template('base.html', posts=posts)

@posts_bp.route('/new', methods=['GET', 'POST'])
@login_required
def new_post():
    form = PostForm()
    if form.validate_on_submit():
        post = Post(title=form.title.data, content=form.content.data, author=current_user)
        db.session.add(post)
        db.session.commit()
        flash("Post muvaffaqiyatli yaratildi!", "success")
        return redirect(url_for('posts.index'))
    return render_template('post_form.html', legend="Yangi Post yaratish", form=form)

@posts_bp.route('/<int:id>/edit', methods=['GET', 'POST'])
@login_required
def edit_post(id):
    post = Post.query.get_or_404(id)
    if post.author != current_user and not current_user.is_admin:
        abort(403)
        
    form = PostForm(obj=post) # Post ma'lumotlarini formaga avtomatik yuklash
    if form.validate_on_submit():
        post.title = form.title.data
        post.content = form.content.data
        db.session.commit()
        flash("Post yangilandi!", "success")
        return redirect(url_for('posts.index'))
        
    return render_template('post_form.html', legend="Postni tahrirlash", form=form)
🎨 3. HTML Shablonlar (WTForms va CSRF bilan)
Har bir shablonda {{ form.hidden_tag() }} qo'shilgan va xatoliklar bevosita input maydonlarining tagida chiqadi.

app/templates/base.html (Navbarda SearchForm integratsiyasi)
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">
</head>
<body class="container mt-4">
    <nav class="navbar navbar-expand-lg navbar-light bg-light mb-4 p-3 rounded">
        <a class="navbar-brand" href="{{ url_for('posts.index') }}">My Blog</a>
        
        <form method="GET" action="{{ url_for('posts.index') }}" class="d-flex ms-3">
            {{ search_form.q(class="form-control form-control-sm me-2") }}
            <button class="btn btn-outline-success btn-sm" type="submit">Qidirish</button>
        </form>

        <div class="navbar-nav ms-auto">
            {% if current_user.is_authenticated %}
                <span class="navbar-text me-3">Salom, <b>{{ current_user.username }}</b></span>
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

    {% block content %}{% endblock %}
</body>
</html>
app/templates/register.html (Xatolar maydon ostida)
HTML
{% extends "base.html" %}
{% block content %}
<div class="row justify-content-center">
    <div class="col-md-6">
        <h2>Ro'yxatdan o'tish</h2>
        <form method="POST">
            {{ form.hidden_tag() }} <div class="mb-3">
                {{ form.username.label(class="form-label") }}
                {{ form.username(class="form-control " + ("is-invalid" if form.username.errors else "")) }}
                {% for error in form.username.errors %}
                    <div class="invalid-feedback">{{ error }}</div>
                {% endfor %}
            </div>

            <div class="mb-3">
                {{ form.email.label(class="form-label") }}
                {{ form.email(class="form-control " + ("is-invalid" if form.email.errors else "")) }}
                {% for error in form.email.errors %}
                    <div class="invalid-feedback">{{ error }}</div>
                {% endfor %}
            </div>

            <div class="mb-3">
                {{ form.password.label(class="form-label") }}
                {{ form.password(class="form-control " + ("is-invalid" if form.password.errors else "")) }}
                {% for error in form.password.errors %}
                    <div class="invalid-feedback">{{ error }}</div>
                {% endfor %}
            </div>

            <div class="mb-3">
                {{ form.confirm_password.label(class="form-label") }}
                {{ form.confirm_password(class="form-control " + ("is-invalid" if form.confirm_password.errors else "")) }}
                {% for error in form.confirm_password.errors %}
                    <div class="invalid-feedback">{{ error }}</div>
                {% endfor %}
            </div>

            {{ form.submit(class="btn btn-primary") }}
        </form>
    </div>
</div>
{% endblock %}
app/templates/login.html
HTML
{% extends "base.html" %}
{% block content %}
<div class="row justify-content-center">
    <div class="col-md-6">
        <h2>Tizimga kirish</h2>
        <form method="POST">
            {{ form.hidden_tag() }}
            
            <div class="mb-3">
                {{ form.login_input.label(class="form-label") }}
                {{ form.login_input(class="form-control " + ("is-invalid" if form.login_input.errors else "")) }}
                {% for error in form.login_input.errors %}
                    <div class="invalid-feedback">{{ error }}</div>
                {% endfor %}
            </div>

            <div class="mb-3">
                {{ form.password.label(class="form-label") }}
                {{ form.password(class="form-control " + ("is-invalid" if form.password.errors else "")) }}
                {% for error in form.password.errors %}
                    <div class="invalid-feedback">{{ error }}</div>
                {% endfor %}
            </div>

            <div class="mb-3 form-check">
                {{ form.remember(class="form-check-input") }}
                {{ form.remember.label(class="form-check-label") }}
            </div>

            {{ form.submit(class="btn btn-primary") }}
        </form>
    </div>
</div>
{% endblock %}
app/templates/post_form.html
HTML
{% extends "base.html" %}
{% block content %}
<h2>{{ legend }}</h2>
<form method="POST">
    {{ form.hidden_tag() }}
    
    <div class="mb-3">
        {{ form.title.label(class="form-label") }}
        {{ form.title(class="form-control " + ("is-invalid" if form.title.errors else "")) }}
        {% for error in form.title.errors %}
            <div class="invalid-feedback">{{ error }}</div>
        {% endfor %}
    </div>

    <div class="mb-3">
        {{ form.content.label(class="form-label") }}
        {{ form.content(class="form-control " + ("is-invalid" if form.content.errors else ""), rows="5") }}
        {% for error in form.content.errors %}
            <div class="invalid-feedback">{{ error }}</div>
        {% endfor %}
    </div>

    {{ form.submit(class="btn btn-success") }}
</form>
{% endblock %}
