// ===================================================
// Review 2 — AbortController, Workers, Observers birgalikda
// ===================================================

// Barcha texnologiyalarni birlashtirgan kontent yuklovchi
class SmartContentLoader {
  constructor(containerSelector) {
    this.container = document.querySelector(containerSelector);
    this.controllers = new Map(); // AbortController'lar
    this.worker = null;
    this.observers = new Map(); // Observer'lar
  }

  // Web Worker yaratish
  initWorker() {
    const workerCode = `
      self.onmessage = function(e) {
        const { id, data } = e.data;
        // Og'ir qayta ishlash (filtrlash, sorting, transform)
        const processed = data
          .filter(item => item.active)
          .map(item => ({ ...item, label: item.name.toUpperCase() }))
          .sort((a, b) => b.score - a.score);
        self.postMessage({ id, processed });
      };
    `;
    const blob = new Blob([workerCode], { type: 'text/javascript' });
    this.worker = new Worker(URL.createObjectURL(blob));
  }

  // Element ko'rinavchi bo'lganda lazy loading
  setupLazyLoad(element, itemId) {
    const io = new IntersectionObserver(async ([entry]) => {
      if (!entry.isIntersecting) return;
      io.disconnect();

      // AbortController bilan fetch
      const controller = new AbortController();
      this.controllers.set(itemId, controller);

      try {
        const res = await fetch(`/api/items/${itemId}`, {
          signal: controller.signal
        });
        const rawData = await res.json();

        // Web Worker da qayta ishlash
        const processed = await this.processInWorker(rawData);
        this.renderItems(element, processed);

      } catch (err) {
        if (err.name !== 'AbortError') console.error('Xatolik:', err);
      } finally {
        this.controllers.delete(itemId);
      }
    }, { rootMargin: '100px', threshold: 0 });

    io.observe(element);
    this.observers.set(itemId, io);
  }

  // Web Worker da qayta ishlash
  processInWorker(data) {
    return new Promise((resolve, reject) => {
      const id = Date.now();
      const handler = (e) => {
        if (e.data.id === id) {
          this.worker.removeEventListener('message', handler);
          resolve(e.data.processed);
        }
      };
      this.worker.addEventListener('message', handler);
      this.worker.postMessage({ id, data });
    });
  }

  renderItems(el, items) {
    el.innerHTML = items.map(i => `<div>${i.label}: ${i.score}</div>`).join('');
  }

  // Barcha resurslarni tozalash
  destroy() {
    // Barcha fetch so'rovlarini bekor qilish
    this.controllers.forEach(ctrl => ctrl.abort());
    // Barcha observer'larni to'xtatish
    this.observers.forEach(obs => obs.disconnect());
    // Worker ni to'xtatish
    if (this.worker) this.worker.terminate();
  }
}

// Ishlatish
const loader = new SmartContentLoader('#app');
loader.initWorker();

// Har bir element uchun lazy loading
document.querySelectorAll('[data-item-id]').forEach(el => {
  loader.setupLazyLoad(el, el.dataset.itemId);
});

// Sahifa o'chirilganda tozalash
window.addEventListener('unload', () => loader.destroy());
