// Merge Sort - divide and conquer
function mergeSort(arr) {
  if (arr.length <= 1) return arr; // Baza holati

  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));

  return merge(left, right);
}

// Ikkita tartiblangan massivni birlashtirish
function merge(left, right) {
  const result = [];
  let i = 0;
  let j = 0;

  while (i < left.length && j < right.length) {
    if (left[i] <= right[j]) {
      result.push(left[i]);
      i++;
    } else {
      result.push(right[j]);
      j++;
    }
  }

  // Qolgan elementlarni qo'shish
  return result.concat(left.slice(i)).concat(right.slice(j));
}

// Quick Sort - pivot orqali bo'lish (partition)
function quickSort(arr) {
  if (arr.length <= 1) return arr; // Baza holati

  const pivot = arr[arr.length - 1];
  const left = [];
  const right = [];

  for (let i = 0; i < arr.length - 1; i++) {
    if (arr[i] < pivot) {
      left.push(arr[i]);
    } else {
      right.push(arr[i]);
    }
  }

  return [...quickSort(left), pivot, ...quickSort(right)];
}

// Quick Sort - in-place (xotirani tejaydigan) variant
function quickSortInPlace(arr, low = 0, high = arr.length - 1) {
  if (low < high) {
    const pivotIndex = partition(arr, low, high);
    quickSortInPlace(arr, low, pivotIndex - 1);
    quickSortInPlace(arr, pivotIndex + 1, high);
  }
  return arr;
}

function partition(arr, low, high) {
  const pivot = arr[high];
  let i = low - 1;

  for (let j = low; j < high; j++) {
    if (arr[j] < pivot) {
      i++;
      [arr[i], arr[j]] = [arr[j], arr[i]];
    }
  }
  [arr[i + 1], arr[high]] = [arr[high], arr[i + 1]];
  return i + 1;
}

// Amaliyot
const data = [8, 3, 7, 4, 2, 9, 1, 5, 6];

console.log("Merge Sort:", mergeSort(data));
console.log("Quick Sort:", quickSort(data));
console.log("Quick Sort (in-place):", quickSortInPlace([...data]));
