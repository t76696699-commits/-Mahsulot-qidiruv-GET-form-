Partials nima?
SCSS partial — pastki chiziq (_) bilan boshlangan fayl (_variables.scss). Bu fayl mustaqil compile bo'lmaydi — faqat boshqa fayl tomonidan import qilinadi. Katta loyihalarni kichik bo'laklarga bo'lish uchun ishlatiladi.

@use vs @import
// ESKI USUL: @import (deprecated)
@import 'variables';
@import 'mixins';
// Muammo: global namespace, har safar yuklanadi

// YANGI USUL: @use (tavsiya etiladi)
@use 'abstracts/variables';
@use 'abstracts/mixins';

// Namespace bilan ishlatish:
.btn { color: variables.$color-primary; }
Haqiqiy loyiha tuzilmasi (7-1 pattern)
scss/
├── abstracts/
│   ├── _variables.scss    // Barcha o'zgaruvchilar
│   ├── _mixins.scss       // Barcha mixinlar
│   ├── _functions.scss    // SCSS funksiyalar
│   └── _index.scss        // @forward bilan birlashtirish
├── base/
│   ├── _reset.scss        // CSS reset / normalize
│   ├── _typography.scss   // Font, heading, paragraph
│   └── _index.scss
├── components/
│   ├── _buttons.scss
│   ├── _cards.scss
│   ├── _alerts.scss
│   └── _index.scss
├── layouts/
│   ├── _header.scss
│   ├── _footer.scss
│   ├── _grid.scss
│   └── _index.scss
├── pages/
│   ├── _home.scss
│   └── _about.scss
└── main.scss              // Asosiy fayl
_index.scss va @forward
// abstracts/_index.scss
@forward 'variables';
@forward 'mixins';
@forward 'functions';

// Endi main.scss da bitta @use yetarli:
// main.scss
@use 'abstracts';  // _index.scss ni topadi

.btn { color: abstracts.$color-primary; }
_variables.scss misol
// abstracts/_variables.scss

// Colors
$color-primary:   #6366f1 !default;
$color-secondary: #a855f7 !default;
$color-success:   #22c55e !default;
$color-warning:   #f59e0b !default;
$color-danger:    #ef4444 !default;
$color-text:      #1e293b !default;
$color-muted:     #64748b !default;
$color-bg:        #ffffff !default;

// Typography
$font-family:   'Inter', -apple-system, sans-serif !default;
$font-size-xs:  0.75rem !default;
$font-size-sm:  0.875rem !default;
$font-size-md:  1rem !default;
$font-size-lg:  1.125rem !default;
$font-size-xl:  1.25rem !default;
$font-size-2xl: 1.5rem !default;
$font-size-3xl: 2rem !default;

// Spacing
$space-1: 4px !default;
$space-2: 8px !default;
$space-3: 12px !default;
$space-4: 16px !default;
$space-6: 24px !default;
$space-8: 32px !default;
$space-12: 48px !default;
$space-16: 64px !default;

// Border radius
$radius-sm:   4px !default;
$radius-md:   8px !default;
$radius-lg:   12px !default;
$radius-xl:   20px !default;
$radius-full: 9999px !default;
main.scss yig'ish
// main.scss

// 1. Abstracts (o'zgaruvchilar, mixinlar)
@use 'abstracts';

// 2. Base (reset, tipografiya)
@use 'base';

// 3. Komponentlar
@use 'components';

// 4. Layout
@use 'layouts';

// 5. Sahifaga xos
@use 'pages/home';
namespace bilan qisqartirish
// namespace o'zgartirish
@use 'abstracts/variables' as v;
@use 'abstracts/mixins' as m;

.card {
  background: v.$color-bg;
  border-radius: v.$radius-lg;
  @include m.hover-lift;
}
Mini-Loyiha: Modular SCSS Loyiha
Maqsad: Haqiqiy SCSS loyiha tuzilmasini yarating va barcha oldingi bilimlarni birlashtiring.

// abstracts/_index.scss
@forward 'variables';
@forward 'mixins';

// main.scss
@use 'abstracts';
// Qolganlarni qo'shing...
3 ta vazifa:

7-1 pattern asosida papkalar strukturasini yarating va kamida 5 ta partial fayl yozing
_index.scss fayllarini @forward bilan sozlang
main.scss da @use bilan hamma narsani yig'ing va haqiqiy Header komponenti yarating
