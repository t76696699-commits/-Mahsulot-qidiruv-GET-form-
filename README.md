Besh kunlik muddat (deadline) ichida Flask va Jinja2 yordamida retseptlar loyihasini tayyorlash uchun tayyor andoza va kodlar tuzilmasi.

Quyidagi ko‘rsatmalarga rioya qilib fayllarni yaratsangiz, barcha talabalar bajarilgan to‘liq ishchi loyihaga ega bo‘lasiz.

Proyekt Tuzilmasi (File Structure)
Loyihangiz papkasida fayllarni quyidagicha joylashtiring:

Plaintext
recipe_project/
│
├── app.py
└── templates/
    ├── base.html
    ├── index.html
    └── detail.html
1. Backend: app.py
Bu yerda Flask ilovasi, kamida 5 ta retseptdan iborat ma'lumotlar bazasi (dict ro'yxati) va sahifalarga yo'naltiruvchi routing funksiyalari joylashgan.

Python
from flask import Flask, render_template, abort

app = Flask(__name__)

# Kamida 5 ta retseptdan iborat ma'lumotlar ro'yxati (dict)
RECIPES = [
    {
        "id": 1, 
        "nomi": "Palov (Osh)", 
        "tavsif": "O'zbek milliy taomlarining shohi. Guruch, go'sht va sabzining ajoyib uyg'unligi.", 
        "narxi": 45000, 
        "masalliqlar": ["Guruch", "Go'sht", "Sabzi", "Piyoz", "Yog'"]
    },
    {
        "id": 2, 
        "nomi": "Somsa", 
        "tavsif": "Tandirda yoki pechda pishirilgan, ichi go'sht va piyozli tansiq taom.", 
        "narxi": 8000, 
        "masalliqlar": ["Un", "Go'sht", "Dumba yog'i", "Piyoz", "Ziravorlar"]
    },
    {
        "id": 3, 
        "nomi": "Manti", 
        "tavsif": "Bug'da pishiriladigan, mayin xamir va sershira qiymali taom.", 
        "narxi": 6000, 
        "masalliqlar": ["Un", "Go'sht", "Piyoz", "Qovoq (ixtiyoriy)"]
    },
    {
        "id": 4, 
        "nomi": "Uydagidek Muzqaymoq", 
        "tavsif": "Issiq kunlarda muzdek va shirin retsept. Master-klass bepul taqdim etiladi.", 
        "narxi": 0,  # Narxi 0 bo'lgan bepul retsept
        "masalliqlar": ["Sut", "Qaymoq", "Shakar", "Vanilin"]
    },
    {
        "id": 5, 
        "nomi": "Shakarob (Achchik-chuchuk)", 
        "tavsif": "Palovning ajralmas yo'ldoshi bo'lgan eng oddiy va mazali salat.", 
        "narxi": 0,  # Narxi 0 bo'lgan yana bir bepul retsept
        "masalliqlar": ["Pomidor", "Piyoz", "Murch", "Tuz"]
    }
]

# Bosh sahifa: Barcha retseptlar ro'yxati
@app.route('/')
def index():
    return render_template('index.html', recipes=RECIPES)

# Detal sahifa: Har bir retsept uchun alohida /recipes/<id>
@app.route('/recipes/<int:recipe_id>')
def recipe_detail(recipe_id):
    # Ro'yxatdan ID bo'yicha retseptni qidiramiz
    recipe = next((r for r in RECIPES if r['id'] == recipe_id), None)
    
    # Agar retsept topilmasa, 404 xatolik qaytaramiz
    if recipe is None:
        abort(404)
        
    return render_template('detail.html', recipe=recipe)

if __name__ == '__main__':
    app.run(debug=True)
2. Frontend: Jinja2 Andoza Tizimi (Templates)
A. templates/base.html (Template Inheritance)
Barcha sahifalar uchun umumiy bo'lgan asosiy maket (navbar va dizayn).

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Retseptlar Kitobi{% endblock %}</title>
    <!-- Minimal dizayn uchun oddiy CSS styling -->
    <style>
        body { font-family: Arial, sans-serif; line-height: 1.6; margin: 0; padding: 20px; background-color: #f4f4f4; }
        .container { max-width: 800px; margin: auto; background: white; padding: 20px; border-radius: 8px; box-shadow: 0 0 10px rgba(0,0,0,0.1); }
        h1 { color: #333; }
        .recipe-card { border-bottom: 1px solid #ddd; padding: 15px 0; }
        .badge-free { background-color: #28a745; color: white; padding: 3px 8px; border-radius: 4px; font-size: 0.9em; }
        .price { font-weight: bold; color: #d9534f; }
        .btn { display: inline-block; background: #007bff; color: white; padding: 5px 10px; text-decoration: none; border-radius: 4px; margin-top: 5px; }
        .btn:hover { background: #0056b3; }
        .back-link { display: inline-block; margin-bottom: 20px; text-decoration: none; color: #007bff; }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>👨‍🍳 Mazali Retseptlar Olami</h1>
            <hr>
        </header>
        
        <main>
            {% block content %}
            <!-- Sahifalar tarkibi shu yerga joylashadi -->
            {% endblock %}
        </main>
    </div>
</body>
</html>
B. templates/index.html (Ro‘yxat sahifasi)
{% for %} tsikli va narxni tekshiruvchi {% if %} sharti bilan boyitilgan bosh sahifa.

HTML
{% extends "base.html" %}

{% block title %}Bosh sahifa - Retseptlar ro'yxati{% endblock %}

{% block content %}
    <h2>Bizning retseptlarimiz</h2>
    
    {% for recipe in recipes %}
        <div class="recipe-card">
            <h3>{{ recipe.nomi }}</h3>
            <p>{{ recipe.tavsif }}</p>
            
            <p>
                Narxi: 
                {# Narx 0 bo'lganda 'Bepul' deb chiqarish sharti #}
                {% if recipe.narxi == 0 %}
                    <span class="badge-free">Bepul Retsept</span>
                {% else %}
                    <span class="price">{{ recipe.narxi }} so'm</span>
                {% endif %}
            </p>
            
            <a href="/recipes/{{ recipe.id }}" class="btn">Retseptni ko'rish →</a>
        </div>
    {% endfor %}
{% endblock %}
C. templates/detail.html (Detali sahifa)
Har bir retsept uchun alohida ma'lumotlarni ko'rsatuvchi ichki sahifa.

HTML
{% extends "base.html" %}

{% block title %}{{ recipe.nomi }} - Retsept Tafsiloti{% endblock %}

{% block content %}
    <a href="/" class="back-link">← Bosh sahifaga qaytish</a>
    
    <h2>{{ recipe.nomi }}</h2>
    <p><strong>Tavsif:</strong> {{ recipe.tavsif }}</p>
    
    <p>
        <strong>Kategoriya narxi:</strong> 
        {% if recipe.narxi == 0 %}
            <span class="badge-free">Bepul kirish</span>
        {% else %}
            <span class="price">{{ recipe.narxi }} so'm</span>
        {% endif %}
    </p>

    <h3>Zaruriy masalliqlar:</h3>
    <ul>
        {% for ing in recipe.masalliqlar %}
            <li>{{ ing }}</li>
        {% endfor %}
    </ul>
{% endblock %}
