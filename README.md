// ==========================================
// 1-QISM: Big O Murakkablikdagi Funksiyalar
// ==========================================

// O(1) - Doimiy (Constant) murakkablik
// Kiruvchi ma'lumot hajmidan qat'iy nazar, bajarilish vaqti bir xil bo'ladi.
function getFirstElement(arr) {
    // O(1)
    return arr.length > 0 ? arr[0] : null;
}

// O(n) - Chiziqli (Linear) murakkablik
// Massiv hajmi o'sishi bilan operatsiyalar soni ham chiziqli ravishda o'sadi.
function findMaxElement(arr) {
    // O(n)
    let max = arr[0];
    for (const num of arr) {
        if (num > max) {
            max = num;
        }
    }
    return max;
}

// O(n²) - Kvadratik (Quadratic) murakkablik
// Ikki qavatli sikl sababli elementlar soni ortishi bilan vaqt juda tez o'sadi.
function findPairSums(arr) {
    // O(n²) - Massiv ichidagi juftliklarni tekshirish simulyatsiyasi
    let count = 0;
    // n soni 100 000 bo'lganda n² = 10 000 000 000 operatsiya bo'ladi va brauzer qotib qolishi mumkin.
    // Shuning uchun tahlil xavfsizligi uchun cheklov qo'yilgan.
    const limit = Math.min(arr.length, 10000); 
    for (let i = 0; i < limit; i++) {
        for (let j = 0; j < limit; j++) {
            if ((arr[i] + arr[j]) % 2 === 0) {
                count++;
            }
        }
    }
    return count;
}

// O(log n) - Logarifmik (Logarithmic) murakkablik
// Har bir qadamda qidiruv maydoni ikkiga bo'linadi. Massiv saralangan bo'lishi shart.
function binarySearch(arr, target) {
    // O(log n)
    let left = 0;
    let right = arr.length - 1;
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        if (arr[mid] === target) return mid;
        if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}

// O(n log n) - Chiziqli-logarifmik murakkablik
// Samarali saralash algoritmlariga xos (masalan, Merge Sort).
function mergeSort(arr) {
    // O(n log n)
    if (arr.length <= 1) return arr;
    const mid = Math.floor(arr.length / 2);
    const left = mergeSort(arr.slice(0, mid));
    const right = mergeSort(arr.slice(mid));
    
    return merge(left, right);
}

function merge(left, right) {
    let result = [], l = 0, r = 0;
    while (l < left.length && r < right.length) {
        if (left[l] < right[r]) result.push(left[l++]);
        else result.push(right[r++]);
    }
    return result.concat(left.slice(l)).concat(right.slice(r));
}

// ==========================================
// 2-QISM: Test Ma'lumotlari va Vaqt O'lchash
// ==========================================

const sizes = [100, 1000, 10000, 100000];
const results = [];

for (const n of sizes) {
    // Test uchun tasodifiy sonlardan iborat saralangan massiv yaratamiz
    const testArray = Array.from({ length: n }, (_, i) => i);
    const targetValue = n - 1; // Binary search uchun oxirgi element

    // 1. O(1) tahlili
    const t0_1 = performance.now();
    getFirstElement(testArray);
    const t1_1 = performance.now();

    // 2. O(n) tahlili
    const t0_n = performance.now();
    findMaxElement(testArray);
    const t1_n = performance.now();

    // 3. O(n²) tahlili
    const t0_n2 = performance.now();
    findPairSums(testArray);
    const t1_n2 = performance.now();

    // 4. O(log n) tahlili
    const t0_log = performance.now();
    binarySearch(testArray, targetValue);
    const t1_log = performance.now();

    // 5. O(n log n) tahlili
    // n = 100000 bo'lganda mergeSort ko'p xotira oladi, xavfsiz ishlashi uchun:
    let time_nlog = 0;
    if (n <= 10000) {
        const t0_nlog = performance.now();
        mergeSort([...testArray]);
        const t1_nlog = performance.now();
        time_nlog = t1_nlog - t0_nlog;
    } else {
        time_nlog = "N/A (Juda ko'p)";
    }

    // Natijalarni saqlash (millisekundlarda)
    results.push({
        "Massiv Hajmi (n)": n,
        "O(1) [ms]": (t1_1 - t0_1).toFixed(4),
        "O(log n) [ms]": (t1_log - t0_log).toFixed(4),
        "O(n) [ms]": (t1_n - t0_n).toFixed(4),
        "O(n log n) [ms]": typeof time_nlog === 'number' ? time_nlog.toFixed(4) : time_nlog,
        "O(n²) [ms]": (n > 10000) ? "Cheklangan (Juda sekin)" : (t1_n2 - t0_n2).toFixed(4)
    });
}

// Natijalarni konsolga jadval ko'rinishida chiqarish
console.table(results);
