// Binary Search - iterativ variant
function binarySearchIterative(sortedArr, target) {
  let left = 0;
  let right = sortedArr.length - 1;

  while (left <= right) {
    const mid = Math.floor((left + right) / 2);

    if (sortedArr[mid] === target) {
      return mid; // Topildi
    } else if (sortedArr[mid] < target) {
      left = mid + 1; // O'ng yarimda davom etish
    } else {
      right = mid - 1; // Chap yarimda davom etish
    }
  }

  return -1; // Topilmadi
}

// Binary Search - rekursiv variant
function binarySearchRecursive(sortedArr, target, left = 0, right = sortedArr.length - 1) {
  if (left > right) {
    return -1; // Baza holati: qidiruv maydoni tugadi
  }

  const mid = Math.floor((left + right) / 2);

  if (sortedArr[mid] === target) {
    return mid;
  } else if (sortedArr[mid] < target) {
    return binarySearchRecursive(sortedArr, target, mid + 1, right);
  } else {
    return binarySearchRecursive(sortedArr, target, left, mid - 1);
  }
}

// Amaliy misol: birinchi element indeksini topish (lower bound)
function findFirstOccurrence(sortedArr, target) {
  let left = 0;
  let right = sortedArr.length - 1;
  let result = -1;

  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    if (sortedArr[mid] === target) {
      result = mid;
      right = mid - 1; // Chapga davom etib, birinchi uchrashni izlaymiz
    } else if (sortedArr[mid] < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }
  return result;
}

// Amaliyot
const numbers = [1, 3, 5, 7, 9, 11, 13, 15];
console.log("Iterativ qidiruv (7):", binarySearchIterative(numbers, 7));
console.log("Rekursiv qidiruv (13):", binarySearchRecursive(numbers, 13));
console.log("Topilmagan qiymat (4):", binarySearchIterative(numbers, 4));

const withDuplicates = [1, 2, 2, 2, 3, 4, 5];
console.log("Birinchi uchrash (2):", findFirstOccurrence(withDuplicates, 2));
