Geolocation API nima?
Geolocation API — foydalanuvchining joriy geografik joylashuvini (kenglik va uzunlik) olish imkonini beruvchi brauzer interfeysi. U foydalanuvchidan ruxsat so'raydi va faqat xavfsiz (HTTPS) kontekstda ishlaydi.

Asosiy metodlar
Metod	Vazifasi
getCurrentPosition()	Joriy joylashuvni bir marta oladi
watchPosition()	Joylashuv o'zgarishini uzluksiz kuzatadi
clearWatch()	watchPosition kuzatuvini to'xtatadi
Media API (getUserMedia)
Media API (aniqrog'i, navigator.mediaDevices.getUserMedia()) — foydalanuvchining kamera va mikrofoniga kirish imkonini beradi. Bu video chat, QR-kod skanerlash, ovoz yozish kabi ilovalarda ishlatiladi.

getUserMedia constraints
Qaysi qurilmalar kerakligini { video: true, audio: true } kabi obyekt orqali belgilash mumkin. Video uchun kamera tomonini (old/orqa), o'lchamini ham sozlash mumkin.

Ruxsat va xavfsizlik
Ikkala API ham foydalanuvchi ruxsatisiz ishlamaydi — brauzer avtomatik ravishda ruxsat so'rash oynasini ko'rsatadi. Agar foydalanuvchi rad etsa, error callback chaqiriladi.

Geolocation va Media ishlash jarayoni
Ruxsat berdi

Rad etdi

getCurrentPosition yoki getUserMedia chaqiriladi

Brauzer ruxsat so'raydi

Foydalanuvchi javobi

success callback: ma'lumot qaytadi

error callback: xato qaytadi

Joylashuv yoki media stream ishlatiladi

Bu ikkala API — xarita ilovalari, yaqin atrofdagi joylarni topish, video chat va real vaqtli kamera funksiyalari uchun zamonaviy veb ilovalarning muhim qismidir.

💻
Код
Kod namunasi
#2
code
 Копировать
// location-media.js — Geolocation va Media API birgalikda
// 1. Joriy joylashuvni bir marta olish
function getCurrentLocation() {
  if (!navigator.geolocation) {
    console.error("Geolocation qo'llab-quvvatlanmaydi");
    return;
  }

  navigator.geolocation.getCurrentPosition(
    (position) => {
      const { latitude, longitude, accuracy } = position.coords;
      console.log(`Joylashuv: ${latitude}, ${longitude} (aniqlik: ${accuracy}m)`);
    },
    (error) => {
      console.error("Joylashuvni olishda xato:", error.message);
    },
    {
      enableHighAccuracy: true,
      timeout: 5000,
      maximumAge: 0,
    }
  );
}

// 2. Joylashuvni uzluksiz kuzatish
let watchId = null;

function startWatchingLocation() {
  watchId = navigator.geolocation.watchPosition(
    (position) => {
      const { latitude, longitude } = position.coords;
      updateMapMarker(latitude, longitude);
    },
    (error) => console.error("Kuzatish xatosi:", error.message)
  );
}

function stopWatchingLocation() {
  if (watchId !== null) {
    navigator.geolocation.clearWatch(watchId);
    watchId = null;
  }
}

function updateMapMarker(lat, lng) {
  console.log(`Xarita belgisi yangilandi: ${lat}, ${lng}`);
}

// 3. Kamera va mikrofonga kirish (getUserMedia)
async function startCamera(videoElement) {
  try {
    const stream = await navigator.mediaDevices.getUserMedia({
      video: { facingMode: 'user', width: 640, height: 480 },
      audio: true,
    });

    videoElement.srcObject = stream;
    console.log('Kamera ulandi');

    return stream;
  } catch (error) {
    if (error.name === 'NotAllowedError') {
      console.error("Foydalanuvchi ruxsat bermadi");
    } else if (error.name === 'NotFoundError') {
      console.error("Kamera yoki mikrofon topilmadi");
    } else {
      console.error("Xato:", error.message);
    }
    return null;
  }
}

// 4. Kamerani to'xtatish (resurslarni bo'shatish)
function stopCamera(stream) {
  if (stream) {
    stream.getTracks().forEach((track) => track.stop());
    console.log('Kamera to\'xtatildi');
  }
}

// Foydalanish
getCurrentLocation();
const videoEl = document.getElementById('videoPreview');
startCamera(videoEl).then((stream) => {
  setTimeout(() => stopCamera(stream), 10000);
});
