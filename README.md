# -Mahsulot-qidiruv-GET-form-
Procfile: Fayl nomi aniq Procfile (kattalik bilan, hech qanday .txt kengaytmasiz) bo‘lishi shart. Ichidagi yozuv aynan shundaymi: web: gunicorn app:app? (Agar asosiy faylingiz main.py bo‘lsa, u web: gunicorn main:app bo‘lishi kerak).
Procfile: Fayl nomi aniq Procfile (kattalik bilan, hech qanday .txt kengaytmasiz) bo‘lishi shart. Ichidagi yozuv aynan shundaymi: web: gunicorn app:app? (Agar asosiy faylingiz main.py bo‘lsa, u web: gunicorn main:app bo‘lishi kerak).

requirements.txt: Fayl ichida gunicorn va python-dotenv kutubxonalari yozilganmi? (Terminalda pip freeze > requirements.txt buyrug‘ini ishlatib, uni yangilab ko‘ring).

Hosting sozlamalari (Environment Variables): Siz .env faylini GitHub'ga yuklamadingiz, bu juda to‘g‘ri! Lekin siz Render/Railway dashboardida (Settings -> Environment Variables) SECRET_KEY va ma'lumotlar bazasi yo'llarini qo'shdingizmi? Hosting uni koddan emas, o‘zining sozlamalaridan olishi kerak.

Database: Agar siz SQLite ishlatayotgan bo‘lsangiz, u production'da (Render'da) muammo tug‘dirishi mumkin, chunki u fayllar tizimini vaqtincha (ephemeral) saqlaydi va ilova restart bo‘lganda ma'lumotlar o‘chib ketadi. (Deploy uchun PostgreSQL tavsiya etiladi).

REST API: Saytingiz ochilayapti, lekin /api/notes endpoint'iga kirganda xatolik beryaptimi? Agar barcha kodlar app.py ichida bo‘lsa, balki router'lar o‘zaro to‘qnashayotgandir.
