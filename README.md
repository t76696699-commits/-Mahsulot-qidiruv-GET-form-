1. HTML (index.html)
Forma va jadval tuzilmasi:

HTML
<div class="container">
    <h2>Ma'lumotlar bazasi</h2>
    
    <!-- Forma -->
    <form id="dataForm">
        <input type="hidden" id="editIndex">
        <input type="text" id="name" placeholder="Ism" required>
        <input type="email" id="email" placeholder="Email" required>
        <button type="submit" id="submitBtn">Qo'shish</button>
    </form>

    <!-- Jadval -->
    <table id="dataTable">
        <thead>
            <tr><th>Ism</th><th>Email</th><th>Amallar</th></tr>
        </thead>
        <tbody id="tableBody"></tbody>
    </table>
    <div id="emptyMsg" style="display:none; text-align:center; padding: 20px;">Jadval bo'sh</div>
</div>
2. JavaScript (script.js)
Logika, tahrirlash va localStorage bilan ishlash:

JavaScript
let data = JSON.parse(localStorage.getItem('formData')) || [];
const form = document.getElementById('dataForm');
const tableBody = document.getElementById('tableBody');
const emptyMsg = document.getElementById('emptyMsg');

function renderTable() {
    tableBody.innerHTML = '';
    
    if (data.length === 0) {
        emptyMsg.style.display = 'block';
    } else {
        emptyMsg.style.display = 'none';
        data.forEach((item, index) => {
            tableBody.innerHTML += `
                <tr>
                    <td>${item.name}</td>
                    <td>${item.email}</td>
                    <td>
                        <button onclick="editItem(${index})">Tahrirlash</button>
                        <button onclick="deleteItem(${index})">O'chirish</button>
                    </td>
                </tr>
            `;
        });
    }
}

// Qo'shish va Tahrirlash
form.addEventListener('submit', (e) => {
    e.preventDefault();
    const index = document.getElementById('editIndex').value;
    const newData = {
        name: document.getElementById('name').value,
        email: document.getElementById('email').value
    };

    if (index === "") {
        data.push(newData);
    } else {
        data[index] = newData;
        document.getElementById('submitBtn').innerText = "Qo'shish";
        document.getElementById('editIndex').value = "";
    }
    
    saveAndRender();
    form.reset();
});

// Tahrirlashga yuklash
window.editItem = (index) => {
    document.getElementById('name').value = data[index].name;
    document.getElementById('email').value = data[index].email;
    document.getElementById('editIndex').value = index;
    document.getElementById('submitBtn').innerText = "Yangilash";
};

// O'chirish
window.deleteItem = (index) => {
    if (confirm("Rostdan ham o'chirmoqchimisiz?")) {
        data.splice(index, 1);
        saveAndRender();
    }
};

function saveAndRender() {
    localStorage.setItem('formData', JSON.stringify(data));
    renderTable();
}

renderTable();
3. CSS (Qisqacha tavsiya)
Jadval va formani chiroyli ko'rsatish uchun:

CSS
table { width: 100%; border-collapse: collapse; margin-top: 20px; }
th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
form { display: flex; gap: 10px; margin-bottom: 20px; }
button { cursor: pointer; }
