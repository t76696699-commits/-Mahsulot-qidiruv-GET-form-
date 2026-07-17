1. worker.js fayli
Ushbu fayl hisoblash jarayonini asosiy sahifadan ajratib turadi.

JavaScript
// worker.js

// Fibonacci funksiyasi (og'ir hisob-kitob)
function fibonacci(n) {
    if (n < 0) throw new Error("Musbat son kiriting");
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

// Asosiy sahifadan xabar kelganda
self.onmessage = function(e) {
    const n = e.data;
    try {
        const result = fibonacci(n);
        // Natijani qaytarish
        self.postMessage({ success: true, result: result });
    } catch (err) {
        // Xatolik bo'lsa xabar berish
        self.postMessage({ success: false, error: err.message });
    }
};
2. index.html fayli
Ushbu fayl interfeysni ta'minlaydi va Worker bilan bog'lanadi.

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>Web Worker Fibonacci</title>
    <style>
        body { font-family: sans-serif; padding: 20px; }
        #status { color: blue; font-weight: bold; }
    </style>
</head>
<body>

    <h2>Fibonacci Hisoblagich (Web Workers)</h2>
    
    <input type="number" id="numInput" placeholder="n (masalan: 40)">
    <button id="btnStart">Hisoblash</button>
    <button id="btnStop">To'xtatish (Terminate)</button>
    
    <p id="status"></p>
    <h3 id="result"></h3>

    <hr>
    <p>Interfeys sezgirligini tekshirish uchun quyidagi maydonga yozing:</p>
    <input type="text" placeholder="Men hali ham ishlayapman...">

    <script>
        let worker;
        const btnStart = document.getElementById('btnStart');
        const btnStop = document.getElementById('btnStop');
        const status = document.getElementById('status');
        const resultDisplay = document.getElementById('result');

        btnStart.addEventListener('click', () => {
            const n = parseInt(document.getElementById('numInput').value);
            
            // Eski worker bo'lsa, uni to'xtatamiz
            if (worker) worker.terminate();

            // Yangi worker yaratish
            worker = new Worker('worker.js');

            status.textContent = "Hisoblanmoqda...";
            resultDisplay.textContent = "";

            // Worker dan javob kelganda
            worker.onmessage = function(e) {
                if (e.data.success) {
                    resultDisplay.textContent = `Natija: ${e.data.result}`;
                    status.textContent = "Hisoblash tugadi.";
                } else {
                    status.textContent = "Xatolik: " + e.data.error;
                }
            };

            // Xatolarni ushlash (onerror)
            worker.onerror = function(err) {
                status.textContent = "Worker da kritik xatolik yuz berdi!";
                console.error("Worker Error:", err);
                worker.terminate();
            };

            // Worker ga n ni yuborish
            worker.postMessage(n);
        });

        // Worker ni to'xtatish
        btnStop.addEventListener('click', () => {
            if (worker) {
                worker.terminate();
                status.textContent = "Hisoblash majburiy to'xtatildi.";
            }
        });
    </script>
</body>
</html>
Loyihani qanday ishga tushirish kerak?
Fayllarni saqlash: worker.js va index.html fayllarini bitta papkaga joylashtiring.

Server orqali ochish: Web Workers xavfsizlik cheklovlari tufayli brauzerda file:// protokoli orqali ishlamaydi.

Agar VS Code ishlatayotgan bo'lsangiz, "Live Server" kengaytmasini o'rnating va index.html ustiga o'ng tugmani bosib, "Open with Live Server" ni tanlang.

Tekshirish:

n qiymatiga 40 yoki 42 kiriting va "Hisoblash" tugmasini bosing.

Hisoblash ketayotgan paytda sahifaning pastidagi "Men hali ham ishlayapman..." maydoniga yozib ko'ring — interfeys "muzlamayotganini" ko'rasiz.

"To'xtatish" tugmasini bosib, jarayonni bekor qilishni sinab ko'ring.
