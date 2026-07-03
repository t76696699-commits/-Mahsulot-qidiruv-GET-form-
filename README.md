HTML (index.html)
HTML
<nav class="navbar">
    <div class="logo">Logo</div>
    <ul class="nav-links">
        <li><a href="#">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Services</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
    <button class="cta-btn">Sign Up</button>
</nav>

<section class="hero">
    <div class="hero-col">Ustun 1</div>
    <div class="hero-col">Ustun 2</div>
    <div class="hero-col">Ustun 3</div>
</section>
CSS (style.css)
CSS
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

/* Navigatsiya */
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px 5%;
    background-color: #333;
    color: white;
}

.nav-links {
    display: flex;
    gap: 30px;
    list-style: none;
}

.nav-links a {
    color: white;
    text-decoration: none;
    transition: color 0.3s ease;
}

.nav-links a:hover {
    color: #ffcc00; /* Hover effekti */
}

.cta-btn {
    padding: 10px 20px;
    background-color: #ffcc00;
    border: none;
    cursor: pointer;
}

/* Hero qismi */
.hero {
    display: flex;
    gap: 20px; /* Margin o'rniga gap */
    padding: 50px 5%;
}

.hero-col {
    flex: 1; /* 3 ta teng ustun */
    padding: 40px;
    background-color: #f4f4f4;
    border: 1px solid #ddd;
    text-align: center;
}

/* Responsive dizayn */
@media (max-width: 768px) {
    .navbar {
        flex-direction: column;
        gap: 15px;
    }

    .hero {
        flex-direction: column; /* Ustunlarni qatorga aylantirish */
    }
}
Muhim jihatlar:
Flexbox: .navbar va .hero qismlarida flex dan foydalanib, elementlar tekis joylashtirildi.

Gap: margin o'rniga gap xususiyati ishlatildi, bu kodni ancha toza va boshqarishni osonlashtiradi.

Media Query: @media (max-width: 768px) yordamida ekran kichrayganda (masalan, planshet yoki telefonda) elementlar ustun (column) ko'rinishiga o'tadi.

Hover: Navigatsiya linklariga transition va hover qo'shildi, bu foydalanuvchi tajribasini yaxshilaydi.
