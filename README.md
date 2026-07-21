Shartli ifodalar — if/elif/else
True

False

True

False

murakkab shart

bool ifoda

if shart True

asosiy blok

elif shart True

elif blok

else blok

davom

and or not

Hozirgacha dasturlarimiz har doim bir xil ishlagan. Endi qaror qabul qilishni o'rganamiz: agar shart bajarilsa — bir narsa, aks holda — boshqa.

Eng oddiy shart — if
yosh = int(input("Yoshingiz: "))

if yosh >= 18:
    print("Siz balog'at yoshidasiz")
E'tibor bering: Python indentatsiya (bo'sh joylar bilan ko'chirish) ishlatadi blok belgisi sifatida. Boshqa tillarda {} qavslar ishlatiladi — Python'da 4 ta bo'sh joy.

if + else — ikki yo'l
yosh = int(input("Yoshingiz: "))

if yosh >= 18:
    print("Voyaga yetgan")
else:
    print("Voyaga yetmagan")
if + elif + else — bir nechta yo'l
baho = int(input("Bahoyingiz (0-100): "))

if baho >= 90:
    print("A — A'lo")
elif baho >= 75:
    print("B — Yaxshi")
elif baho >= 60:
    print("C — Qoniqarli")
elif baho >= 50:
    print("D — Yetarli")
else:
    print("F — Qoniqarsiz")
elif — "else if" ning qisqartmasi. Faqat bitta blok ishlaydi — birinchi True bo'lganida.

Solishtirish operatorlari
Belgi	Ma'no	Misol
==	teng	x == 5
!=	teng emas	x != 0
<	kichik	x < 10
>	katta	x > 10
<=	kichik yoki teng	x <= 100
>=	katta yoki teng	x >= 0
⚠️ Eng ko'p uchragan xato: = (qiymat berish) va == (solishtirish)ni aralashtirish. if x = 5: — SyntaxError. To'g'risi: if x == 5:.

Mantiqiy operatorlar — and, or, not
yosh = 25
shahar = "Toshkent"

# and — ikkalasi ham True bo'lishi kerak
if yosh >= 18 and shahar == "Toshkent":
    print("Voyaga yetgan toshkentlik")

# or — kamida bittasi True bo'lsa yetadi
if shahar == "Toshkent" or shahar == "Samarqand":
    print("Katta shaharda yashayapsiz")

# not — qiymatni teskari qiladi
mehmon = False
if not mehmon:
    print("Siz mehmon emassiz")
Boolean qiymatlar va "truthy" tushunchasi
Python'da har qiymat True yoki False deb baholanadi:

False	True
False, 0, 0.0, "", None, [], {}	Qolgan deyarli hamma narsa
ism = input("Ismingiz: ")
if ism:                  # bo'sh string False, demak boshqasi True
    print(f"Salom, {ism}")
else:
    print("Ism kiritmadingiz")
Ichki shartlar (nested)
yosh = 25
foydalanuvchi_id = 42

if foydalanuvchi_id:
    if yosh >= 18:
        print("Ruxsat berildi")
    else:
        print("Yosh kichik")
else:
    print("Login qiling")
3 dan ortiq ichki blok — yomon belgi. Ko'pincha bittada birlashtirsa bo'ladi:

if foydalanuvchi_id and yosh >= 18:
    print("Ruxsat berildi")
⚠️ Eng ko'p uchraydigan xatolar
: ni unutish: if x > 5 — SyntaxError. if x > 5: kerak.
Indentatsiya buzilishi: blok ichidagi qatorlar bir xil ko'chirilishi kerak (har doim 4 bo'sh joy).
= vs ==: = qiymat berish, == solishtirish.
Bool natijani solishtirish: if x == True: — keraksiz. Qisqaroq: if x:
💻
Код
Код
#2
python
 Копировать
# conditions.py — shartli ifodalar

# Yosh va guruh aniqlash
yosh = int(input("Yoshingizni kiriting: "))

if yosh < 0:
    print("Yosh manfiy bo'lmaydi!")
elif yosh < 6:
    print("Bola")
elif yosh < 13:
    print("Maktab yoshi")
elif yosh < 18:
    print("O'smir")
elif yosh < 60:
    print("Voyaga yetgan")
else:
    print("Keksa yosh")

# Mantiqiy operatorlar
print()
shahar = input("Qaysi shaharda yashaysiz? ").strip().title()
ish = input("Ishlaysizmi? (ha/yoq): ").strip().lower()

if yosh >= 18 and ish == "ha":
    print("Siz mustaqil yashaysiz")
elif yosh >= 18 and ish != "ha":
    print("Voyaga yetgan, ammo ish topish kerak")
else:
    print("Hali bola — ota-onaga tayanasiz")

if shahar == "Toshkent" or shahar == "Samarqand" or shahar == "Buxoro":
    print(f"{shahar} — katta shahar!")
else:
    print(f"{shahar} — yaxshi joy")

# Truthy va falsy
print()
foydalanuvchi_nomi = input("Foydalanuvchi nomi (bo'sh qoldirish mumkin): ")
if foydalanuvchi_nomi:
    print(f"Xush kelibsiz, {foydalanuvchi_nomi}!")
else:
    print("Mehmon sifatida kirdingiz")

# Juft-toq tekshirish
son = int(input("Sonni kiriting: "))
if son % 2 == 0:
    print(f"{son} — juft son")
else:
    print(f"{son} — toq son")
