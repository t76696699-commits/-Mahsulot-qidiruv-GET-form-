from flask import Flask, request, jsonify, url_for
from flask_sqlalchemy import SQLAlchemy
from sqlalchemy import or_, desc, asc

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///posts.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
db = SQLAlchemy(app)

# Model
class Post(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100), nullable=False)
    body = db.Column(db.Text, nullable=False)
    created_at = db.Column(db.DateTime, default=db.func.current_timestamp())

    def to_dict(self, fields=None):
        """Field selection (Bonus) uchun helper metod"""
        data = {
            'id': self.id,
            'title': self.title,
            'body': self.body,
            'created_at': self.created_at.isoformat() if self.created_at else None
        }
        if fields:
            # Faqat so'ralgan fieldlarni qaytaramiz
            return {k: v for k, v in data.items() if k in fields}
        return data

# Route
@app.route('/api/posts', methods=['get'])
def get_posts():
    # 1. Input Validation & Default values
    try:
        page = int(request.args.get('page', 1))
        per_page = int(request.args.get('per_page', 20))
        if per_page > 50:  # Max 50 cheklovi
            per_page = 50
        if page < 1 or per_page < 1:
            raise ValueError
    except ValueError:
        return jsonify({'error': 'Invalid page or per_page parameters'}), 400

    q = request.args.get('q', '').strip()
    sort_by = request.args.get('sort', 'id')
    order = request.args.get('order', 'desc').lower()
    fields_raw = request.args.get('fields', '')

    # 2. Whitelist validation (SQL injection oldini olish)
    allowed_sorts = ['created_at', 'title', 'id']
    if sort_by not in allowed_sorts:
        sort_by = 'id'  # fallback yoki 400 error qaytarish mumkin

    if order not in ['asc', 'desc']:
        order = 'desc'

    # Field selection uchun whitelist
    fields = [f.strip() for f in fields_raw.split(',')] if fields_raw else None

    # 3. Query qurish (Qidiruv qismi)
    query = Post.query
    if q:
        query = query.filter(or_(Post.title.ilike(f'%{q}%'), Post.body.ilike(f'%{q}%')))

    # 4. Saralash (Sorting)
    direction = desc if order == 'desc' else asc
    query = query.order_by(direction(getattr(Post, sort_by)))

    # 5. Paginate
    pagination = query.paginate(page=page, per_page=per_page, error_out=False)

    # 6. To'liq tashqi URL (Links) yaratish uchun helper
    def generate_url(p):
        if p is None:
            return None
        return url_for('get_posts', page=p, per_page=per_page, q=q, sort=sort_by, order=order, fields=fields_raw, _external=True)

    # 7. JSON Response shakllantirish
    response_data = {
        'items': [item.to_dict(fields) for item in pagination.items],
        'meta': {
            'page': pagination.page,
            'per_page': pagination.per_page,
            'total': pagination.total,
            'pages': pagination.pages,
            'has_next': pagination.has_next,
            'has_prev': pagination.has_prev
        },
        'links': {
            'self': generate_url(pagination.page),
            'next': generate_url(pagination.next_num) if pagination.has_next else None,
            'prev': generate_url(pagination.prev_num) if pagination.has_prev else None
        }
    }

    return jsonify(response_data)

if __name__ == '__main__':
    # DB yaratish va test ma'lumotlar qo'shish
    with app.app_context():
        db.create_all()
        if not Post.query.first():
            db.session.add_all([
                Post(title="Python haqida", body="Python juda zo'r til."),
                Post(title="Flask darslari", body="Flask yordamida API yozamiz."),
                Post(title="SQLAlchemy qo'llanma", body="ORM orqali bazaga xavfsiz ulanish."),
            ])
            db.session.commit()
    app.run(debug=True)
