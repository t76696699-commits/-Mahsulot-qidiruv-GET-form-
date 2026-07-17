1. Yangilik Class (Model)
Ma'lumotlar tuzilmasini standartlashtirish uchun:

JavaScript
class Yangilik {
  constructor(id, title, content) {
    this.id = id;
    this.title = title;
    this.content = this.cleanHTML(content);
  }

  // Regex bilan tozalash
  cleanHTML(str) {
    return str
      .replace(/<[^>]*>/g, '') // HTML teglarini olib tashlash
      .replace(/\s+/g, ' ')    // Keraksiz bo'shliqlarni tozalash
      .trim();
  }
}
2. Async Pipeline
Promise.allSettled yordamida bir nechta manbadan ma'lumot olish va localStorage bilan ishlash:

JavaScript
async function fetchPipeline(urls) {
  const container = document.getElementById('news-container');
  container.innerHTML = '<p>Yuklanmoqda...</p>'; // Loading state

  try {
    const results = await Promise.allSettled(urls.map(url => fetch(url).then(r => r.json())));
    
    const validData = results
      .filter(res => res.status === 'fulfilled')
      .flatMap(res => res.value.map(item => new Yangilik(item.id, item.title, item.body)));

    localStorage.setItem('news-v1', JSON.stringify(validData));
    renderNews(validData);
  } catch (error) {
    console.error("Xatolik yuz berdi, keshdan o'qilmoqda...", error);
    const cached = JSON.parse(localStorage.getItem('news-v1') || '[]');
    renderNews(cached);
  }
}
3. DOM Render
Ma'lumotlarni brauzer sahifasida ko‘rsatish:

JavaScript
function renderNews(newsList) {
  const container = document.getElementById('news-container');
  container.innerHTML = newsList.map(news => `
    <article>
      <h3>${news.title}</h3>
      <p>${news.content}</p>
    </article>
  `).join('');
}
