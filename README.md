1. HTML (index.html)
Interfeys uchun asosiy elementlar:

HTML
<div class="container">
    <h2>Film Katalogi</h2>
    <!-- Qo'shish formasi -->
    <div class="form-group">
        <input type="text" id="name" placeholder="Film nomi">
        <input type="text" id="genre" placeholder="Janri">
        <input type="number" id="year" placeholder="Yili">
        <input type="number" id="rating" placeholder="Reyting (1-10)" step="0.1">
        <button onclick="addMovie()">Qo'shish</button>
    </div>

    <!-- Qidiruv va Filtr -->
    <div class="controls">
        <input type="text" id="searchInput" oninput="renderMovies()" placeholder="Nom bo'yicha qidirish...">
        <select id="filterGenre" onchange="renderMovies()">
            <option value="all">Barcha janrlar</option>
        </select>
    </div>

    <!-- Kartochkalar chiqadigan joy -->
    <div id="movieList" class="movie-grid"></div>
</div>
2. JavaScript (script.js)
Asosiy logika, localStorage va DOM manipulyatsiyasi:

JavaScript
let movies = JSON.parse(localStorage.getItem('movies')) || [];

function saveAndRender() {
    localStorage.setItem('movies', JSON.stringify(movies));
    updateGenreFilter();
    renderMovies();
}

function addMovie() {
    const name = document.getElementById('name').value;
    const genre = document.getElementById('genre').value;
    const year = document.getElementById('year').value;
    const rating = document.getElementById('rating').value;

    if (!name || !genre) return alert("Barcha maydonlarni to'ldiring!");

    movies.push({ id: Date.now(), name, genre, year, rating });
    saveAndRender();
}

function deleteMovie(id) {
    movies = movies.filter(m => m.id !== id);
    saveAndRender();
}

function renderMovies() {
    const searchTerm = document.getElementById('searchInput').value.toLowerCase();
    const filterGenre = document.getElementById('filterGenre').value;
    const list = document.getElementById('movieList');
    
    list.innerHTML = '';

    const filtered = movies.filter(m => {
        const matchesName = m.name.toLowerCase().includes(searchTerm);
        const matchesGenre = filterGenre === 'all' || m.genre === filterGenre;
        return matchesName && matchesGenre;
    });

    filtered.forEach(m => {
        list.innerHTML += `
            <div class="card">
                <h3>${m.name}</h3>
                <p>Janr: ${m.genre}</p>
                <p>Yil: ${m.year} | Reyting: ${m.rating}</p>
                <button onclick="deleteMovie(${m.id})">O'chirish</button>
            </div>
        `;
    });
}

function updateGenreFilter() {
    const select = document.getElementById('filterGenre');
    const genres = [...new Set(movies.map(m => m.genre))];
    select.innerHTML = '<option value="all">Barcha janrlar</option>';
    genres.forEach(g => select.innerHTML += `<option value="${g}">${g}</option>`);
}

// Boshlang'ich holat
saveAndRender();
3. CSS (style.css)
Kartochkalar ko'rinishi uchun:

CSS
.movie-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 15px;
    margin-top: 20px;
}
.card {
    border: 1px solid #ccc;
    padding: 15px;
    border-radius: 8px;
    background: #f9f9f9;
}
Muhim eslatmalar:
LocalStorage: JSON.stringify yordamida obyektlar massivini matn ko'rinishida saqlaymiz, o'qiyotganda esa JSON.parse orqali qayta obyektga aylantiramiz.

Qidiruv: oninput hodisasi orqali har bir harf kiritilganda renderMovies() funksiyasini chaqirib, natijalarni real vaqtda yangilab boramiz.

Filtrlash: filter() metodi orqali faqat tanlangan janrga mos keladigan filmlarni ajratib olamiz.
