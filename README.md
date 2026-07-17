Canvas API nima?
Canvas API — <canvas> HTML elementi orqali brauzerda 2D (va WebGL yordamida 3D) grafika chizish imkonini beruvchi kuchli veb interfeys. U o'yinlar, diagrammalar, rasm muharrirlari va animatsiyalar yaratishda keng qo'llaniladi.

Kontekst olish
Chizishni boshlash uchun avval canvas elementining "kontekst"ini olish kerak: canvas.getContext('2d'). Bu metod chizish uchun barcha kerakli funksiyalarni o'z ichiga olgan obyektni qaytaradi.

Asosiy chizish metodlari
Metod	Vazifasi
fillRect(x,y,w,h)	To'ldirilgan to'rtburchak chizadi
strokeRect(x,y,w,h)	Faqat chegarasi bo'lgan to'rtburchak
arc(x,y,r,start,end)	Aylana yoki yoy chizadi
fillText(text,x,y)	Matn chizadi
drawImage(img,x,y)	Rasm joylashtiradi
Yo'l (Path) tushunchasi
Murakkab shakllar beginPath(), moveTo(), lineTo() va closePath() orqali quriladi, so'ngra fill() yoki stroke() bilan chiziladi.

Animatsiya
Canvas'da animatsiya yaratish uchun requestAnimationFrame() ishlatiladi — u brauzerning ekran yangilanish chastotasiga moslashib, silliq animatsiya beradi. Har bir kadrda avvalgi rasm clearRect() bilan tozalanadi va yangi holat chiziladi.

Canvas ishlash jarayoni
Ha

Yo'q

canvas elementi HTML da

getContext 2d chaqiriladi

Chizish buyruqlari: fillRect, arc, fillText

Animatsiya kerakmi?

requestAnimationFrame sikli

clearRect - eski kadr tozalanadi

Yangi holat chiziladi

Statik rasm tayyor

Canvas — pixel darajasida ishlaydi, ya'ni chizilgan shakllar alohida DOM elementlari emas, shuning uchun ular event listener'larga ega bo'lmaydi (koordinatalar orqali qo'lda hisoblash kerak).

💻
Код
Kod namunasi
#2
code
 Копировать
// canvas-demo.js — Canvas API asoslari va animatsiya
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');
canvas.width = 400;
canvas.height = 300;

// 1. Oddiy shakllar chizish
function drawShapes() {
  // To'rtburchak
  ctx.fillStyle = '#8b5cf6';
  ctx.fillRect(20, 20, 120, 80);

  // Chegarali to'rtburchak
  ctx.strokeStyle = '#06b6d4';
  ctx.lineWidth = 3;
  ctx.strokeRect(160, 20, 120, 80);

  // Aylana
  ctx.beginPath();
  ctx.arc(80, 180, 50, 0, Math.PI * 2);
  ctx.fillStyle = '#f59e0b';
  ctx.fill();

  // Matn
  ctx.font = '20px sans-serif';
  ctx.fillStyle = '#111827';
  ctx.fillText("Salom, Canvas!", 200, 200);
}

// 2. Murakkab yo'l (path) chizish — uchburchak
function drawTriangle() {
  ctx.beginPath();
  ctx.moveTo(300, 250);
  ctx.lineTo(350, 150);
  ctx.lineTo(390, 250);
  ctx.closePath();
  ctx.fillStyle = '#ef4444';
  ctx.fill();
}

// 3. Sodda animatsiya — sakrab yuruvchi to'p
let ballX = 50;
let ballDirection = 1;
const ballSpeed = 3;

function animateBall() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  drawShapes();
  drawTriangle();

  ballX += ballSpeed * ballDirection;
  if (ballX > canvas.width - 20 || ballX < 20) {
    ballDirection *= -1;
  }

  ctx.beginPath();
  ctx.arc(ballX, 270, 15, 0, Math.PI * 2);
  ctx.fillStyle = '#10b981';
  ctx.fill();

  requestAnimationFrame(animateBall);
}

// 4. Sichqoncha bosilgan joyni aniqlash (koordinata hisoblash)
canvas.addEventListener('click', (event) => {
  const rect = canvas.getBoundingClientRect();
  const x = event.clientX - rect.left;
  const y = event.clientY - rect.top;
  console.log(`Bosilgan koordinata: x=${x}, y=${y}`);

  ctx.beginPath();
  ctx.arc(x, y, 5, 0, Math.PI * 2);
  ctx.fillStyle = 'red';
  ctx.fill();
});

// Ishga tushirish
animateBall();
