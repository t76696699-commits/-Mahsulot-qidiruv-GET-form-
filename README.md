IndexedDB nima?
IndexedDB — brauzerda to'liq NoSQL ma'lumotlar bazasini saqlash uchun mo'ljallangan kuchli client-side storage mexanizmi. U localStoragedan farqli o'laroq, katta hajmli strukturaviy ma'lumotlarni (obyektlar, fayllar, blob'lar) saqlashga qodir va asinxron ishlaydi.

IndexedDB va localStorage farqi
Xususiyat	localStorage	IndexedDB
Hajm chegarasi	~5-10MB	Yuzlab MB (brauzerga bog'liq)
Ma'lumot turi	Faqat string	Har qanday obyekt, fayl, blob
Ishlash usuli	Sinxron	Asinxron (bloklamaydi)
Qidirish	Yo'q	Index orqali tez qidirish
Asosiy tushunchalar
Database — ma'lumotlar bazasining o'zi, nom va versiyaga ega
Object Store — jadval kabi, obyektlarni saqlaydi
Index — ma'lum maydon bo'yicha tez qidirish uchun
Transaction — barcha o'qish/yozish amallari tranzaksiya ichida bajariladi
Ma'lumotlar bazasini ochish
indexedDB.open(name, version) chaqirilganda onupgradeneeded hodisasi orqali object store'lar yaratiladi (faqat versiya o'zgarganda ishga tushadi).

IndexedDB ish jarayoni
Yo'q yoki versiya yangi

Ha

indexedDB.open name, version

Baza mavjudmi?

onupgradeneeded: Object Store yaratiladi

onsuccess: baza ochiladi

transaction yaratiladi

objectStore orqali add/get/put/delete

onsuccess yoki onerror natijasi

IndexedDB — offlayn ishlaydigan ilovalar (masalan, PWA), katta hajmli keshlash va murakkab client-side ma'lumotlarni saqlash uchun ideal yechim hisoblanadi.

💻
Код
Kod namunasi
#2
code
 Копировать
// indexeddb-notes.js — IndexedDB bilan eslatmalar ilovasi
const DB_NAME = 'NotesApp';
const DB_VERSION = 1;
const STORE_NAME = 'notes';

function openDatabase() {
  return new Promise((resolve, reject) => {
    const request = indexedDB.open(DB_NAME, DB_VERSION);

    request.onupgradeneeded = (event) => {
      const db = event.target.result;
      if (!db.objectStoreNames.contains(STORE_NAME)) {
        const store = db.createObjectStore(STORE_NAME, {
          keyPath: 'id',
          autoIncrement: true,
        });
        store.createIndex('by_title', 'title', { unique: false });
        store.createIndex('by_date', 'createdAt', { unique: false });
      }
    };

    request.onsuccess = (event) => resolve(event.target.result);
    request.onerror = (event) => reject(event.target.error);
  });
}

async function addNote(db, title, content) {
  return new Promise((resolve, reject) => {
    const tx = db.transaction(STORE_NAME, 'readwrite');
    const store = tx.objectStore(STORE_NAME);

    const note = { title, content, createdAt: new Date().toISOString() };
    const request = store.add(note);

    request.onsuccess = () => resolve(request.result);
    request.onerror = () => reject(request.error);
  });
}

async function getAllNotes(db) {
  return new Promise((resolve, reject) => {
    const tx = db.transaction(STORE_NAME, 'readonly');
    const store = tx.objectStore(STORE_NAME);
    const request = store.getAll();

    request.onsuccess = () => resolve(request.result);
    request.onerror = () => reject(request.error);
  });
}

async function deleteNote(db, id) {
  return new Promise((resolve, reject) => {
    const tx = db.transaction(STORE_NAME, 'readwrite');
    const store = tx.objectStore(STORE_NAME);
    const request = store.delete(id);

    request.onsuccess = () => resolve();
    request.onerror = () => reject(request.error);
  });
}

async function searchByTitle(db, title) {
  return new Promise((resolve, reject) => {
    const tx = db.transaction(STORE_NAME, 'readonly');
    const index = tx.objectStore(STORE_NAME).index('by_title');
    const request = index.getAll(title);

    request.onsuccess = () => resolve(request.result);
    request.onerror = () => reject(request.error);
  });
}

// Foydalanish
async function main() {
  const db = await openDatabase();

  const id = await addNote(db, 'Birinchi eslatma', "IndexedDB o'rganish");
  console.log('Qo\'shildi, ID:', id);

  const allNotes = await getAllNotes(db);
  console.log('Barcha eslatmalar:', allNotes);

  await deleteNote(db, id);
  console.log('Eslatma o\'chirildi');
}

main();
