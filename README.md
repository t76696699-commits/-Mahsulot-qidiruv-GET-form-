// Singly LinkedList klassi
class ListNode {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

class LinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.length = 0;
  }
  // Ro'yxat oxiriga qo'shish - O(1)
  append(value) {
    const node = new ListNode(value);
    if (!this.head) {
      this.head = node;
      this.tail = node;
    } else {
      this.tail.next = node;
      this.tail = node;
    }
    this.length++;
    return this;
  }
  // Ro'yxat boshiga qo'shish - O(1)
  prepend(value) {
    const node = new ListNode(value);
    if (!this.head) {
      this.head = node;
      this.tail = node;
    } else {
      node.next = this.head;
      this.head = node;
    }
    this.length++;
    return this;
  }
  // Elementni o'chirish - O(n)
  delete(value) {
    if (!this.head) return false;
    if (this.head.value === value) {
      this.head = this.head.next;
      this.length--;
      return true;
    }
    let current = this.head;
    while (current.next) {
      if (current.next.value === value) {
        current.next = current.next.next;
        this.length--;
        return true;
      }
      current = current.next;
    }
    return false;
  }
  // Ro'yxatni massivga aylantirish (debug uchun qulay)
  toArray() {
    const result = [];
    let current = this.head;
    while (current) {
      result.push(current.value);
      current = current.next;
    }
    return result;
  }
}

// Amaliyot
const list = new LinkedList();
list.append(10);
list.append(20);
list.prepend(5);
console.log("Ro'yxat:", list.toArray()); // [5, 10, 20]
list.delete(10);
console.log("O'chirilgandan keyin:", list.toArray()); // [5, 20]
console.log("Uzunlik:", list.length);
