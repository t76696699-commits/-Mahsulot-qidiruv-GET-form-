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
