Loyiha strukturasi
Plaintext
my_flask_app/
│
├── app/
│   └── __init__.py
├── config.py
├── wsgi.py
├── .env
└── README.md
1. config.py
Konfiguratsiyalar klasslar koʻrinishida yozildi. SECRET_KEY esa xavfsizlik nuqtai nazaridan .env faylidan yoki tizim muhitidan (os.environ) oʻqib olinadi.

Python
import os
from dotenv import load_dotenv

# .env faylini yuklash
load_dotenv()

class Config:
    """Asosiy (baza) konfiguratsiya klassi"""
    SECRET_KEY = os.environ.get('SECRET_KEY', 'default-very-secret-key-12345')
    FLASK_RUN_PORT = 5000

class DevelopmentConfig(Config):
    """Development (Ishlab chiqish) rejimi"""
    DEBUG = True
    ENV = 'development'

class TestingConfig(Config):
    """Testing (Testlash) rejimi"""
    TESTING = True
    DEBUG = True

class ProductionConfig(Config):
    """Production (Jonli efir) rejimi"""
    DEBUG = False
    TESTING = False
    ENV = 'production'

# Rejimlarni matnli kalitlar orqali chaqirish uchun lug'at
config_by_name = {
    'development': DevelopmentConfig,
    'testing': TestingConfig,
    'production': ProductionConfig
}
2. app/__init__.py
Application Factory (create_app) funksiyasi yaratildi. Standart holatda u development rejimini qabul qiladi.

Python
from flask import Flask
from config import config_by_name

def create_app(config_name='development'):
    app = Flask(__name__)
    
    # Konfiguratsiyani yuklash
    app.config.from_object(config_by_name[config_name])
    
    # Oddiy endpoint (tekshirish uchun)
    @app.route('/')
    def index():
        return f"Hello! App is running in **{app.config['ENV']}** mode."
        
    return app
3. wsgi.py
Faqatgina factory funksiyasini chaqirib, dasturni ishga tushiruvchi eng sodda va toʻgʻri koʻrinish (jami 4 qator).

Python
import os
from app import create_app

# Muhitdan rejimni oladi (agar berilmagan bo'lsa, development)
env = os.environ.get('FLASK_ENV', 'development')
app = create_app(env)
4. .env (Namuna)
Loyiha ildiz papkasida .env nomli fayl ochib, maxfiy kalitni joylashtiring:

Фрагмент кода
SECRET_KEY=SizningJudaHamMaxfiyKalitingiz123!
5. README.md
Loyiha qanday ishga tushishi va rejimlar oʻrtasidagi farqlar tushuntirilgan qoʻllanma.

Markdown
# Flask App Factory Project

Ushbu loyiha Flask'ning eng to'g'ri arxitektura strukturalaridan biri bo'lgan **Application Factory** patterni asosida qurilgan.

## Ishga tushirish (Running the Application)

Loyihani ikki xil rejimda ishga tushirish mumkin: **Development** va **Production**. Ularning asosiy farqi muhit o'zgaruvchilari (environment variables) va server tanlovidadir.

---

### 1. Development (Ishlab chiqish rejimi)
Bu rejim dasturchi kod yozayotgan jarayon uchun mo'ljallangan. Unda **Hot Reload** (kod o'zgarganda server avtomat yangilanishi) va **Debug Mode** (xatoliklarni brauzerda ko'rsatish) faol bo'ladi.

**Ishga tushirish buyruqlari:**
```bash
# Terminalda muhitni o'rnatish
export FLASK_ENV=development  # Linux/macOS
set FLASK_ENV=development     # Windows (CMD)

# Flask'ning ichki serverida ishga tushirish
flask run
2. Production (Jonli efir rejimi)
Bu rejim dastur tayyor bo'lib, serverga (jonli efirga) yuklanganda ishlatiladi. Unda xavfsizlik va tezlik uchun Debug Mode o'chiriladi. Shuningdek, Flask'ning ichki serveri o'rniga WSGI serverdan (Gunicorn yoki uWSGI) foydalaniladi.

Ishga tushirish buyruqlari:

Bash
# Terminalda muhitni o'rnatish
export FLASK_ENV=production

# Gunicorn (WSGI server) orqali wsgi.py faylini ishga tushirish
gunicorn wsgi:app
Rejimlar orasidagi asosiy farqlar (Tabela)
Xususiyat	Development	Production
Debug Mode	True (Yoqilgan)	False (O'chirilgan)
Xatoliklar (Errors)	Brauzerda batafsil ko'rinadi	Maxfiy qoladi (500 Internal Error)
Server	Flask built-in server (Faqat test uchun)	Gunicorn / uWSGI (Yuqori yuklamalar uchun)
Kod o'zgarishi	Server avtomat o'zini yangilaydi	Serverni qo'lda qayta start qilish kerak
