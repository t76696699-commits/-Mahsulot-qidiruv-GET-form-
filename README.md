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
