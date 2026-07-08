Mana, loyihangiz uchun barcha REST API standartlariga, xavfsizlik qoidalariga va ko'rsatilgan texnologik stekka to'liq javob beradigan kodlar to'plami.

Tizimda Web UI va API alohida ishlashi uchun Blueprint arxitekturasidan foydalanildi. Xatoliklar yuz berganda API har doim HTML emas, toza JSON qaytaradi.

1. API Blueprint kodi (api.py)
Ushbu faylda barcha API endpointlari, request.get_json(silent=True) parsi, validatsiya mantiqi va JSON xatoliklar qaytaruvchi app_errorhandler o'rin olgan.

Python
from flask import Blueprint, jsonify, request, abort, url_for

api_bp = Blueprint('api', __name__, url_prefix='/api')

# Mock ma'lumotlar bazasi (Loyiha turi va testlar uchun)
posts_db = {
    1: {"id": 1, "title": "Birinchi post", "content": "Bu birinchi post mazmuni."},
    2: {"id": 2, "title": "Ikkinchi post", "content": "Bu ikkinchi post mazmuni."}
}
current_id = 2

# -------------------------------------------------------------
# GLOBAL API ERROR HANDLERS (JSON javob qaytaradi)
# -------------------------------------------------------------
@api_bp.app_errorhandler(400)
def bad_request(error):
    # Agar xatolik obyektida maxsus xabar bo'lsa, o'shani qaytaramiz
    message = getattr(error, 'description', 'Bad request')
    if isinstance(message, dict) and 'error' in message:
        return jsonify(message), 400
    return jsonify({"error": message}), 400

@api_bp.app_errorhandler(404)
def not_found(error):
    return jsonify({"error": "Resource not found"}), 404

@api_bp.app_errorhandler(500)
def internal_server_error(error):
    return jsonify({"error": "Internal server error"}), 500


# -------------------------------------------------------------
# ENDPOINTS
# -------------------------------------------------------------

# GET /api/posts — barcha postlar
@api_bp.route('/posts', methods=['GET'])
def get_posts():
    return jsonify(list(posts_db.values())), 200


# GET /api/posts/<id> — bitta post
@api_bp.route('/posts/<int:post_id>', methods=['GET'])
def get_post(post_id):
    post = posts_db.get(post_id)
    if not post:
        abort(404)
    return jsonify(post), 200


# POST /api/posts — yangi post
@api_bp.route('/posts', methods=['POST'])
def create_post():
    data = request.get_json(silent=True)
    
    # JSON noto'g'ri bo'lsa yoki umuman yuborilmagan bo'lsa
    if data is None:
        abort(400, description="Invalid JSON format")
        
    # Title validatsiyasi
    title = data.get('title', '').strip() if data.get('title') else ''
    if not title:
        abort(400, description={'error': 'title required'})
        
    global current_id
    current_id += 1
    
    new_post = {
        "id": current_id,
        "title": title,
        "content": data.get('content', '')
    }
    posts_db[current_id] = new_post
    
    # Location header bilan 201 Created javobi
    response = jsonify(new_post)
    response.headers['Location'] = url_for('api.get_post', post_id=current_id, _external=True)
    return response, 201


# PUT /api/posts/<id> — yangilash
@api_bp.route('/posts/<int:post_id>', methods=['PUT'])
def update_post(post_id):
    if post_id not in posts_db:
        abort(404)
        
    data = request.get_json(silent=True)
    if data is None:
        abort(400, description="Invalid JSON format")
        
    title = data.get('title', '').strip() if data.get('title') else ''
    if not title:
        abort(400, description={'error': 'title required'})
        
    posts_db[post_id]['title'] = title
    posts_db[post_id]['content'] = data.get('content', '')
    
    return jsonify(posts_db[post_id]), 200


# DELETE /api/posts/<id> — o'chirish
@api_bp.route('/posts/<int:post_id>', methods=['DELETE'])
def delete_post(post_id):
    if post_id not in posts_db:
        abort(404)
        
    del posts_db[post_id]
    return '', 204
2. Asosiy Ilova Fayli (app.py)
Bu yerda asosiy Flask ilovasi yaratiladi, API Blueprint ro'yxatdan o'tkaziladi va mavjud Web UI brauzer uchun odatiy rejimda ishlashda davom etadi.

Python
from flask import Flask, render_template
from api import api_bp

app = Flask(__name__)

# API Blueprint'ni ro'yxatdan o'tkazish
app.register_blueprint(api_bp)

# -------------------------------------------------------------
# WEB UI (Mavjud CRUD sahifalari ishlashda davom etadi)
# -------------------------------------------------------------
@app.route('/')
def index():
    return "<h1>Web UI: Bosh sahifa (CRUD shu yerda ishlaydi)</h1>"

@app.route('/posts/<int:id>')
def web_post_detail(id):
    return f"<h1>Web UI: Post {id} tafsilotlari</h1>"

if __name__ == '__main__':
    app.run(debug=True, port=5000)
3. Hujjatlashtirish (README.md)
Har bir endpointni terminal orqali tekshirish uchun tayyor curl buyruqlari:

Markdown
# REST API Dokumentatsiyasi

Barcha so'rovlar uchun `Content-Type: application/json` header'i yuborilishi shart (POST va PUT so'rovlarida).

### 1. Barcha postlarni olish
* **Metod:** `GET`
* **URL:** `http://127.0.0.1:5000/api/posts`
* **Status:** 200 OK
```bash
curl -X GET [http://127.0.0.1:5000/api/posts](http://127.0.0.1:5000/api/posts)
2. Bitta postni olish
Metod: GET

URL: http://127.0.0.1:5000/api/posts/1

Status: 200 OK yoki 404 Not Found

Bash
curl -X GET [http://127.0.0.1:5000/api/posts/1](http://127.0.0.1:5000/api/posts/1)
3. Yangi post yaratish
Metod: POST

URL: http://127.0.0.1:5000/api/posts

Status: 201 Created (+ Location header)

Bash
curl -X POST [http://127.0.0.1:5000/api/posts](http://127.0.0.1:5000/api/posts) \
     -H "Content-Type: application/json" \
     -d '{"title": "Yangi Texnologiyalar", "content": "Flask API haqida maqola."}'
Xatolik testi (title bo'sh bo'lsa - 400 Bad Request):

Bash
curl -X POST [http://127.0.0.1:5000/api/posts](http://127.0.0.1:5000/api/posts) \
     -H "Content-Type: application/json" \
     -d '{"title": "", "content": "Kontent bor lekin sarlavha yo'q"}'
4. Postni yangilash
Metod: PUT

URL: http://127.0.0.1:5000/api/posts/1

Status: 200 OK

Bash
curl -X PUT [http://127.0.0.1:5000/api/posts/1](http://127.0.0.1:5000/api/posts/1) \
     -H "Content-Type: application/json" \
     -d '{"title": "Yangilangan Sarlavha", "content": "Yangi kontent matni."}'
