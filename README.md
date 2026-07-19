// Bubble Sort - qo'shni elementlarni solishtirib almashtirish
function bubbleSort(arr) {
  const result = [...arr];
  const n = result.length;

  for (let i = 0; i < n - 1; i++) {
    let swapped = false;
    // Har bir pass'da eng katta element oxiriga "ko'tariladi"
    for (let j = 0; j < n - 1 - i; j++) {
      if (result[j] > result[j + 1]) {
        [result[j], result[j + 1]] = [result[j + 1], result[j]];
        swapped = true;
      }
    }
    // Agar hech qanday almashtirish bo'lmasa, massiv allaqachon tartiblangan
    if (!swapped) break;
  }
  return result;
}

// Selection Sort - eng kichik elementni tanlab joylashtirish
function selectionSort(arr) {
  const result = [...arr];
  const n = result.length;

  for (let i = 0; i < n - 1; i++) {
    let minIndex = i;
    // Tartiblanmagan qismdan eng kichik elementni topish
    for (let j = i + 1; j < n; j++) {
      if (result[j] < result[minIndex]) {
        minIndex = j;
      }
    }
    if (minIndex !== i) {
      [result[i], result[minIndex]] = [result[minIndex], result[i]];
    }
  }
  return result;
}

// Insertion Sort - har bir elementni to'g'ri joyga kiritish
function insertionSort(arr) {
  const result = [...arr];

  for (let i = 1; i < result.length; i++) {
    const current = result[i];
    let j = i - 1;

    // Current'dan katta elementlarni bir pozitsiyaga suramiz
    while (j >= 0 && result[j] > current) {
      result[j + 1] = result[j];
      j--;
    }
    result[j + 1] = current;
  }
  return result;
}

// Amaliyot
const data = [5, 3, 8, 1, 9, 2, 7];
console.log("Bubble Sort:", bubbleSort(data));
console.log("Selection Sort:", selectionSort(data));
console.log("Insertion Sort:", insertionSort(data));

// Deyarli tartiblangan massivda Insertion Sort tezroq ishlaydi
const almostSorted = [1, 2, 4, 3, 5, 6, 7];
console.log("Deyarli tartiblangan:", insertionSort(almostSorted));
