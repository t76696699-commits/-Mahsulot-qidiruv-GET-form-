from flask import Flask, render_template, request

app = Flask(__name__)

# GLOBAL AI BAZASI: Narxlar 50 dollarga arzonlashtirildi!
AI_NARXLAR_BAZASI = {
    # Apple (50$ arzonlashdi)
    "iphone_11_promax": {"chiroyli_nomi": "Apple iPhone 11 Pro Max", "narx": 330.0},
    "iphone_12_promax": {"chiroyli_nomi": "Apple iPhone 12 Pro Max", "narx": 450.0},
    "iphone_13_promax": {"chiroyli_nomi": "Apple iPhone 13 Pro Max", "narx": 600.0},
    "iphone_14_promax": {"chiroyli_nomi": "Apple iPhone 14 Pro Max", "narx": 750.0},
    "iphone_15_promax": {"chiroyli_nomi": "Apple iPhone 15 Pro Max", "narx": 1000.0},
    "iphone_16_promax": {"chiroyli_nomi": "Apple iPhone 16 Pro Max", "narx": 1200.0},
    "iphone_17_promax": {"chiroyli_nomi": "Apple iPhone 17 Pro Max (2026)", "narx": 1400.0},

    # Samsung (50$ arzonlashdi)
    "samsung_s20_ultra": {"chiroyli_nomi": "Samsung Galaxy S20 Ultra", "narx": 330.0},
    "samsung_s21_ultra": {"chiroyli_nomi": "Samsung Galaxy S21 Ultra", "narx": 400.0},
    "samsung_s22_ultra": {"chiroyli_nomi": "Samsung Galaxy S22 Ultra", "narx": 530.0},
    "samsung_s23_ultra": {"chiroyli_nomi": "Samsung Galaxy S23 Ultra", "narx": 700.0},
    "samsung_s24_ultra": {"chiroyli_nomi": "Samsung Galaxy S24 Ultra", "narx": 900.0},
    "samsung_s25_ultra": {"chiroyli_nomi": "Samsung Galaxy S25 Ultra", "narx": 1100.0},
    "samsung_s26_ultra": {"chiroyli_nomi": "Samsung Galaxy S26 Ultra (2026)", "narx": 1300.0},

    # Xiaomi (50$ arzonlashdi)
    "xiaomi_10_ultra": {"chiroyli_nomi": "Xiaomi Mi 10 Ultra", "narx": 210.0},
    "xiaomi_11_ultra": {"chiroyli_nomi": "Xiaomi Mi 11 Ultra", "narx": 300.0},
    "xiaomi_12_pro": {"chiroyli_nomi": "Xiaomi 12 Pro", "narx": 270.0},
    "xiaomi_13_ultra": {"chiroyli_nomi": "Xiaomi 13 Ultra", "narx": 500.0},
    "xiaomi_14_ultra": {"chiroyli_nomi": "Xiaomi 14 Ultra", "narx": 700.0},
    "redmi_note_14_pro": {"chiroyli_nomi": "Redmi Note 14 Pro+", "narx": 300.0},
    "xiaomi_16_ultra": {"chiroyli_nomi": "Xiaomi 16 Ultra (2026)", "narx": 950.0},

    # Google Pixel (50$ arzonlashdi)
    "pixel_4_xl": {"chiroyli_nomi": "Google Pixel 4 XL", "narx": 90.0},
    "pixel_5_xl": {"chiroyli_nomi": "Google Pixel 5 XL", "narx": 150.0},
    "pixel_6_pro": {"chiroyli_nomi": "Google Pixel 6 Pro", "narx": 230.0},
    "pixel_7_pro": {"chiroyli_nomi": "Google Pixel 7 Pro", "narx": 330.0},
    "pixel_8_pro": {"chiroyli_nomi": "Google Pixel 8 Pro", "narx": 500.0},
    "pixel_9_pro": {"chiroyli_nomi": "Google Pixel 9 Pro XL", "narx": 750.0},
    "pixel_10_pro": {"chiroyli_nomi": "Google Pixel 10 Pro XL (2026)", "narx": 1000.0},

    # OnePlus & Poco (50$ arzonlashdi)
    "oneplus_8_pro": {"chiroyli_nomi": "OnePlus 8 Pro", "narx": 150.0},
    "oneplus_9_pro": {"chiroyli_nomi": "OnePlus 9 Pro", "narx": 220.0},
    "oneplus_10_pro": {"chiroyli_nomi": "OnePlus 10 Pro", "narx": 300.0},
    "oneplus_11_pro": {"chiroyli_nomi": "OnePlus 11 Premium", "narx": 430.0},
    "oneplus_12_pro": {"chiroyli_nomi": "OnePlus 12 Premium", "narx": 550.0},
    "oneplus_14_pro": {"chiroyli_nomi": "OnePlus 14 Pro (2026)", "narx": 800.0},
    "poco_f6_pro": {"chiroyli_nomi": "Poco F6 Pro", "narx": 430.0},

    # Gaming (50$ arzonlashdi)
    "asus_rog_3": {"chiroyli_nomi": "Asus ROG Phone 3 Strix", "narx": 200.0},
    "asus_rog_5": {"chiroyli_nomi": "Asus ROG Phone 5 Ultimate", "narx": 330.0},
    "asus_rog_6": {"chiroyli_nomi": "Asus ROG Phone 6 Pro", "narx": 430.0},
    "asus_rog_8": {"chiroyli_nomi": "Asus ROG Phone 8 Edition", "narx": 800.0},
    "asus_rog_10": {"chiroyli_nomi": "Asus ROG Phone 10 Ultimate (2026)", "narx": 1150.0},
    "redmagic_9_pro": {"chiroyli_nomi": "Nubia RedMagic 9 Pro+", "narx": 650.0},
    "redmagic_11_pro": {"chiroyli_nomi": "Nubia RedMagic 11 Pro+ (2026)", "narx": 900.0},

    # Nothing & Infinix (50$ arzonlashdi)
    "nothing_phone_1": {"chiroyli_nomi": "Nothing Phone (1)", "narx": 230.0},
    "nothing_phone_2": {"chiroyli_nomi": "Nothing Phone (2)", "narx": 400.0},
    "nothing_phone_3": {"chiroyli_nomi": "Nothing Phone (3) (2026)", "narx": 600.0},
    "infinix_zero_8": {"chiroyli_nomi": "Infinix Zero 8i", "narx": 80.0},
    "infinix_zero_30": {"chiroyli_nomi": "Infinix Zero 30 5G", "narx": 170.0},
    "infinix_gt_20": {"chiroyli_nomi": "Infinix GT 20 Pro", "narx": 240.0},
    "infinix_gt_ultra": {"chiroyli_nomi": "Infinix GT Ultra Flagman", "narx": 350.0}
}


@app.route('/', methods=['GET', 'POST'])
def kalkulyator():
    if request.method == 'GET':
        return render_template('index.html', natija=None)

    if request.method == 'POST':
        model_kalit = request.form.get('nomi')
        karobka_holati = request.form.get('karobka')
        ekran_holati = request.form.get('ekran')

        telefon_ma_lumoti = AI_NARXLAR_BAZASI.get(model_kalit)

        if not telefon_ma_lumoti:
            return "Xatolik: Model topilmadi!", 400

        real_nomi = telefon_ma_lumoti["chiroyli_nomi"]
        bozor_narxi = telefon_ma_lumoti["narx"]

        # Flagmanlar uchun maxsus AI narx tushirish mantiqi
        yakuniy_narx = bozor_narxi

        # Karobka va pasport bo'lmasa, qimmat telefonlar ko'proq narx yo'qotadi (shubhali bo'lgani uchun)
        if karobka_holati == 'yoq':
            yakuniy_narx = yakuniy_narx * 0.80  # 20% chegirildi!

        # Ekran qimmat flagmanlarda juda qimmat turadi, shuning uchun foizda yondashamiz
        if ekran_holati == 'qirilgan':
            yakuniy_narx = yakuniy_narx - (bozor_narxi * 0.08)  # 8% ayiriladi
        elif ekran_holati == 'siniq':
            yakuniy_narx = yakuniy_narx - (bozor_narxi * 0.25)  # 25% ayiriladi (Flagman ekrani juda qimmat!)

        yakuniy_narx = round(yakuniy_narx, 2)

        # Faqat return qismini mana buga almashtiring:
        return render_template('index.html',
                               natija=True,
                               nomi=real_nomi,
                               original_narx=bozor_narxi,
                               narx=yakuniy_narx,
                               sel_model=model_kalit,
                               sel_karobka=karobka_holati,
                               sel_ekran=ekran_holati)


if __name__ == '__main__':
    app.run(debug=True)



<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>AI Flagman Narxini Baholash</title>
    <style>
        body { font-family: 'Segoe UI', sans-serif; background-color: #f8f9fa; padding: 30px; color: #333; }
        .forma-quti { background: white; padding: 25px; border-radius: 12px; max-width: 550px; margin: 0 auto; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
        h2 { text-align: center; color: #1e293b; margin-bottom: 25px; }
        p { margin-bottom: 20px; }
        label { font-weight: 600; color: #475569; }
        .btn { width: 100%; padding: 12px; background-color: #2563eb; color: white; border: none; border-radius: 6px; font-size: 16px; font-weight: bold; cursor: pointer; }
        .btn:hover { background-color: #1d4ed8; }
        .natija-quti { background-color: #f0fdf4; border-left: 5px solid #16a34a; color: #14532d; padding: 20px; margin-top: 25px; border-radius: 8px; }
    </style>
</head>
<body>

    <div class="forma-quti">
        <h2>AI Tizim: Flagmanlarni Baholash</h2>

        <form action="/" method="POST">
            <!-- 1. Elita Telefonlar Ro'yxati -->
            <p>
                <label for="nomi">Telefoningiz modelini tanlang:</label><br>
                <select id="nomi" name="nomi" style="padding: 10px; width: 100%; border-radius: 6px; font-size: 15px; border: 1px solid #cbd5e1;">
                    <optgroup label="Apple iPhone (Faqat eng yuqori Max-lar)">
                        <option value="iphone_11_promax">Apple iPhone 11 Pro Max</option>
                        <option value="iphone_12_promax">Apple iPhone 12 Pro Max</option>
                        <option value="iphone_13_promax">Apple iPhone 13 Pro Max</option>
                        <option value="iphone_14_promax">Apple iPhone 14 Pro Max</option>
                        <option value="iphone_15_promax">Apple iPhone 15 Pro Max</option>
                        <option value="iphone_16_promax">Apple iPhone 16 Pro Max</option>
                        <option value="iphone_17_promax">Apple iPhone 17 Pro Max (2026 Yangi)</option>
                    </optgroup>
                    <optgroup label="Samsung Galaxy (Faqat eng kuchli Ultra-lar)">
                        <option value="samsung_s20_ultra">Samsung Galaxy S20 Ultra</option>
                        <option value="samsung_s21_ultra">Samsung Galaxy S21 Ultra</option>
                        <option value="samsung_s22_ultra">Samsung Galaxy S22 Ultra</option>
                        <option value="samsung_s23_ultra">Samsung Galaxy S23 Ultra</option>
                        <option value="samsung_s24_ultra">Samsung Galaxy S24 Ultra</option>
                        <option value="samsung_s25_ultra">Samsung Galaxy S25 Ultra</option>
                        <option value="samsung_s26_ultra">Samsung Galaxy S26 Ultra (2026 Kosmos)</option>
                    </optgroup>
                    <optgroup label="Xiaomi & Redmi (Faqat Top Flagmanlar)">
                        <option value="xiaomi_10_ultra">Xiaomi Mi 10 Ultra</option>
                        <option value="xiaomi_11_ultra">Xiaomi Mi 11 Ultra</option>
                        <option value="xiaomi_12_pro">Xiaomi 12 Pro</option>
                        <option value="xiaomi_13_ultra">Xiaomi 13 Ultra</option>
                        <option value="xiaomi_14_ultra">Xiaomi 14 Ultra</option>
                        <option value="redmi_note_14_pro">Redmi Note 14 Pro+</option>
                        <option value="xiaomi_16_ultra">Xiaomi 16 Ultra (2026 Masterpiece)</option>
                    </optgroup>
                    <optgroup label="Google Pixel (Toza Android Qirollari)">
                        <option value="pixel_4_xl">Google Pixel 4 XL</option>
                        <option value="pixel_5_xl">Google Pixel 5 XL</option>
                        <option value="pixel_6_pro">Google Pixel 6 Pro</option>
                        <option value="pixel_7_pro">Google Pixel 7 Pro</option>
                        <option value="pixel_8_pro">Google Pixel 8 Pro</option>
                        <option value="pixel_9_pro">Google Pixel 9 Pro XL</option>
                        <option value="pixel_10_pro">Google Pixel 10 Pro XL (2026)</option>
                    </optgroup>
                    <optgroup label="OnePlus & Poco (Tezkorlik Maxluqlari)">
                        <option value="oneplus_8_pro">OnePlus 8 Pro</option>
                        <option value="oneplus_9_pro">OnePlus 9 Pro</option>
                        <option value="oneplus_10_pro">OnePlus 10 Pro</option>
                        <option value="oneplus_11_pro">OnePlus 11 Premium</option>
                        <option value="oneplus_12_pro">OnePlus 12 Premium</option>
                        <option value="oneplus_14_pro">OnePlus 14 Pro (2026)</option>
                        <option value="poco_f6_pro">Poco F6 Pro</option>
                    </optgroup>
                    <optgroup label="Asus ROG & RedMagic (Cheksiz Geiming)">
                        <option value="asus_rog_3">Asus ROG Phone 3 Strix</option>
                        <option value="asus_rog_5">Asus ROG Phone 5 Ultimate</option>
                        <option value="asus_rog_6">Asus ROG Phone 6 Pro</option>
                        <option value="asus_rog_8">Asus ROG Phone 8 Edition</option>
                        <option value="asus_rog_10">Asus ROG Phone 10 Ultimate (2026)</option>
                        <option value="redmagic_9_pro">Nubia RedMagic 9 Pro+</option>
                        <option value="redmagic_11_pro">Nubia RedMagic 11 Pro+ (2026)</option>
                    </optgroup>
                    <optgroup label="Nothing & Infinix (Trenddagi Modellar)">
                        <option value="nothing_phone_1">Nothing Phone (1)</option>
                        <option value="nothing_phone_2">Nothing Phone (2)</option>
                        <option value="nothing_phone_3">Nothing Phone (3) (2026)</option>
                        <option value="infinix_zero_8">Infinix Zero 8i</option>
                        <option value="infinix_zero_30">Infinix Zero 30 5G</option>
                        <option value="infinix_gt_20">Infinix GT 20 Pro</option>
                        <option value="infinix_gt_ultra">Infinix GT Ultra Flagman</option>
                    </optgroup>
                </select>
            </p>

            <!-- 2. Karobka so'rovi -->
            <p>
                <label>Karobka va hujjatlari bormi?</label><br>
                <input type="radio" id="karobka_bor" name="karobka" value="bor" checked>
                <label for="karobka_bor" style="font-weight: normal;">Ha, hamma narsasi joyida</label><br>
                <input type="radio" id="karobka_yoq" name="karobka" value="yoq">
                <label for="karobka_yoq" style="font-weight: normal;">Yo'q, faqat telefonning o'zi</label>
            </p>

            <!-- 3. Ekran holati -->
            <p>
                <label for="ekran">Ekran holati qanday?</label><br>
                <select id="ekran" name="ekran" style="padding: 10px; width: 100%; border-radius: 6px; font-size: 15px; border: 1px solid #cbd5e1;">
                    <option value="toza">Mutloqo toza (Ideal)</option>
                    <option value="qirilgan">Yengil qirilgan joylari bor</option>
                    <option value="siniq">Siniq yoki dars ketgan (Ekran almashishi kerak)</option>
                </select>
            </p>

            <button type="submit" class="btn">AI Orqali Narxni Baholash</button>
        </form>

        <!-- Natija bloki -->
        {% if natija %}
            <div class="natija-quti">
                <h3>AI tahlil natijasi:</h3>
                <p><strong>Model:</strong> {{ nomi }}</p>
                <p><strong>Bozordagi real original narxi:</strong> {{ original_narx }} $</p>
                <p><strong>Sizning telefoningiz baholandi:</strong> <span style="font-size: 22px; font-weight: bold; color: #16a34a;">{{ narx }} $</span></p>
            </div>
        {% endif %}
    </div>

    <!-- Eski holatni tiklash uchun xavfsiz JavaScript -->
    {% if natija %}
    <script>
        if ("{{ sel_model }}") document.getElementById('nomi').value = "{{ sel_model }}";
        if ("{{ sel_ekran }}") document.getElementById('ekran').value = "{{ sel_ekran }}";
        if ("{{ sel_karobka }}" === "yoq") {
            document.getElementById('karobka_yoq').checked = true;
        } else {
            document.getElementById('karobka_bor').checked = true;
        }
    </script>
    {% endif %}

</body>
</html>

blinker==1.9.0
click==8.4.2
colorama==0.4.6
Flask==3.1.3
gunicorn==26.0.0
itsdangerous==2.2.0
Jinja2==3.1.6
MarkupSafe==3.0.3
packaging==26.2
setuptools==65.5.1
Werkzeug==3.1.8
wheel==0.38.4
