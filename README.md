# ════════════════════════════════════════════════════════════════════
# DARS 6: Merge conflict — yechish
# ════════════════════════════════════════════════════════════════════

cd mening-loyiham

# ─────────────────────────────────────────────────────────────────────
# 1) Toza holatdan boshlash
# ─────────────────────────────────────────────────────────────────────

git switch main
git pull

# Test fayl
echo "matn versiya 1" > test.txt
git add test.txt
git commit -m "test: boshlang'ich fayl"
git push

# ─────────────────────────────────────────────────────────────────────
# 2) Conflict yaratamiz
# ─────────────────────────────────────────────────────────────────────

# Feature branch
git switch -c feature/o-zgartirish
echo "matn — feature versiyasi" > test.txt
git add test.txt
git commit -m "feat: matn feature uslubida"

# Main'ga qaytib boshqa o'zgarish
git switch main
echo "matn — main versiyasi" > test.txt
git add test.txt
git commit -m "feat: matn main uslubida"

# Merge — CONFLICT!
git merge feature/o-zgartirish
# Auto-merging test.txt
# CONFLICT (content): Merge conflict in test.txt
# Automatic merge failed; fix conflicts and then commit the result.

# ─────────────────────────────────────────────────────────────────────
# 3) Conflict'ni ko'rish
# ─────────────────────────────────────────────────────────────────────

git status
# Unmerged paths:
#   both modified: test.txt

cat test.txt
# <<<<<<< HEAD
# matn — main versiyasi
# =======
# matn — feature versiyasi
# >>>>>>> feature/o-zgartirish

# ─────────────────────────────────────────────────────────────────────
# 4) Yechish — 4 ta variant
# ─────────────────────────────────────────────────────────────────────

# Variant 1: faqat main
cat > test.txt <<'EOF'
matn — main versiyasi
EOF

# yoki avtomatik:
git checkout --ours test.txt

# Variant 2: faqat feature
git checkout --theirs test.txt

# Variant 3: ikkalasi birga
cat > test.txt <<'EOF'
matn — main versiyasi
matn — feature versiyasi
EOF

# Variant 4: yangi mazmun
cat > test.txt <<'EOF'
matn — main va feature versiyalari birlashtirildi
EOF

# Hozir variant 4 ni tanlaymiz
cat > test.txt <<'EOF'
matn — birlashtirildi
EOF

# ─────────────────────────────────────────────────────────────────────
# 5) Yechilganini Git'ga aytish
# ─────────────────────────────────────────────────────────────────────

git add test.txt

git status
# All conflicts fixed but you are still merging.

git commit
# Editor ochiladi (yoki avtomatik xabar bilan)
# Default xabar: "Merge branch 'feature/o-zgartirish'"

# ─────────────────────────────────────────────────────────────────────
# 6) Tarix
# ─────────────────────────────────────────────────────────────────────

git log --oneline --graph --all
# *   merge_sha Merge branch 'feature/o-zgartirish'
# |\
# | * feat_sha feat: matn feature uslubida
# * | main_sha feat: matn main uslubida
# |/
# * old_sha test: boshlang'ich fayl

# ─────────────────────────────────────────────────────────────────────
# 7) Merge'ni bekor qilish
# ─────────────────────────────────────────────────────────────────────

# Conflict paytida — eski holatga qaytish
# git merge --abort

# Bu — eski main'ga qaytish. Hech narsa o'zgarmagan.

# ─────────────────────────────────────────────────────────────────────
# 8) Ko'p faylda conflict
# ─────────────────────────────────────────────────────────────────────

# Faraz — 3 ta faylda
git switch main
echo "main a" > a.txt && echo "main b" > b.txt && echo "main c" > c.txt
git add . && git commit -m "main: a,b,c"

git switch -c feature/multi
echo "feat a" > a.txt && echo "feat b" > b.txt && echo "feat c" > c.txt
git add . && git commit -m "feat: a,b,c"

git switch main
echo "yana main" > a.txt && echo "yana main b" > b.txt
git add . && git commit -m "main: a,b yangilandi"

git merge feature/multi
# CONFLICT in: a.txt, b.txt (c.txt avtomatik mergeli)

# Har birini alohida
echo "yakuniy a" > a.txt && git add a.txt
echo "yakuniy b" > b.txt && git add b.txt

git status
# all conflicts fixed

git commit

# ─────────────────────────────────────────────────────────────────────
# 9) VS Code'ni mergetool sifatida
# ─────────────────────────────────────────────────────────────────────

# git config --global merge.tool vscode
# git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# Conflict paytida
# git mergetool

# VS Code ochiladi, har conflict joyida tugmalar:
# - Accept Current Change (ours)
# - Accept Incoming Change (theirs)
# - Accept Both Changes
# - Compare Changes

# ─────────────────────────────────────────────────────────────────────
# 10) PR'da conflict — lokal yechish
# ─────────────────────────────────────────────────────────────────────

# GitHub: "This branch has conflicts that must be resolved"

# Lokal:
git switch feature/login
git fetch origin
git merge origin/main         # main'ni feature'ga
# CONFLICT...

# Yechib commit qiling
# git add . && git commit

# Push — PR yangilanadi
git push
