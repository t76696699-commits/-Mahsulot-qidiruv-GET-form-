<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tariflar va FAQ</title>
    <style>
        /* CSS Custom Properties */
        :root {
            --primary: #4f46e5;
            --primary-hover: #4338ca;
            --secondary: #0f172a;
            --text-main: #1e293b;
            --text-muted: #64748b;
            --bg-light: #f8fafc;
            --white: #ffffff;
            --border: #e2e8f0;
            --radius-lg: 24px;
            --radius-md: 16px;
            --shadow: 0 10px 15px -3px rgba(0,0,0,0.05), 0 4px 6px -4px rgba(0,0,0,0.05);
            --shadow-hover: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
            --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            background-color: var(--bg-light);
            color: var(--text-main);
            line-height: 1.6;
            padding: 60px 20px;
        }

        .container {
            max-width: 1100px;
            margin: 0 auto;
        }

        .header {
            text-align: center;
            margin-bottom: 50px;
        }

        .header h1 {
            font-size: 2.5rem;
            font-weight: 800;
            color: var(--secondary);
            margin-bottom: 15px;
        }

        .header p {
            color: var(--text-muted);
            font-size: 1.1rem;
        }

        /* 1. Toggle - Oylik/Yillik (Faqat CSS yordamida) */
        .toggle-wrapper {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
            margin-bottom: 50px;
        }

        .toggle-label {
            font-weight: 600;
            font-size: 0.95rem;
            color: var(--text-muted);
            transition: var(--transition);
        }

        .switch {
            position: relative;
            display: inline-block;
            width: 60px;
            height: 34px;
        }

        .switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }

        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #cbd5e1;
            transition: var(--transition);
            border-radius: 34px;
        }

        .slider:before {
            position: absolute;
            content: "";
            height: 26px;
            width: 26px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: var(--transition);
            border-radius: 50%;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        input:checked + .slider {
            background-color: var(--primary);
        }

        input:focus-visible + .slider {
            outline: 2px solid var(--primary);
            outline-offset: 2px;
        }

        input:checked + .slider:before {
            transform: translateX(26px);
        }

        /* Narxlarni almashish mantig'i */
        .price-yearly {
            display: none;
        }

        /* Input belgilanganda oylikni yashirib, yillikni ko'rsatish */
        #billing-toggle:checked ~ .pricing-grid .price-monthly {
            display: none;
        }
        #billing-toggle:checked ~ .pricing-grid .price-yearly {
            display: inline-block;
        }

        /* Aktiv holatdagi tekst rangini o'zgartirish */
        #billing-toggle:not(:checked) ~ .toggle-wrapper .label-monthly {
            color: var(--secondary);
        }
        #billing-toggle:checked ~ .toggle-wrapper .label-yearly {
            color: var(--secondary);
        }

        /* 2. 3 ta tarif kartasi (Grid Layout) */
        .pricing-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 30px;
            align-items: center;
            margin-bottom: 80px;
        }

        .card {
            background-color: var(--white);
            border-radius: var(--radius-lg);
            padding: 40px 30px;
            border: 1px solid var(--border);
            box-shadow: var(--shadow);
            position: relative;
            transition: var(--transition);
            display: flex;
            flex-direction: column;
            height: 100%;
        }

        /* 7. Hover & Focus animatsiyalari kartalarda */
        .card:hover {
            transform: translateY(-8px);
            box-shadow: var(--shadow-hover);
        }

        /* 4. Pro karta vizual ajratilgan */
        .card.featured {
            border-color: var(--primary);
            border-width: 2px;
            transform: scale(1.03);
            box-shadow: 0 20px 25px -5px rgba(79, 70, 229, 0.1);
        }

        .card.featured:hover {
            transform: scale(1.03) translateY(-8px);
        }

        .badge-popular {
            position: absolute;
            top: -15px;
            left: 50%;
            transform: translateX(-50%);
            background-color: var(--primary);
            color: var(--white);
            padding: 6px 16px;
            border-radius: 50px;
            font-size: 0.8rem;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.05em;
        }

        .card-header h3 {
            font-size: 1.5rem;
            color: var(--secondary);
            margin-bottom: 10px;
        }

        .card-header .description {
            color: var(--text-muted);
            font-size: 0.95rem;
            margin-bottom: 25px;
            height: 48px;
        }

        .card-price {
            margin-bottom: 25px;
        }

        .amount {
            font-size: 2.75rem;
            font-weight: 800;
            color: var(--secondary);
        }

        .period {
            font-size: 1rem;
            color: var(--text-muted);
        }

        .features-list {
            list-style: none;
            margin-bottom: 35px;
            flex-grow: 1;
        }

        .features-list li {
            margin-bottom: 14px;
            font-size: 0.95rem;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .features-list li.unavailable {
            color: var(--text-muted);
            text-decoration: line-through;
        }

        .icon-check { color: #10b981; font-weight: bold; }
        .icon-x { color: #ef4444; font-weight: bold; }

        /* CTA Tugmalari */
        .btn-pricing {
            display: block;
            text-align: center;
            padding: 14px;
            border-radius: 12px;
            text-decoration: none;
            font-weight: 700;
            transition: var(--transition);
            border: 2px solid var(--primary);
            background-color: transparent;
            color: var(--primary);
        }

        .btn-pricing:hover, .btn-pricing:focus-visible {
            background-color: var(--primary);
            color: var(--white);
            outline: none;
            box-shadow: 0 0 0 4px rgba(79, 70, 229, 0.2);
        }

        .featured .btn-pricing {
            background-color: var(--primary);
            color: var(--white);
        }

        .featured .btn-pricing:hover, .featured .btn-pricing:focus-visible {
            background-color: var(--primary-hover);
            border-color: var(--primary-hover);
        }

        /* 5. Xususiyatlar taqqoslash jadvali */
        .table-section {
            margin-bottom: 80px;
        }

        .table-section h2 {
            text-align: center;
            font-size: 1.75rem;
            margin-bottom: 35px;
            color: var(--secondary);
        }

        .comparison-table-wrapper {
            background-color: var(--white);
            border-radius: var(--radius-md);
            border: 1px solid var(--border);
            box-shadow: var(--shadow);
            overflow-x: auto;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            text-align: left;
        }

        th, td {
            padding: 16px 24px;
            border-bottom: 1px solid var(--border);
            font-size: 0.95rem;
        }

        th {
            background-color: #f1f5f9;
            color: var(--secondary);
            font-weight: 700;
        }

        tr:hover td {
            background-color: var(--bg-light);
        }

        /* 6. FAQ Section (details/summary) */
        .faq-section {
            max-width: 800px;
            margin: 0 auto;
        }

        .faq-section h2 {
            text-align: center;
            font-size: 1.75rem;
            margin-bottom: 35px;
            color: var(--secondary);
        }

        .faq-list {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        details {
            background-color: var(--white);
            border: 1px solid var(--border);
            border-radius: 12px;
            padding: 20px;
            box-shadow: var(--shadow);
            transition: var(--transition);
        }

        details[open] {
            border-color: var(--primary);
        }

        summary {
            font-size: 1.1rem;
            font-weight: 700;
            color: var(--secondary);
            cursor: pointer;
            list-style: none;
            display: flex;
            justify-content: space-between;
            align-items: center;
            outline: none;
        }

        summary::-webkit-details-marker {
            display: none; /* Safari uchun standart o'qchani olib tashlash */
        }

        summary::after {
            content: "+";
            font-size: 1.5rem;
            font-weight: bold;
            color: var(--text-muted);
            transition: var(--transition);
        }

        details[open] summary::after {
            content: "−";
            transform: rotate(180deg);
            color: var(--primary);
        }

        details p {
            margin-top: 15px;
            color: var(--text-muted);
            font-size: 0.95rem;
            line-height: 1.6;
        }

        /* Interactive focus support */
        summary:focus-visible {
            color: var(--primary);
        }

        /* Responsive */
        @media (max-width: 768px) {
            .pricing-grid {
                grid-template-columns: 1fr;
                gap: 40px;
            }

            .card.featured {
                transform: none;
            }

            .card.featured:hover {
                transform: translateY(-8px);
            }
        }
    </style>
</head>
<body>

    <div class="container">
        
        <!-- Sarlavha -->
        <div class="header">
            <h1>Sizga mos keladigan tarif</h1>
            <p>Hech qanday yashirin to'lovlarsiz shaffof va qulay narxlar.</p>
        </div>

        <!-- 1. Toggle Element (Narxlar almashishi uchun) -->
        <input type="checkbox" id="billing-toggle" style="display: none;">
        
        <div class="toggle-wrapper">
            <span class="toggle-label label-monthly">Oylik</span>
            <label class="switch" for="billing-toggle">
                <span class="slider"></span>
            </label>
            <span class="toggle-label label-yearly">Yillik (20% chegirma)</span>
        </div>

        <!-- 2. Tarif Kartalari -->
        <div class="pricing-grid">
            
            <!-- Starter karta -->
            <div class="card">
                <div class="card-header">
                    <h3>Starter</h3>
                    <p class="description">Shaxsiy loyihalar va endi boshlovchilar uchun.</p>
                </div>
                <div class="card-price">
                    <span class="amount price-monthly">$19</span>
                    <span class="amount price-yearly">$15</span>
                    <span class="period">/ oyiga</span>
                </div>
                <ul class="features-list">
                    <li><span class="icon-check">✓</span> 1 ta foydalanuvchi</li>
                    <li><span class="icon-check">✓</span> 5 GB xotira</li>
                    <li><span class="icon-check">✓</span> Asosiy tahlillar (analytics)</li>
                    <li class="unavailable"><span class="icon-x">✗</span> Hamkorlik asboblari</li>
                    <li class="unavailable"><span class="icon-x">✗</span> 24/7 Premium qo'llab-quvvatlash</li>
                </ul>
                <a href="#" class="btn-pricing">Hozir boshlash</a>
            </div>

            <!-- Pro karta (Featured) -->
            <div class="card featured">
                <span class="badge-popular">Eng ommabop</span>
                <div class="card-header">
                    <h3>Pro</h3>
                    <p class="description">Tez rivojlanayotgan jamoalar va professionallar uchun.</p>
                </div>
                <div class="card-price">
                    <span class="amount price-monthly">$49</span>
                    <span class="amount price-yearly">$39</span>
                    <span class="period">/ oyiga</span>
                </div>
                <ul class="features-list">
                    <li><span class="icon-check">✓</span> 5 tagacha foydalanuvchi</li>
                    <li><span class="icon-check">✓</span> 50 GB xotira</li>
                    <li><span class="icon-check">✓</span> Kengaytirilgan tahlillar</li>
                    <li><span class="icon-check">✓</span> Jamoaviy hamkorlik</li>
                    <li class="unavailable"><span class="icon-x">✗</span> 24/7 Premium yordam</li>
                </ul>
                <a href="#" class="btn-pricing">Sinab ko'rish</a>
            </div>

            <!-- Enterprise karta -->
            <div class="card">
                <div class="card-header">
                    <h3>Enterprise</h3>
                    <p class="description">Katta korxonalar va maxsus talablar uchun.</p>
                </div>
                <div class="card-price">
                    <span class="amount price-monthly">$99</span>
                    <span class="amount price-yearly">$79</span>
                    <span class="period">/ oyiga</span>
                </div>
                <ul class="features-list">
                    <li><span class="icon-check">✓</span> Cheksiz foydalanuvchi</li>
                    <li><span class="icon-check">✓</span> 500 GB xotira</li>
                    <li><span class="icon-check">✓</span> To'liq maxsus tahlillar</li>
                    <li><span class="icon-check">✓</span> Kengaytirilgan xavfsizlik</li>
                    <li><span class="icon-check">✓</span> 24/7 Premium yordam</li>
                </ul>
                <a href="#" class="btn-pricing">Aloqaga chiqish</a>
            </div>

        </div>

        <!-- 5. Taqqoslash Jadvali -->
        <section class="table-section">
            <h2>Imkoniyatlarni taqqoslang</h2>
            <div class="comparison-table-wrapper">
                <table>
                    <thead>
                        <tr>
                            <th>Xususiyat</th>
                            <th>Starter</th>
                            <th>Pro</th>
                            <th>Enterprise</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>Loyiha soni</td>
                            <td>3 ta loyiha</td>
                            <td>15 ta loyiha</td>
                            <td>Cheksiz</td>
                        </tr>
                        <tr>
                            <td>Ma'lumotlarni saqlash</td>
                            <td>5 GB</td>
                            <td>50 GB</td>
                            <td>500 GB</td>
                        </tr>
                        <tr>
                            <td>API kirish huquqi</td>
                            <td><span class="icon-x">✗</span> Yo'q</td>
                            <td><span class="icon-check">✓</span> Bor</td>
                            <td><span class="icon-check">✓</span> Bor</td>
                        </tr>
                        <tr>
                            <td>Maxsus domen ulanishi</td>
                            <td><span class="icon-x">✗</span> Yo'q</td>
                            <td><span class="icon-check">✓</span> Bor</td>
                            <td><span class="icon-check">✓</span> Bor</td>
                        </tr>
                        <tr>
                            <td>Qo'llab-quvvatlash</td>
                            <td>Email orqali</td>
                            <td>Ish vaqtida</td>
                            <td>24/7 Premium VIP</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </section>

        <!-- 6. FAQ Section (Savol-javoblar) -->
        <section class="faq-section">
            <h2>Ko'p so'raladigan savollar</h2>
            <div class="faq-list">
                
                <details>
                    <summary>Sotib olishdan oldin bepul sinab ko'rsam bo'ladimi?</summary>
                    <p>Ha, albatta! Istalgan tarifimizni 14 kun davomida mutlaqo bepul va hech qanday bank kartasisiz sinab ko'rishingiz mumkin.</p>
                </details>

                <details>
                    <summary>Tarifni istalgan vaqtda o'zgartirsam yoki bekor qilsam bo'ladimi?</summary>
                    <p>Ha, xohlagan vaqtingizda profilingiz orqali tarifingizni oshirishingiz, pasaytirishingiz yoki obunani butunlay bekor qilishingiz mumkin. Hech qanday jarima to'lovlari yo'q.</p>
                </details>

                <details>
                    <summary>Yillik to'lovning qanday afzalligi bor?</summary>
                    <p>Yillik obunani tanlasangiz, jami summadan 20% gacha tejab qolasiz. Bu oylik to'lovga qaraganda ancha foydalidir.</p>
                </details>

                <details>
                    <summary>To'lovlar xavfsizligi qanday ta'minlanadi?</summary>
                    <p>Biz barcha to'lovlarni xalqaro xavfsizlik talablariga javob beradigan va shifrlangan shlyuzlar (Stripe/PayPal) orqali qabul qilamiz. Karta ma'lumotlaringiz bizning serverlarda saqlanmaydi.</p>
                </details>

            </div>
        </section>

    </div>

</body>
</html>
