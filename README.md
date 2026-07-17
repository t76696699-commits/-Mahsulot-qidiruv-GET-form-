File API nima?
File API — foydalanuvchi tanlagan (yoki sudrab tashlagan) fayllarni JavaScript orqali o'qish imkonini beruvchi veb interfeys. U <input type="file"> yoki drag-and-drop orqali tanlangan fayllarga kirish, ularning ma'lumotlarini (nomi, hajmi, turi) olish va kontentini o'qishga imkon beradi.

Asosiy obyektlar
File — bitta faylni ifodalaydi (name, size, type, lastModified)
FileList — bir nechta fayllar to'plami (masalan, ko'p fayl tanlanganda)
FileReader — fayl kontentini asinxron o'qish uchun
FileReader metodlari
Metod	Vazifasi
readAsText(file)	Faylni matn sifatida o'qiydi
readAsDataURL(file)	Base64 formatida o'qiydi (rasm ko'rsatish uchun)
readAsArrayBuffer(file)	Ikkilik ma'lumot sifatida o'qiydi
Drag and Drop API
Drag and Drop API — foydalanuvchilarga elementlarni sichqoncha yordamida sudrab ko'chirish imkonini beradi. Fayl yuklash uchun eng ko'p qo'llaniladigan holatlardan biri — foydalanuvchi kompyuteridagi faylni brauzer oynasiga sudrab tashlashi.

Muhim drag-drop hodisalari
dragover — element ustida sudralayotganda (default xatti-harakatni to'xtatish kerak: preventDefault())
drop — element tashlanganda, event.dataTransfer.files orqali fayllarga kirish mumkin
Fayl yuklash oqimi
input type=file

Drag and Drop

Foydalanuvchi faylni tanlaydi yoki sudrab tashlaydi

Usul

change hodisasi, input.files

drop hodisasi, dataTransfer.files

FileReader yaratiladi

readAsDataURL yoki readAsText chaqiriladi

onload: natija tayyor

Fayl ko'rsatiladi yoki qayta ishlanadi

Bu ikki API birgalikda zamonaviy fayl yuklash interfeyslarini (masalan, rasm preview, drag-drop fayl yuklash zonasi) yaratishga imkon beradi.

💻
Код
Kod namunasi
#2
code
 Копировать
// file-upload.js — File API va Drag-and-Drop birgalikda
const dropZone = document.getElementById('dropZone');
const fileInput = document.getElementById('fileInput');
const preview = document.getElementById('preview');

// 1. Oddiy input orqali fayl tanlash
fileInput.addEventListener('change', (event) => {
  const files = event.target.files;
  handleFiles(files);
});

// 2. Drag and Drop hodisalari
dropZone.addEventListener('dragover', (event) => {
  event.preventDefault(); // Muhim: default xatti-harakatni to'xtatish
  dropZone.classList.add('drag-active');
});

dropZone.addEventListener('dragleave', () => {
  dropZone.classList.remove('drag-active');
});

dropZone.addEventListener('drop', (event) => {
  event.preventDefault();
  dropZone.classList.remove('drag-active');

  const files = event.dataTransfer.files;
  handleFiles(files);
});

// 3. Fayllarni qayta ishlash
function handleFiles(fileList) {
  const files = Array.from(fileList);

  files.forEach((file) => {
    console.log(`Fayl: ${file.name}, hajmi: ${formatSize(file.size)}, turi: ${file.type}`);

    if (file.type.startsWith('image/')) {
      previewImage(file);
    } else if (file.type === 'text/plain') {
      previewText(file);
    } else {
      console.warn(`Qo'llab-quvvatlanmaydigan fayl turi: ${file.type}`);
    }
  });
}

// 4. Rasmni preview qilish (readAsDataURL)
function previewImage(file) {
  const reader = new FileReader();

  reader.onload = (event) => {
    const img = document.createElement('img');
    img.src = event.target.result;
    img.style.maxWidth = '200px';
    preview.appendChild(img);
  };

  reader.onerror = () => {
    console.error("Faylni o'qishda xato yuz berdi");
  };

  reader.readAsDataURL(file);
}

// 5. Matn faylini o'qish (readAsText)
function previewText(file) {
  const reader = new FileReader();

  reader.onload = (event) => {
    const pre = document.createElement('pre');
    pre.textContent = event.target.result.slice(0, 500);
    preview.appendChild(pre);
  };

  reader.readAsText(file);
}

// 6. Fayl hajmini o'qishga qulay formatga o'tkazish
function formatSize(bytes) {
  if (bytes < 1024) return `${bytes} B`;
  if (bytes < 1024 * 1024) return `${(bytes / 1024).toFixed(1)} KB`;
  return `${(bytes / (1024 * 1024)).toFixed(1)} MB`;
}
