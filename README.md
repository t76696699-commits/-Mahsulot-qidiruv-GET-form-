SASS va SCSS nima?
SASS (Syntactically Awesome Style Sheets) — CSS ni kuchaytiruvchi preprocessor. U o'zgaruvchilar, nesting, mixins, funksiyalar kabi imkoniyatlarni beradi. SCSS — SASSning yangi sintaksisi bo'lib, odatdagi CSS ga o'xshaydi (figurali qavslar va nuqta-vergul ishlatadi).

SASS vs SCSS sintaksis farqi
// SCSS (tavsiya etiladi) — CSS ga o'xshash
.nav {
  display: flex;
  gap: 16px;

  &__item {
    color: #333;
  }
}

// SASS — tab asosida, qavslar yo'q
.nav
  display: flex
  gap: 16px

  &__item
    color: #333
O'rnatish
// npm orqali
npm install -g sass

// Compile qilish
sass input.scss output.css

// Watch rejimi (avtomatik compile)
sass --watch input.scss:output.css
VS Code uchun Live Sass Compiler extension ham mavjud — saqlashda avtomatik compile qiladi.

$O'zgaruvchilar (Variables)
SCSS da o'zgaruvchilar $ belgisi bilan boshlanadi. Bir joyda o'zgartirsangiz, hamma joyda o'zgaradi:

// _variables.scss
$color-primary:   #6366f1;
$color-secondary: #a855f7;
$color-success:   #22c55e;
$color-danger:    #ef4444;
$color-text:      #1e293b;
$color-muted:     #64748b;
$color-bg:        #ffffff;
$color-surface:   #f8fafc;

$font-size-sm:  0.875rem;  // 14px
$font-size-md:  1rem;      // 16px
$font-size-lg:  1.125rem;  // 18px
$font-heading:  2.25rem;   // 36px

$space-xs:  4px;
$space-sm:  8px;
$space-md:  16px;
$space-lg:  24px;
$space-xl:  48px;
$space-2xl: 80px;

$radius-sm: 6px;
$radius-md: 12px;
$radius-lg: 20px;
$radius-full: 9999px;

$shadow-sm: 0 1px 3px rgba(0,0,0,0.08);
$shadow-md: 0 4px 16px rgba(0,0,0,0.10);
$shadow-lg: 0 8px 32px rgba(0,0,0,0.14);
Nesting (Ichma-ich yozish)
SCSS da selectorlarni ichma-ich yozish mumkin — HTML tuzilmasini aks ettiradi:

.nav {
  display: flex;
  align-items: center;
  gap: $space-lg;
  padding: $space-md $space-xl;
  background: $color-bg;
  box-shadow: $shadow-sm;

  // & — parent selectori (.nav)
  &__brand {
    font-size: $font-size-lg;
    font-weight: 800;
    color: $color-primary;
    text-decoration: none;
  }

  &__links {
    display: flex;
    gap: $space-md;
    list-style: none;
  }

  &__link {
    color: $color-muted;
    text-decoration: none;
    font-weight: 500;
    transition: color 0.2s;

    &:hover  { color: $color-primary; }
    &.active { color: $color-primary; font-weight: 700; }
  }

  &__cta {
    margin-left: auto;
    padding: $space-sm $space-md;
    background: $color-primary;
    color: white;
    border-radius: $radius-full;
    text-decoration: none;
    font-weight: 600;

    &:hover { opacity: 0.9; }
  }
}
Compile natijasi
/* output.css */
.nav { display: flex; align-items: center; gap: 24px; padding: 16px 48px; }
.nav__brand { font-size: 1.125rem; font-weight: 800; color: #6366f1; }
.nav__link { color: #64748b; text-decoration: none; }
.nav__link:hover { color: #6366f1; }
.nav__link.active { color: #6366f1; font-weight: 700; }
Amaliy maslahatlar
O'zgaruvchilarni alohida _variables.scss faylida saqlang
Nesting chuqurligini 3-4 darajadan oshirmang — kodni o'qish qiyinlashadi
& belgisi parent selectorni ifodalaydi — &:hover, &.active, &--modifier
SCSS o'zgaruvchilari CSS custom properties dan farqli — compile vaqtida ishlanadi
Mini-Loyiha: Navigatsiya Komponenti
Maqsad: SCSS o'zgaruvchilari va nesting yordamida to'liq responsive navigatsiya yarating.

// Starter
$color-primary: #6366f1;
$color-text: #1e293b;
$space-md: 16px;

.nav {
  // Bu yerni to'ldiring
}
3 ta vazifa:

O'zgaruvchilar faylida kamida 10 ta token aniqlang
.nav ni nesting bilan yozing: brand, links, link (hover + active holatlari)
Mobile uchun hamburger menu toggler CSS i yozing (media query ichida)
