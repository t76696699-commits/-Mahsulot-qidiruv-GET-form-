# ════════════════════════════════════════════════════════════════════
# DARS 5: Pull Request lifecycle
# ════════════════════════════════════════════════════════════════════

# ─────────────────────────────────────────────────────────────────────
# 1) O'z repo'da PR
# ─────────────────────────────────────────────────────────────────────

cd mening-loyiham

# Yangi feature
git switch -c feature/dark-mode

cat > theme.css <<'EOF'
:root {
    --bg: #fff;
    --text: #000;
}

[data-theme="dark"] {
    --bg: #222;
    --text: #fff;
}
EOF

git add theme.css
git commit -m "feat: dark mode CSS variables"

# UI tugmasi
echo '<button id="theme-toggle">🌙</button>' >> index.html

git add index.html
git commit -m "feat: theme toggle button"

# Push
git push -u origin feature/dark-mode
# remote: Create a pull request for 'feature/dark-mode'...
# remote: https://github.com/olim/mening-loyiham/pull/new/feature/dark-mode

# URL'ga o'tib PR yarating
# yoki gh CLI bilan:
gh pr create \
    --title "feat: dark mode qo'shildi" \
    --body "$(cat <<'PR'
## Nima o'zgardi
- CSS variables bilan dark mode
- Theme toggle button

## Nima uchun
Foydalanuvchilar so'rovi (issue #15).

## Test
- [x] Chrome'da sinangan
- [x] Firefox'da sinangan
- [ ] Safari (TODO)

## Screenshot
(rasmni qo'shing)
PR
)"

# PR'ni ko'rish
gh pr view --web

# ─────────────────────────────────────────────────────────────────────
# 2) Reviewer feedback: o'zgartirish so'raldi
# ─────────────────────────────────────────────────────────────────────

# Reviewer dedi: "localStorage'da saqlash kerak"

cat >> theme.js <<'EOF'
const toggle = document.getElementById('theme-toggle');
toggle.onclick = () => {
    const yangi = document.body.dataset.theme === 'dark' ? 'light' : 'dark';
    document.body.dataset.theme = yangi;
    localStorage.setItem('theme', yangi);
};

// Saqlangan tema
document.body.dataset.theme = localStorage.getItem('theme') || 'light';
EOF

git add theme.js
git commit -m "feat: tema localStorage'da saqlanadi"

# Push — PR avtomatik yangilanadi!
git push

# ─────────────────────────────────────────────────────────────────────
# 3) Merge — 3 ta variant
# ─────────────────────────────────────────────────────────────────────

# GitHub UI'da Merge dropdown:
# - Create a merge commit
# - Squash and merge       (mashhur)
# - Rebase and merge

# Yoki gh CLI:
gh pr merge --squash --delete-branch
# yoki:
# gh pr merge --merge --delete-branch
# gh pr merge --rebase --delete-branch

# ─────────────────────────────────────────────────────────────────────
# 4) Lokal toza
# ─────────────────────────────────────────────────────────────────────

git switch main
git pull              # main yangiliklar (squash commit shu yerda)
git branch -d feature/dark-mode

# Remote'da ham o'chirish (agar gh --delete-branch ishlatmagansiz)
# git push origin --delete feature/dark-mode

# Eskirgan local branchlarni tozalash
git fetch --prune
git branch --merged main | grep -v "main" | xargs -n 1 git branch -d

# ─────────────────────────────────────────────────────────────────────
# 5) Fork workflow (open source)
# ─────────────────────────────────────────────────────────────────────

# 1. GitHub'da Fork tugmasini bosing
#    github.com/anthropic/cool-project → Fork → github.com/olim/cool-project

# 2. Clone (SIZNING fork)
git clone git@github.com:olim/cool-project.git
cd cool-project

# 3. upstream — asl repo'ni qo'shing
git remote add upstream git@github.com:anthropic/cool-project.git

git remote -v
# origin    git@github.com:olim/cool-project.git
# upstream  git@github.com:anthropic/cool-project.git

# 4. Asosiy ish
git switch -c fix/readme-typo

# Tahrir...
sed -i.bak 's/recieve/receive/g' README.md
rm README.md.bak

git add README.md
git commit -m "fix: typo 'recieve' -> 'receive'"

git push -u origin fix/readme-typo

# 5. PR yarating — upstream/main ga
gh pr create \
    --title "fix: README typo" \
    --body "Recieve -> receive" \
    --base main \
    --head olim:fix/readme-typo

# 6. Reviewer'lar javob, siz tuzatish — qaytadan push
# 7. Merge bo'lgach, fork'ni yangilang:

git switch main
git pull upstream main
git push origin main      # sizning fork'da main yangilanadi

git branch -d fix/readme-typo
git push origin --delete fix/readme-typo

# ─────────────────────────────────────────────────────────────────────
# 6) gh CLI — kunlik buyruqlar
# ─────────────────────────────────────────────────────────────────────

# Auth
gh auth login

# PR ro'yxat (joriy repo)
gh pr list

# PR sizning (har repo'da)
gh pr list --author "@me"

# Bitta PR'ni ko'rish
gh pr view 42
gh pr view 42 --web

# PR checkout (sherikingiz PR'iga o'tish)
gh pr checkout 42

# Comment qo'shish
gh pr comment 42 --body "LGTM 🎉"

# Approve
gh pr review 42 --approve

# Request changes
gh pr review 42 --request-changes --body "Test yo'q"

# Draft PR
gh pr create --draft

# Ready for review
gh pr ready

# ─────────────────────────────────────────────────────────────────────
# 7) Fork'ni yangilash — qisqa
# ─────────────────────────────────────────────────────────────────────

# Variant A: terminal
git fetch upstream
git switch main
git merge upstream/main
git push origin main

# Variant B: gh CLI (zamonaviy)
gh repo sync olim/cool-project --branch main

# Variant C: GitHub UI
# Fork sahifasida "Sync fork" tugmasi
