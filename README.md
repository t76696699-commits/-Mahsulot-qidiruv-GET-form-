# ════════════════════════════════════════════════════════════════════
# DARS 3: Branching — branch, switch, merge
# ════════════════════════════════════════════════════════════════════

cd mening-loyiham

# ─────────────────────────────────────────────────────────────────────
# 1) Hozirgi branchni ko'rish
# ─────────────────────────────────────────────────────────────────────

git branch
# * main

git status
# On branch main

# ─────────────────────────────────────────────────────────────────────
# 2) Yangi branch yaratish va o'tish
# ─────────────────────────────────────────────────────────────────────

# Variant A: 2 ta buyruq
git branch feature/login
git switch feature/login

# Variant B: 1 buyruq (tavsiya)
git switch -c feature/profile

# Tekshirish
git branch
#   feature/login
# * feature/profile
#   main

# ─────────────────────────────────────────────────────────────────────
# 3) Branch'da ishlash
# ─────────────────────────────────────────────────────────────────────

# Hozir feature/profile branchidamiz
echo "def profil(id):" > profil.py
echo "    return {'id': id, 'ism': 'Olim'}" >> profil.py

git add profil.py
git commit -m "feat: profil() funksiyasi qo'shildi"

echo "def yangilash(id, ism):" >> profil.py
echo "    return {'id': id, 'ism': ism}" >> profil.py

git add profil.py
git commit -m "feat: yangilash() funksiyasi"

# Tarix faqat shu branchda
git log --oneline
# def5678 (HEAD -> feature/profile) feat: yangilash()
# abc1234 feat: profil()
# 9012345 ... (main'dan keyin)

# ─────────────────────────────────────────────────────────────────────
# 4) main'ga qaytish — profil.py YO'Q
# ─────────────────────────────────────────────────────────────────────

git switch main

ls
# main.py  README.md
# (profil.py yo'q — feature/profile branchda)

git log --oneline
# 9012345 ... (oldingi commit'lar)
# (feature/profile commitlari ko'rinmaydi)

# ─────────────────────────────────────────────────────────────────────
# 5) Merge — fast-forward
# ─────────────────────────────────────────────────────────────────────

# main hech o'zgarmagan, feature ilgariga ketgan → fast-forward
git merge feature/profile
# Fast-forward
#  profil.py | 3 +++
#  1 file changed, 3 insertions(+)

# Endi profil.py main'da
ls
# main.py  profil.py  README.md

git log --oneline
# def5678 (HEAD -> main, feature/profile) feat: yangilash()
# abc1234 feat: profil()
# 9012345 ...

# ─────────────────────────────────────────────────────────────────────
# 6) Branchni o'chirish
# ─────────────────────────────────────────────────────────────────────

git branch -d feature/profile
# Deleted branch feature/profile

git branch
# * main
# feature/login (hali bor)

# ─────────────────────────────────────────────────────────────────────
# 7) Merge commit (3-way merge) — main ham o'zgargan
# ─────────────────────────────────────────────────────────────────────

# Yana yangi branch
git switch -c feature/api

echo "def get_api(): return 'API data'" > api.py
git add api.py
git commit -m "feat: API client"

# main'ga qaytib boshqa o'zgarish
git switch main
echo "# Loyiha yangiliklar" > CHANGELOG.md
git add CHANGELOG.md
git commit -m "docs: CHANGELOG qo'shildi"

# Endi ikkalasi ham ilgariga ketgan
git log --oneline --graph --all
# * abc111 (HEAD -> main) docs: CHANGELOG
# | * def222 (feature/api) feat: API client
# |/
# * 9012345 ...

# Merge — endi merge commit yaratiladi
git merge feature/api
# Merge made by the 'ort' strategy.

git log --oneline --graph --all
# *   M (HEAD -> main) Merge branch 'feature/api'
# |\
# | * def222 (feature/api) feat: API client
# * | abc111 docs: CHANGELOG
# |/
# * 9012345 ...

# ─────────────────────────────────────────────────────────────────────
# 8) Branch'da o'zgarishlar bor — switch bloklanadi
# ─────────────────────────────────────────────────────────────────────

echo "test" >> main.py
git switch feature/login
# error: Your local changes... would be overwritten

# Yechim 1: commit
git add main.py
git commit -m "WIP: hali tugamadi"

# Yechim 2: stash (7-darsda)
# git stash
# git switch feature/login

# ─────────────────────────────────────────────────────────────────────
# 9) Branch nomlarini renomlash
# ─────────────────────────────────────────────────────────────────────

git switch feature/login

# Joriy branchni rename
git branch -m feature/auth

# Boshqa branchni rename
git branch -m feature/login feature/login-form
