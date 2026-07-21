def todo_app():
    # Asosiy vazifalar ro'yxati (har bir vazifa dictionary ko'rinishida saqlanadi)
    vazifalar = []

    while True:
        print("\n" + "=" * 42)
        print("📌 VAZIFALAR BOSHQARUVCHISI (TO-DO LIST)")
        print("=" * 42)
        print("1. ➕ Vazifa qo'shish")
        print("2. 📋 Vazifalarni ko'rish")
        print("3. ✅ Vazifani bajarilgan deb belgilash")
        print("4. 🗑️ Vazifani o'chirish")
        print("5. 📊 Statistika")
        print("6. 🔄 Saralash (Muhimlik bo'yicha)")
        print("7. 🔍 Filtrlash (Bajarilmaganlar)")
        print("8. 🚪 Chiqish")
        print("-" * 42)

        tanlov = input("Tanlovingiz (1-8): ").strip()

        # 1. VAZIFA QO'SHISH
        if tanlov == "1":
            matn = input("Vazifa matnini kiriting: ").strip()
            if not matn:
                print("❌ Xatolik: Vazifa matni bo'sh bo'lishi mumkin emas!")
                continue

            print("Muhimlik darajasini tanlang:")
            print("  1. Low (Past)")
            print("  2. Medium (O'rta)")
            print("  3. High (Yuqori)")
            daraja_tanlov = input("Daraja raqamini kiriting (1-3): ").strip()

            if daraja_tanlov == "1":
                muhimlik = "Low"
            elif daraja_tanlov == "2":
                muhimlik = "Medium"
            elif daraja_tanlov == "3":
                muhimlik = "High"
            else:
                print("⚠️ Noto'g'ri tanlov! Standart bo'lib 'Medium' o'rnatildi.")
                muhimlik = "Medium"

            yangi_vazifa = {
                "matn": matn,
                "muhimlik": muhimlik,
                "bajarilgan": False,
            }
            vazifalar.append(yangi_vazifa)
            print(f"✅ Vazifa muvaffaqiyatli qo'shildi! [{muhimlik}]")

        # 2. VAZIFALARNI KO'RISH
        elif tanlov == "2":
            if not vazifalar:
                print("\n📭 Hozircha vazifalar ro'yxati bo'sh.")
                continue

            print("\n--- BARCHA VAZIFALAR ---")
            for idx, v in enumerate(vazifalar, 1):
                status = "✅ Bajarilgan" else "⏳ Bajarilmagan"
                # Yuqoridagi qatorni to'g'ri yozish uchun ternar operator:
                status_matn = "✅" if v["bajarilgan"] else "⏳"
                print(
                    f"{idx}. [{status_matn}] {v['matn']} (Muhimlik: {v['muhimlik']})"
                )

        # 3. BAJARILGANINI BELGILASH
        elif tanlov == "3":
            if not vazifalar:
                print("\n📭 Vazifalar ro'yxati bo'sh.")
                continue

            # Faqat bajarilmaganlarni ko'rsatish
            print("\n--- BAJARILMAGAN VAZIFALAR ---")
            bajarilmaganlar = [
                (i, v) for i, v in enumerate(vazifalar) if not v["bajarilgan"]
            ]

            if not bajarilmaganlar:
                print("🎉 Barcha vazifalar bajarib bo'lingan!")
                continue

            for haqiqiy_indeks, v in bajarilmaganlar:
                print(
                    f"{haqiqiy_indeks + 1}. {v['matn']} (Muhimlik: {v['muhimlik']})"
                )

            raqam_str = input(
                "\nBajarilgan deb belgilamoqchi bo'lgan vazifa raqamini kiriting: "
            ).strip()
            if raqam_str.isdigit():
                idx = int(raqam_str) - 1
                if 0 <= idx < len(vazifalar):
                    if not vazifalar[idx]["bajarilgan"]:
                        vazifalar[idx]["bajarilgan"] = True
                        print(
                            f"🎉 '{vazifalar[idx]['matn']}' bajarilgan deb belgilandi!"
                        )
                    else:
                        print("ℹ️ Bu vazifa allaqachon bajarilgan!")
                else:
                    print("❌ Bunday raqamdagi vazifa mavjud emas!")
            else:
                print("❌ Iltimos, raqam kiriting!")

        # 4. O'CHIRISH
        elif tanlov == "4":
            if not vazifalar:
                print("\n📭 O'chirish uchun vazifalar yo'q.")
                continue

            print("\n--- VAZIFALARNI O'CHIRISH ---")
            for idx, v in enumerate(vazifalar, 1):
                status_matn = "✅" if v["bajarilgan"] else "⏳"
                print(
                    f"{idx}. [{status_matn}] {v['matn']} (Muhimlik: {v['muhimlik']})"
                )

            raqam_str = input(
                "\nO'chirmoqchi bo'lgan vazifa raqamini kiriting: "
            ).strip()
            if raqam_str.isdigit():
                idx = int(raqam_str) - 1
                if 0 <= idx < len(vazifalar):
                    ochirilgan = vazifalar.pop(idx)
                    print(f"🗑️ '{ochirilgan['matn']}' vazifasi o'chirildi.")
                else:
                    print("❌ Bunday raqamdagi vazifa mavjud emas!")
            else:
                print("❌ Iltimos, raqam kiriting!")

        # 5. STATISTIKA
        elif tanlov == "5":
            jami = len(vazifalar)
            if jami == 0:
                print(
                    "\n📊 Hozircha statistika uchun ma'lumotlar mavjud emas."
                )
                continue

            # List comprehension yordamida hisoblash
            bajarilgan_soni = len([v for v in vazifalar if v["bajarilgan"]])
            qolgan_soni = jami - bajarilgan_soni
            foiz = (bajarilgan_soni / jami) * 100

            print("\n" + "=" * 30)
            print("📊 VAZIFALAR STATISTIKASI")
            print("=" * 30)
            print(f"• Jami vazifalar: {jami}")
            print(f"• Bajarilganlar: {bajarilgan_soni}")
            print(f"• Qolgan (bajarilmagan): {qolgan_soni}")
            print(f"• Bajarilish foizi: {foiz:.1f}%")
            print("=" * 30)

        # 6. SARALASH: MUHIMLIKKA QARAB
        elif tanlov == "6":
            if not vazifalar:
                print("\n📭 Saralash uchun vazifalar mavjud emas.")
                continue

            # Muhimlik darajalariga vazn beramiz: High=1, Medium=2, Low=3
            muhimlik_tartibi = {"High": 1, "Medium": 2, "Low": 3}

            # sort() funksiyasi uchun maxsus kalit (key) funksiyasi
            saralangan = sorted(
                vazifalar, key=lambda x: muhimlik_tartibi[x["muhimlik"]]
            )

            print("\n--- MUHIMLIK BO'YICHA SARALANGAN VAZIFALAR ---")
            for idx, v in enumerate(saralangan, 1):
                status_matn = "✅" if v["bajarilgan"] else "⏳"
                print(
                    f"{idx}. [{status_matn}] {v['matn']} (Muhimlik: {v['muhimlik']})"
                )

        # 7. BONUS: BAJARILMAGAN VAZIFALARNI FILTRLASH
        elif tanlov == "7":
            if not vazifalar:
                print("\n📭 Vazifalar ro'yxati bo'sh.")
                continue

            # List comprehension orqali faqat bajarilmaganlarni ajratib olamiz
            bajarilmagan_list = [v for v in vazifalar if not v["bajarilgan"]]

            if not bajarilmagan_list:
                print(
                    "\n🎉 Ajoyib! Hozirda bajarilmagan vazifalarning o'zi yo'q."
                )
            else:
                print("\n--- 🔍 BAJARILMAGAN VAZIFALAR FILTRI ---")
                for idx, v in enumerate(bajarilmagan_list, 1):
                    print(
                        f"{idx}. ⏳ {v['matn']} (Muhimlik: {v['muhimlik'])})"
                    )

        # 8. CHIQISH
        elif tanlov == "8":
            print("\nDastur tugatildi. Xayr! 👋")
            break
        else:
            print("❌ Noto'g'ri tanlov! Iltimos, 1 dan 8 gacha bo'lgan raqamni tanlang.")


if __name__ == "__main__":
    todo_app()
