@extend nima?
@extend bir selectorning barcha CSS xususiyatlarini boshqa selectorda takrorlash imkonini beradi. Compile qilinsa, selectorlar vergul bilan birlashtiriladi.

Oddiy @extend
.message {
  padding: 12px 16px;
  border-radius: 8px;
  font-size: 0.9rem;
  font-weight: 500;
}

.message-success {
  @extend .message;
  background: #dcfce7;
  color: #16a34a;
  border-left: 4px solid #22c55e;
}

.message-error {
  @extend .message;
  background: #fee2e2;
  color: #dc2626;
  border-left: 4px solid #ef4444;
}

// Compile natijasi:
// .message, .message-success, .message-error { padding: 12px 16px; ... }
// .message-success { background: #dcfce7; ... }
%Placeholder selector
Placeholder (%) — faqat extend uchun yaratilgan "mavhum" selector. U CSS output da chiqmaydi, faqat extend qilganda kiritiladi:

%card-base {
  background: white;
  border-radius: 12px;
  padding: 24px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  transition: transform 0.2s, box-shadow 0.2s;
}

%flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

%hover-lift {
  &:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 24px rgba(0,0,0,0.12);
  }
}

// Ishlatish
.feature-card {
  @extend %card-base;
  @extend %hover-lift;
  text-align: center;
}

.pricing-card {
  @extend %card-base;
  @extend %hover-lift;
  border: 2px solid transparent;

  &--featured {
    border-color: #6366f1;
  }
}

.stat-card {
  @extend %card-base;
  display: flex;
  gap: 16px;
}
@extend vs @mixin — qachon qaysinisini ishlatish kerak?
// MIXIN — parametr kerak bo'lganda, har xil qiymatlar bilan
@mixin button($bg: #6366f1, $color: white) {
  padding: 10px 20px;
  background: $bg;
  color: $color;
  border-radius: 8px;
}

.btn-primary { @include button(#6366f1); }
.btn-danger  { @include button(#ef4444); }

// EXTEND/PLACEHOLDER — bir xil CSS ni takrorlash shart bo'lganda
%card { padding: 24px; border-radius: 12px; background: white; }

.user-card    { @extend %card; }
.product-card { @extend %card; }
.stats-card   { @extend %card; }
// Natija: .user-card, .product-card, .stats-card { padding: 24px; ... }
// MIXIN ishlatilganda: har birida alohida takrorlanadi
Haqiqiy loyiha: Alert komponenti
%alert-base {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  padding: 14px 16px;
  border-radius: 8px;
  border-left: 4px solid;
  font-size: 0.9rem;
  line-height: 1.5;
}

.alert {
  &--info {
    @extend %alert-base;
    background: #eff6ff;
    color: #1d4ed8;
    border-color: #3b82f6;
  }

  &--success {
    @extend %alert-base;
    background: #f0fdf4;
    color: #15803d;
    border-color: #22c55e;
  }

  &--warning {
    @extend %alert-base;
    background: #fffbeb;
    color: #b45309;
    border-color: #f59e0b;
  }

  &--error {
    @extend %alert-base;
    background: #fef2f2;
    color: #b91c1c;
    border-color: #ef4444;
  }
}
Mini-Loyiha: Komponent Sistemi
Maqsad: Placeholder va @extend yordamida umumiy komponent tizimi yarating.

// Starter
%base-interactive {
  cursor: pointer;
  transition: all 0.2s ease;
  user-select: none;
}
// Placeholder va @extend qo'shing...
3 ta vazifa:

%card-base, %flex-center, %hover-lift placeholderlarini yarating
Kamida 3 xil karta komponenti yarating (@extend bilan)
4 xil alert variantini (.alert--success, --warning, --error, --info) yarating
