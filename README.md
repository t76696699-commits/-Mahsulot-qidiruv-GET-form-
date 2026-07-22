# ════════════════════════════════════════════════════════════════════
# DARS 4: GitHub, SSH, remote
# ════════════════════════════════════════════════════════════════════

# ─────────────────────────────────────────────────────────────────────
# 1) SSH key yaratish va GitHub'ga qo'shish
# ─────────────────────────────────────────────────────────────────────

# Avval — bormi?
ls ~/.ssh/
# id_ed25519, id_ed25519.pub (bor bo'lsa)

# Yo'q bo'lsa — yaratish
ssh-keygen -t ed25519 -C "olim@example.uz"
# Enter file in which to save the key: [Enter — default]
# Enter passphrase: [Enter yoki kuchli parol]

# Public key'ni ko'rsatish
cat ~/.ssh/id_ed25519.pub
# ssh-ed25519 AAAAC3NzaC1lZDI1... olim@example.uz

# Buni nusxalang va GitHub'ga qo'shing:
# GitHub → Settings → SSH and GPG keys → New SSH key

# Tekshirish
ssh -T git@github.com
# Hi olim! You've successfully authenticated...

# ─────────────────────────────────────────────────────────────────────
# 2) Lokal repo'ni GitHub'ga bog'lash
# ─────────────────────────────────────────────────────────────────────

# Avval GitHub'da repo yarating (web UI):
# github.com → New repository → nom: "mening-loyiham"
# README qo'shmang!

cd mening-loyiham

# Remote qo'shish
git remote add origin git@github.com:olim/mening-loyiham.git

# Tekshirish
git remote -v
# origin  git@github.com:olim/mening-loyiham.git (fetch)
# origin  git@github.com:olim/mening-loyiham.git (push)

# Birinchi push — -u bilan upstream
git push -u origin main
# Enumerating objects: 12, done.
# To github.com:olim/mening-loyiham.git
#  * [new branch]      main -> main

# Endi GitHub sahifasini yangilang — kodingiz ko'rinadi!

# ─────────────────────────────────────────────────────────────────────
# 3) Yangi commit + push
# ─────────────────────────────────────────────────────────────────────

echo "Yangi qator" >> README.md
git add README.md
git commit -m "docs: README yangilandi"

git push
# Endi -u kerak emas — birinchi marta sozlangan
# To github.com:olim/mening-loyiham.git
#    abc1234..def5678  main -> main

# ─────────────────────────────────────────────────────────────────────
# 4) clone — boshqa joydan olish
# ─────────────────────────────────────────────────────────────────────

cd ~/Desktop

# To'liq clone
git clone git@github.com:olim/mening-loyiham.git
cd mening-loyiham

# Tarix, branchlar — hammasi keldi
git log --oneline

# Boshqa nom bilan
# git clone git@github.com:olim/mening-loyiham.git mening-loyiham-2

# Faqat oxirgi commit (tezroq)
# git clone --depth 1 git@github.com:olim/mening-loyiham.git

# Belgilangan branch
# git clone -b develop git@github.com:olim/mening-loyiham.git

# ─────────────────────────────────────────────────────────────────────
# 5) pull — yangiliklar olish
# ─────────────────────────────────────────────────────────────────────

# Faraz: GitHub'da yoki boshqa kompyuterda commit bo'ldi
# Lokal'da ko'ramiz:
git pull
# Already up to date.
# yoki:
# Updating abc1234..def5678
# Fast-forward
#  README.md | 1 +
#  1 file changed, 1 insertion(+)

# pull = fetch + merge
git fetch origin
git merge origin/main
# (yuqorisi bilan teng)

# Faqat ko'rib chiqish (merge qilmasdan)
git fetch origin
git log --oneline HEAD..origin/main
# (yangi commit'lar ko'rinadi)

# ─────────────────────────────────────────────────────────────────────
# 6) Push xato — reject
# ─────────────────────────────────────────────────────────────────────

# Sinariy: boshqa joyda commit bo'ldi, siz lokal'da ham commit qildingiz
# Push qilmoqchisiz...

# git push
# ! [rejected]        main -> main (fetch first)
# error: failed to push some refs
# hint: Updates were rejected because the remote contains work...

# Yechim:
git pull --rebase     # yoki: git pull
git push

# (rebase — 8-darsda batafsil)

# ─────────────────────────────────────────────────────────────────────
# 7) Boshqa branchni push
# ─────────────────────────────────────────────────────────────────────

git switch -c feature/footer

echo "<footer>© 2026</footer>" > footer.html
git add footer.html
git commit -m "feat: footer komponenti"

# Birinchi marta — upstream sozlash
git push -u origin feature/footer
# To github.com:olim/mening-loyiham.git
#  * [new branch]      feature/footer -> feature/footer
# branch 'feature/footer' set up to track 'origin/feature/footer'.

# Keyingi safar — git push yetadi

# ─────────────────────────────────────────────────────────────────────
# 8) Remote boshqarish
# ─────────────────────────────────────────────────────────────────────

# Ro'yxat
git remote -v

# URL o'zgartirish (HTTPS → SSH ga)
git remote set-url origin git@github.com:olim/mening-loyiham.git

# O'chirish
# git remote remove origin

# Rename
# git remote rename origin upstream

# ─────────────────────────────────────────────────────────────────────
# 9) Branch o'chirish (lokal va remote)
# ─────────────────────────────────────────────────────────────────────

# Lokal
git branch -d feature/footer

# Remote
git push origin --delete feature/footer

# Ikkalasi birga
# (qisqa skript yo'q — alohida buyruq)

# ─────────────────────────────────────────────────────────────────────
# 10) Force push — XAVFLI
# ─────────────────────────────────────────────────────────────────────

# Faqat shaxsiy branch'da, jamoadosh bilan kelishilgan paytda
# git push --force                  # ESKI — eng xavfli
# git push --force-with-lease       # YAXSHI — eski versiyaga override qilmaydi
