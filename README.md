<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>To'g'ri Semantika Masalaning Mohiyati</title>
</head>
<body>

    <!-- 1 va 5-qoida: Header, Nav va ro'yxat kombinatsiyasi -->
    <header>
        <h1>Dasturlash Akademiyasi</h1>
        <nav>
            <ul>
                <li><a href="#home">Bosh sahifa</a></li>
                <li><a href="#courses">Kurslar</a></li>
                <li><a href="#blog">Blog</a></li>
            </ul>
        </nav>
    </header>

    <!-- 1-qoida: Asosiy kontent bloki -->
    <main>
        
        <!-- 3-qoida: Mavzuni guruhlash uchun Section -->
        <section id="blog">
            <!-- 2-qoida: Ierarxiya bo'yicha h2 kelmoqda -->
            <h2>So'nggi maqolalar</h2>

            <!-- 3-qoida: Mustaqil maqola uchun Article -->
            <article>
                <h3>HTML5 Semantikasi nima?</h3> <!-- h2 ichidagi kichik sarlavha h3 -->
                <p>Semantik teglari brauzer va dasturchiga elementning vazifasini aniq aytib turadi...</p>
                
                <!-- 4-qoida: Rasm va uning izohi -->
                <figure>
                    <img src="html5-structure.jpg" alt="HTML5 semantik tuzilishi sxemasi">
                    <figcaption>1-rasm. HTML5 sahifasining asosiy teglari taqsimoti.</figcaption>
                </figure>
            </article>

            <article>
                <h3>CSS Grid haqida asoslar</h3>
                <p>Grid yordamida murakkab maketlarni osongina chizish mumkin...</p>
            </article>
        </section>

    </main>

    <!-- 1-qoida: Sayt oxiri -->
    <footer>
        <p>&copy; 2026 Dasturlash Akademiyasi. Barcha huquqlar himoyalangan.</p>
    </footer>

</body>
</html>
