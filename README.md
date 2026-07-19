Binary Search Tree (BST) nima?
Binary Search Tree (BST) — har bir node'i maksimum ikkita farzandga (chap va o'ng) ega bo'lgan, va muhim tartib xususiyatiga (BST property) ega bo'lgan daraxt ko'rinishidagi ma'lumotlar tuzilmasi: chap farzand qiymati ota-nodedan kichik, o'ng farzand qiymati esa katta bo'lishi shart.

Bu xususiyat tufayli BST'da qidirish, qo'shish va o'chirish amallari o'rtacha holatda O(log n) vaqtda amalga oshadi — xuddi Binary Search kabi, lekin dinamik (o'zgaruvchan) ma'lumotlar uchun.

Daraxt aylanib chiqish turlari (Traversal)
Inorder (chap → ildiz → o'ng) — elementlarni o'suvchi tartibda beradi.
Preorder (ildiz → chap → o'ng) — daraxt strukturasini nusxalash uchun foydali.
Postorder (chap → o'ng → ildiz) — node'larni o'chirishda foydali.
Murakkablik jadvali
Amal	O'rtacha holat (balanslangan)	Eng yomon holat (nomutanosib)
Qidirish	O(log n)	O(n)
Qo'shish	O(log n)	O(n)
O'chirish	O(log n)	O(n)
Eng yomon holat daraxt "nomutanosib" (skewed) bo'lganda, ya'ni har bir node faqat bitta farzandga ega bo'lib, aslida LinkedList'ga aylanganda yuzaga keladi. Shuning uchun amaliyotda AVL Tree yoki Red-Black Tree kabi o'z-o'zini balanslaydigan (self-balancing) daraxtlar qo'llaniladi.

Quyidagi diagramma tipik BST tuzilishini ko'rsatadi:

8

3

10

1

6

14

BST daraxt tuzilmalari, saralangan ma'lumotlar to'plami, avtomatik to'ldirish (autocomplete) funksiyalari va diapazon bo'yicha qidiruv (range query) kabi vazifalarda keng qo'llaniladi. Rekursiya BST bilan ishlashda asosiy vosita hisoblanadi, chunki har bir kichik daraxt (subtree) o'zi ham BST xususiyatiga ega bo'ladi.

💻
Код
Kod namunasi
#2
code
 Копировать
// Binary Search Tree (BST) implementatsiyasi

class TreeNode {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }

  // Yangi qiymat qo'shish - O(log n) o'rtacha
  insert(value) {
    const newNode = new TreeNode(value);
    if (!this.root) {
      this.root = newNode;
      return this;
    }

    let current = this.root;
    while (true) {
      if (value < current.value) {
        if (!current.left) {
          current.left = newNode;
          return this;
        }
        current = current.left;
      } else {
        if (!current.right) {
          current.right = newNode;
          return this;
        }
        current = current.right;
      }
    }
  }

  // Qiymatni qidirish - O(log n) o'rtacha
  search(value) {
    let current = this.root;
    while (current) {
      if (value === current.value) return true;
      current = value < current.value ? current.left : current.right;
    }
    return false;
  }

  // Inorder traversal - o'suvchi tartibda barcha qiymatlarni qaytaradi
  inorderTraversal(node = this.root, result = []) {
    if (node) {
      this.inorderTraversal(node.left, result);
      result.push(node.value);
      this.inorderTraversal(node.right, result);
    }
    return result;
  }

  // Daraxtning minimal qiymatini topish
  findMin(node = this.root) {
    while (node && node.left) {
      node = node.left;
    }
    return node ? node.value : null;
  }
}

// Amaliyot
const bst = new BinarySearchTree();
[8, 3, 10, 1, 6, 14, 4, 7].forEach((value) => bst.insert(value));

console.log("Inorder (tartiblangan):", bst.inorderTraversal());
console.log("6 ni qidirish:", bst.search(6)); // true
console.log("100 ni qidirish:", bst.search(100)); // false
console.log("Minimal qiymat:", bst.findMin());
