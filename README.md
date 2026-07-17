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
