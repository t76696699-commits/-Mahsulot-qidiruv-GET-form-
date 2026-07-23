# ════════════════════════════════════════════════════════════════════
# DARS 10: Tag, release, GitHub Actions
# ════════════════════════════════════════════════════════════════════
# Bu fayl 2 qism: bash buyruqlar + YAML workflow misollari

# ─────────────────────────────────────────────────────────────────────
# QISM A: BASH — tag va release
# ─────────────────────────────────────────────────────────────────────

cd mening-loyiham

# Tag — lightweight
git tag v0.1.0

# Annotated tag (tavsiya)
git tag -a v1.0.0 -m "Birinchi rasmiy versiya"

# Belgilangan commit'ga
git tag -a v0.9.0 abc1234 -m "Beta versiyasi"

# Ro'yxat
git tag
git tag -l "v1.*"     # patternga mos

# Tag'ni ko'rish
git show v1.0.0

# Tag'ni remote'ga push
git push origin v1.0.0
git push origin --tags     # hammasi

# Lokal tag o'chirish
git tag -d v0.1.0

# Remote tag o'chirish
git push origin --delete v0.1.0

# ─────────────────────────────────────────────────────────────────────
# Release yaratish — gh CLI bilan
# ─────────────────────────────────────────────────────────────────────

gh release create v1.0.0 \
    --title "v1.0.0 — Birinchi versiya" \
    --notes "$(cat <<'EOF'
## 🚀 Yangi xususiyatlar
- Login va register
- Profile sahifa
- Dark mode

## 🐛 Bug fixes
- Email validatsiyasi to'g'rilandi

## 📦 Dependencies
- React 19
- Vite 7
EOF
)"

# Fayl yuklash
gh release upload v1.0.0 dist/app.zip

# Release ro'yxati
gh release list

# Bitta'ni ko'rish
gh release view v1.0.0

# ─────────────────────────────────────────────────────────────────────
# QISM B: YAML workflow misollari
# ─────────────────────────────────────────────────────────────────────

# Repo ildizida:
# mkdir -p .github/workflows
# .github/workflows/test.yml fayl yarating

# ════════════════════════════════════════════════════════════════════
# Misol 1: Python test
# ════════════════════════════════════════════════════════════════════

cat > .github/workflows/test-python.yml << 'YAML'
name: Python Tests

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        python-version: ["3.10", "3.11", "3.12"]

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Python ${{ matrix.python-version }}
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}
          cache: 'pip'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Lint with ruff
        run: |
          pip install ruff
          ruff check .

      - name: Test with pytest
        run: pytest --cov --cov-report=xml

      - name: Upload coverage
        uses: codecov/codecov-action@v4
        with:
          file: ./coverage.xml
YAML

# ════════════════════════════════════════════════════════════════════
# Misol 2: Node.js build va deploy
# ════════════════════════════════════════════════════════════════════

cat > .github/workflows/deploy-node.yml << 'YAML'
name: Build and Deploy

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'

      - name: Install
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Test
        run: npm test

      - name: Build
        run: npm run build

      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          vercel-args: '--prod'
YAML

# ════════════════════════════════════════════════════════════════════
# Misol 3: Release tag'ga binary build
# ════════════════════════════════════════════════════════════════════

cat > .github/workflows/release.yml << 'YAML'
name: Release Binary

on:
  push:
    tags:
      - 'v*'

jobs:
  build:
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest, windows-latest]

    runs-on: ${{ matrix.os }}

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-go@v5
        with:
          go-version: '1.22'

      - name: Build
        run: go build -o myapp${{ matrix.os == 'windows-latest' && '.exe' || '' }}

      - name: Upload to release
        uses: softprops/action-gh-release@v2
        with:
          files: myapp*
YAML

# ════════════════════════════════════════════════════════════════════
# Misol 4: Scheduled (har kun)
# ════════════════════════════════════════════════════════════════════

cat > .github/workflows/daily-report.yml << 'YAML'
name: Daily Report

on:
  schedule:
    - cron: '0 8 * * *'   # har kun ertalab 8:00 UTC
  workflow_dispatch:       # qo'l bilan ham ishlatish

jobs:
  report:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Generate stats
        run: |
          echo "## Daily report" > report.md
          git log --oneline --since="yesterday" >> report.md

      - name: Send to Slack
        env:
          WEBHOOK: ${{ secrets.SLACK_WEBHOOK }}
        run: |
          curl -X POST "$WEBHOOK" \
            -H "Content-Type: application/json" \
            -d "{\"text\": \"Daily report:\n$(cat report.md)\"}"
YAML

# ─────────────────────────────────────────────────────────────────────
# Hammasini commit va push
# ─────────────────────────────────────────────────────────────────────

git add .github/
git commit -m "ci: GitHub Actions workflows qo'shildi"
git push

# GitHub'da Actions tab — workflow'lar ishga tushadi
# Yashil ✅ — passing
# Qizil ❌ — log'larni ko'ring

# ─────────────────────────────────────────────────────────────────────
# README ga badge qo'shish
# ─────────────────────────────────────────────────────────────────────

cat >> README.md << 'EOF'

## Status

![Tests](https://github.com/olim/mening-loyiham/workflows/Python%20Tests/badge.svg)
![Deploy](https://github.com/olim/mening-loyiham/workflows/Build%20and%20Deploy/badge.svg)
EOF
