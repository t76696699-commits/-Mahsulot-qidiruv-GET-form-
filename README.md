<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Loyihamiz Tariflari va Aloqa formasi</title>
</head>
<body>

    <!-- 1. Sayt Navigatsiyasi (Header va Nav) -->
    <header>
        <nav>
            <ul>
                <li><a href="#tariflarlar">Tariflarimiz</a></li>
                <li><a href="#taqqoslash">Taqqoslash Jadvali</a></li>
                <li><a href="#savol-javob">Ko'p So'raladigan Savollar</a></li>
                <li><a href="#aloqa-formasi">Ariza Qoldirish</a></li>
            </ul>
        </nav>
    </header>

    <hr>

    <!-- 2. Asosiy Kontent -->
    <main>

        <!-- Tariflar Bo'limi -->
        <section id="tariflarlar">
            <header>
                <h1>Siz uchun qulay tarif rejalari</h1>
                <p>O'zingizga mos tarifni tanlang va bugunoq ishni boshlang.</p>
            </header>

            <!-- Starter Tarif -->
            <article>
                <h2>Starter</h2>
                <p><strong>Narxi:</strong> $19 / oyiga</p>
                <p>Yangi boshlovchilar va kichik shaxsiy loyihalar uchun mo'ljallangan.</p>
                <ul>
                    <li>1 ta faol loyiha</li>
                    <li>5 GB xavfsiz xotira</li>
                    <li>Asosiy statistik ma'lumotlar</li>
                    <li>Hamjamiyat orqali yordam olish</li>
                </ul>
                <p><a href="#aloqa-formasi">Sotib olish</a></p>
            </article>

            <hr>

            <!-- Pro Tarif (Ommabop) -->
            <article>
                <h2>Pro (Eng ommabop)</h2>
                <p><strong>Narxi:</strong> $49 / oyiga</p>
                <p>Kattalashayotgan jamoalar va professional dasturchilar uchun eng yaxshi tanlov.</p>
                <ul>
                    <li>10 tagacha faol loyiha</li>
                    <li>50 GB tezkor SSD xotira</li>
                    <li>Kengaytirilgan tahlillar (Advanced Analytics)</li>
                    <li>Texnik guruh tomonidan tezkor yordam</li>
                </ul>
                <p><a href="#aloqa-formasi">Sotib olish</a></p>
            </article>

            <hr>

            <!-- Enterprise Tarif -->
            <article>
                <h2>Enterprise</h2>
                <p><strong>Narxi:</strong> $99 / oyiga</p>
                <p>Katta kompaniyalar va maxsus talablarga ega bizneslar uchun mukammal yechim.</p>
                <ul>
                    <li>Cheksiz loyihalar soni</li>
                    <li>500 GB bulutli xotira</li>
                    <li>Barcha integratsiyalar va API ulanishlar</li>
                    <li>24/7 Shaxsiy menedjer yordami</li>
                </ul>
                <p><a href="#aloqa-formasi">Biz bilan bog'lanish</a></p>
            </article>
        </section>

        <br><hr><br>

        <!-- 3. Taqqoslash Jadvali Bo'limi -->
        <section id="taqqoslash">
            <h2>Tariflar imkoniyatlarini batafsil taqqoslash</h2>
            
            <table border="1" cellpadding="10" cellspacing="0">
                <thead>
                    <tr>
                        <th>Xususiyatlar va Imkoniyatlar</th>
                        <th>Starter</th>
                        <th>Pro</th>
                        <th>Enterprise</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td><strong>Foydalanuvchilar soni</strong></td>
                        <td>1 ta</td>
                        <td>5 tagacha</td>
                        <td>Cheksiz</td>
                    </tr>
                    <tr>
                        <td><strong>API bilan ishlash</strong></td>
                        <td>Mavjud emas (✗)</td>
                        <td>Cheklangan (✓)</td>
                        <td>To'liq ruxsat (✓)</td>
                    </tr>
                    <tr>
                        <td><strong>Xavfsizlik tizimi</strong></td>
                        <td>Standart</td>
                        <td>Ikki bosqichli (2FA)</td>
                        <td>Maksimal (SSL/IP limit)</td>
                    </tr>
                    <tr>
                        <td><strong>Sinov muddati</strong></td>
                        <td>7 kun</td>
                        <td>14 kun</td>
                        <td>30 kun</td>
                    </tr>
                </tbody>
            </table>
        </section>

        <br><hr><br>

        <!-- 4. FAQ (Savol-javoblar) Bo'limi -->
        <section id="savol-javob">
            <h2>Ko'p beriladigan savollar (FAQ)</h2>

            <details>
                <summary>To'lovni amalga oshirishda qanday usullardan foydalansa bo'ladi?</summary>
                <p>Biz barcha turdagi xalqaro bank kartalari (Visa, MasterCard), PayPal hamda milliy to'lov tizimlari orqali to'lovlarni qabul qilamiz.</p>
            </details>
            <br>
            <details>
                <summary>Tarifni istalgan vaqtda o'zgartira olamanmi?</summary>
                <p>Albatta! Istalgan vaqtda shaxsiy kabinetingiz orqali tarifingizni osonlikcha o'zgartirishingiz yoki butunlay bekor qilishingiz mumkin.</p>
            </details>
            <br>
            <details>
                <summary>Qo'shimcha maxsus xotira sotib olish imkoni bormi?</summary>
                <p>Ha, agar sizga standart berilgan xotira hajmi kamlik qilsa, qo'shimcha to'lov evaziga xotirani kengaytirib olishingiz mumkin.</p>
            </details>
        </section>

        <br><hr><br>

        <!-- 5. Aloqa va Arizalar uchun Forma (Form) -->
        <section id="aloqa-formasi">
            <h2>Bizga ariza yoki savolingizni qoldiring</h2>
            
            <form action="#" method="POST">
                
                <p>
                    <label for="ism">Ismingiz (Majburiy):</label><br>
                    <input type="text" id="ism" name="foydalanuvchi_ismi" placeholder="Ismingizni kiriting" required>
                </p>

                <p>
                    <label for="email">Elektron pochta manzilingiz (Majburiy):</label><br>
                    <input type="email" id="email" name="foydalanuvchi_maili" placeholder="misol@pochta.uz" required>
                </p>

                <p>
                    <label for="tarif-tanlash">Sizga qaysi tarif ma'qul keldi?</label><br>
                    <select id="tarif-tanlash" name="tanlangan_tarif">
                        <option value="starter">Starter — $19</option>
                        <option value="pro">Pro — $49</option>
                        <option value="enterprise">Enterprise — $99</option>
                    </select>
                </p>

                <p>
                    <label for="xabar">Qo'shimcha izoh yoki xabaringiz:</label><br>
                    <textarea id="xabar" name="xabar_matni" rows="5" cols="40" placeholder="Savollaringiz bo'lsa, bu yerga yozing..."></textarea>
                </p>

                <p>
                    <input type="checkbox" id="rozilik" name="shartlarga_rozilik" required>
                    <label for="rozilik">Xizmat ko'rsatish shartlariga roziman</label>
                </p>

                <p>
                    <button type="submit">Arizani yuborish</button>
                    <button type="reset">Tozalash</button>
                </p>

            </form>
        </section>

    </main>

    <hr>

    <!-- 6. Footer (Sayt pastki qismi) -->
    <footer>
        <p>&copy; 2026 Biznes Loyihamiz. Barcha huquqlar himoyalangan.</p>
    </footer>

</body>
</html>
