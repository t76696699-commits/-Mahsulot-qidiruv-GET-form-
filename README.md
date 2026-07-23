# ════════════════════════════════════════════════════════════════════
# REVISION 3: Yo'qotilgan kodni qutqarish — laboratoriyasi
# Modul 3: reset + revert + reflog
# ════════════════════════════════════════════════════════════════════

# Toza laboratoriya
mkdir qutqarish-lab && cd qutqarish-lab
git init
echo "boshlang'ich" > README.md
git add . && git commit -m "init"

# ─────────────────────────────────────────────────────────────────────
# SENARIY 1: Muhim branchni tasodifan o'chirish
# ─────────────────────────────────────────────────────────────────────

# Sozlash
git switch -c feature/payment
for i in 1 2 3 4 5; do
    echo "payment kod step $i" >> payment.py
    git add . && git commit -m "feat: payment step $i"
done

git log --oneline
# 5 ta commit + init

git switch main

# ❌ KATASTROFA
git branch -D feature/payment
# Deleted branch feature/payment (was abc1234).

# 🚨 QUTQARISH
git reflog
# Eng yuqori — oxirgi harakatlar
# abc1234 HEAD@{0}: checkout: moving from feature/payment to main
# def5678 HEAD@{1}: commit: feat: payment step 5

# SHA topish — oxirgi feature/payment HEAD
SHA=$(git reflog | grep "payment step 5" | head -1 | awk '{print $1}')
echo "Topildi: $SHA"

# Branchni qaytarish
git branch feature/payment $SHA

# Tekshirish
git switch feature/payment
git log --oneline
# ✅ 5 ta commit qaytdi

# ─────────────────────────────────────────────────────────────────────
# SENARIY 2: reset --hard bilan ish yo'qotish
# ─────────────────────────────────────────────────────────────────────

git switch main

for i in 1 2 3; do
    echo "ish $i" >> ish.py
    git add . && git commit -m "feat: ish $i"
done

git log --oneline | head -5

# ❌ KATASTROFA — 3 commit yo'qotish
git reset --hard HEAD~3

ls ish.py
# ❌ ish.py YO'Q!

# 🚨 QUTQARISH
git reflog
# HEAD@{0}: reset: moving to HEAD~3
# HEAD@{1}: commit: feat: ish 3   ← bu eski holat

# Eski holatga
git reset --hard HEAD@{1}

ls ish.py
# ✅ ish.py qaytdi
git log --oneline | head -5
# 3 ta commit ham qaytdi

# ─────────────────────────────────────────────────────────────────────
# SENARIY 3: Push qilingan yomon commit (revert)
# ─────────────────────────────────────────────────────────────────────

# Push'siz simulyatsiya — yomon commit qilingan
echo "DELETE EVERYTHING" > bad.py
git add . && git commit -m "BUG: delete everything"

git log --oneline | head -3

# ❌ Yomon commit allaqachon push qilingan (faraz)
# Reset xavfli — chunki push qilingan
# Yechim — REVERT

git revert HEAD --no-edit
# Yangi commit yaratiladi:
# Revert "BUG: delete everything"

ls bad.py
# ❌ bad.py YO'Q (revert bekor qildi)

git log --oneline | head -3
# revert_sha Revert "BUG..."
# bad_sha BUG: delete everything
# ✅ Tarix saqlanadi, lekin bad effect bekor

# ─────────────────────────────────────────────────────────────────────
# SENARIY 4: Wrong branch'da commit
# ─────────────────────────────────────────────────────────────────────

git switch main

# Tasodifan main'da 3 commit
echo "profile UI 1" > profile1.py && git add . && git commit -m "feat: profile 1"
echo "profile UI 2" > profile2.py && git add . && git commit -m "feat: profile 2"
echo "profile UI 3" > profile3.py && git add . && git commit -m "feat: profile 3"

# Voy — bu feature/profile branchda bo'lishi kerak edi
# 🚨 QUTQARISH (push qilmagan deb faraz):

# 1) Joriy holat'dan branch yaratish
git branch feature/profile

# 2) main'ni 3 commit oldinga qaytarish
git reset --hard HEAD~3

# 3) Yangi branchga o'tish
git switch feature/profile

git log --oneline | head -5
# ✅ 3 ta commit shu yerda
# main toza

# ─────────────────────────────────────────────────────────────────────
# SENARIY 5: Detached HEAD'da ish
# ─────────────────────────────────────────────────────────────────────

git switch main

# 3 commit orqaga
git checkout HEAD~3
# Note: switching to 'abc1234'.
# You are in 'detached HEAD' state.

# Eksperiment
echo "ekspriment 1" > exp.py
git add . && git commit -m "exp: 1"

echo "ekspriment 2" >> exp.py
git add . && git commit -m "exp: 2"

# Voy — branch'ga qaytmoqchi
git switch main
# ❌ exp commit'lar ko'rinmaydi (branch'siz qoldi)

ls exp.py
# ❌ YO'Q

# 🚨 QUTQARISH
git reflog | grep "exp"
# abc HEAD@{N}: commit: exp: 2

SHA=$(git reflog | grep "exp: 2" | head -1 | awk '{print $1}')

# Branch yaratish o'sha SHA dan
git branch experiment $SHA

git switch experiment
ls exp.py
# ✅ exp.py qaytdi
git log --oneline | head -3

# ─────────────────────────────────────────────────────────────────────
# BONUS SENARIY 6: Rebase --abort
# ─────────────────────────────────────────────────────────────────────

git switch main
echo "shared" > shared.txt && git add . && git commit -m "main: shared"

git switch -c feature/conflict
echo "feature shared" > shared.txt && git add . && git commit -m "feat: shared"

git switch main
echo "yana main" > shared.txt && git add . && git commit -m "main: yana"

git switch feature/conflict

# Rebase
git rebase main
# CONFLICT...

# Yarim yechdim, lekin xato qildim
echo "wrong" > shared.txt
git add shared.txt

# 🚨 QUTQARISH — bekor qilish
git rebase --abort
# Eski holatga qaytadi

git status
# clean — hech narsa o'zgarmagan

# ─────────────────────────────────────────────────────────────────────
# Yakuniy hisobot
# ─────────────────────────────────────────────────────────────────────

cat > hisobot.md <<'EOF'
# Recovery laboratoriyasi — hisobot

## Senariy 1: Branch o'chirish
- ❌ git branch -D feature/payment
- 🚨 git reflog + git branch feature/payment <SHA>
- ✅ Tiklandi

## Senariy 2: reset --hard
- ❌ git reset --hard HEAD~3
- 🚨 git reflog + git reset --hard HEAD@{1}
- ✅ Tiklandi

## Senariy 3: Push qilingan yomon commit
- ❌ Bad commit + push
- 🚨 git revert HEAD (reset emas, chunki push qilingan)
- ✅ Yangi commit bilan bekor

## Senariy 4: Wrong branch
- ❌ main'da feature commit'lar
- 🚨 git branch <yangi> + git reset --hard HEAD~N main'da
- ✅ Tiklandi

## Senariy 5: Detached HEAD
- ❌ checkout SHA + commit'lar
- 🚨 git reflog + git branch <yangi> <SHA>
- ✅ Tiklandi

## Senariy 6: Rebase abort
- ❌ Yarim conflict yechish — adashish
- 🚨 git rebase --abort
- ✅ Eski holatga
EOF

git add hisobot.md
git commit -m "docs: recovery lab hisoboti"
