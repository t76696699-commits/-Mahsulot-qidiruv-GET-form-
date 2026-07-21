Takrorlash: Modul 2 — Shartlar, sikllar, listlar
qoshish

ochirish

korsatish

saralash

chiqish

items list

menu while True

input then append

input then remove

for then print

sort or sorted

break

dastur tugaydi

Modul 2 da siz shartli ifodalar, sikllar, list va tuple bilan ishlashni o'rgandingiz. Endi bu 3 ta konseptni birga ishlatib, o'z tovarlar ro'yxati (TODO list) ilovasini yaratamiz.

📋 Modul 2 da nimalarni o'rgandingiz
Dars	Asosiy konsept	Misol
4	if/elif/else, and/or/not, ==/!=	if x > 0 and y < 10:
5	for/while, range, break/continue	for i in range(10):
6	list, tuple, slicing, comprehension	[x*2 for x in nums]
🧩 Modul 1 + 2 = haqiqiy ilova
Endi bizda quyidagi imkoniyatlar bor:

Foydalanuvchi bilan muloqot: input, print, f-string (1-modul)
Qaror qabul qilish: if/elif/else (4-dars)
Takrorlash: while True bilan menu (5-dars)
Ma'lumotni saqlash: list.append, list.remove (6-dars)
Bu to'rt narsa birga — interaktiv ilovaning poydevori. Tovarlar ro'yxati, kontakt kitobi, to-do list — hammasi shu shaklda yoziladi.

🏗 Menu pattern — har bir interaktiv ilovaning poydevori
tovarlar = []

while True:
    print()
    print("1. Tovar qo'shish")
    print("2. Tovar o'chirish")
    print("3. Hammasini ko'rsatish")
    print("4. Chiqish")
    tanlov = input("Tanlovingiz: ").strip()

    if tanlov == "1":
        # ... qo'shish ...
    elif tanlov == "2":
        # ... o'chirish ...
    elif tanlov == "3":
        # ... ko'rsatish ...
    elif tanlov == "4":
        break
    else:
        print("Noma'lum tanlov")
Bu shakl juda muhim — uni eslab qoling. Kelajakda har xil ilovalarga bu pattern asos bo'ladi.

⚠️ Modul 2 da eng ko'p uchragan xatolar
Sikl ichida o'zgaruvchini o'zgartirishni unutish: while x < 10: — agar x ichkarida o'zgarmasa, cheksiz sikl.
range() chegarasi: range(1, 10) da 10 KIRMAYDI. Faqat 1..9.
List o'zgartirish vaqtida aylanish: for x in lst: lst.remove(x) — xato. Yangi list yarating.
List index xatosi: bo'sh listdan l[0] olish — IndexError. Avval if l: tekshiring.
Strani solishtirish: foydalanuvchi javobi katta/kichik harf bo'lishi mumkin. .strip().lower() ishlating.
🎯 Endi navbat sizda
Pastdagi kod — to'liq ishlaydigan tovarlar ro'yxati ilovasi. Birinchi navbatda kodni o'qib chiqing va har qatorni tushuning. Keyin o'zingizning loyihangizni qurishga o'ting — masalan, vazifalar ro'yxati (TODO) yoki kontakt kitobi.

💻
Код
Код
#2
python
 Копировать
# tovarlar_royxati.py — Modul 2 takrorlash loyihasi
# Menu pattern + if/elif/else + while + list

tovarlar = []

print("🛒 Tovarlar ro'yxati ilovasi")
print("=" * 40)

while True:
    print()
    print("1. Tovar qo'shish")
    print("2. Tovar o'chirish")
    print("3. Hammasini ko'rsatish")
    print("4. Alifbo tartibida saralash")
    print("5. Qidirish")
    print("6. Chiqish")

    tanlov = input("Tanlov (1-6): ").strip()

    if tanlov == "1":
        nom = input("Tovar nomi: ").strip()
        if not nom:
            print("⚠️ Bo'sh nom bo'lmaydi")
        elif nom.lower() in [t.lower() for t in tovarlar]:
            print(f"⚠️ '{nom}' allaqachon ro'yxatda")
        else:
            tovarlar.append(nom)
            print(f"✅ '{nom}' qo'shildi")

    elif tanlov == "2":
        if not tovarlar:
            print("⚠️ Ro'yxat bo'sh")
            continue
        nom = input("O'chirilsin: ").strip()
        # Case-insensitive remove
        for t in tovarlar:
            if t.lower() == nom.lower():
                tovarlar.remove(t)
                print(f"🗑 '{t}' o'chirildi")
                break
        else:
            print(f"⚠️ '{nom}' topilmadi")

    elif tanlov == "3":
        if not tovarlar:
            print("Ro'yxat bo'sh — birinchi tovarni qo'shing")
        else:
            print(f"\n📋 Jami {len(tovarlar)} ta tovar:")
            for i, t in enumerate(tovarlar, start=1):
                print(f"  {i}. {t}")

    elif tanlov == "4":
        tovarlar.sort(key=str.lower)
        print("🔤 Alifbo tartibida saralandi")

    elif tanlov == "5":
        if not tovarlar:
            print("Ro'yxat bo'sh")
            continue
        kalit = input("Qidirilsin: ").strip().lower()
        topildi = [t for t in tovarlar if kalit in t.lower()]
        if topildi:
            print(f"🔍 {len(topildi)} ta natija:")
            for t in topildi:
                print(f"  • {t}")
        else:
            print(f"'{kalit}' uchun hech narsa topilmadi")

    elif tanlov == "6":
        print(f"\nYakuniy ro'yxat ({len(tovarlar)} ta): {tovarlar}")
        print("Xayr!")
        break

    else:
        print(f"⚠️ Noma'lum tanlov: {tanlov!r}")
