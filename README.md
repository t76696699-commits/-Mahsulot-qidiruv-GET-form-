// ===================================================
// Web Workers - Amaliy misollar
// ===================================================

// === worker.js (alohida fayl) ===
// self — Worker ichidagi global ob'ekt
self.onmessage = function(event) {
  const { type, data } = event.data;

  if (type === 'COMPUTE_PRIMES') {
    const primes = findPrimes(data.limit);
    self.postMessage({ type: 'PRIMES_RESULT', primes });
  }

  if (type === 'SORT_ARRAY') {
    const sorted = [...data.array].sort((a, b) => a - b);
    self.postMessage({ type: 'SORTED', sorted });
  }
};

function findPrimes(limit) {
  // Sieve of Eratosthenes algoritmi
  const sieve = new Uint8Array(limit + 1).fill(1);
  sieve[0] = sieve[1] = 0;
  for (let i = 2; i * i <= limit; i++) {
    if (sieve[i]) {
      for (let j = i * i; j <= limit; j += i) sieve[j] = 0;
    }
  }
  return sieve.reduce((acc, val, idx) => val ? [...acc, idx] : acc, []);
}

// === main.js (asosiy fayl) ===
class WorkerManager {
  constructor(workerPath) {
    this.worker = new Worker(workerPath);
    this.pending = new Map(); // ID -> { resolve, reject }
    this.nextId = 0;

    this.worker.onmessage = (e) => {
      const { id, result, error } = e.data;
      const callbacks = this.pending.get(id);
      if (!callbacks) return;
      this.pending.delete(id);
      error ? callbacks.reject(new Error(error)) : callbacks.resolve(result);
    };

    this.worker.onerror = (err) => {
      console.error('Worker xatoligi:', err);
    };
  }

  // Promise asosida worker ga vazifa berish
  run(task, data) {
    return new Promise((resolve, reject) => {
      const id = this.nextId++;
      this.pending.set(id, { resolve, reject });
      this.worker.postMessage({ id, task, data });
    });
  }

  terminate() {
    this.worker.terminate();
  }
}

// Inline Worker — alohida fayl shart emas
function createInlineWorker(fn) {
  const blob = new Blob([`(${fn.toString()})()`], { type: 'text/javascript' });
  return new Worker(URL.createObjectURL(blob));
}

const inlineWorker = createInlineWorker(function() {
  self.onmessage = (e) => {
    // Hisob-kitob va natijani qaytarish
    const result = e.data.numbers.reduce((a, b) => a + b, 0);
    self.postMessage({ sum: result });
  };
});

inlineWorker.postMessage({ numbers: [1, 2, 3, 4, 5] });
inlineWorker.onmessage = (e) => {
  console.log('Yig\'indi:', e.data.sum); // 15
  inlineWorker.terminate();
};
