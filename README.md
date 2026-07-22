# ════════════════════════════════════════════════════════════════════
# DARS 1: Git nima va birinchi repo
# Maqsad: init → add → commit → log
# ════════════════════════════════════════════════════════════════════

# ─────────────────────────────────────────────────────────────────────
# 0) Tekshirish va sozlash
# ─────────────────────────────────────────────────────────────────────

git --version
# git version 2.43.0 (siznikida boshqacha bo'lishi mumkin)

# Birinchi marta — ism va email
git config --global user.name "Olim Karimov"
git config --global user.email "olim@example.uz"

# Default branch — main
git config --global init.defaultBranch main

# Tekshirish
git config --list
git config user.name
git config user.email

# ─────────────────────────────────────────────────────────────────────
# 1) Yangi repo
# ─────────────────────────────────────────────────────────────────────

mkdir mening-loyiham
cd mening-loyiham

git init
# Initialized empty Git repository in .../mening-loyiham/.git/

# .git papka paydo bo'ldi
ls -la

# ─────────────────────────────────────────────────────────────────────
# 2) Birinchi fayl va commit
# ─────────────────────────────────────────────────────────────────────

# Fayl yaratamiz
echo "# Mening loyiham" > README.md
echo "Bu mening birinchi Git loyihalarim" >> README.md

# Status — Git nima ko'rdi?
git status
# On branch main
# No commits yet
# Untracked files:
#   README.md
# nothing added to commit but untracked files present

# Staging'ga qo'shamiz
git add README.md

# Yana status
git status
# Changes to be committed:
#   new file: README.md

# Commit
git commit -m "docs: birinchi README qo'shildi"
# [main (root-commit) abc1234] docs: birinchi README qo'shildi
#  1 file changed, 2 insertions(+)
#  create mode 100644 README.md

# Tarix
git log
# commit abc1234... (HEAD -> main)
# Author: Olim Karimov <olim@example.uz>
# Date: ...
#
#     docs: birinchi README qo'shildi

# Bir qatorli
git log --oneline

# ─────────────────────────────────────────────────────────────────────
# 3) Ko'p fayl, ko'p commit
# ─────────────────────────────────────────────────────────────────────

echo "print('Salom, Git!')" > main.py
echo "Hello from JS" > script.js

git status
# Untracked: main.py, script.js

# Hammasini birga
git add .

# Yoki alohida
# git add main.py
# git add script.js

git commit -m "feat: main.py va script.js qo'shildi"

# Tarix
git log --oneline
# def5678 (HEAD -> main) feat: main.py va script.js qo'shildi
# abc1234 docs: birinchi README qo'shildi

# ─────────────────────────────────────────────────────────────────────
# 4) Faylni o'zgartirish — modify
# ─────────────────────────────────────────────────────────────────────

# main.py ni o'zgartirib qo'yamiz
echo "print('Yangilangan salom!')" > main.py

git status
# modified: main.py

# Nimani aniq o'zgartirdik?
git diff
# - print('Salom, Git!')
# + print('Yangilangan salom!')

# Stage va commit
git add main.py
git commit -m "fix: main.py salom matni yangilandi"

# ─────────────────────────────────────────────────────────────────────
# 5) Multi-line commit (batafsil)
# ─────────────────────────────────────────────────────────────────────

echo "# Sozlamalar" > config.md

git add config.md
git commit -m "docs: sozlamalar fayli qo'shildi" \
           -m "Bu commit foydalanuvchi sozlamalari uchun config.md faylini qo'shadi. Hozircha bo'sh — keyingi commitda ma'lumot to'ldiriladi."

# ─────────────────────────────────────────────────────────────────────
# 6) Tarix turli ko'rinishlarda
# ─────────────────────────────────────────────────────────────────────

git log                    # to'liq
git log --oneline          # qisqa
git log -n 3               # oxirgi 3
git log --author="Olim"    # faqat Olim'ning commitlari
git log --since="1 week ago"
git log --grep="fix"       # xabarda "fix" so'zi borlari

# Oxirgi commit'ning to'liq
git show

# Belgilangan commit
git show abc1234

# ─────────────────────────────────────────────────────────────────────
# 7) Ataylab xato — add'siz commit
# ─────────────────────────────────────────────────────────────────────

echo "yangi qator" >> README.md
git commit -m "test"
# nothing to commit, working tree clean
# (chunki staging area bo'sh — add qilmagansiz)

# To'g'risi:
git add README.md
git commit -m "docs: README ga yangi qator qo'shildi"
