Mixin — qayta ishlatiluvchi CSS kod bloki. Funksiyaga o'xshaydi: parametr qabul qilishi va turli joylarda chaqirilishi mumkin. @mixin bilan e'lon qilinadi, @include bilan chaqiriladi.

Oddiy mixin
// E'lon qilish
@mixin flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

// Ishlatish
.hero {
  @include flex-center;
  min-height: 100vh;
}

.card__icon {
  @include flex-center;
  width: 48px;
  height: 48px;
}
Parametrli mixin
// Default qiymat bilan
@mixin flex($direction: row, $justify: flex-start, $align: stretch, $gap: 0) {
  display: flex;
  flex-direction: $direction;
  justify-content: $justify;
  align-items: $align;
  gap: $gap;
}

// Ishlatish
.header {
  @include flex(row, space-between, center, 16px);
}

.sidebar {
  @include flex(column, flex-start, stretch, 8px);
}

.card-grid {
  @include flex(row, flex-start, stretch, 24px);
  flex-wrap: wrap;
}
Responsive breakpoints mixin
// _breakpoints.scss
$breakpoints: (
  'sm':  576px,
  'md':  768px,
  'lg':  1024px,
  'xl':  1280px,
  '2xl': 1536px,
);

@mixin respond-to($name) {
  $value: map-get($breakpoints, $name);
  @if $value {
    @media (max-width: $value) {
      @content;
    }
  } @else {
    @warn "Breakpoint '#{$name}' topilmadi.";
  }
}

// Ishlatish
.nav {
  display: flex;
  gap: 24px;

  @include respond-to('md') {
    flex-direction: column;
    gap: 8px;
  }
}

.hero__title {
  font-size: 4rem;

  @include respond-to('lg') { font-size: 3rem; }
  @include respond-to('sm') { font-size: 2rem; }
}
Button variant mixin
@mixin button-base {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 10px 20px;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  font-size: 0.9rem;
  cursor: pointer;
  transition: all 0.2s ease;
  text-decoration: none;
}

@mixin button-variant($bg, $color: white, $hover-bg: darken($bg, 10%)) {
  @include button-base;
  background: $bg;
  color: $color;

  &:hover {
    background: $hover-bg;
    transform: translateY(-1px);
    box-shadow: 0 4px 12px rgba($bg, 0.4);
  }

  &:active { transform: translateY(0); }
}

// Ishlatish
.btn-primary   { @include button-variant(#6366f1); }
.btn-success   { @include button-variant(#22c55e); }
.btn-danger    { @include button-variant(#ef4444); }
.btn-outline {
  @include button-base;
  background: transparent;
  border: 2px solid #6366f1;
  color: #6366f1;
  &:hover { background: #6366f1; color: white; }
}
Matn truncate mixin
@mixin truncate($lines: 1) {
  @if $lines == 1 {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  } @else {
    display: -webkit-box;
    -webkit-line-clamp: $lines;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
}

// Ishlatish
.card__title { @include truncate(1); }
.card__desc  { @include truncate(3); }
Mini-Loyiha: UI Komponent Kutubxonasi
Maqsad: Mixinlar asosida to'liq UI komponentlar yarating.

// Starter
@mixin flex($direction: row, $justify: flex-start, $align: center, $gap: 0) {
  display: flex;
  flex-direction: $direction;
  justify-content: $justify;
  align-items: $align;
  gap: $gap;
}
// Qolgan mixinlarni qo'shing...
3 ta vazifa:

respond-to mixin yordamida 3 xil screen size uchun adaptive card grid yarating
button-variant mixin bilan primary, secondary, danger, ghost — 4 xil tugma yarating
flex mixin yordamida header komponenti yarating (logo chapda, nav o'rtada, CTA o'ngda)
