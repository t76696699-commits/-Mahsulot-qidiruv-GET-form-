// ===================================================
// Observers - Amaliy misollar
// ===================================================

// 1. IntersectionObserver — Lazy Loading
function setupLazyLoading() {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          const img = entry.target;
          // data-src atributidan haqiqiy src ni o'rnatish
          img.src = img.dataset.src;
          img.removeAttribute('data-src');
          observer.unobserve(img); // Endi kuzatish shart emas
          console.log('Rasm yuklandi:', img.src);
        }
      });
    },
    {
      root: null,        // viewport ishlatiladi
      rootMargin: '50px', // 50px oldin yuklashni boshlash
      threshold: 0.1     // 10% ko'ringanda trigger
    }
  );

  // Barcha lazy rasmlarni kuzatishga qo'shish
  document.querySelectorAll('img[data-src]').forEach(img => {
    observer.observe(img);
  });

  return observer;
}

// 2. IntersectionObserver — Infinite Scroll
function setupInfiniteScroll(container, loadMore) {
  // Sentinel element — ro'yxat oxiriga qo'yiladi
  const sentinel = document.createElement('div');
  container.appendChild(sentinel);

  const observer = new IntersectionObserver(async (entries) => {
    if (entries[0].isIntersecting) {
      observer.unobserve(sentinel); // Yuklanayotganda to'xtatish
      await loadMore();
      observer.observe(sentinel);   // Tugagach davom etish
    }
  }, { threshold: 1.0 });

  observer.observe(sentinel);
  return () => observer.disconnect();
}

// 3. ResizeObserver — Responsive Canvas
function setupResponsiveCanvas(canvas) {
  const ctx = canvas.getContext('2d');

  const resizeObserver = new ResizeObserver((entries) => {
    for (const entry of entries) {
      const { width, height } = entry.contentRect;
      canvas.width = width;
      canvas.height = height;
      // Canvas qayta chizish
      drawContent(ctx, width, height);
      console.log(`Canvas: ${width}x${height}`);
    }
  });

  resizeObserver.observe(canvas.parentElement);
  return () => resizeObserver.disconnect();
}

function drawContent(ctx, w, h) {
  ctx.clearRect(0, 0, w, h);
  ctx.fillStyle = '#4A90D9';
  ctx.fillRect(w/4, h/4, w/2, h/2);
}

// 4. MutationObserver — DOM o'zgarishlarini kuzatish
function watchDOMChanges(targetElement) {
  const observer = new MutationObserver((mutations) => {
    mutations.forEach(mutation => {
      if (mutation.type === 'childList') {
        mutation.addedNodes.forEach(node => {
          console.log('Yangi element qo\'shildi:', node.nodeName);
        });
        mutation.removedNodes.forEach(node => {
          console.log('Element o\'chirildi:', node.nodeName);
        });
      }

      if (mutation.type === 'attributes') {
        console.log(`Atribut o'zgardi: ${mutation.attributeName}`);
        console.log('  Eski:', mutation.oldValue);
      }
    });
  });

  observer.observe(targetElement, {
    childList: true,       // Bolalar o'zgarishini kuzatish
    attributes: true,      // Atributlarni kuzatish
    attributeOldValue: true, // Eski qiymatni saqlash
    subtree: true          // Barcha ichki elementlarni ham kuzatish
  });

  return () => observer.disconnect();
}

// 5. Ko'rish animatsiyasi uchun IntersectionObserver
function animateOnVisible(elements, animationClass = 'fade-in') {
  const io = new IntersectionObserver((entries) => {
    entries.forEach(({ target, isIntersecting }) => {
      target.classList.toggle(animationClass, isIntersecting);
    });
  }, { threshold: 0.2 });

  elements.forEach(el => io.observe(el));
  return () => io.disconnect();
}
