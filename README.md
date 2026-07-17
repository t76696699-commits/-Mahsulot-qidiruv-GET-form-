// ===================================================
// WeakMap va WeakSet - Amaliy misollar
// ===================================================

// 1. WeakMap asosiy ishlatish
const weakMap = new WeakMap();
let user = { name: 'Alisher', age: 25 };

// Ob'ektni kalit, ma'lumotni qiymat sifatida saqlash
weakMap.set(user, { lastLogin: new Date(), visits: 42 });

console.log(weakMap.get(user));  // { lastLogin: ..., visits: 42 }
console.log(weakMap.has(user));  // true

// Havola o'chirilgach GC WeakMap'ni ham tozalaydi
user = null;
// Endi GC ishga tushganda yozuv ham o'chadi

// 2. WeakMap bilan Private ma'lumotlar
const _privateData = new WeakMap();

class BankAccount {
  constructor(owner, balance) {
    // Private ma'lumotlar tashqaridan ko'rinmaydi
    _privateData.set(this, {
      owner,
      balance,
      transactions: []
    });
  }

  getBalance() {
    return _privateData.get(this).balance;
  }

  deposit(amount) {
    if (amount <= 0) throw new Error('Summa musbat bo\'lishi kerak!');
    const data = _privateData.get(this);
    data.balance += amount;
    data.transactions.push({ type: 'kirim', amount, date: new Date() });
    console.log(`+${amount} so'm. Balans: ${data.balance}`);
  }

  withdraw(amount) {
    const data = _privateData.get(this);
    if (data.balance < amount) throw new Error('Mablag\' yetarli emas!');
    data.balance -= amount;
    data.transactions.push({ type: 'chiqim', amount, date: new Date() });
    console.log(`-${amount} so'm. Balans: ${data.balance}`);
  }

  getHistory() {
    return [..._privateData.get(this).transactions];
  }
}

const acc = new BankAccount('Kamola', 500_000);
acc.deposit(100_000);   // +100000 so'm. Balans: 600000
acc.withdraw(50_000);   // -50000 so'm. Balans: 550000
console.log(acc.getBalance()); // 550000
// acc._privateData ko'rinmaydi — xavfsiz!

// 3. WeakSet — Ob'ektlarni belgilash
const visited = new WeakSet();

function processNode(node) {
  if (visited.has(node)) {
    console.log(`Tugun '${node.id}' allaqachon ko'rilgan — o'tkazib yuborildi.`);
    return;
  }
  visited.add(node);
  console.log(`Tugun '${node.id}' qayta ishlanmoqda...`);
  if (node.children) node.children.forEach(processNode);
}

const tree = {
  id: 'root',
  children: [
    { id: 'A', children: [] },
    { id: 'B', children: [{ id: 'C', children: [] }] }
  ]
};

processNode(tree);
processNode(tree); // Takroriy chaqiruv — o'tkazib yuboriladi
console.log(visited.has(tree)); // true
