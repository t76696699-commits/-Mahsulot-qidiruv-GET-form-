<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Professional Portfolio</title>
    <style>
        /* 7. CSS Custom Properties (Dark Professional Scheme) */
        :root {
            --bg-main: #0f172a;       /* To'q ko'k-kulrang fon */
            --bg-card: #1e293b;       /* Kartalar uchun ochroq to'q rang */
            --primary: #38bdf8;       /* Neon havorang (asosiy urg'u) */
            --primary-hover: #0ea5e9; /* Hover holatidagi havorang */
            --text-main: #f8fafc;     /* Oq-kumushrang matn */
            --text-muted: #94a3b8;    /* Xira matn */
            --border: #334155;        /* Chegaralar */
            --radius: 12px;
            --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        /* 6. Silliq scroll (Smooth Scroll) */
        html {
            scroll-behavior: smooth;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            background-color: var(--bg-main);
            color: var(--text-main);
            line-height: 1.6;
        }

        .container {
            max-width: 1140px;
            margin: 0 auto;
            padding: 0 24px;
        }

        section {
            padding: 100px 0;
            border-bottom: 1px solid var(--border);
        }

        .section-title {
            font-size: 2.25rem;
            font-weight: 800;
            margin-bottom: 45px;
            text-align: center;
            position: relative;
        }

        .section-title span {
            color: var(--primary);
        }

        /* 6. Navigatsiya paneli */
        nav {
            background-color: rgba(15, 23, 42, 0.8);
            backdrop-filter: blur(12px);
            position: fixed;
            top: 0;
            width: 100%;
            z-index: 1000;
            border-bottom: 1px solid var(--border);
            padding: 20px 0;
        }

        .nav-container {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo {
            font-weight: 800;
            font-size: 1.3rem;
            color: var(--white);
            text-decoration: none;
        }

        .logo span { color: var(--primary); }

        .nav-links {
            display: flex;
            gap: 30px;
            list-style: none;
        }

        .nav-links a {
            color: var(--text-muted);
            text-decoration: none;
            font-weight: 500;
            transition: var(--transition);
        }

        .nav-links a:hover {
            color: var(--primary);
        }

        /* Umumiy tugma stili */
        .btn {
            display: inline-block;
            background-color: var(--primary);
            color: var(--bg-main);
            padding: 12px 28px;
            border-radius: 30px;
            text-decoration: none;
            font-weight: 700;
            border: 2px solid transparent;
            cursor: pointer;
            transition: var(--transition);
        }

        .btn:hover {
            background-color: var(--primary-hover);
            transform: translateY(-2px);
        }

        .btn-outline {
            background-color: transparent;
            border: 2px solid var(--primary);
            color: var(--primary);
        }

        .btn-outline:hover {
            background-color: var(--primary);
            color: var(--bg-main);
        }

        /* 1. Hero Section */
        .hero {
            min-height: 100vh;
            display: flex;
            align-items: center;
            padding-top: 140px;
        }

        .hero-content {
            max-width: 700px;
        }

        .hero h2 {
            color: var(--primary);
            font-size: 1.25rem;
            font-weight: 600;
            margin-bottom: 10px;
        }

        .hero h1 {
            font-size: 4rem;
            font-weight: 900;
            line-height: 1.1;
            margin-bottom: 20px;
        }

        .hero p {
            font-size: 1.2rem;
            color: var(--text-muted);
            margin-bottom: 35px;
        }

        /* 2. About Section */
        .about-wrapper {
            display: flex;
            align-items: center;
            gap: 60px;
        }

        .about-img {
            flex-shrink: 0;
            width: 320px;
            height: 320px;
            border-radius: var(--radius);
            background: linear-gradient(135deg, var(--primary), #a855f7);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 5rem;
            box-shadow: 0 20px 25px -5px rgba(0,0,0,0.3);
        }

        .about-text p {
            color: var(--text-muted);
            margin-bottom: 25px;
            font-size: 1.1rem;
        }

        .experience-badge {
            display: inline-flex;
            align-items: center;
            gap: 15px;
            background-color: var(--bg-card);
            padding: 16px 28px;
            border-radius: var(--radius);
            border: 1px solid var(--border);
        }

        .exp-number {
            font-size: 2.5rem;
            font-weight: 800;
            color: var(--primary);
        }

        /* 3. Loyihalar Grid Layout */
        .projects-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 30px;
        }

        .project-card {
            background-color: var(--bg-card);
            border-radius: var(--radius);
            border: 1px solid var(--border);
            overflow: hidden;
            transition: var(--transition);
        }

        .project-card:hover {
            transform: translateY(-6px);
            border-color: var(--primary);
            box-shadow: 0 20px 25px -5px rgba(0,0,0,0.2);
        }

        .project-img-placeholder {
            height: 200px;
            background-color: #1e293b;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 3rem;
            border-bottom: 1px solid var(--border);
        }

        .project-info {
            padding: 24px;
        }

        .project-info h3 {
            font-size: 1.3rem;
            margin-bottom: 12px;
        }

        .project-tags {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin-bottom: 20px;
        }

        .tag {
            background-color: rgba(56, 189, 248, 0.1);
            color: var(--primary);
            padding: 4px 10px;
            border-radius: 50px;
            font-size: 0.8rem;
            font-weight: 600;
        }

        .project-link {
            color: var(--primary);
            text-decoration: none;
            font-weight: 600;
            display: inline-flex;
            align-items: center;
            gap: 5px;
        }

        .project-link:hover {
            text-decoration: underline;
        }

        /* 4. Ko'nikmalar (Skills - Badges & Progress Bars) */
        .skills-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 40px;
        }

        .skill-group h3 {
            margin-bottom: 20px;
            font-size: 1.25rem;
        }

        .skills-badges {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
        }

        .skill-badge {
            background-color: var(--bg-card);
            border: 1px solid var(--border);
            padding: 12px 20px;
            border-radius: var(--radius);
            font-weight: 500;
            transition: var(--transition);
        }

        .skill-badge:hover {
            border-color: var(--primary);
            background-color: rgba(56, 189, 248, 0.05);
        }

        .progress-item {
            margin-bottom: 20px;
        }

        .progress-info {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
            font-weight: 500;
        }

        .progress-bar-bg {
            background-color: var(--border);
            height: 8px;
            border-radius: 10px;
            overflow: hidden;
        }

        .progress-bar-fill {
            background-color: var(--primary);
            height: 100%;
            border-radius: 10px;
        }

        /* 5. Aloqa shakli (Contact Form) */
        .contact-wrapper {
            max-width: 600px;
            margin: 0 auto;
            background-color: var(--bg-card);
            padding: 40px;
            border-radius: var(--radius);
            border: 1px solid var(--border);
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 500;
            font-size: 0.95rem;
        }

        .form-group input,
        .form-group textarea {
            width: 100%;
            background-color: var(--bg-main);
            border: 1px solid var(--border);
            padding: 14px;
            border-radius: 8px;
            color: var(--text-main);
            font-family: inherit;
            outline: none;
            transition: var(--transition);
        }

        .form-group input:focus,
        .form-group textarea:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(56, 189, 248, 0.15);
        }

        .contact-wrapper .btn {
            width: 100%;
        }

        /* Footer */
        footer {
            padding: 40px 0;
            text-align: center;
            color: var(--text-muted);
            font-size: 0.9rem;
        }

        /* Responsive Layout */
        @media (max-width: 992px) {
            .about-wrapper {
                flex-direction: column;
                text-align: center;
            }
            .skills-container {
                grid-template-columns: 1fr;
            }
        }

        @media (max-width: 768px) {
            .hero h1 {
                font-size: 2.75rem;
            }
            .nav-links {
                display: none; /* Mobil qurilmalarda menyuni soddalashtirish uchun */
            }
        }
    </style>
</head>
<body>

    <!-- Navigatsiya paneli -->
    <nav>
        <div class="container nav-container">
            <a href="#" class="logo">Dev<span>Portfolio</span></a>
            <ul class="nav-links">
                <li><a href="#hero">Asosiy</a></li>
                <li><a href="#about">Men haqimda</a></li>
                <li><a href="#projects">Loyihalar</a></li>
                <li><a href="#skills">Ko'nikmalar</a></li>
                <li><a href="#contact">Aloqa</a></li>
            </ul>
        </div>
    </nav>

    <!-- 1. Hero Section -->
    <section class="hero" id="hero">
        <div class="container">
            <div class="hero-content">
                <h2>Salom, mening ismim</h2>
                <h1>Asilbek Kamolov</h1>
                <p>Men Full-Stack veb-dasturchiman. Zamonaviy, tezkor va foydalanuvchilar uchun qulay bo'lgan veb-ilovalarni noldan boshlab loyihalashtiraman va yarataman.</p>
                <a href="#" class="btn" download>CV Yuklab Olish</a>
            </div>
        </div>
    </section>

    <!-- 2. About Section -->
    <section id="about">
        <div class="container">
            <h2 class="section-title">Men <span>Haqimda</span></h2>
            <div class="about-wrapper">
                <div class="about-img">👨‍💻</div>
                <div class="about-text">
                    <p>Men raqamli dunyoda chiroyli va mukammal kod yozishga ishtiyoqmand dasturchiman. Oxirgi yillarda ko'plab startap loyihalar va korporativ veb-saytlarni muvaffaqiyatli ishlab chiqishda qatnashdim. Har doim yangi texnologiyalarni o'rganishga va bilimlarimni amalda sinashga tayyorman.</p>
                    <div class="experience-badge">
                        <span class="exp-number">4+</span>
                        <div>
                            <h4 style="font-weight: 700;">Yillik Tajriba</h4>
                            <p style="font-size: 0.85rem; color: var(--text-muted);">Dasturlash sohasida</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- 3. Loyihalar Grid Section -->
    <section id="projects">
        <div class="container">
            <h2 class="section-title">Mening <span>Loyihalarim</span></h2>
            <div class="projects-grid">
                <!-- Loyer 1 -->
                <div class="project-card">
                    <div class="project-img-placeholder">🛒</div>
                    <div class="project-info">
                        <h3>E-Commerce Platforma</h3>
                        <div class="project-tags">
                            <span class="tag">React</span>
                            <span class="tag">Node.js</span>
                            <span class="tag">MongoDB</span>
                        </div>
                        <a href="#" class="project-link">Loyihani ko'rish →</a>
                    </div>
                </div>
                <!-- Loyer 2 -->
                <div class="project-card">
                    <div class="project-img-placeholder">📊</div>
                    <div class="project-info">
                        <h3>SaaS Dashboard</h3>
                        <div class="project-tags">
                            <span class="tag">Vue.js</span>
                            <span class="tag">Tailwind CSS</span>
                            <span class="tag">Firebase</span>
                        </div>
                        <a href="#" class="project-link">Loyihani ko'rish →</a>
                    </div>
                </div>
                <!-- Loyer 3 -->
                <div class="project-card">
                    <div class="project-img-placeholder">💬</div>
                    <div class="project-info">
                        <h3>Chat Ilovasi</h3>
                        <div class="project-tags">
                            <span class="tag">Socket.io</span>
                            <span class="tag">Express</span>
                            <span class="tag">JavaScript</span>
                        </div>
                        <a href="#" class="project-link">Loyihani ko'rish →</a>
                    </div>
                </div>
                <!-- Loyer 4 -->
                <div class="project-card">
                    <div class="project-img-placeholder">📝</div>
                    <div class="project-info">
                        <h3>Task Manager App</h3>
                        <div class="project-tags">
                            <span class="tag">Next.js</span>
                            <span class="tag">TypeScript</span>
                            <span class="tag">PostgreSQL</span>
                        </div>
                        <a href="#" class="project-link">Loyihani ko'rish →</a>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- 4. Ko'nikmalar Section -->
    <section id="skills">
        <div class="container">
            <h2 class="section-title">Mening <span>Ko'nikmalarim</span></h2>
            <div class="skills-container">
                <!-- Progress bars -->
                <div class="skill-group">
                    <h3>Asosiy yo'nalishlar</h3>
                    <div class="progress-item">
                        <div class="progress-info"><span>Frontend (React / Vue)</span><span>90%</span></div>
                        <div class="progress-bar-bg"><div class="progress-bar-fill" style="width: 90%;"></div></div>
                    </div>
                    <div class="progress-item">
                        <div class="progress-info"><span>Backend (Node.js / Python)</span><span>80%</span></div>
                        <div class="progress-bar-bg"><div class="progress-bar-fill" style="width: 80%;"></div></div>
                    </div>
                    <div class="progress-item">
                        <div class="progress-info"><span>Ma'lumotlar bazasi (SQL / NoSQL)</span><span>75%</span></div>
                        <div class="progress-bar-bg"><div class="progress-bar-fill" style="width: 75%;"></div></div>
                    </div>
                </div>
                <!-- Badges -->
                <div class="skill-group">
                    <h3>Texnologiyalar & Asboblar</h3>
                    <div class="skills-badges">
                        <span class="skill-badge">HTML5 / CSS3</span>
                        <span class="skill-badge">JavaScript (ES6+)</span>
                        <span class="skill-badge">TypeScript</span>
                        <span class="skill-badge">Tailwind CSS</span>
                        <span class="skill-badge">Git & GitHub</span>
                        <span class="skill-badge">Docker</span>
                        <span class="skill-badge">REST API</span>
                        <span class="skill-badge">GraphQL</span>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- 5. Aloqa shakli Section -->
    <section id="contact">
        <div class="container">
            <h2 class="section-title">Men bilan <span>Bog'lanish</span></h2>
            <div class="contact-wrapper">
                <form action="#" onsubmit="event.preventDefault(); alert('Xabaringiz yuborildi! Rahmat.');">
                    <div class="form-group">
                        <label for="name">Ismingiz</label>
                        <input type="text" id="name" required placeholder="Ismingizni kiriting">
                    </div>
                    <div class="form-group">
                        <label for="email">Email manzilingiz</label>
                        <input type="email" id="email" required placeholder="example@mail.com">
                    </div>
                    <div class="form-group">
                        <label for="message">Xabaringiz</label>
                        <textarea id="message" rows="5" required placeholder="Xabaringizni bu yerga yozing..."></textarea>
                    </div>
                    <button type="submit" class="btn">Xabarni Yuborish</button>
                </form>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer>
        <div class="container">
            <p>&copy; 2026 DevPortfolio. Barcha huquqlar himoyalangan.</p>
        </div>
    </footer>

</body>
</html>
