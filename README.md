# ════════════════════════════════════════════════════════════════════
# DARS 2: .gitignore, log, diff
# Maqsad: keraksiz fayllarni yashirish + tarix bilan ishlash
# ════════════════════════════════════════════════════════════════════

# ─────────────────────────────────────────────────────────────────────
# 1) .gitignore yaratish
# ─────────────────────────────────────────────────────────────────────

cd mening-loyiham

# .gitignore — pattern'lar
cat > .gitignore <<EOF
# Secrets
.env
.env.*
!.env.example
*.key
*.pem

# Dependencies
node_modules/
venv/
__pycache__/
*.pyc

# IDE
.vscode/
.idea/
*.swp

# OS
.DS_Store
Thumbs.db

# Logs and builds
*.log
dist/
build/
EOF

git add .gitignore
git commit -m "chore: .gitignore qo'shildi"

# ─────────────────────────────────────────────────────────────────────
# 2) Sinash — secret fayl
# ─────────────────────────────────────────────────────────────────────

echo "SECRET_KEY=abc123-very-secret" > .env

git status
# (.env ko'rinmaydi! gitignore ishladi)

# Tasodifan add qilishga urinsangiz:
git add .env
# Bu xato chiqarmaydi, lekin status'da ko'rinmaydi
# (gitignore'dagi fayllar add qilinmaydi)

# Tekshirish — Git nima ko'radi
git check-ignore -v .env
# .gitignore:2:.env  .env

# ─────────────────────────────────────────────────────────────────────
# 3) Tarix uchun ko'p commit yaratamiz
# ─────────────────────────────────────────────────────────────────────

# Commit 1
echo "def salom(ism): return f'Salom, {ism}!'" > main.py
git add main.py
git commit -m "feat: salom() funksiyasi qo'shildi"

# Commit 2
echo "def xayr(ism): return f'Xayr, {ism}!'" >> main.py
git add main.py
git commit -m "feat: xayr() funksiyasi qo'shildi"

# Commit 3
echo "if __name__ == '__main__': print(salom('Dunyo'))" >> main.py
git add main.py
git commit -m "feat: main blok qo'shildi"

# Commit 4
sed -i.bak "s/Salom/Assalomu alaykum/g" main.py
rm main.py.bak
git add main.py
git commit -m "refactor: rasmiyroq salomlashish"

# ─────────────────────────────────────────────────────────────────────
# 4) Log — turli ko'rinishlarda
# ─────────────────────────────────────────────────────────────────────

# Oddiy
git log

# Bir qatorli
git log --oneline

# Grafik bilan
git log --oneline --graph --all

# Filtr
git log --author="Olim"
git log --since="1 hour ago"
git log --grep="feat"
git log -n 3                  # oxirgi 3

# To'liq diff bilan
git log -p

# Faqat fayl statistika
git log --stat

# Alias yaratamiz — keyingilar uchun qulay
git config --global alias.lg "log --oneline --graph --decorate --all"

git lg

# ─────────────────────────────────────────────────────────────────────
# 5) Diff — nima o'zgargan?
# ─────────────────────────────────────────────────────────────────────

# Yangi o'zgarish (hali add'siz)
echo "# Yangi komment" >> main.py

git diff
# - if __name__ == '__main__': print(salom('Dunyo'))
# + if __name__ == '__main__': print(salom('Dunyo'))
# + # Yangi komment

# Add qilamiz
git add main.py

git diff             # bo'sh — working = staging
git diff --staged    # endi farq ko'rinadi
git diff HEAD        # working vs oxirgi commit (hammasi)

# Commit qilamiz
git commit -m "docs: komment qo'shildi"

# ─────────────────────────────────────────────────────────────────────
# 6) Show — bir commit batafsil
# ─────────────────────────────────────────────────────────────────────

# Oxirgi
git show

# 2 oldingi
git show HEAD~2

# Belgilangan SHA bilan
git show abc1234

# Aniq commit'dagi fayl mazmuni
git show HEAD:main.py

# ─────────────────────────────────────────────────────────────────────
# 7) Blame — qaysi qator kim tomonidan
# ─────────────────────────────────────────────────────────────────────

git blame main.py
# abc1234 (Olim 2026-06-08) def salom(ism):
# def5678 (Olim 2026-06-08) def xayr(ism):
# ...

# Aniq qatorlar (10-20)
git blame -L 10,20 main.py

# ─────────────────────────────────────────────────────────────────────
# 8) Tarix qidirish — log bilan
# ─────────────────────────────────────────────────────────────────────

# "salom" so'zi qaysi commit'da paydo bo'lgan?
git log -S "salom" --oneline

# Funksiya tarixini ko'rsatish
git log -L :salom:main.py

# ─────────────────────────────────────────────────────────────────────
# 9) Allaqachon kuzatilgan faylni ignore qilish
# ─────────────────────────────────────────────────────────────────────

# Faraz: config.json allaqachon commit qilingan
echo "config.json" >> .gitignore

# Tarixdan emas, hozirgi kuzatuvdan olib tashlash
# git rm --cached config.json

# Endi commit qilsangiz, Git uni unutadi
# git commit -m "chore: config.json endi gitignore'da"

# ─────────────────────────────────────────────────────────────────────
# 10) Global .gitignore (kompyuteringizdagi har repo uchun)
# ─────────────────────────────────────────────────────────────────────

git config --global core.excludesfile ~/.gitignore_global

cat >> ~/.gitignore_global <<EOF
.DS_Store
.vscode/
.idea/
Thumbs.db
EOF
