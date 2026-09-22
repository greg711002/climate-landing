# 1. финальная структура
mkdir -p public api/_lib legacy-php
git mv site/index.html    public/index.html       2>/dev/null || mv site/index.html public/
git mv site/404.html      public/404.html         2>/dev/null || mv site/404.html public/ 2>/dev/null
git mv site/robots.txt    public/robots.txt       2>/dev/null || mv site/robots.txt public/
git mv site/send.php site/Mailer.php site/config.example.php site/.htaccess legacy-php/ 2>/dev/null || true
rm -rf site/CNAME site/.nojekyll 2>/dev/null || true

# GitHub Pages больше не нужен — workflow будет падать
git rm -r --cached .github/workflows/pages.yml 2>/dev/null || true
rm -f .github/workflows/pages.yml

# 2. проверка ПЕРЕД коммитом — секретов быть не должно
echo "── файлы с секретами (должно быть пусто) ──"
git status --porcelain | grep -E 'config\.php|\.env$|\.csv$' || echo "чисто ✓"

echo "── что уже в индексе (должно быть пусто) ──"
git ls-files | grep -E 'config\.php$|\.env$|\.csv$|^site/data/' || echo "чисто ✓"

# 3. коммит
git add -A
git commit -m "feat: финальная структура — статика в public/, API на Vercel"

# 4. публикация
git push origin main
