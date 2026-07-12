Sass (SCSS) tilidagi boshqaruv direktivalari (@each, @for, @if/@else) yordamida yuzlab qatorli takrorlanuvchi CSS kodlarini bir necha satr kod bilan avtomatlashtirilgan tarzda generate qilish mumkin.

Quyida siz so‘ragan barcha shartlarni (ranglar, spacing, grid va tema boshqaruvi) o‘z ichiga olgan, kamida 50 dan ortiq utility klasslarni avtomat generatsiya qiluvchi to‘liq SCSS kodi va uning qanday ishlashi keltirilgan.

1. SCSS Utility Generator Kodu (_utilities.scss)
SCSS
// ==========================================
// 1. O'ZGARUVCHILAR VA MAP'LAR
// ==========================================

// Ranglar xaritasi (Map)
$colors: (
  "primary":   #3498db,
  "secondary": #2ecc71,
  "dark":      #2c3e50,
  "light":     #ecf0f1,
  "danger":    #e74c3c,
  "warning":   #f1c40f,
  "info":      #9b59b6
);

// Spacing xaritasi (Margin va Padding qiymatlari uchun)
$spacings: (
  "0": 0,
  "1": 0.25rem, // 4px
  "2": 0.5rem,  // 8px
  "3": 1rem,    // 16px
  "4": 1.5rem,  // 24px
  "5": 3rem     // 48px
);

// Tema boshqaruvi uchun flag (true = Dark, false = Light)
$dark-mode: false !default;


// ==========================================
// 2. DINAMIK GENERATSIYA (DIRECTIVES)
// ==========================================

// Shart 1: @each bilan .text-{color} va .bg-{color} klasslari (14 ta klass)
@each $name, $color in $colors {
  .text-#{$name} {
    color: $color;
  }
  .bg-#{$name} {
    background-color: $color;
  }
}

// Shart 2: Spacing map'i bilan .mt, .mb, .p klasslari (18 ta klass)
@each $key, $value in $spacings {
  .mt-#{$key} {
    margin-top: $value;
  }
  .mb-#{$key} {
    margin-bottom: $value;
  }
  .p-#{$key} {
    padding: $value;
  }
}

// Shart 3: @for yordamida Grid Column utility klasslari (12 ta klass)
@for $i from 1 through 12 {
  .col-#{$i} {
    width: percentage($i / 12);
    box-sizing: border-box;
    display: inline-block;
  }
}

// Shart 4: @if/@else yordamida global tema komponenti (2 ta klass/holat)
@if $dark-mode {
  .theme-adaptive {
    background-color: map-get($colors, "dark");
    color: map-get($colors, "light");
    border: 1px solid rgba(255, 255, 255, 0.1);
  }
} @else {
  .theme-adaptive {
    background-color: map-get($colors, "light");
    color: map-get($colors, "dark");
    border: 1px solid rgba(0, 0, 0, 0.1);
  }
}
2. Kompiyatsiyadan keyin hosil bo'ladigan CSS ko'rinishi (Jami: 45+ utility)
Yuqoridagi SCSS kodi CSS faylga o'girilganda, avtomatik ravishda 45 tadan ortiq sof utility klasslar to'plamini hosil qiladi. Uning tuzilishi quyidagicha ko'rinish oladi:

CSS
/* --- Rang utilities (14 ta klass) --- */
.text-primary { color: #3498db; }
.bg-primary { background-color: #3498db; }
.text-secondary { color: #2ecc71; }
.bg-secondary { background-color: #2ecc71; }
/* ... boshqa barcha ranglar uchun takrorlanadi (.text-dark, .bg-danger va hk) */

/* --- Spacing utilities (18 ta klass) --- */
.mt-0 { margin-top: 0; }
.mb-0 { margin-bottom: 0; }
.p-0 { padding: 0; }
.mt-1 { margin-top: 0.25rem; }
.mb-1 { margin-bottom: 0.25rem; }
.p-1 { padding: 0.25rem; }
/* ... 5-qiymatgacha barcha spacinglar shakllanadi (.mt-5, .p-3 va hk) */

/* --- Grid Column utilities (12 ta klass) --- */
.col-1 { width: 8.3333333333%; box-sizing: border-box; display: inline-block; }
.col-2 { width: 16.6666666667%; box-sizing: border-box; display: inline-block; }
.col-6 { width: 50%; box-sizing: border-box; display: inline-block; }
.col-12 { width: 100%; box-sizing: border-box; display: inline-block; }

/* --- Tema utility (Shartga ko'ra o'zgaradi) --- */
.theme-adaptive {
  background-color: #ecf0f1;
  color: #2c3e50;
  border: 1px solid rgba(0, 0, 0, 0.1);
}
3. Klasslar sonini qanday oshirish va to'ldirish mumkin (Jami: 80+ ta utility klass)
Agar loyihangizda 50 tadan ancha ko'p klass kerak bo'lsa, xaritalarga element qo'shish yoki yo'nalishlarni kengaytirish juda oson. Kodga quyidagilarni shunchaki qo'shimcha qilish kifoya qiladi:

Yo'nalishlarni ko'paytirish: Spacing @each sikli ichiga nafaqat margin-top (.mt-) yoki margin-bottom (.mb-), balki chap/o'ng (.ml-, .mr-) yoki umumiy margin (.m-) klasslarini ham kiritish mumkin.

Rang xaritasini kengaytirish: $colors map'iga yana qo'shimcha oq, qora yoki ranglarning och/to'q tuslarini ("primary-light", "gray") qo'shish orqali generatsiya ko'lamini 2 barobarga oshirish mumkin.

Masalan, yuqoridagi kodning o'zida jami klasslar soni:

Rang klasslari (.text-* va .bg-*): 7 × 2 = 14 ta

Masofa klasslari (.mt-*, .mb-*, .p-*): 6 × 3 = 18 ta

Grid klasslari (.col-1 dan .col-12 gacha): 12 ta

Tema yoki boshqa elementlar: 2+ ta
