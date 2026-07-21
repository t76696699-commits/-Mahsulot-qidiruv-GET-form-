Sikllar — for va while
yes

no

yes

break

continue

no

for i in range 5

body har iter

yana qoldi

loop tugadi

while shart

shart True

body

Ba'zan bir xil amalni bir necha marta takrorlash kerak: 100 ta foydalanuvchini ro'yxatdan o'tkazish, 1 dan 1000 gacha sonlarni hisoblash, fayldagi har qatorni qayta ishlash. Sikl (loop) shu uchun.

for sikli — ma'lum miqdorda takrorlash
# range(5) → 0, 1, 2, 3, 4
for i in range(5):
    print("i =", i)
Natija:

i = 0
i = 1
i = 2
i = 3
i = 4
range — son ketma-ketligi
Chaqirish	Natija
range(5)	0, 1, 2, 3, 4
range(1, 6)	1, 2, 3, 4, 5
range(0, 10, 2)	0, 2, 4, 6, 8
range(10, 0, -1)	10, 9, 8, ..., 1
String yoki list bo'ylab aylanish
matn = "Python"
for harf in matn:
    print(harf)

mevalar = ["olma", "banan", "uzum"]
for meva in mevalar:
    print(meva)
while sikli — shart bajarilguncha
son = 1
while son <= 5:
    print(son)
    son = son + 1
Sikl shart True bo'lguncha takrorlanadi. Ehtiyot: shartni o'zgartirishni unutmang — aks holda cheksiz sikl (infinite loop) yuz beradi va dastur to'xtamaydi.

while True + break — foydalanuvchidan kutish
while True:
    javob = input("Davom etamizmi? (ha/yoq): ")
    if javob == "yoq":
        break  # sikldan chiqish
    print("Davom etyapmiz")
print("Tugadi")
break va continue
break — sikldan butunlay chiqish
continue — joriy iteratsiyani o'tkazib yuborish va keyingisiga o'tish
for i in range(10):
    if i == 5:
        break       # 0,1,2,3,4 chiqadi va to'xtaydi
    print(i)

for i in range(10):
    if i % 2 == 0:
        continue    # juftlarni o'tkazib yuboradi
    print(i)        # 1, 3, 5, 7, 9
Nested loops — sikl ichida sikl
# Ko'paytirish jadvali
for i in range(1, 4):
    for j in range(1, 4):
        print(f"{i} * {j} = {i * j}")
    print("---")
enumerate — indeks va element birga
mevalar = ["olma", "banan", "uzum"]
for indeks, meva in enumerate(mevalar):
    print(f"{indeks}: {meva}")
# 0: olma
# 1: banan
# 2: uzum
Foydali pattern — yig'ish (accumulator)
# 1 dan 100 gacha sonlar yig'indisi
yigindi = 0
for son in range(1, 101):
    yigindi = yigindi + son
print("Yig'indi:", yigindi)  # 5050

# Eng katta sonni topish
sonlar = [3, 7, 2, 9, 4]
eng_katta = sonlar[0]
for son in sonlar:
    if son > eng_katta:
        eng_katta = son
print("Eng katta:", eng_katta)
⚠️ Eng ko'p uchraydigan xatolar
Cheksiz sikl: while x < 10: print(x) — x hech qachon o'zgarmagani uchun to'xtamaydi. Ctrl+C bilan to'xtating.
range(1, 10) → 10 ham bormi? Yo'q. range(1, 10) = 1, 2, ..., 9. 10 kirmaydi.
Sikl ichidagi o'zgaruvchini tashqarida ishlatish: ishlaydi, lekin oxirgi qiymat saqlanadi.
💻
Код
Код
#2
python
 Копировать
# loops.py — for va while sikllari

# 1. Oddiy for sikli
print("1 dan 5 gacha:")
for i in range(1, 6):
    print(i, end=" ")
print()

# 2. Yig'indi hisoblash
yigindi = 0
for son in range(1, 11):
    yigindi += son
print(f"1+2+...+10 = {yigindi}")

# 3. Ko'paytirish jadvali (5 ga)
print()
print("5 ga ko'paytirish jadvali:")
for i in range(1, 11):
    print(f"5 x {i} = {5 * i}")

# 4. Stringdagi unli harflar sanash
matn = input("Matn kiriting: ")
unlilar = "aeiouAEIOU"
soni = 0
for harf in matn:
    if harf in unlilar:
        soni += 1
print(f"Unli harflar soni: {soni}")

# 5. while bilan menyu
print()
sonlar = []
while True:
    javob = input("Son kiriting (chiqish uchun 'q'): ")
    if javob == "q":
        break
    try:
        sonlar.append(float(javob))
    except ValueError:
        print("Bu son emas, qayta urinib ko'ring")
        continue

if sonlar:
    print(f"Kiritilgan sonlar: {sonlar}")
    print(f"O'rtacha: {sum(sonlar) / len(sonlar):.2f}")
else:
    print("Hech narsa kiritilmadi")
