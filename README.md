# 1. DOIRA (Aylana)
def doira_maydoni(radius, pi=3.14159):
    """Doiraning maydonini hisoblaydi. Agar radius manfiy bo'lsa ValueError beradi."""
    if radius < 0:
        raise ValueError("Radius manfiy bo'lishi mumkin emas!")
    return pi * (radius ** 2)

def doira_perimetri(radius, pi=3.14159):
    """Doiraning perimetrini (aylana uzunligini) hisoblaydi."""
    if radius < 0:
        raise ValueError("Radius manfiy bo'lishi mumkin emas!")
    return 2 * pi * radius


# 2. KVADRAT
def kvadrat_maydoni(tomon):
    """Kvadratning maydonini hisoblaydi."""
    if tomon < 0:
        raise ValueError("Tomon manfiy bo'lishi mumkin emas!")
    return tomon ** 2

def kvadrat_perimetri(tomon):
    """Kvadratning perimetrini hisoblaydi."""
    if tomon < 0:
        raise ValueError("Tomon manfiy bo'lishi mumkin emas!")
    return 4 * tomon


# 3. TO'G'RI TO'RTBURCHAK
def tortburchak_maydoni(eni, boyi):
    """To'g'ri to'rtburchakning maydonini hisoblaydi."""
    if eni < 0 or boyi < 0:
        raise ValueError("O'lchamlar manfiy bo'lishi mumkin emas!")
    return eni * boyi

def tortburchak_perimetri(eni, boyi):
    """To'g'ri to'rtburchakning perimetrini hisoblaydi."""
    if eni < 0 or boyi < 0:
        raise ValueError("O'lchamlar manfiy bo'lishi mumkin emas!")
    return 2 * (eni + boyi)


# ASOSIY MENU FUNKSIYASI
def asosiy_menu():
    while True:
        print("\n--- GEOMETRIK SHAKLLAR ---")
        print("1. Doira")
        print("2. Kvadrat")
        print("3. To'g'ri to'rtburchak")
        print("4. Chiqish")
        
        tanlov = input("Tanlovingiz (1-4): ").strip()
        
        if tanlov == "1":
            try:
                r = float(input("Doira radiusini kiriting: "))
                m = doira_maydoni(r)
                p = doira_perimetri(r)
                print(f"Natija -> Maydoni: {m:.2f}, Perimetri: {p:.2f}")
            except ValueError as e:
                print(f"Xatolik: {e}")
                
        elif tanlov == "2":
            try:
                t = float(input("Kvadrat tomonini kiriting: "))
                m = kvadrat_maydoni(t)
                p = kvadrat_perimetri(t)
                print(f"Natija -> Maydoni: {m}, Perimetri: {p}")
            except ValueError as e:
                print(f"Xatolik: {e}")
                
        elif tanlov == "3":
            try:
                eni = float(input("Enini kiriting: "))
                boyi = float(input("Bo'yini kiriting: "))
                m = tortburchak_maydoni(eni, boyi)
                p = tortburchak_perimetri(eni, boyi)
                print(f"Natija -> Maydoni: {m}, Perimetri: {p}")
            except ValueError as e:
                print(f"Xatolik: {e}")
                
        elif tanlov == "4":
            print("Dastur tugatildi.")
            break
        else:
            print("Noto'g'ri tanlov! 1 dan 4 gacha raqam kiriting.")

if __name__ == "__main__":
    asosiy_menu()
