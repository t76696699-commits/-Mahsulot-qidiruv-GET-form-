<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Zamonaviy Portfolio & Xizmatlar</title>
    <style>
        /* 6. CSS Custom Properties (Variables) */
        :root {
            --primary-color: #6366f1;
            --primary-hover: #4f46e5;
            --secondary-color: #0f172a;
            --text-color: #334155;
            --light-bg: #f8fafc;
            --white: #ffffff;
            --card-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
            --card-shadow-hover: 0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1);
            --transition-speed: 0.3s;
            --border-radius: 12px;
        }

        /* Umumiy stillar */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            color: var(--text-color);
            background-color: var(--white);
            line-height: 1.6;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 20px;
        }

        section {
            padding: 80px 0;
        }

        .section-title {
            text-align: center;
            font-size: 2.25rem;
            color: var(--secondary-color);
            margin-bottom: 40px;
            font-weight: 700;
        }

        /* 1. Hero Section */
        .hero {
            background: linear-gradient(135deg, #e0e7ff 0%, #f1f5f9 100%);
            min-height: 80vh;
            display: flex;
            align-items: center;
            text-align: center;
        }

        .hero-content {
            max-width: 800px;
            margin: 0 auto;
        }

        .hero h1 {
            font-size: 3.5rem;
            color: var(--secondary-color);
            line-height: 1.2;
            margin-bottom: 20px;
            font-weight: 800;
        }

        .hero p {
            font-size: 1.25rem;
            margin-bottom: 35px;
            color: var(--text-color);
        }

        /* Tugma va Hover animatsiyasi */
        .btn {
            display: inline-block;
            background-color: var(--primary-color);
            color: var(--white);
            padding: 14px 32px;
            border-radius: 30px;
            text-decoration: none;
            font-weight: 600;
            transition: all var(--transition-speed) ease;
            box-shadow: 0 4px 14px rgba(99, 102, 241, 0.4);
        }

        .btn:hover {
            background-color: var(--primary-hover);
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(99, 102, 241, 0.6);
        }

        /* 2. Features/Services Section (Grid layout) */
        .services {
            background-color: var(--white);
        }

        .services-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 30px;
        }

        .service-card {
            background-color: var(--light-bg);
            padding: 40px 30px;
            border-radius: var(--border-radius);
            box-shadow: var(--card-shadow);
            transition: all var(--transition-speed) ease;
            border: 1px solid rgba(0, 0, 0, 0.03);
        }

        /* Kartadagi Hover animatsiyasi */
        .service-card:hover {
            transform: translateY(-8px);
            box-shadow: var(--card-shadow-hover);
            background-color: var(--white);
            border-color: var(--primary-color);
        }

        .service-icon {
            font-size: 2.5rem;
            color: var(--primary-color);
            margin-bottom: 20px;
        }

        .service-card h3 {
            font-size: 1.5rem;
            color: var(--secondary-color);
            margin-bottom: 15px;
        }

        /* 3. Testimonials Section (Flexbox layout) */
        .testimonials {
            background-color: var(--light-bg);
        }

        .testimonials-container {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 30px;
        }

        .testimonial-card {
            background-color: var(--white);
            padding: 35px;
            border-radius: var(--border-radius);
            box-shadow: var(--card-shadow);
            flex: 1 1 450px;
            max-width: 550px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        .testimonial-text {
            font-style: italic;
            font-size: 1.1rem;
            margin-bottom: 20px;
            color: var(--text-color);
        }

        .testimonial-author {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .author-avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background-color: #cbd5e1;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: var(--secondary-color);
        }

        .author-info h4 {
            color: var(--secondary-color);
            font-weight: 600;
        }

        .author-info p {
            font-size: 0.85rem;
            color: #64748b;
        }

        /* 4. Footer */
        footer {
            background-color: var(--secondary-color);
            color: #94a3b8;
            padding: 60px 0 30px 0;
        }

        .footer-content {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            gap: 40px;
            margin-bottom: 40px;
        }

        .footer-logo h2 {
            color: var(--white);
            margin-bottom: 15px;
        }

        .footer-contact p {
            margin-bottom: 10px;
        }

        .social-links {
            display: flex;
            gap: 15px;
        }

        .social-link {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background-color: #1e293b;
            color: var(--white);
            text-decoration: none;
            transition: all var(--transition-speed) ease;
        }

        .social-link:hover {
            background-color: var(--primary-color);
            transform: translateY(-3px);
        }

        .footer-bottom {
            text-align: center;
            border-top: 1px solid #1e293b;
            padding-top: 30px;
            font-size: 0.9rem;
        }

        /* 5. Responsive Layout (Media Queries) */
        @media (max-width: 768px) {
            .hero h1 {
                font-size: 2.5rem;
            }
            
            section {
                padding: 60px 0;
            }

            .footer-content {
                flex-direction: column;
                text-align: center;
            }

            .social-links {
                justify-content: center;
            }
        }
    </style>
</head>
<body>

    <!-- 1. Hero Section -->
    <section class="hero">
        <div class="container">
            <div class="hero-content">
                <h1>Raqamli Kelajagingizni Biz bilan Quring</h1>
                <p>Biz biznesingizni yangi bosqichga olib chiquvchi zamonaviy veb-saytlar, mobil ilovalar va dizayn xizmatlarini taqdim etamiz.</p>
                <a href="#services" class="btn">Xizmatlarni Ko'rish</a>
            </div>
        </div>
    </section>

    <!-- 2. Features/Services Section -->
    <section class="services" id="services">
        <div class="container">
            <h2 class="section-title">Bizning Xizmatlar</h2>
            <div class="services-grid">
                <!-- 1-karta -->
                <div class="service-card">
                    <div class="service-icon">💻</div>
                    <h3>Veb Dasturlash</h3>
                    <p>Siz xohlagan murakkablikdagi tezkor, xavfsiz va barcha qurilmalarga moslashuvchi veb-saytlarni yaratamiz.</p>
                </div>
                <!-- 2-karta -->
                <div class="service-card">
                    <div class="service-icon">📱</div>
                    <h3>Mobil Ilovalar</h3>
                    <p>iOS va Android platformalari uchun qulay interfeysga ega bo'lgan zamonaviy va funksional ilovalar.</p>
                </div>
                <!-- 3-karta -->
                <div class="service-card">
                    <div class="service-icon">🎨</div>
                    <h3>UI/UX Dizayn</h3>
                    <p>Foydalanuvchilarga yoqadigan va mahsulotingiz sotuvlarini oshiradigan jozibador interfeys dizayni.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- 3. Testimonials Section -->
    <section class="testimonials">
        <div class="container">
            <h2 class="section-title">Mijozlarimiz Fikrlari</h2>
            <div class="testimonials-container">
                <!-- 1-izoh -->
                <div class="testimonial-card">
                    <p class="testimonial-text">"Ushbu jamoa bilan ishlash juda ajoyib tajriba bo'ldi. Biz kutganimizdan ham tezroq va sifatliroq veb-sayt tayyorlab berishdi. Tavsiya qilaman!"</p>
                    <div class="testimonial-author">
                        <div class="author-avatar">AS</div>
                        <div class="author-info">
                            <h4>Asror Solihov</h4>
                            <p>"ApexTech" Asoschisi</p>
                        </div>
                    </div>
                </div>
                <!-- 2-izoh -->
                <div class="testimonial-card">
                    <p class="testimonial-text">"Dizayn borasida juda injiq mijozman, lekin yigitlar barcha istaklarimni mukammal darajada amalga oshirishdi. UI/UX dizayn shunchaki daho darajasida!"</p>
                    <div class="testimonial-author">
                        <div class="author-avatar">MO</div>
                        <div class="author-info">
                            <h4>Madina Omarova</h4>
                            <p>Art-Direktor, "Creativo"</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- 4. Footer Section -->
    <footer>
        <div class="container">
            <div class="footer-content">
                <div class="footer-logo">
                    <h2>BrandName</h2>
                    <p>Sizning raqamli muvaffaqiyatingiz bizning maqsadimiz.</p>
                </div>
                <div class="footer-contact">
                    <h3>Aloqa</h3>
                    <p>📍 Toshkent sh., Chilonzor tumani</p>
                    <p>📞 +998 (90) 123-45-67</p>
                    <p>✉️ info@brandname.uz</p>
                </div>
                <div>
                    <h3 style="margin-bottom: 15px; color: var(--white)">Bizni Kuzating</h3>
                    <div class="social-links">
                        <a href="#" class="social-link">TG</a>
                        <a href="#" class="social-link">IN</a>
                        <a href="#" class="social-link">FB</a>
                    </div>
                </div>
            </div>
            <div class="footer-bottom">
                <p>&copy; 2026 BrandName. Barcha huquqlar himoyalangan.</p>
            </div>
        </div>
    </footer>

</body>
</html>
