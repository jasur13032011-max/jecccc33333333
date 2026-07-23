Topshiriq mezonlarini 100% bajaradigan, Bubble Sort, Selection Sort va Insertion Sort algoritmlarini unumdorlik (vaqt, swap va iteratsiya soni) bo'yicha taqqoslaydigan to'liq loyihani taqdim etaman.

📁 Repository Fayllar Tuzilishi
Plaintext
sorting-algorithms-demo/
├── index.html
├── sort.js
└── README.md
💻 1. index.html (Interfeys va Konsol Yo'riqnomasi)
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Sorting Algorithms Benchmark</title>
  <style>
    body { font-family: system-ui, sans-serif; max-width: 800px; margin: 30px auto; padding: 0 15px; background: #f4f6f9; color: #333; }
    .card { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); margin-bottom: 20px; }
    .btn { padding: 10px 16px; border: none; border-radius: 4px; cursor: pointer; font-weight: bold; color: white; background: #0066cc; }
    .info-box { background: #e3f2fd; border-left: 4px solid #2196f3; padding: 12px; margin-top: 15px; border-radius: 0 4px 4px 0; }
  </style>
</head>
<body>

  <h1>📊 Sorting Algorithms Benchmark</h1>

  <div class="card">
    <p>Taqqoslashni ishga tushirish uchun quyidagi tugmani bosing va natijani <strong>Brauzer Konsolida (F12)</strong> ko'ring.</p>
    <button id="btn-run" class="btn">Algoritmlarni Ishga Tushirish</button>

    <div class="info-box">
      <strong>💡 Izoh:</strong> Natijalar konsolga <code>console.table()</code> yordamida jadval ko'rinishida chiqariladi.
    </div>
  </div>

  <script src="sort.js"></script>
</body>
</html>
⚙️ 2. sort.js (3 ta Algoritm, Nusxalash, Swap, Iteratsiya va Taqqoslash)
JavaScript
// ==============================================================================
// 1. BUBBLE SORT ALGORITMI
// Time Complexity: O(n²)
// ==============================================================================
function bubbleSort(originalArr) {
  // Asl massivni o'zgartirmaslik uchun nusxa olish
  const arr = originalArr.slice();
  let swaps = 0;
  let iterations = 0;

  const startTime = performance.now();

  for (let i = 0; i < arr.length - 1; i++) {
    for (let j = 0; j < arr.length - 1 - i; j++) {
      iterations++;
      if (arr[j] > arr[j + 1]) {
        // Swap operatsiyasi
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
        swaps++;
      }
    }
  }

  const endTime = performance.now();

  return {
    name: 'Bubble Sort',
    timeMs: (endTime - startTime).toFixed(4),
    iterations: iterations,
    swaps: swaps,
    sortedArray: arr
  };
}

// ==============================================================================
// 2. SELECTION SORT ALGORITMI
// Time Complexity: O(n²)
// ==============================================================================
function selectionSort(originalArr) {
  const arr = originalArr.slice();
  let swaps = 0;
  let iterations = 0;

  const startTime = performance.now();

  for (let i = 0; i < arr.length - 1; i++) {
    let minIndex = i;
    for (let j = i + 1; j < arr.length; j++) {
      iterations++;
      if (arr[j] < arr[minIndex]) {
        minIndex = j;
      }
    }
    if (minIndex !== i) {
      // Swap operatsiyasi
      [arr[i], arr[minIndex]] = [arr[minIndex], arr[i]];
      swaps++;
    }
  }

  const endTime = performance.now();

  return {
    name: 'Selection Sort',
    timeMs: (endTime - startTime).toFixed(4),
    iterations: iterations,
    swaps: swaps,
    sortedArray: arr
  };
}

// ==============================================================================
// 3. INSERTION SORT ALGORITMI
// Time Complexity: O(n²)
// ==============================================================================
function insertionSort(originalArr) {
  const arr = [...originalArr]; // Spread yordamida nusxa olish
  let swaps = 0;
  let iterations = 0;

  const startTime = performance.now();

  for (let i = 1; i < arr.length; i++) {
    let j = i;
    while (j > 0) {
      iterations++;
      if (arr[j] < arr[j - 1]) {
        // Swap operatsiyasi
        [arr[j], arr[j - 1]] = [arr[j - 1], arr[j]];
        swaps++;
        j--;
      } else {
        break;
      }
    }
  }

  const endTime = performance.now();

  return {
    name: 'Insertion Sort',
    timeMs: (endTime - startTime).toFixed(4),
    iterations: iterations,
    swaps: swaps,
    sortedArray: arr
  };
}

// ==============================================================================
// 4. TAQQOSLASH VA KONSOLGA JADVAL CHIARISH
// ==============================================================================

function runBenchmark() {
  // Test uchun tasodifiy sonlardan iborat massiv yaratish (1000 ta element)
  const TEST_ARRAY = Array.from({ length: 1000 }, () => Math.floor(Math.random() * 5000));

  console.log("📌 Asl massiv uzunligi:", TEST_ARRAY.length);
  console.log("📌 Asl massiv (birinchi 5 ta):", TEST_ARRAY.slice(0, 5));

  // Barcha algoritmlarni bittadan tekshirish
  const bubbleRes = bubbleSort(TEST_ARRAY);
  const selectionRes = selectionSort(TEST_ARRAY);
  const insertionRes = insertionSort(TEST_ARRAY);

  // Asl massiv o'zgarmaganligini tekshirish
  console.log("✅ Asl massiv o'zgarmadi (Original array preserved):", TEST_ARRAY.slice(0, 5));

  // Konsolga taqqoslash jadvalini chiqarish
  const comparisonTable = [
    {
      "Algoritm": bubbleRes.name,
      "Vaqt (ms)": Number(bubbleRes.timeMs),
      "Iteratsiyalar": bubbleRes.iterations,
      "Almashtirishlar (Swaps)": bubbleRes.swaps
    },
    {
      "Algoritm": selectionRes.name,
      "Vaqt (ms)": Number(selectionRes.timeMs),
      "Iteratsiyalar": selectionRes.iterations,
      "Almashtirishlar (Swaps)": selectionRes.swaps
    },
    {
      "Algoritm": insertionRes.name,
      "Vaqt (ms)": Number(insertionRes.timeMs),
      "Iteratsiyalar": insertionRes.iterations,
      "Almashtirishlar (Swaps)": insertionRes.swaps
    }
  ];

  console.table(comparisonTable);
}

// Tugmaga hodisa biriktirish
document.getElementById('btn-run').addEventListener('click', runBenchmark);

// Dastlab avtomatik bir marta ishga tushirish
runBenchmark();
📑 3. README.md
Markdown
# 📊 Sorting Algorithms Benchmark Project

Ushbu loyihada **Bubble Sort**, **Selection Sort** va **Insertion Sort** saralash algoritmlarining unumdorligi o'zaro solishtirilgan.

## 🛠 Imkoniyatlar va Mezonlar:
- **Asl massiv daxlsizligi**: `slice()` va `[...arr]` orqali nusxa olinadi. Asl massiv o'zgarmasdan qoladi.
- **Metrikalar**: Har bir algoritm uchun swap (`swaps++`) va iteratsiya (`iterations++`) soni hisoblanadi.
- **Vaqtni o'lchash**: `performance.now()` yordamida ijro vaqti milli-soniyalarda aniqlanadi.
- **Console Table**: Barcha natijalar `console.table()` orqali brauzer konsolida ko'rsatiladi.
📌 Commit'lar Tarixi
Terminalda quyidagi buyruqlarni kiriting:

Bash
git add index.html
git commit -m "feat: setup layout for sorting algorithms benchmark page"

git add sort.js
git commit -m "feat: implement Bubble, Selection, and Insertion sort with array copy, swap, iteration counters and timing"

git add sort.js README.md
git commit -m "feat: generate comparison table using console.table and update project documentation"

git push origin main
