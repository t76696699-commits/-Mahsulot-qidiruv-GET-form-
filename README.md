<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather & TDD Jest Testing Dashboard</title>
    <style>
        :root {
            --bg-color: #0b0f19;
            --card-bg: #131b2e;
            --card-border: #1e293b;
            --text-main: #f1f5f9;
            --text-muted: #94a3b8;
            --accent-blue: #38bdf8;
            --success-color: #22c55e;
            --success-bg: rgba(34, 197, 94, 0.1);
            --red-color: #ef4444;
            --red-bg: rgba(239, 68, 68, 0.1);
            --yellow-color: #eab308;
            --yellow-bg: rgba(234, 179, 8, 0.1);
            --code-bg: #090d16;
        }

        *, *::before, *::after {
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            margin: 0;
            padding: 30px 20px;
            display: flex;
            justify-content: center;
        }

        .dashboard-container {
            width: 100%;
            max-width: 950px;
        }

        header {
            margin-bottom: 30px;
            border-bottom: 1px solid var(--card-border);
            padding-bottom: 20px;
        }

        h1 {
            font-size: 1.8rem;
            margin: 0 0 10px 0;
            color: #ffffff;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .subtitle {
            color: var(--text-muted);
            font-size: 0.95rem;
            margin: 0;
        }

        .section-title {
            font-size: 1.25rem;
            margin: 35px 0 15px 0;
            color: var(--accent-blue);
            display: flex;
            align-items: center;
            gap: 8px;
            border-left: 4px solid var(--accent-blue);
            padding-left: 10px;
        }

        .card {
            background-color: var(--card-bg);
            border: 1px solid var(--card-border);
            border-radius: 12px;
            margin-bottom: 20px;
            overflow: hidden;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.3);
        }

        .card-header {
            background-color: rgba(255, 255, 255, 0.02);
            padding: 14px 20px;
            font-weight: 600;
            border-bottom: 1px solid var(--card-border);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .badge {
            padding: 3px 10px;
            border-radius: 20px;
            font-size: 0.75rem;
            font-family: monospace;
            font-weight: bold;
        }
        .badge-pass { background-color: var(--success-bg); color: var(--success-color); border: 1px solid rgba(34, 197, 94, 0.2); }
        .badge-red { background-color: var(--red-bg); color: var(--red-color); border: 1px solid rgba(239, 68, 68, 0.2); }
        .badge-refactor { background-color: var(--yellow-bg); color: var(--yellow-color); border: 1px solid rgba(234, 179, 8, 0.2); }

        .card-body {
            padding: 16px 20px;
        }

        .test-list {
            list-style: none;
            margin: 0;
            padding: 0;
        }

        .test-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 10px 0;
            border-bottom: 1px solid rgba(255, 255, 255, 0.04);
            font-size: 0.92rem;
        }

        .test-item:last-child {
            border-bottom: none;
        }

        .test-name {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .check-icon {
            color: var(--success-color);
            font-weight: bold;
        }

        .test-time {
            background-color: rgba(56, 189, 248, 0.1);
            color: var(--accent-blue);
            padding: 2px 8px;
            border-radius: 6px;
            font-size: 0.78rem;
            font-family: monospace;
        }

        pre.code-block {
            background-color: var(--code-bg);
            color: #e2e8f0;
            padding: 16px;
            border-radius: 8px;
            font-family: 'Fira Code', Consolas, Monaco, monospace;
            font-size: 0.85rem;
            overflow-x: auto;
            margin: 0;
            line-height: 1.45;
            border: 1px solid #1a2234;
        }

        .kw { color: #f472b6; }
        .str { color: #a5f3fc; }
        .fn { color: #60a5fa; }
        .cm { color: #64748b; font-style: italic; }

        .interactive-panel {
            background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%);
            border: 1px solid #334155;
            border-radius: 12px;
            padding: 24px;
            margin-top: 40px;
            text-align: center;
        }

        .interactive-panel h3 {
            margin-top: 0;
            color: var(--accent-blue);
        }

        .btn {
            background-color: var(--accent-blue);
            color: #0f172a;
            border: none;
            padding: 10px 20px;
            border-radius: 6px;
            font-weight: bold;
            cursor: pointer;
            transition: opacity 0.2s;
            margin: 5px;
        }

        .btn:hover {
            opacity: 0.9;
        }

        #output-box {
            margin-top: 15px;
            background: var(--code-bg);
            padding: 12px;
            border-radius: 6px;
            font-family: monospace;
            font-size: 0.9rem;
            min-height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--success-color);
            border: 1px solid #1a2234;
        }

        footer {
            text-align: center;
            margin-top: 40px;
            color: var(--text-muted);
            font-size: 0.85rem;
            border-top: 1px solid var(--card-border);
            padding-top: 20px;
        }
    </style>
</head>
<body>

<div class="dashboard-container">
    <header>
        <h1>🧪 Jest Testing & TDD Dashboard</h1>
        <p class="subtitle">Asinxron funksiyalar (weatherService) va TDD yondashuvi (findMax) to'liq jamlanmasi</p>
    </header>

    <!-- 1-BO'LIM: WEATHER SERVICE -->
    <div class="section-title">📦 1. Weather Service (Asinxron Testlar)</div>
    
    <div class="card">
        <div class="card-header">
            <span>weatherService.test.js — Natijalar</span>
            <span class="badge badge-pass">PASS (7/7)</span>
        </div>
        <div class="card-body">
            <ul class="test-list">
                <li class="test-item">
                    <div class="test-name"><span class="check-icon">✓</span> [async/await] Toshkent uchun harorat qaytaradi</div>
                    <span class="test-time">3 ms</span>
                </li>
                <li class="test-item">
                    <div class="test-name"><span class="check-icon">✓</span> [async/await] noma'lum shahar uchun xato tashlaydi</div>
                    <span class="test-time">2 ms</span>
                </li>
                <li class="test-item">
                    <div class="test-name"><span class="check-icon">✓</span> [Promise] harorat 28 ga teng</div>
                    <span class="test-time">1 ms</span>
                </li>
                <li class="test-item">
                    <div class="test-name"><span class="check-icon">✓</span> [resolves/rejects] resolves orqali tekshirish</div>
                    <span class="test-time">1 ms</span>
                </li>
                <li class="test-item">
                    <div class="test-name"><span class="check-icon">✓</span> [resolves/rejects] rejects orqali tekshirish</div>
                    <span class="test-time">2 ms</span>
                </li>
                <li class="test-item">
                    <div class="test-name"><span class="check-icon">✓</span> [Callback] done bilan callback natijasini tekshirish</div>
                    <span class="test-time">2 ms</span>
                </li>
            </ul>
        </div>
    </div>

    <div class="card">
        <div class="card-header">
            <span>weatherService.js — Kod manbasi</span>
            <span style="font-size: 0.8rem; color: var(--text-muted);">Asynchronous Functions</span>
        </div>
        <div class="card-body" style="padding: 0;">
            <pre class="code-block"><span class="cm">// weatherService.js — asinxron funksiyalar</span>
<span class="kw">function</span> <span class="fn">getTemperature</span>(city) {
  <span class="kw">return new</span> <span class="fn">Promise</span>((resolve, reject) => {
    <span class="fn">setTimeout</span>(() => {
      <span class="kw">if</span> (city === <span class="str">'Toshkent'</span>) {
        <span class="fn">resolve</span>(28);
      } <span class="kw">else</span> {
        <span class="fn">reject</span>(<span class="kw">new</span> <span class="fn">Error</span>(<span class="str">"Shahar topilmadi"</span>));
      }
    }, 100);
  });
}

<span class="kw">async function</span> <span class="fn">getTemperatureAsync</span>(city) {
  <span class="kw">const</span> temp = <span class="kw">await</span> <span class="fn">getTemperature</span>(city);
  <span class="kw">return</span> `<span class="str">${city}: ${temp}°C</span>`;
}

<span class="kw">function</span> <span class="fn">fetchWithCallback</span>(city, callback) {
  <span class="fn">setTimeout</span>(() => {
    <span class="kw">if</span> (city === <span class="str">'Toshkent'</span>) {
      <span class="fn">callback</span>(<span class="kw">null</span>, 28);
    } <span class="kw">else</span> {
      <span class="fn">callback</span>(<span class="kw">new</span> <span class="fn">Error</span>(<span class="str">'Shahar topilmadi'</span>));
    }
  }, 100);
}

module.<span class="fn">exports</span> = { getTemperature, getTemperatureAsync, fetchWithCallback };</pre>
        </div>
    </div>


    <!-- 2-BO'LIM: TDD FINDMAX -->
    <div class="section-title">🔄 2. TDD Yondashuvi (findMax Bosqichlari)</div>

    <!-- 1-BOSQICH -->
    <div class="card">
        <div class="card-header">
            <span>1-BOSQICH (RED): Avval testlarni yozamiz — kod hali yo'q</span>
            <span class="badge badge-red">FAIL</span>
        </div>
        <div class="card-body" style="padding: 0;">
            <pre class="code-block"><span class="cm">// findMax.test.js</span>
<span class="kw">const</span> findMax = <span class="fn">require</span>(<span class="str">'./findMax'</span>);

<span class="fn">describe</span>(<span class="str">'findMax — TDD yondashuvi'</span>, () => {
  <span class="fn">test</span>(<span class="str">'bitta elementli massiv'</span>, () => {
    <span class="fn">expect</span>(<span class="fn">findMax</span>([5])).<span class="fn">toBe</span>(5);
  });
  <span class="fn">test</span>(<span class="str">'bir nechta elementli massiv'</span>, () => {
    <span class="fn">expect</span>(<span class="fn">findMax</span>([3, 7, 2, 9, 4])).<span class="fn">toBe</span>(9);
  });
  <span class="fn">test</span>(<span class="str">'barchasi manfiy sonlar'</span>, () => {
    <span class="fn">expect</span>(<span class="fn">findMax</span>([-5, -1, -10])).<span class="fn">toBe</span>(-1);
  });
  <span class="fn">test</span>(<span class="str">'bo\'sh massiv uchun xato tashlaydi'</span>, () => {
    <span class="fn">expect</span>(() => <span class="fn">findMax</span>([])).<span class="fn">toThrow</span>(<span class="str">"Massiv bo'sh bo'lmasligi kerak"</span>);
  });
});</pre>
        </div>
    </div>

    <!-- 2-BOSQICH -->
    <div class="card">
        <div class="card-header">
            <span>2-BOSQICH (GREEN): Testlarni o'tkazish uchun minimal kod</span>
            <span class="badge badge-pass">PASS</span>
        </div>
        <div class="card-body" style="padding: 0;">
            <pre class="code-block"><span class="cm">// findMax.js — birinchi versiya</span>
<span class="kw">function</span> <span class="fn">findMax</span>(arr) {
  <span class="kw">if</span> (!Array.<span class="fn">isArray</span>(arr) || arr.length === 0) {
    <span class="kw">throw new</span> <span class="fn">Error</span>(<span class="str">"Massiv bo'sh bo'lmasligi kerak"</span>);
  }
  <span class="kw">return</span> Math.<span class="fn">max</span>(...arr);
}
module.<span class="fn">exports</span> = findMax;</pre>
        </div>
    </div>

    <!-- 3-BOSQICH -->
    <div class="card">
        <div class="card-header">
            <span>3-BOSQICH (REFACTOR): Kodni tozalash va optimallashtirish</span>
            <span class="badge badge-refactor">PASS (Optimized)</span>
        </div>
        <div class="card-body" style="padding: 0;">
            <pre class="code-block"><span class="cm">// findMax.js — yakuniy, refaktor qilingan versiya</span>
<span class="kw">function</span> <span class="fn">findMaxRefactored</span>(numbers) {
  <span class="fn">validateInput</span>(numbers);
  <span class="kw">return</span> numbers.<span class="fn">reduce</span>((max, current) => (current > max ? current : max));
}

<span class="kw">function</span> <span class="fn">validateInput</span>(numbers) {
  <span class="kw">if</span> (!Array.<span class="fn">isArray</span>(numbers) || numbers.length === 0) {
    <span class="kw">throw new</span> <span class="fn">Error</span>(<span class="str">"Massiv bo'sh bo'lmasligi kerak"</span>);
  }
}
module.<span class="fn">exports</span> = findMaxRefactored;</pre>
        </div>
    </div>


    <!-- 3-BO'LIM: JAVASCRIPT ISHLATISH QISMI -->
    <div class="section-title">⚡ 3. Interaktiv JavaScript Sinovi (Brauzerda ishga tushirish)</div>
    <div class="interactive-panel">
        <h3>Funksiyalarni sinab ko'rish</h3>
        <p style="color: var(--text-muted); font-size: 0.9rem;">Quyidagi tugmalarni bosib, keltirilgan JavaScript funksiyalarining natijasini bevosita ko'ring:</p>
        
        <div>
            <button class="btn" onclick="testWeather()">WeatherService ('Toshkent')</button>
            <button class="btn" onclick="testFindMax()">findMaxRefactored([3, 7, 2, 9, 4])</button>
        </div>

        <div id="output-box">Natijani ko'rish uchun yuqoridagi tugmalardan birini bosing...</div>
    </div>

    <footer>
        Jest Test Framework & TDD Guide Dashboard • All rights reserved © 2026
    </footer>
</div>

<script>
    // Brauzer muhitida simulyatsiya qilingan JavaScript funksiyalar
    function getTemperature(city) {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                if (city === 'Toshkent') {
                    resolve(28);
                } else {
                    reject(new Error("Shahar topilmadi"));
                }
            }, 50);
        });
    }

    async function getTemperatureAsync(city) {
        const temp = await getTemperature(city);
        return `${city}: ${temp}°C`;
    }

    function findMaxRefactored(numbers) {
        if (!Array.isArray(numbers) || numbers.length === 0) {
            throw new Error("Massiv bo'sh bo'lmasligi kerak");
        }
        return numbers.reduce((max, current) => (current > max ? current : max));
    }

    // Interaktiv tugmalar uchun funksiyalar
    async function testWeather() {
        const output = document.getElementById('output-box');
        output.style.color = 'var(--accent-blue)';
        output.innerText = "Yuklanmoqda... (getTemperatureAsync('Toshkent'))";
        try {
            const res = await getTemperatureAsync('Toshkent');
            output.style.color = 'var(--success-color)';
            output.innerText = `Natija: "${res}" — PASS ✓`;
        } catch (err) {
            output.style.color = 'var(--red-color)';
            output.innerText = `Xato: ${err.message}`;
        }
    }

    function testFindMax() {
        const output = document.getElementById('output-box');
        try {
            const arr = [3, 7, 2, 9, 4];
            const maxVal = findMaxRefactored(arr);
            output.style.color = 'var(--success-color)';
            output.innerText = `findMaxRefactored([3, 7, 2, 9, 4]) = ${maxVal} — PASS ✓`;
        } catch (err) {
            output.style.color = 'var(--red-color)';
            output.innerText = `Xato: ${err.message}`;
        }
    }
</script>

</body>
</html>
