index.html
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>Budjet Menejeri</title>
    <style>
        body { font-family: sans-serif; max-width: 400px; margin: 20px auto; padding: 20px; }
        .card { border: 1px solid #ccc; padding: 15px; border-radius: 8px; margin-bottom: 15px; }
        .input-group { display: flex; flex-direction: column; gap: 10px; }
        .transaction { display: flex; justify-content: space-between; padding: 5px; border-bottom: 1px solid #eee; }
        .income { color: green; }
        .expense { color: red; }
        button { cursor: pointer; }
    </style>
</head>
<body>

    <h2>Mening Budjetim</h2>
    
    <div class="card">
        <h3>Balans: <span id="balance">0</span></h3>
        <p>Daromad: <span id="total-income" class="income">0</span></p>
        <p>Xarajat: <span id="total-expense" class="expense">0</span></p>
    </div>

    <div class="input-group">
        <input type="text" id="desc" placeholder="Tavsif (masalan: Ish haqi)">
        <input type="number" id="amount" placeholder="Summa">
        <select id="type">
            <option value="income">Daromad</option>
            <option value="expense">Xarajat</option>
        </select>
        <button onclick="addTransaction()">Qo'shish</button>
    </div>

    <h3>Tranzaksiyalar</h3>
    <div id="list"></div>

    <script>
        let transactions = JSON.parse(localStorage.getItem('transactions')) || [];

        // 1. Hisoblash logikasi
        function updateUI() {
            const list = document.getElementById('list');
            const balanceEl = document.getElementById('balance');
            const incomeEl = document.getElementById('total-income');
            const expenseEl = document.getElementById('total-expense');

            list.innerHTML = '';
            let income = 0, expense = 0;

            transactions.forEach((t, index) => {
                if (t.type === 'income') income += t.amount;
                else expense += t.amount;

                list.innerHTML += `
                    <div class="transaction ${t.type}">
                        <span>${t.desc} (${t.amount})</span>
                        <button onclick="deleteTransaction(${index})">❌</button>
                    </div>
                `;
            });

            balanceEl.innerText = income - expense;
            incomeEl.innerText = income;
            expenseEl.innerText = expense;
            localStorage.setItem('transactions', JSON.stringify(transactions));
        }

        // 2. Tranzaksiya qo'shish
        function addTransaction() {
            const desc = document.getElementById('desc').value;
            const amount = parseFloat(document.getElementById('amount').value);
            const type = document.getElementById('type').value;

            if (!desc || !amount) return alert("Barcha maydonlarni to'ldiring!");

            transactions.push({ desc, amount, type });
            updateUI();
        }

        // 3. O'chirish
        function deleteTransaction(index) {
            transactions.splice(index, 1);
            updateUI();
        }

        updateUI();
    </script>
</body>
</html>
