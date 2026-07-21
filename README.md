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
