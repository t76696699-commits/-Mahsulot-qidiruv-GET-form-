# 1. Foydalanuvchidan sonlarni kiritish va casting (float ga o'tkazish)
try:
    num1 = float(input("Birinchi sonni kiriting: "))
    num2 = float(input("Ikkinchi sonni kiriting: "))
    amal = input("Amalni kiriting (+, -, *, /): ")

    # 2. To'rtta amalni bajarish va 0 ga bo'lishni tekshirish
    if amal == "+":
        natija = num1 + num2
        # 3. Natijalarni f-string yordamida 2 xona aniqlikda formatlash
        print(f"Natija: {num1} + {num2} = {natija:.2f}")
    elif amal == "-":
        natija = num1 - num2
        print(f"Natija: {num1} - {num2} = {natija:.2f}")
    elif amal == "*":
        natija = num1 * num2
        print(f"Natija: {num1} * {num2} = {natija:.2f}")
    elif amal == "/":
        if num2 == 0:
            print("Xatolik: 0 ga bo'lish mumkin emas!")
        else:
            natija = num1 / num2
            print(f"Natija: {num1} / {num2} = {natija:.2f}")
    else:
        print("Noto'g'ri amal kiritildi!")

    # 4. type() yordamida natija o'zgaruvchisining turini chiqarish (agar amal to'g'ri bajarilgan bo'lsa)
    if amal in ["+", "-", "*"] or (amal == "/" and num2 != 0):
        print(f"O'zgaruvchi turi (type): {type(natija)}")

except ValueError:
    print("Xatolik: Faqat raqam kiritilishi kerak!")
