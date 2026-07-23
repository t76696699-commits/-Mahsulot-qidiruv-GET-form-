# ════════════════════════════════════════════════════════════════════
# 🚀 CAPSTONE: Jamoaviy loyiha workflow — qadamlar
# ════════════════════════════════════════════════════════════════════

# ─────────────────────────────────────────────────────────────────────
# BOSQICH 1: Repo sozlash
# ─────────────────────────────────────────────────────────────────────

# GitHub'da yangi org (yoki shaxsiy account) ostida public repo
# Misol: github.com/our-team/quiz-app

# Lokal'da
git clone git@github.com:our-team/quiz-app.git
cd quiz-app

# README
cat > README.md <<'EOF'
# Quiz App 🎯

Real-time quiz application with multiplayer support.

![Tests](https://github.com/our-team/quiz-app/workflows/Tests/badge.svg)
![License](https://img.shields.io/badge/license-MIT-blue)

## 🚀 Tezda boshlash

\`\`\`bash
git clone https://github.com/our-team/quiz-app.git
cd quiz-app
npm install
npm run dev
\`\`\`

## 📦 Texnologiyalar

- **Frontend:** React 19, Vite, TailwindCSS
- **Backend:** Flask, SQLAlchemy, PostgreSQL
- **Tests:** Vitest, Pytest
- **CI/CD:** GitHub Actions
- **Deploy:** Vercel (FE) + Railway (BE)

## 🌐 Live demo

https://quiz-app.vercel.app

## 📋 Xususiyatlar

- [x] Foydalanuvchi register/login
- [x] Quiz yaratish
- [x] Multiplayer rejim
- [ ] Leaderboard (in progress)

## 🤝 Hissa qo'shish

[CONTRIBUTING.md](./CONTRIBUTING.md) ni o'qing.

## 📄 Litsenziya

MIT
EOF

# .gitignore
cat > .gitignore <<'EOF'
node_modules/
dist/
.env
__pycache__/
venv/
.DS_Store
*.log
EOF

# LICENSE
curl -o LICENSE https://raw.githubusercontent.com/licenses/license-templates/master/templates/mit.txt
sed -i.bak "s/\[year\]/2026/g; s/\[fullname\]/Our Team/g" LICENSE
rm LICENSE.bak

# CONTRIBUTING
cat > CONTRIBUTING.md <<'EOF'
# Contributing

## Workflow

1. Fork or branch from main
2. Create feature branch: `git switch -c feature/your-feature`
3. Commit using Conventional Commits: `feat:`, `fix:`, `docs:`
4. Push and open PR
5. Link PR to issue: `Closes #N`
6. Wait for review (at least 1 approval)

## Branch naming

- `feature/login`
- `fix/login-bug`
- `refactor/api-client`
- `docs/readme-update`

## Commit format

\`\`\`
<type>: <description>

[optional body]
\`\`\`

## Code style

- TypeScript strict mode
- Prettier + ESLint
- Tests for new features
EOF

git add .
git commit -m "chore: initial project setup"
git push origin main

# ─────────────────────────────────────────────────────────────────────
# BOSQICH 2: Branch protection (GitHub UI)
# ─────────────────────────────────────────────────────────────────────

# Settings → Branches → Add rule
# Branch name pattern: main
# - [x] Require pull request before merging
# - [x] Require approvals: 1
# - [x] Require status checks (after first CI run)
# - [x] Require branches to be up to date
# - [x] Include administrators

# ─────────────────────────────────────────────────────────────────────
# BOSQICH 3: Issues
# ─────────────────────────────────────────────────────────────────────

# gh CLI bilan 10 ta issue
gh issue create --title "feat: User registration" \
    --body "Email + password registration with validation" \
    --label "feature,frontend,backend"

gh issue create --title "feat: User login" \
    --body "JWT-based authentication" \
    --label "feature,backend"

gh issue create --title "feat: Quiz creation form" \
    --body "Form to create quiz with multiple questions" \
    --label "feature,frontend"

gh issue create --title "feat: Real-time multiplayer" \
    --body "WebSocket-based multiplayer rooms" \
    --label "feature,backend"

gh issue create --title "feat: Leaderboard" \
    --body "Top players ranking" \
    --label "feature,frontend"

gh issue create --title "fix: Login form validation" \
    --body "Email validation not working on Firefox" \
    --label "bug,frontend"

gh issue create --title "docs: API documentation" \
    --body "OpenAPI spec for backend" \
    --label "documentation"

gh issue create --title "chore: Setup ESLint" \
    --body "Add ESLint + Prettier config" \
    --label "good first issue,tooling"

gh issue create --title "test: Unit tests for auth" \
    --body "Cover login/register edge cases" \
    --label "tests,backend"

gh issue create --title "feat: Dark mode" \
    --body "Toggle for dark/light theme" \
    --label "good first issue,frontend"

# Assign issues
# gh issue edit 1 --add-assignee olim
# gh issue edit 2 --add-assignee vali

# Milestone
gh api repos/our-team/quiz-app/milestones -f title="v1.0.0" -f description="MVP"

# ─────────────────────────────────────────────────────────────────────
# BOSQICH 4: GitHub Actions workflow
# ─────────────────────────────────────────────────────────────────────

mkdir -p .github/workflows

cat > .github/workflows/ci.yml <<'YAML'
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  frontend:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: frontend
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'
          cache-dependency-path: frontend/package-lock.json
      - run: npm ci
      - run: npm run lint
      - run: npm test
      - run: npm run build

  backend:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: backend
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
          cache: 'pip'
          cache-dependency-path: backend/requirements.txt
      - run: pip install -r requirements.txt
      - run: ruff check .
      - run: pytest --cov
YAML

git add .github/
git commit -m "ci: setup CI workflows for FE and BE"
git push

# ─────────────────────────────────────────────────────────────────────
# BOSQICH 5: PR template
# ─────────────────────────────────────────────────────────────────────

mkdir -p .github

cat > .github/pull_request_template.md <<'EOF'
## Nima o'zgardi

-

## Nima uchun

Closes #

## Test

- [ ] Manual test
- [ ] Unit tests
- [ ] Lint passing

## Screenshot (UI bo'lsa)

EOF

git add .github/pull_request_template.md
git commit -m "chore: PR template"
git push

# ─────────────────────────────────────────────────────────────────────
# BOSQICH 6: Birinchi feature PR
# ─────────────────────────────────────────────────────────────────────

# Issue #1 — User registration
gh issue view 1
gh issue edit 1 --add-assignee "@me"

git switch -c feature/user-registration

mkdir -p backend/app
cat > backend/app/auth.py <<'EOF'
from flask import Blueprint, request, jsonify
# ... auth code ...

bp = Blueprint('auth', __name__)

@bp.post('/register')
def register():
    # TODO
    return jsonify({'ok': True})
EOF

git add backend/
git commit -m "feat(auth): scaffold registration endpoint"

# Davom
cat > backend/tests/test_auth.py <<'EOF'
def test_register():
    # TODO
    pass
EOF

git add backend/tests/
git commit -m "test(auth): registration test scaffold"

# Push
git push -u origin feature/user-registration

# PR
gh pr create \
    --title "feat: User registration endpoint" \
    --body "$(cat <<'PR'
## Nima o'zgardi
- /auth/register POST endpoint
- Test scaffold

## Nima uchun
Closes #1

## Test
- [x] Lint passing
- [ ] Full tests (TODO in next PR)
PR
)" \
    --assignee "@me" \
    --label "feature,backend"

# Review kelishini kuting...
# Review bo'lgach (approve), squash and merge

# ─────────────────────────────────────────────────────────────────────
# BOSQICH 7: Reviewer sifatida boshqa PR
# ─────────────────────────────────────────────────────────────────────

# Sherigingiz PR yubordi (#15)
gh pr list

# Checkout
gh pr checkout 15

# Tekshirish — manual test, kod o'qish
# ... ish ...

# Comment
gh pr comment 15 --body "Looks great! One suggestion: extract validation to utils."

# Yoki approve
gh pr review 15 --approve

# Yoki request changes
# gh pr review 15 --request-changes --body "Please add tests"

# ─────────────────────────────────────────────────────────────────────
# BOSQICH 8: Conflict yechish (real)
# ─────────────────────────────────────────────────────────────────────

git switch feature/user-registration
git fetch origin
git rebase origin/main
# CONFLICT in backend/app/__init__.py

# Yeching, marker'larni o'chiring
# vim backend/app/__init__.py

git add backend/app/__init__.py
git rebase --continue
git push --force-with-lease

# ─────────────────────────────────────────────────────────────────────
# BOSQICH 9: Release v1.0.0
# ─────────────────────────────────────────────────────────────────────

# Hammasi tugadi, main toza
git switch main
git pull

# Tag
git tag -a v1.0.0 -m "v1.0.0 — MVP release"
git push origin v1.0.0

# Release
gh release create v1.0.0 \
    --title "v1.0.0 — MVP" \
    --notes "$(cat <<'EOF'
## 🚀 Yangi xususiyatlar

- Foydalanuvchi register/login (JWT)
- Quiz yaratish formasi
- Quiz o'ynash (single player)
- Dark mode

## 🐛 Bug fixes

- Login form Firefox'da to'g'rilandi

## 📦 Dependencies

- React 19
- Flask 3.0
- PostgreSQL 16

## 🙏 Contributors

- @olim
- @vali
- @karim

## 🔗 Links

- [Live demo](https://quiz-app.vercel.app)
- [API docs](https://quiz-app-api.railway.app/docs)
EOF
)"

# ─────────────────────────────────────────────────────────────────────
# 🎉 BAJARILDI
# ─────────────────────────────────────────────────────────────────────

# GitHub Insights → Contributors — har jamoadosh hissasi
# Release sahifa — v1.0.0 download link
# Live demo — hammasi ishlayapti
# README'da yashil CI badge

# CV uchun:
# "Quiz App — fullstack jamoaviy loyiha (4 odam)"
# "GitHub: github.com/our-team/quiz-app"
# "Live: quiz-app.vercel.app"

# 🏆 Siz endi haqiqiy dasturchisiz!
