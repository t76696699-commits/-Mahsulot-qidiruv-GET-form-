Pozitsiyalash va Z-index
Урок 5 из 5
· 2 раздела
📝
Текст
Текст
#1
Pozitsiyalash va Z-index
CSS pozitsiyalash elementlarni sahifada aniq joylashtirishga imkon beradi. position xususiyati elementning qanday joylashuvini belgilaydi.

1. position: static (Standart)
Barcha elementlar uchun standart qiymat. top, right, bottom, left va z-index ta'sir qilmaydi.

2. position: relative
Element o'zining asl joyidan siljiydi, lekin sahifa oqimida o'z o'rnini saqlab qoladi.

.relative-box {
  position: relative;
  top: 20px;
  left: 30px;
}
3. position: absolute
Element hujjat oqimidan chiqariladi va eng yaqin position: relative bo'lgan ota elementga nisbatan joylashtiriladi.

.container {
  position: relative;
  width: 300px;
  height: 200px;
}

.absolute-badge {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: #dc3545;
  color: white;
  padding: 4px 10px;
  border-radius: 12px;
}
4. position: fixed
Element brauzer oynasiga nisbatan joylashtiriladi va sahifa aylantirilganda ham o'z joyida qoladi.

.site-header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 64px;
  z-index: 1000;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.35);
}

body {
  padding-top: 64px;
}
5. position: sticky
Element avval relative kabi harakat qiladi, lekin belgilangan chegara ga yetgach, fixed ga o'xshab o'sha joyda qolib ketadi.

.section-title {
  position: sticky;
  top: 64px;
  background-color: #f8f9fa;
  z-index: 100;
}
Z-index va Stek konteksti
z-index pozitsiyalangan elementlarning z-o'qi bo'ylab tartibini belgilaydi. Faqat static bo'lmagan elementlarda ishlaydi.

.modal-overlay {
  position: fixed;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.55);
  z-index: 500;
}

.modal-dialog {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #ffffff;
  padding: 32px;
  border-radius: 12px;
  z-index: 501;
}
Amaliy misol: Tooltip
.tooltip-host {
  position: relative;
  display: inline-block;
}

.tooltip-bubble {
  visibility: hidden;
  opacity: 0;
  position: absolute;
  bottom: calc(100% + 8px);
  left: 50%;
  transform: translateX(-50%);
  background-color: #212529;
  color: #ffffff;
  padding: 7px 14px;
  border-radius: 6px;
  white-space: nowrap;
  font-size: 13px;
  z-index: 20;
  transition: opacity 0.25s ease;
}

.tooltip-host:hover .tooltip-bubble {
  visibility: visible;
  opacity: 1;
}
Mini-Loyiha: Pozitsiyalash bilan Modal Oyna
Quyidagi vazifalarni bajaring:

Tugma bosganda ochiluvchi modal oyna yarating (position: fixed + to'liq ekran overlay).
Modal ichidagi yopish (×) tugmasini position: absolute bilan yuqori o'ng burchakka qo'ying.
z-index bilan modal boshqa elementlarning ustida tursin.
.overlay { position: fixed; inset: 0; background: rgba(0,0,0,.5); z-index: 100;
           display: flex; align-items: center; justify-content: center; }
.modal { position: relative; background: #fff; padding: 32px; border-radius: 12px; }
Maqsad: Pozitsiyalash va z-index bilan real UI komponent yaratish.Pozitsiyalash va Z-index
Урок 5 из 5
· 2 раздела
📝
Текст
Текст
#1
Pozitsiyalash va Z-index
CSS pozitsiyalash elementlarni sahifada aniq joylashtirishga imkon beradi. position xususiyati elementning qanday joylashuvini belgilaydi.

1. position: static (Standart)
Barcha elementlar uchun standart qiymat. top, right, bottom, left va z-index ta'sir qilmaydi.

2. position: relative
Element o'zining asl joyidan siljiydi, lekin sahifa oqimida o'z o'rnini saqlab qoladi.

.relative-box {
  position: relative;
  top: 20px;
  left: 30px;
}
3. position: absolute
Element hujjat oqimidan chiqariladi va eng yaqin position: relative bo'lgan ota elementga nisbatan joylashtiriladi.

.container {
  position: relative;
  width: 300px;
  height: 200px;
}

.absolute-badge {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: #dc3545;
  color: white;
  padding: 4px 10px;
  border-radius: 12px;
}
4. position: fixed
Element brauzer oynasiga nisbatan joylashtiriladi va sahifa aylantirilganda ham o'z joyida qoladi.

.site-header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 64px;
  z-index: 1000;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.35);
}

body {
  padding-top: 64px;
}
5. position: sticky
Element avval relative kabi harakat qiladi, lekin belgilangan chegara ga yetgach, fixed ga o'xshab o'sha joyda qolib ketadi.

.section-title {
  position: sticky;
  top: 64px;
  background-color: #f8f9fa;
  z-index: 100;
}
Z-index va Stek konteksti
z-index pozitsiyalangan elementlarning z-o'qi bo'ylab tartibini belgilaydi. Faqat static bo'lmagan elementlarda ishlaydi.

.modal-overlay {
  position: fixed;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.55);
  z-index: 500;
}

.modal-dialog {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #ffffff;
  padding: 32px;
  border-radius: 12px;
  z-index: 501;
}
Amaliy misol: Tooltip
.tooltip-host {
  position: relative;
  display: inline-block;
}

.tooltip-bubble {
  visibility: hidden;
  opacity: 0;
  position: absolute;
  bottom: calc(100% + 8px);
  left: 50%;
  transform: translateX(-50%);
  background-color: #212529;
  color: #ffffff;
  padding: 7px 14px;
  border-radius: 6px;
  white-space: nowrap;
  font-size: 13px;
  z-index: 20;
  transition: opacity 0.25s ease;
}

.tooltip-host:hover .tooltip-bubble {
  visibility: visible;
  opacity: 1;
}
Mini-Loyiha: Pozitsiyalash bilan Modal Oyna
Quyidagi vazifalarni bajaring:

Tugma bosganda ochiluvchi modal oyna yarating (position: fixed + to'liq ekran overlay).
Modal ichidagi yopish (×) tugmasini position: absolute bilan yuqori o'ng burchakka qo'ying.
z-index bilan modal boshqa elementlarning ustida tursin.
.overlay { position: fixed; inset: 0; background: rgba(0,0,0,.5); z-index: 100;
           display: flex; align-items: center; justify-content: center; }
.modal { position: relative; background: #fff; padding: 32px; border-radius: 12px; }
Maqsad: Pozitsiyalash va z-index bilan real UI komponent yaratish.
