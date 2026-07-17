// sw.js — Service Worker fayli (root papkada joylashadi)
const CACHE_NAME = 'my-pwa-cache-v1';
const URLS_TO_CACHE = [
  '/',
  '/index.html',
  '/styles.css',
  '/app.js',
  '/offline.html',
];

// 1. INSTALL — kerakli fayllarni keshlash
self.addEventListener('install', (event) => {
  console.log('Service Worker: install');
  event.waitUntil(
    caches.open(CACHE_NAME).then((cache) => {
      return cache.addAll(URLS_TO_CACHE);
    })
  );
  self.skipWaiting();
});

// 2. ACTIVATE — eski keshlarni tozalash
self.addEventListener('activate', (event) => {
  console.log('Service Worker: activate');
  event.waitUntil(
    caches.keys().then((cacheNames) => {
      return Promise.all(
        cacheNames
          .filter((name) => name !== CACHE_NAME)
          .map((name) => caches.delete(name))
      );
    })
  );
  self.clients.claim();
});

// 3. FETCH — Cache First strategiyasi
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((cachedResponse) => {
      if (cachedResponse) {
        return cachedResponse;
      }

      return fetch(event.request)
        .then((networkResponse) => {
          return caches.open(CACHE_NAME).then((cache) => {
            cache.put(event.request, networkResponse.clone());
            return networkResponse;
          });
        })
        .catch(() => {
          // Internet yo'q va keshda ham topilmasa — offlayn sahifa
          return caches.match('/offline.html');
        });
    })
  );
});

// app.js — Service Worker'ni ro'yxatdan o'tkazish (asosiy sahifada)
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker
      .register('/sw.js')
      .then((registration) => {
        console.log('SW ro\'yxatdan o\'tdi:', registration.scope);
      })
      .catch((error) => {
        console.error('SW ro\'yxatdan o\'tishda xato:', error);
      });
  });
}

// manifest.json — PWA uchun manifest fayli namunasi
// {
//   "name": "Mening PWA Ilovam",
//   "short_name": "MyPWA",
//   "start_url": "/",
//   "display": "standalone",
//   "background_color": "#ffffff",
//   "theme_color": "#8b5cf6",
//   "icons": [
//     { "src": "/icon-192.png", "sizes": "192x192", "type": "image/png" },
//     { "src": "/icon-512.png", "sizes": "512x512", "type": "image/png" }
//   ]
// }
