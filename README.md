# 1. Foydalanuvchidan matn kiritish va strip() orqali ortiqcha bo'shliqlarni olib tashlash
matn = input("Matn kiriting: ").strip()

# Bo'sh matn holatini tekshirish
if not matn:
    print("Xatolik: Hech qanday matn kiritilmadi!")
else:
    # 2. String metodlarini qo'llash (kamida 5 ta: upper, lower, replace, strip, count)
    katta_harf = matn.upper()
    kichik_harf = matn.lower()
    almashtirilgan = matn.replace("a", "@").replace("A", "@")
    bosh_oxir_kesilgan = matn.strip()
    belgi_soni_e = matn.lower().count('e')

    # 3. Matn uzunligi va so'zlar sonini aniqlash
    uzunlik = len(matn)
    sozlar = matn.split()
    soz_soni = len(sozlar)

    # 4. Slicing orqali matnning bir qismini olish (masalan, birinchi 10 ta belgi)
    qism = matn[:10]

    # 5. f-string yordamida chiroyli natija chiqarish
    print("\n" + "=" * 40)
    print("MATN TAHLILI NATIJALARI")
    print("=" * 40)
    print(f"Asliy matn:              {matn}")
    print(f"Matn uzunligi:           {uzunlik} ta belgi")
    print(f"So'zlar soni:            {soz_soni} ta")
    print(f"Boshlang'ich 10 belgi:   {qism}")
    print("-" * 40)
    print(f"Barchasi katta harfda:   {katta_harf}")
    print(f"Barchasi kichik harfda:  {kichik_harf}")
    print(f"'a' harfini '@' ga almashtirib: {almashtirilgan}")
    print(f"'e' harfi qatnashish soni: {belgi_soni_e} ta")
    print("=" * 40)
