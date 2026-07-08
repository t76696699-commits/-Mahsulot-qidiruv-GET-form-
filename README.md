<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML Display turlari: Block, Inline, Inline-Block</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            margin: 20px;
            background-color: #f9f9f9;
            color: #333;
        }
        
        .section {
            background-color: #ffffff;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            padding: 20px;
            margin-bottom: 30px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        }

        h3 {
            color: #2c3e50;
            margin-top: 0;
            border-bottom: 2px solid #3498db;
            padding-bottom: 5px;
        }

        p.description {
            color: #7f8c8d;
            font-style: italic;
            margin-bottom: 15px;
        }

        /* Block elementlar stili */
        .block-element {
            background-color: #e74c3c;
            color: white;
            padding: 10px;
            margin: 10px 0;
            border-radius: 4px;
            font-weight: bold;
        }

        /* Inline elementlar stili */
        .inline-element {
            background-color: #2ecc71;
            color: white;
            padding: 5px 10px;
            margin: 0 5px;
            border-radius: 4px;
            font-weight: bold;
        }

        /* Inline-block elementlar stili */
        .inline-block-element {
            display: inline-block;
            background-color: #f1c40f;
            color: #2c3e50;
            width: 150px;
            height: 80px;
            padding: 10px;
            margin: 10px 5px;
            border-radius: 4px;
            font-weight: bold;
            box-sizing: border
