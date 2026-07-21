def talaba_bahlari_boshqaruvchisi():
    print("--- TALABA BAHLARI BOSHQARUVCHISI ---")
    print("Iltimos, kamida 5 ta bahoni kiriting (0 dan 100 gacha).")
    print("Tugatish uchun 'stop' deb yozing (kamida 5 ta kiritilgandan keyin).\n")

    baholar = []

    # 1. Baholarni qabul qilish va validatsiya qilish
    while True:
        kiritish = input(
            f"{len(baholar) + 1}-bahoni kiriting (yoki 'stop'): "
        ).strip()

        if kiritish.lower() == "stop":
            if len(baholar) >= 5:
                break
            else:
                print(
                    f"❌ Xatolik: Hali atigi {len(baholar)} ta baho kiritildi. Kamida 5 ta bo'lishi shart!"
                )
                continue

        # Raqam ekanligini tekshirish
        if not kiritish.isdigit():
            print(
                "❌ Xatolik: Iltimos, faqat 0-100 oralig'idagi butun son kiriting!"
            )
            continue

        baho = int(kiritish)

        # 0-100 oralig'i validatsiyasi
        if 0 <= baho <= 100:
            baholar.append(baho)
            print(f"✅ Qo'shildi: {baho}")
        else:
            print("❌ Xatolik: Baho 0 va 100 orasida bo'lishi kerak!")

    # 2. Baholarni oshib boruvchi tartibda tartiblash
    baholar.sort()

    print("\n" + "=" * 40)
    print("📊 NATIJALAR VA STATISTIKA")
    print("=" * 40)
    print(f"Kiritilgan baholar (tartiblangan): {baholar}")

    # 3. Statistikani hisoblash (sum, max, min)
    jami_baho = sum(baholar)
    soni = len(baholar)
    ortacha = jami_baho / soni
    eng_yuqori = max(baholar)
    eng_past = min(baholar)

    # Mediana hisoblash
    if soni % 2 == 1:
        mediana = baholar[soni // 2]
    else:
        orta_1 = baholar[(soni // 2) - 1]
        orta_2 = baholar[soni // 2]
        mediana = (orta_1 + orta_2) / 2

    print(f"• O'rtacha baho: {ortacha:.2f}")
    print(f"• Eng yuqori baho: {eng_yuqori}")
    print(f"• Eng past baho: {eng_past}")
    print(f"• Mediana: {mediana}")

    # 4. 60 dan kam baholarni ajratish ('qoniqarsiz')
    qoniqarsizlar = [b for b in baholar if b < 60]
    if qoniqarsizlar:
        print(f"• Qoniqarsiz baholar (<60): {qoniqarsizlar}")
    else:
        print("• Qoniqarsiz baholar (<60): Mavjud emas (Barakalla! 🎉)")

    # 5. List comprehension yordamida A'lo baholarni (90+) filtrlash
    alo_baholar = [b for b in baholar if b >= 90]
    print(f"• A'lo baholar (90+): {alo_baholar}")
    print("=" * 40)


# O'yin/dasturni ishga tushirish
if __name__ == "__main__":
    talaba_bahlari_boshqaruvchisi()
