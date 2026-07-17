
Чат с Gemini
Fonda Hisob-Kitob Tizimi (Web Workers)

📋 Vazifa sharti: Web Worker yordamida og'ir hisob-kitoblarni asosiy interfeys bilan parallel olib boruvchi tizim yarating. 1-qism: Worker yaratish: - worker.js faylida n-chi Fibonacci sonini hisoblash funksiyasini yozing - onmessage orqali n qiymatini qabul qilib, natijani postMessage bilan qaytaring 2-qism: Asosiy sahifada: - Foydalanuvchi n qiymatini kiritib "Hisoblash" tugmasini bossin - new Worker("worker.js") yaratib unga n ni postMessage orqali yuboring - Natija kelguncha "Hisoblanmoqda..." ko'rsating, keyin natijani chiqaring 3-qism: Interfeys sezgirligini namoyish qiling: - Hisoblash davom etayotganda sahifadagi boshqa elementlar ishlashini ko'rsating - Worker xatosi bo'lganda onerror orqali uni ushlang - Worker.terminate() ni tugma orqali sinab ko'ring

✗ 50/100 — Отклонено

 Требования

• Alohida worker.js fayl yaratilgan bo'lsin

• postMessage va onmessage to'g'ri ishlatilgan bo'lsin

• Asosiy sahifa Worker ishlayotganda ham sezgir bo'lishi ko'rsatilsin

• onerror xatolarni ushlashi kerak

• Worker.terminate() ishlashi kerak

• n >= 40 uchun hisoblash Worker da amalga oshirilsin



🛠 Ishlatiladigan konstruktsiyalar:

new Worker("worker.js"); worker.postMessage(data);

self.onmessage = (e) => { self.postMessage(result); };

worker.onerror = (e) => {}; worker.terminate();

 Стек технологий

JavaScript

Web Workers

postMessage

onmessage

Fibonacci

Ushbu topshiriqni bajarish uchun biz kodni ikki qismga: worker.js (hisoblash qismi) va index.html (interfeys qismi) ga ajratamiz.

