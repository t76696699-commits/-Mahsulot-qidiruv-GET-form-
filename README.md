class HashMap {
  constructor(size = 53) {
    this.keyMap = new Array(size);
    this.collisions = 0;
  }

  _hash(key) {
    let total = 0;
    for (let char of key.toString()) {
      total += char.charCodeAt(0);
    }
    return total % this.keyMap.length;
  }

  set(key, value) {
    let index = this._hash(key);
    if (!this.keyMap[index]) {
      this.keyMap[index] = [];
    } else {
      this.collisions++; // To'qnashuv sodir bo'ldi
    }
    
    // Agar kalit mavjud bo'lsa, qiymatni yangilash
    for (let pair of this.keyMap[index]) {
      if (pair[0] === key) {
        pair[1] = value;
        return;
      }
    }
    this.keyMap[index].push([key, value]);
  }

  get(key) {
    let index = this._hash(key);
    if (this.keyMap[index]) {
      for (let pair of this.keyMap[index]) {
        if (pair[0] === key) return pair[1];
      }
    }
    return undefined;
  }

  delete(key) {
    let index = this._hash(key);
    if (this.keyMap[index]) {
      for (let i = 0; i < this.keyMap[index].length; i++) {
        if (this.keyMap[index][i][0] === key) {
          this.keyMap[index].splice(i, 1);
          return true;
        }
      }
    }
    return false;
  }

  keys() {
    let keysArr = [];
    for (let bucket of this.keyMap) {
      if (bucket) for (let pair of bucket) keysArr.push(pair[0]);
    }
    return keysArr;
  }

  values() {
    let valuesArr = [];
    for (let bucket of this.keyMap) {
      if (bucket) for (let pair of bucket) valuesArr.push(pair[1]);
    }
    return valuesArr;
  }
}
