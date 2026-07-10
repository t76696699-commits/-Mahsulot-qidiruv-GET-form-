<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML va CSS Jadvallar</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <div class="container">
        
        <table class="custom-table simple-table">
            <caption>Talabalar roʻyxati (Oddiy jadval)</caption>
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Ism va Familiya</th>
                    <th>Yoʻnalish</th>
                    <th>Stipendiya</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td>#1001</td>
                    <td>Asrorbek Olimov</td>
                    <td>Dasturlash</td>
                    <td>750,000 so'm</td>
                </tr>
                <tr>
                    <td>#1002</td>
                    <td>Dilnoza Toirova</td>
                    <td>Grafik Dizayn</td>
                    <td>800,000 so'm</td>
                </tr>
                <tr>
                    <td>#1003</td>
                    <td>Sardor Rahimov</td>
                    <td>Ma'lumotlar tahlili</td>
                    <td>750,000 so'm</td>
                </tr>
                <tr>
                    <td>#1004</td>
                    <td>Madina Axmedova</td>
                    <td>Kiberxavfsizlik</td>
                    <td>900,000 so'm</td>
                </tr>
            </tbody>
        </table>

        <br><br><hr><br><br>

        <table class="custom-table complex-table">
            <caption>Haftalik Dars Jadvali va Xonalar (Murakkab jadval)</caption>
            <thead>
                <tr>
                    <th>Kun</th>
                    <th>Vaqt</th>
                    <th>Fan</th>
                    <th>O'qituvchi</th>
                    <th>Xona</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td rowspan="2" class="day-cell">Dushanba</td>
                    <td>09:00 - 10:30</td>
                    <td>HTML/CSS Asoslari</td>
                    <td>Anvar Shokirov</td>
                    <td>305-xona</td>
                </tr>
                <tr>
                    <td>11:00 - 12:30</td>
                    <td>JavaScript Intensive</td>
                    <td>Malika Rixsiyeva</td>
                    <td>Mavjud emas (Online)</td>
                </tr>
                <tr>
                    <td rowspan="2" class="day-cell">Seshanba</td>
                    <td>09:00 - 10:30</td>
                    <td>Ma'lumotlar Bazasi</td>
                    <td>Bobur Mansurov</td>
                    <td>102-lab</td>
                </tr>
                <tr>
                    <td>11:00 - 12:30</td>
                    <td colspan="3" class="break-cell">Mustaqil ish va Amaliyot vaqti</td>
                </tr>
                <tr>
                    <td class="day-cell">Chorshanba</td>
                    <td>14:00 - 15:30</td>
                    <td>UX/UI Dizayn</td>
                    <td>Elena Pak</td>
                    <td>201-xona</td>
                </tr>
            </tbody>
        </table>

    </div>

</body>
</html>
