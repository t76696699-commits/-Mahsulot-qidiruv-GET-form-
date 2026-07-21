# 1. Foydalanuvchidan ma'lumotlarni qabul qilish va casting
try:
    print("--- Kengaytirilgan Kalkulyator ---")
    print("Amallar: +, -, *, /, //, %, **, foiz (f), ildiz (i)")
    
    amal = input("Amalni kiriting: ").strip().lower()

    # Kvadrat ildiz faqat bitta sonni talab qilgani uchun uni alohida tekshiramiz
    if amal == "i" or amal == "ildiz":
        num = float(input("Ildiz ostidagi sonni kiriting: "))
        
        if num < 0:
            print("Xatolik: Manfiy sondan kvadrat ildiz chiqarib bo'lmaydi!")
        else:
            # math kutubxonasiz kvadrat ildiz hisoblash
            natija = num ** 0.5
            print(f"Natija: √{num} = {natija:.2f}")
            print(f"O'zgaruvchi turi: {type(natija)}")
            
    else:
        # Ikki sonli amallar uchun
        num1 = float(input("Birinchi sonni kiriting: "))
        num2 = float(input("Ikkinchi sonni kiriting: "))

        # 2. Arifmetik amallar va maxsus xatoliklarni boshqarish
        if amal == "+":
            natija = num1 + num2
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
                
        elif amal == "//":
            if num2 == 0:
                print("Xatolik: 0 ga bo'lib butun qismni olib bo'lmaydi!")
            else:
                natija = num1 // num2
                print(f"Natija: {num1} // {num2} = {natija:.2f}")
                
        elif amal == "%":
            if num2 == 0:
                print("Xatolik: 0 ga bo'lib qoldiq topib bo'lmaydi!")
            else:
                natija = num1 % num2
                print(f"Natija: {num1} % {num2} = {natija:.2f}")
                
        elif amal == "**":
            natija = num1 ** num2
            print(f"Natija: {num1} ** {num2} = {natija:.2f}")
            
        elif amal == "f" or amal == "foiz":
            # a dan b foiz (masalan: 200 dan 15 foiz -> 200 * 15 / 100)
            natija = (num1 * num2) / 100
            print(f"Natija: {num1} ning {num2}% foizi = {natija:.2f}")
            
        else:
            print(f"Aniq xato: '{amal}' nomli noma'lum amal kiritildi! Iltimos, ro'yxatdagi amallardan birini tanlang.")

        # Agar amal muvaffaqiyatli bajarilgan bo'lsa, turning chiqarilishi
        if amal in ["+", "-", "*", "/", "//", "%", "**", "f", "foiz"] and not (amal in ["/", "//", "%"] and num2 == 0):
            print(f"O'zgaruvchi turi: {type(natija)}")

except ValueError:
    print("Aniq xato: Faqat raqamlar kiritilishi shart! Harf yoki bo'sh joy kiritmang.")
