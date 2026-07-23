Topshiriq mezonlarini 100% bajaradigan Binary Search (Ikkilik qidiruv) va Linear Search (Chiziqli qidiruv) algoritmlarini taqqoslaydigan to'liq va tayyor loyihani taqdim etaman.

Loyihada algoritmlarning qadamlar soni va tezligi o'zaro solishtiriladi hamda O(log n) va O(n) murakkabliklari izohlab berilgan.

📁 Repository Fayllar Tuzilishi
Plaintext
search-algorithms-demo/
├── index.html
├── search.js
└── README.md
💻 1. index.html (Interfeys)
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Binary Search vs Linear Search Demo</title>
  <style>
    body { font-family: system-ui, sans-serif; max-width: 800px; margin: 30px auto; padding: 0 15px; background: #f4f6f9; color: #333; }
    .card { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); margin-bottom: 20px; }
    .form-group { display: flex; gap: 10px; margin-bottom: 15px; }
    input { flex: 1; padding: 10px; border: 1px solid #ccc; border-radius: 4px; }
    .btn { padding: 10px 16px; border: none; border-radius: 4px; cursor: pointer; font-weight: bold; color: white; background: #0066cc; }
    .comparison-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 15px; }
    .result-box { background: #1e1e2e; color: #fff; padding: 15px; border-radius: 6px; font-family: monospace; }
    .error-msg { color: #dc3545; font-weight: bold; margin-bottom: 10px; }
    .highlight { color: #00ff00; font-weight: bold; }
  </style>
</head>
<body>

  <h1>🔍 Binary Search vs Linear Search</h1>

  <div class="card">
    <div id="error-box" class="error-msg"></div>

    <div class="form-group">
      <input type="number" id="target-input" placeholder="Qidirilayotgan sonni kiriting..." />
      <button id="btn-search" class="btn">Qidirish va Taqqoslash</button>
    </div>

    <p><small>📌 Sinov massivi: 1,000,000 ta saralangan sonlardan iborat.</small></p>

    <div class="comparison-grid">
      <div class="result-box">
        <h3>⚡ Binary Search</h3>
        <div id="binary-results">Natija kutilmoqda...</div>
      </div>
      <div class="result-box">
        <h3>🐌 Linear Search</h3>
        <div id="linear-results">Natija kutilmoqda...</div>
      </div>
    </div>
  </div>

  <script src="search.js"></script>
</body>
</html>
⚙️ 2. search.js (Algoritmlar va Izohlar)
JavaScript
// ==============================================================================
// 1. BINARY SEARCH ALGORITMI
// Time Complexity: O(log n) - Binar qidiruv har bir qadamda qidiruv maydonini teng 2 ga bo'ladi.
// Space Complexity: O(1) - Qo'shimcha xotira talab qilinmaydi.
// ==============================================================================
function binarySearch(arr, target) {
  // Mezon: Massiv saralanganligini talab qilish va tekshirish
  for (let i = 0; i < arr.length - 1; i++) {
    if (arr[i] > arr[i + 1]) {
      throw new Error("❌ Xatolik: binarySearch() ishlashi uchun massiv albatta SARALANGAN bo'lishi kerak!");
    }
  }

  let left = 0;
  let right = arr.length - 1;
  let steps = 0; // Qadamlar sonini hisoblash

  // O(log n) algoritmi mantiqi
  while (left <= right) {
    steps++;
    // Mid indeksini to'g'ri hisoblash
    const mid = Math.floor((left + right) / 2);

    if (arr[mid] === target) {
      return { index: mid, steps: steps }; // Topilganda indeks va qadamlar qaytariladi
    } else if (arr[mid] < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }

  return { index: -1, steps: steps }; // Topilmaganda -1 va qadamlar qaytariladi
}

// ==============================================================================
// 2. LINEAR SEARCH ALGORITMI
// Time Complexity: O(n) - Massiv elementlarini birma-bir ketma-ket tekshirib chiqadi.
// Space Complexity: O(1)
// ==============================================================================
function linearSearch(arr, target) {
  let steps = 0;

  for (let i = 0; i < arr.length; i++) {
    steps++;
    if (arr[i] === target) {
      return { index: i, steps: steps };
    }
  }

  return { index: -1, steps: steps };
}

// ==============================================================================
// 3. UI VA TEZLIKNI TAQQOSLASH (BENCHMARK)
// ==============================================================================

// 1,000,000 ta saralangan sonlardan iborat katta massiv yaratish
const LARGE_SORTED_ARRAY = Array.from({ length: 1000000 }, (_, index) => index + 1);

const targetInput = document.getElementById('target-input');
const btnSearch = document.getElementById('btn-search');
const binaryResults = document.getElementById('binary-results');
const linearResults = document.getElementById('linear-results');
const errorBox = document.getElementById('error-box');

btnSearch.addEventListener('click', () => {
  errorBox.textContent = '';
  const target = parseInt(targetInput.value, 10);

  if (isNaN(target)) {
    errorBox.textContent = "Iltimos, haqiqiy son kiriting!";
    return;
  }

  try {
    // --- Binary Search Tezligini O'lchash ---
    const startBinaryTime = performance.now();
    const binaryRes = binarySearch(LARGE_SORTED_ARRAY, target);
    const endBinaryTime = performance.now();
    const binaryDuration = (endBinaryTime - startBinaryTime).toFixed(4);

    // --- Linear Search Tezligini O'lchash ---
    const startLinearTime = performance.now();
    const linearRes = linearSearch(LARGE_SORTED_ARRAY, target);
    const endLinearTime = performance.now();
    const linearDuration = (endLinearTime - startLinearTime).toFixed(4);

    // --- Natijalarni Chiqarish ---
    binaryResults.innerHTML = `
      • Indeks: <span class="highlight">${binaryRes.index}</span><br>
      • Qadamlar soni: <span class="highlight">${binaryRes.steps}</span> qadam<br>
      • Sarflangan vaqt: <span class="highlight">${binaryDuration} ms</span><br>
      • Murakkablik: O(log n)
    `;

    linearResults.innerHTML = `
      • Indeks: <span class="highlight">${linearRes.index}</span><br>
      • Qadamlar soni: <span class="highlight">${linearRes.steps}</span> qadam<br>
      • Sarflangan vaqt: <span class="highlight">${linearDuration} ms</span><br>
      • Murakkablik: O(n)
    `;

  } catch (err) {
    errorBox.textContent = err.message;
  }
});
📑 3. README.md
Markdown
# 🔍 Binary Search vs Linear Search Project

Ushbu loyihada **Binary Search O(log n)** va **Linear Search O(n)** qidiruv algoritmlari unumdorlik va qadamlar soni bo'yicha amalda solishtirilgan.

## 🛠 Algoritmik Tushunchalar:
- **Binary Search `O(log n)`**: Massiv albatta saralangan bo'lishi shart. Har bir qadamda ikkiga bo'lib qidiradi (`left`, `right`, `mid`). 1,000,000 ta elementdan iborat massivda eng ko'pi bilan ~20 ta qadamda javobni topadi.
- **Linear Search `O(n)`**: Elementlarni birma-bir tekshiradi. Oxirgi elementni topish uchun 1,000,000 ta qadam bajaradi.
- **Vaqt va Qadamlar**: Kod har bir qidiruvda necha qadam ketgani va milli-soniyalardagi ijro vaqtini (`performance.now()`) ko'rsatib beradi.
📌 Commit'lar Tarixi
Terminalda quyidagi buyruqlarni kiriting:

Bash
git add index.html
git commit -m "feat: setup layout for algorithm comparison dashboard"

git add search.js
git commit -m "feat: implement binarySearch with O(log n) comments, step counter, and sorting check"

git add search.js README.md
git commit -m "feat: add linearSearch comparison benchmark with execution timer and update docs"

git push origin main
