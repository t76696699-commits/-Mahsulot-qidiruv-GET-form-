# tovarlar_royxati.py — Modul 2 takrorlash loyihasi
# Menu pattern + if/elif/else + while + list

tovarlar = []

print("🛒 Tovarlar ro'yxati ilovasi")
print("=" * 40)

while True:
    print()
    print("1. Tovar qo'shish")
    print("2. Tovar o'chirish")
    print("3. Hammasini ko'rsatish")
    print("4. Alifbo tartibida saralash")
    print("5. Qidirish")
    print("6. Chiqish")

    tanlov = input("Tanlov (1-6): ").strip()

    if tanlov == "1":
        nom = input("Tovar nomi: ").strip()
        if not nom:
            print("⚠️ Bo'sh nom bo'lmaydi")
        elif nom.lower() in [t.lower() for t in tovarlar]:
            print(f"⚠️ '{nom}' allaqachon ro'yxatda")
        else:
            tovarlar.append(nom)
            print(f"✅ '{nom}' qo'shildi")

    elif tanlov == "2":
        if not tovarlar:
            print("⚠️ Ro'yxat bo'sh")
            continue
        nom = input("O'chirilsin: ").strip()
        # Case-insensitive remove
        for t in tovarlar:
            if t.lower() == nom.lower():
                tovarlar.remove(t)
                print(f"🗑 '{t}' o'chirildi")
                break
        else:
            print(f"⚠️ '{nom}' topilmadi")

    elif tanlov == "3":
        if not tovarlar:
            print("Ro'yxat bo'sh — birinchi tovarni qo'shing")
        else:
            print(f"\n📋 Jami {len(tovarlar)} ta tovar:")
            for i, t in enumerate(tovarlar, start=1):
                print(f"  {i}. {t}")

    elif tanlov == "4":
        tovarlar.sort(key=str.lower)
        print("🔤 Alifbo tartibida saralandi")

    elif tanlov == "5":
        if not tovarlar:
            print("Ro'yxat bo'sh")
            continue
        kalit = input("Qidirilsin: ").strip().lower()
        topildi = [t for t in tovarlar if kalit in t.lower()]
        if topildi:
            print(f"🔍 {len(topildi)} ta natija:")
            for t in topildi:
                print(f"  • {t}")
        else:
            print(f"'{kalit}' uchun hech narsa topilmadi")

    elif tanlov == "6":
        print(f"\nYakuniy ro'yxat ({len(tovarlar)} ta): {tovarlar}")
        print("Xayr!")
        break

    else:
        print(f"⚠️ Noma'lum tanlov: {tanlov!r}")
