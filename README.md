# ════════════════════════════════════════════════════════════════════
# REVISION 1: Kunlik commit jurnal
# Modul 1: init + gitignore + add + commit + branch + merge
# ════════════════════════════════════════════════════════════════════

# ─────────────────────────────────────────────────────────────────────
# Vazifa 1: loyihani boshlash
# ─────────────────────────────────────────────────────────────────────

mkdir kundalik && cd kundalik

git init

cat > README.md <<EOF
# Kundalik

Mening kundalik commit jurnali. Git bilan o'rganaman.

## Struktura

- 2026/MM/DD.md — har kun yozuv
- shablonlar/ — kun shabloni
EOF

cat > .gitignore <<EOF
# OS
.DS_Store
Thumbs.db

# Editor
.vscode/
.idea/
*.swp

# Vaqtinchalik
*.tmp
*.bak
EOF

git add .
git commit -m "chore: loyiha boshlash"

# ─────────────────────────────────────────────────────────────────────
# Vazifa 2: shablon
# ─────────────────────────────────────────────────────────────────────

mkdir shablonlar

cat > shablonlar/kun-shabloni.md <<'EOF'
# <Sana>

## 🎯 Bugun nima qildim
-

## 🐛 Qiyinchiliklar
-

## 💡 Yangi o'rganganlar
-

## 📋 Ertaga
-
EOF

git add shablonlar/
git commit -m "feat: kun shabloni yaratildi"

# ─────────────────────────────────────────────────────────────────────
# Vazifa 3: feature/teglar branch
# ─────────────────────────────────────────────────────────────────────

git switch -c feature/teglar

# Shablonga teglar bo'limi
cat >> shablonlar/kun-shabloni.md <<'EOF'

## 🏷️ Teglar
-
EOF

# README ga teglar
cat >> README.md <<'EOF'

## Teglar

Har yozuvda foydalanish mumkin:
- `#kod` — dasturlash bilan bog'liq
- `#kitob` — o'qigan
- `#ish` — ish vazifalari
- `#sport` — jismoniy tarbiya
EOF

git add .
git commit -m "feat(teglar): teglar tizimi qo'shildi"

# main'ga qaytib merge
git switch main
git merge feature/teglar
git branch -d feature/teglar

# Tekshirish
git log --oneline --graph --all

# ─────────────────────────────────────────────────────────────────────
# Vazifa 4: 5 kunlik yozuv
# ─────────────────────────────────────────────────────────────────────

mkdir -p 2026/06

for kun in 08 09 10 11 12; do
  cp shablonlar/kun-shabloni.md 2026/06/$kun.md
  # macOS sed
  sed -i.bak "s/<Sana>/2026-06-$kun/" 2026/06/$kun.md
  rm 2026/06/$kun.md.bak
  git add 2026/06/$kun.md
  git commit -m "docs(daily): $kun-iyun yozuv"
done

# Linux sed (agar -i.bak ishlamasa):
# sed -i "s/<Sana>/2026-06-$kun/" 2026/06/$kun.md

# ─────────────────────────────────────────────────────────────────────
# Vazifa 5: tahrir + diff
# ─────────────────────────────────────────────────────────────────────

# 08.md ga to'liq mazmun yozamiz
cat > 2026/06/08.md <<'EOF'
# 2026-06-08

## 🎯 Bugun nima qildim
- Git va GitHub kursini boshlashdim
- Birinchi commit qildim — juda zo'r tuyildi!
- 3 ta branch yaratdim va merge qildim

## 🐛 Qiyinchiliklar
- `git add` ni unutib `commit` qilishga urinardim
- Branch o'zgartirishda "uncommitted changes" xato chiqdi

## 💡 Yangi o'rganganlar
- Conventional Commits formati: `feat:`, `fix:`, `chore:`
- `git log --oneline --graph` — chiroyli tarix
- .gitignore qoidalari

## 📋 Ertaga
- GitHub account ochish
- SSH key sozlash

## 🏷️ Teglar
- #kod #git #boshlanish
EOF

# Diff'ni ko'rish
git diff 2026/06/08.md

# Commit
git add 2026/06/08.md
git commit -m "docs(daily): 08-iyun yozuv to'ldirildi"

# ─────────────────────────────────────────────────────────────────────
# Vazifa 6: tarix tahlili
# ─────────────────────────────────────────────────────────────────────

cat > tarix-tahlili.md <<'EOF'
# Tarix tahlili

## Jami commit'lar
EOF

echo '```' >> tarix-tahlili.md
git log --oneline | wc -l >> tarix-tahlili.md
echo '```' >> tarix-tahlili.md

cat >> tarix-tahlili.md <<'EOF'

## Statistika
EOF

echo '```' >> tarix-tahlili.md
git log --stat | tail -50 >> tarix-tahlili.md
echo '```' >> tarix-tahlili.md

git add tarix-tahlili.md
git commit -m "docs: tarix tahlili"

# ─────────────────────────────────────────────────────────────────────
# Vazifa 7: .gitignore sinash
# ─────────────────────────────────────────────────────────────────────

echo "test" > test.tmp

git status
# (test.tmp YO'Q — gitignore ishladi)

git check-ignore -v test.tmp
# .gitignore:10:*.tmp  test.tmp

rm test.tmp

# ─────────────────────────────────────────────────────────────────────
# Yakuniy holat
# ─────────────────────────────────────────────────────────────────────

git log --oneline --graph --all

# Tipik natija:
# * abc123 (HEAD -> main) docs: tarix tahlili
# * def456 docs(daily): 08-iyun yozuv to'ldirildi
# * ...
# * ghi789 docs(daily): 12-iyun yozuv
# ...
# * jkl012 feat(teglar): teglar tizimi qo'shildi
# * mno345 feat: kun shabloni
# * pqr678 chore: loyiha boshlash
