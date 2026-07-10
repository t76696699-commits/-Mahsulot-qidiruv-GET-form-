CSS O'zgaruvchilari (Custom Properties)
Урок 4 из 5
· 2 раздела
✓ Пройден
📝
Текст
Текст
#1
CSS O'zgaruvchilari (Custom Properties)
CSS o'zgaruvchilari — qayta ishlatiladigan qiymatlarni saqlash va boshqarish imkonini beruvchi zamonaviy xususiyat. Ranglar, o'lchamlar, shriftlarni bir martta belgilab, butun loyiha bo'ylab ishlatish mumkin.

1. :root Ko'lami
:root {
  --primary-color: #3498db;
  --secondary-color: #2ecc71;
  --danger-color: #e74c3c;
  --font-size-base: 16px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --border-radius: 8px;
}
2. var() Funksiyasi
.button {
  background-color: var(--primary-color);
  padding: var(--spacing-sm) var(--spacing-md);
  font-size: var(--font-size-base);
  border-radius: var(--border-radius);
}

.alert-danger {
  background-color: var(--danger-color);
  padding: var(--spacing-md);
}
3. Zaxira Qiymatlar (Fallback Values)
.text-element {
  color: var(--text-color, #333333);
  font-size: var(--font-size-large, var(--font-size-base, 16px));
}
4. Media Query bilan O'zgartirish
:root {
  --font-size-h1: 3rem;
  --columns: 3;
  --padding-section: 80px;
}

@media (max-width: 768px) {
  :root {
    --font-size-h1: 2rem;
    --columns: 1;
    --padding-section: 40px;
  }
}

h1 { font-size: var(--font-size-h1); }
.grid { grid-template-columns: repeat(var(--columns), 1fr); }
5. Light/Dark Mavzu
:root {
  --bg-primary: #ffffff;
  --text-primary: #1a202c;
  --accent-color: #3b82f6;
}

[data-theme='dark'] {
  --bg-primary: #0f172a;
  --text-primary: #f1f5f9;
  --accent-color: #60a5fa;
}

body {
  background-color: var(--bg-primary);
  color: var(--text-primary);
  transition: background-color 0.3s ease, color 0.3s ease;
}
6. Dizayn Tizimi (Design System)
:root {
  --color-primary: #3b82f6;
  --color-success: #22c55e;
  --color-danger: #ef4444;

  --font-sans: 'Inter', sans-serif;
  --text-sm: 0.875rem;
  --text-base: 1rem;

  --space-2: 0.5rem;
  --space-4: 1rem;
  --radius-md: 8px;
  --shadow-sm: 0 1px 2px rgba(0,0,0,0.05);
  --transition-fast: 150ms cubic-bezier(0.4, 0, 0.2, 1);
}

.btn-primary {
  background-color: var(--color-primary);
  padding: var(--space-2) var(--space-4);
  border-radius: var(--radius-md);
  font-size: var(--text-sm);
  font-family: var(--font-sans);
  transition: background-color var(--transition-fast);
  box-shadow: var(--shadow-sm);
}
Mini-Loyiha: CSS O'zgaruvchilari bilan Tema Almashtirish
Quyidagi vazifalarni bajaring:

Asosiy ranglar va o'lchamlarni :root da CSS o'zgaruvchilari sifatida aniqlang.
Tugma bosish bilan qorong'u/yorug' tema almashuvchi mexanizm yarating.
Hech qanday rang to'g'ridan-to'g'ri yozilmasin — faqat o'zgaruvchilardan foydalaning.
:root { --color-bg: #fff; --color-text: #1a1a2e; --color-primary: #4f46e5; }
[data-theme="dark"] { --color-bg: #1a1a2e; --color-text: #f0f0f0; }
Maqsad: CSS o'zgaruvchilari bilan miqyoslanadigan dizayn tizimi yaratish.
