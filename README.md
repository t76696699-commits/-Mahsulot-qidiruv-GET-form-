import random


def son_topish_oyin():
    print("--- SONNI TOPISH O'YINIGA XUSH KELIBSIZ! ---")
    print("Qiyinlik darajasini tanlang:")
    print("1. Oson (1 dan 50 gacha, max 10 ta urinish)")
    print("2. O'rta (1 dan 100 gacha, max 10 ta urinish)")
    print("3. Qiyin (1 dan 200 gacha, max 10 ta urinish)")

    # Qiyinlik darajasini tanlash
    while True:
        daraja = input("Tanlovingiz (1, 2 yoki 3): ").strip()
        if daraja == "1":
            max_son = 50
            break
        elif daraja == "2":
            max_son = 100
            break
        elif daraja == "3":
            max_son = 200
            break
        else:
            print("Iltimos, 1, 2 yoki 3 raqamlaridan birini tanlang!")

    # Tasodifiy sonni tanlash
    sirli_son = random.randint(1, max_son)
    urinishlar_limiti = 10
    urinish = 0

    print(
        f"\nMen 1 dan {max_son} gacha bo'lgan son o'yladim. Topishga harakat qiling!"
    )
    print("O'yinni to'xtatish uchun istalgan vaqtda 'q' harfini yozing.\n")

    # While sikli orqali urinishlar
    while urinish < urinishlar_limiti:
        foydalanuvchi_kiritishi = input(
            f"{urinish + 1}-urinish. Son kiriting: "
        ).strip()

        # Chiqish sharti
        if foydalanuvchi_kiritishi.lower() == "q":
            print(
                f"\nO'yin to'xtatildi. Men o'ylagan son {sirli_son} edi."
            )
            return

        # Raqamga tekshirish
        if not foydalanuvchi_kiritishi.isdigit():
            print("Iltimos, faqat butun son yoki 'q' harfini kiriting!")
            continue

        taxmin = int(foydalanuvchi_kiritishi)
        urinish += 1

        # Natijani tekshirish
        if taxmin < sirli_son:
            print("Kattaroq son kiriting ⬆️")
        elif taxmin > sirli_son:
            print("Kichikroq son kiriting ⬇️")
        else:
            print(
                f"\n🎉 Tabriklayman! Siz {sirli_son} sonini {urinish} ta urinishda topdingiz!"
            )
            break
    else:
        # Agar 10 ta urinishda ham topolmasa
        print(
            f"\n❌ Afsuski, urinishlar limiti tugadi. Men o'ylagan son {sirli_son} edi."
        )


# O'yinni ishga tushirish
if __name__ == "__main__":
    son_topish_oyin()
