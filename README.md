// ===================================================
// AbortController - Amaliy misollar
// ===================================================

// 1. Asosiy fetch bekor qilish
async function fetchWithCancel(url) {
  const controller = new AbortController();
  const { signal } = controller;

  // 5 sekunddan keyin avtomatik bekor qilish
  const timeoutId = setTimeout(() => controller.abort(), 5000);

  try {
    const response = await fetch(url, { signal });
    clearTimeout(timeoutId); // Muvaffaqiyatli bo'lsa timeout ni tozalash
    return await response.json();
  } catch (error) {
    if (error.name === 'AbortError') {
      console.log('So\'rov bekor qilindi (timeout yoki manuel)');
      return null;
    }
    throw error; // Boshqa xatoliklarni yuqoriga uzatish
  }
}

// 2. AbortSignal.timeout — eng qulay timeout usuli
async function fetchWithTimeout(url, timeoutMs = 5000) {
  try {
    const response = await fetch(url, {
      signal: AbortSignal.timeout(timeoutMs)
    });
    return await response.json();
  } catch (error) {
    if (error.name === 'TimeoutError') {
      throw new Error(`So\'rov ${timeoutMs}ms dan oshdi`);
    }
    throw error;
  }
}

// 3. Izlash (debounce + AbortController)
let searchController = null;

async function searchProducts(query) {
  // Avvalgi so'rovni bekor qilish
  if (searchController) {
    searchController.abort();
  }

  searchController = new AbortController();

  try {
    const res = await fetch(`/api/search?q=${query}`, {
      signal: searchController.signal
    });
    const data = await res.json();
    console.log('Natijalar:', data);
    return data;
  } catch (err) {
    if (err.name !== 'AbortError') throw err;
    // AbortError — bu normal holat, e'tibor bermaslik kerak
  }
}

// 4. React komponenti uchun pattern (simulation)
function ReactLikeComponent() {
  let isMounted = false;
  let controller = null;

  function mount() {
    isMounted = true;
    controller = new AbortController();
    loadData();
  }

  async function loadData() {
    try {
      const data = await fetch('/api/data', {
        signal: controller.signal
      });
      if (!isMounted) return; // Qo'shimcha himoya
      console.log('Ma\'lumot yuklandi:', await data.json());
    } catch (err) {
      if (err.name === 'AbortError') return;
      console.error('Xatolik:', err);
    }
  }

  function unmount() {
    isMounted = false;
    controller.abort(); // Komponent o'chirilganda so'rovni bekor qilish
  }

  return { mount, unmount };
}

// 5. Event listener AbortSignal bilan
function setupEventListeners(element) {
  const controller = new AbortController();
  const { signal } = controller;

  // Barcha listenerlar bir signal bilan boshqariladi
  element.addEventListener('click', handleClick, { signal });
  element.addEventListener('keydown', handleKey, { signal });
  document.addEventListener('scroll', handleScroll, { signal });

  // Barcha listenerlarni bir yoki'da o'chirish!
  return () => controller.abort();
}

function handleClick(e) { console.log('Bosildi:', e.target); }
function handleKey(e) { console.log('Tugma:', e.key); }
function handleScroll() { console.log('Scroll:', window.scrollY); }
