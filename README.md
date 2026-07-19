// Stack (LIFO) implementatsiyasi
class Stack {
  constructor() {
    this.items = [];
  }

  push(value) {
    this.items.push(value);
  }

  pop() {
    return this.items.pop();
  }

  peek() {
    return this.items[this.items.length - 1];
  }

  isEmpty() {
    return this.items.length === 0;
  }
}

// Queue (FIFO) implementatsiyasi
class Queue {
  constructor() {
    this.items = [];
  }

  enqueue(value) {
    this.items.push(value);
  }

  dequeue() {
    return this.items.shift();
  }

  front() {
    return this.items[0];
  }

  isEmpty() {
    return this.items.length === 0;
  }
}

// Amaliy misol: Stack yordamida qavslar muvofiqligini tekshirish
function isBalanced(expression) {
  const stack = new Stack();
  const pairs = { ")": "(", "]": "[", "}": "{" };

  for (const char of expression) {
    if (char === "(" || char === "[" || char === "{") {
      stack.push(char);
    } else if (char === ")" || char === "]" || char === "}") {
      // Yopuvchi qavsga mos ochuvchi qavs stackning tepasida bo'lishi kerak
      if (stack.isEmpty() || stack.pop() !== pairs[char]) {
        return false;
      }
    }
  }
  return stack.isEmpty();
}

// Amaliyot
const stack = new Stack();
stack.push(1);
stack.push(2);
stack.push(3);
console.log("Stack pop:", stack.pop()); // 3 (LIFO)

const queue = new Queue();
queue.enqueue("A");
queue.enqueue("B");
queue.enqueue("C");
console.log("Queue dequeue:", queue.dequeue()); // A (FIFO)

console.log(isBalanced("{[()]}")); // true
console.log(isBalanced("{[(])}")); // false
