<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Faqat CSS Bezaklari</title>
    <style>
        /* Umumiy sahifa ko'rinishi */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            max-width: 800px;
            margin: 40px auto;
            padding: 0 20px;
            color: #333;
            line-height: 1.6;
        }

        h2 {
            color: #2c3e50;
            border-bottom: 2px solid #ecf0f1;
            padding-bottom: 8px;
            margin-top: 40px;
        }

        /* 1. CUSTOM LIST MARKER CSS */
        ul {
            list-style: none;
            padding-left: 0;
        }

        ul li {
            position: relative;
            padding-left: 25px;
            margin-bottom: 12px;
        }

        ul li::before {
            content: "✦"; /* Maxsus belgi */
            position: absolute;
            left: 0;
            top: 0;
            color: #e67e22; /* To'q sariq rang */
            font-weight: bold;
        }

        /* 2. BLOCKQUOTE TIRNOQ BEZAGI CSS */
        blockquote {
            position: relative;
            padding: 30px 40px;
            margin: 30px 0;
            font-style: italic;
            font-size: 1.15rem;
            background-color: #f8f9fa;
            border-left: 5px solid #3498db;
        }

        blockquote::before {
            content: "“";
            position: absolute;
            left: 10px;
            top: -10px;
            font-size: 5rem;
            font-family: serif;
            color: #3498db;
            opacity: 0.3;
            line-height: 1;
        }

        blockquote::after {
            content: "”";
            position: absolute;
            right: 15px;
            bottom: -40px;
            font-size: 5rem;
            font-family: serif;
            color: #3498db;
            opacity: 0.3;
            line-height: 1;
        }

        /* 3. JADVAL :NTH-CHILD CSS */
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
        }

        th, td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid #e2e8f0;
        }

        th {
            background-color: #3498db;
            color: white;
        }

        /* Juft qatorlarni (2, 4, 6...) ranglash */
        tr:nth-child(even) {
            background-color: #f1f5f9;
        }

        /* Sichqoncha kelganda effekt */
        tr:hover {
            background-color: #e2e8f0;
            transition: background 0.2s ease;
        }
    </style>
</head>
<body>

    <h2>1. Custom List Marker (::before)</h2>
    <ul>
        <li>HTML va CSS yordamida veb-sahifalar yaratish</li>
        <li>JavaScript bilan dinamik elementlarni boshqarish</li>
        <li>Toza va tushunarli kod yozish qoidalari</li>
    </ul>

    <h2>2. Blockquote Tirnoq Bezagi (::before & ::after)</h2>
    <blockquote>
        Muvaffaqiyatli kod ortida har doim sabr, diqqat va o'nlab chashka qahva turadi. Eng muhimi, HTML tuzilishiga tegmasdan faqat CSS bilan ham ajoyib natijalarga erishish mumkin.
    </blockquote>

    <h2>3. Jadval Qatorlari (::nth-child)</h2>
    <table>
        <thead>
            <tr>
                <th>Texnologiya</th>
                <th>Vazifasi</th>
                <th>Murakkabligi</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>HTML</td>
                <td>Sahifa skleti (tuzilishi)</td>
                <td>Oson</td>
            </tr>
            <tr>
                <td>CSS</td>
                <td>Tashqi ko'rinish va dizayn</td>
                <td>O'rtacha</td>
            </tr>
            <tr>
                <td>JavaScript</td>
                <td>Dinamika va interaktivlik</td>
                <td>Murakkab</td>
            </tr>
            <tr>
                <td>React</td>
                <td>Komponentli UI interfeyslar</td>
                <td>Murakkab</td>
            </tr>
        </tbody>
    </table>

</body>
</html>
