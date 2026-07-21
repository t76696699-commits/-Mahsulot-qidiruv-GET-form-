Ro'yxatlar (list) va kortejlar (tuple)
Syntax error in text
mermaid version 11.15.0
Bir nechta qiymatni bitta o'zgaruvchida saqlash uchun list (ro'yxat) ishlatiladi. List — dasturlashning eng muhim ma'lumot strukturalaridan biri.

List yaratish
mevalar = ["olma", "banan", "uzum"]
sonlar = [10, 20, 30, 40, 50]
aralash = [1, "salom", 3.14, True]
bosh = []

print(mevalar)        # ['olma', 'banan', 'uzum']
print(len(mevalar))   # 3
Indekslash va slicing
mevalar = ["olma", "banan", "uzum", "shaftoli", "anjir"]
#            0       1        2         3         4
#           -5      -4       -3        -2        -1

print(mevalar[0])      # 'olma'
print(mevalar[-1])     # 'anjir'  (oxiridan)
print(mevalar[1:3])    # ['banan', 'uzum']
print(mevalar[:2])     # ['olma', 'banan']
print(mevalar[2:])     # ['uzum', 'shaftoli', 'anjir']
print(mevalar[::-1])   # teskari tartib
List o'zgartirish — eng muhim metodlar
mevalar = ["olma", "banan"]

mevalar.append("uzum")           # oxiriga qo'shish
print(mevalar)                   # ['olma', 'banan', 'uzum']

mevalar.insert(0, "anjir")       # 0-indeksga qo'yish
print(mevalar)                   # ['anjir', 'olma', 'banan', 'uzum']

mevalar.remove("olma")           # qiymat bo'yicha o'chirish
oxirgi = mevalar.pop()           # oxirgini olib tashlash va qaytarish
print("Olib tashlangan:", oxirgi)

mevalar.sort()                   # alifbo tartibida
mevalar.reverse()                # teskari
print(mevalar)

mevalar.clear()                  # hammasini bo'shatish
print(mevalar)                   # []
List ichida bormi? — in operator
mevalar = ["olma", "banan", "uzum"]

if "olma" in mevalar:
    print("Bor!")
if "kiwi" not in mevalar:
    print("Yo'q!")
List bo'ylab aylanish
narxlar = [5.50, 12.30, 8.75]

# Oddiy
for narx in narxlar:
    print(f"${narx:.2f}")

# Indeks bilan
for i, narx in enumerate(narxlar):
    print(f"{i}. ${narx:.2f}")

# Yig'indi
print("Jami:", sum(narxlar))
print("Eng katta:", max(narxlar))
print("Eng kichik:", min(narxlar))
List comprehension — qisqa va kuchli
# Klassik usul
kvadratlar = []
for i in range(1, 6):
    kvadratlar.append(i * i)
print(kvadratlar)  # [1, 4, 9, 16, 25]

# List comprehension — bir qatorda
kvadratlar = [i * i for i in range(1, 6)]
print(kvadratlar)  # [1, 4, 9, 16, 25]

# Shart bilan
juftlar = [i for i in range(1, 11) if i % 2 == 0]
print(juftlar)     # [2, 4, 6, 8, 10]

# Stringdagi unlilarni olish
matn = "Python dasturlash"
unlilar_list = [h for h in matn if h in "aeiou"]
print(unlilar_list)  # ['o', 'a', 'u', 'a']
Tuple — o'zgarmas list
Tuple — list ga o'xshaydi, lekin o'zgartirib bo'lmaydi (immutable). Doimiy ma'lumotlar uchun ishlatiladi.

nuqta = (3.5, 4.8)         # tuple yaratish
ranglar = ("qizil", "yashil", "ko'k")

print(nuqta[0])            # 3.5
print(len(ranglar))        # 3

# nuqta[0] = 5.0   # ❌ TypeError — o'zgartirib bo'lmaydi

# Unpacking — bir nechta o'zgaruvchiga taqsimlash
x, y = nuqta
print(x, y)                # 3.5 4.8

r, g, b = ranglar
print(r, g, b)             # qizil yashil ko'k
List vs Tuple — qachon qaysisi?
List	Tuple
O'zgaradi (mutable)	O'zgarmas (immutable)
[1, 2, 3]	(1, 2, 3)
Ro'yxat, navbat, to'plam	Koordinata, RGB, doimiy ma'lumot
Sekinroq	Tezroq
⚠️ Eng ko'p uchraydigan xatolar
Index out of range: mevalar[10] — agar list 3 elementli bo'lsa.
append vs extend: a.append([1,2]) ichkariga list qo'yadi. a.extend([1,2]) elementlarni qo'shadi.
Tuple o'zgartirish: (1, 2)[0] = 5 — TypeError.
List va string aralashtirish: "abc"[0] = "a" (string), ["a","b","c"][0] = "a" (list elementi).
💻
Код
Код
#2
python
 Копировать
# lists.py — listlar va tuplelar bilan ishlash

# 1. List yaratish va o'zgartirish
mevalar = ["olma", "banan", "uzum"]
print("Boshlang'ich:", mevalar)

mevalar.append("shaftoli")
print("append:", mevalar)

mevalar.insert(0, "anjir")
print("insert(0):", mevalar)

oxirgi = mevalar.pop()
print(f"pop chiqdi: {oxirgi}, qoldi: {mevalar}")

# 2. Slicing
sonlar = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
print()
print("sonlar[2:5]:", sonlar[2:5])
print("sonlar[:3]:", sonlar[:3])
print("sonlar[-3:]:", sonlar[-3:])
print("har 2-chi:", sonlar[::2])
print("teskari:", sonlar[::-1])

# 3. Statistika
print()
print(f"Jami: {sum(sonlar)}")
print(f"O'rtacha: {sum(sonlar) / len(sonlar):.2f}")
print(f"Min: {min(sonlar)}, Max: {max(sonlar)}")

# 4. List comprehension
kvadratlar = [x * x for x in range(1, 11)]
print()
print("Kvadratlar:", kvadratlar)

juftlar = [x for x in sonlar if x % 4 == 0]
print("4 ga karralilar:", juftlar)

# 5. Tuple
nuqta = (3.5, 4.8)
x, y = nuqta
print()
print(f"Nuqta: x={x}, y={y}")

# 6. Foydalanuvchi listi
print()
talabalar = []
for _ in range(3):
    ism = input("Talaba ismi: ").strip()
    if ism:
        talabalar.append(ism)

talabalar.sort()
print("Alifbo tartibida:")
for i, t in enumerate(talabalar, start=1):
    print(f"  {i}. {t}")
