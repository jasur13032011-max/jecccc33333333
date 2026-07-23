📁 Repository Fayllar Tuzilishi
Plaintext
stack-queue-app/
├── index.html
├── navigation.js
└── README.md
💻 1. index.html (Interfeys)
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Stack & Queue Data Structures Demo</title>
  <style>
    body { font-family: system-ui, sans-serif; max-width: 800px; margin: 30px auto; padding: 0 15px; background: #f4f6f9; color: #333; }
    .card { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); margin-bottom: 20px; }
    .form-group { display: flex; gap: 10px; margin-bottom: 15px; }
    input { flex: 1; padding: 8px 12px; border: 1px solid #ccc; border-radius: 4px; }
    .btn { padding: 8px 14px; border: none; border-radius: 4px; cursor: pointer; font-weight: bold; color: white; }
    .btn-primary { background: #0066cc; }
    .btn-nav { background: #ff9800; }
    .btn-success { background: #28a745; }
    .status-box { background: #1e1e2e; color: #fff; padding: 12px; border-radius: 6px; font-family: monospace; margin-bottom: 10px; }
    .error-msg { color: #dc3545; font-weight: bold; min-height: 20px; margin-top: 5px; }
    ul { list-style: none; padding: 0; margin: 0; }
    li { background: #f8f9fa; padding: 8px 12px; border: 1px solid #ddd; margin-bottom: 5px; border-radius: 4px; display: flex; justify-content: space-between; }
  </style>
</head>
<body>

  <h1>📚 Stack (LIFO) & Queue (FIFO) Demo</h1>

  <div id="error-box" class="error-msg"></div>

  <div class="card">
    <h2>1. Brauzer Navigatsiyasi (Stack - LIFO)</h2>
    <div class="status-box">
      Hozirgi sahifa: <strong id="current-page">Yo'q</strong>
    </div>

    <div class="form-group">
      <input type="text" id="url-input" placeholder="https://example.com" />
      <button id="btn-visit" class="btn btn-primary">Tashrif buyurish (Push)</button>
    </div>

    <div>
      <button id="btn-back" class="btn btn-nav">⬅️ Back (Pop)</button>
      <button id="btn-forward" class="btn btn-nav">Forward (Pop) ➡️</button>
    </div>
  </div>

  <div class="card">
    <h2>2. Yuklab Olish Navbati (Queue - FIFO)</h2>
    <div class="form-group">
      <input type="text" id="file-input" placeholder="file_name.zip" />
      <button id="btn-enqueue" class="btn btn-primary">Navbatga qo'shish (Enqueue)</button>
    </div>

    <div>
      <button id="btn-dequeue" class="btn btn-success">Bitta faylni yuklash (Dequeue)</button>
    </div>

    <h3>📥 Yuklash Navbati (Queue status):</h3>
    <ul id="download-list"></ul>
  </div>

  <script src="navigation.js"></script>
</body>
</html>
⚙️ 2. navigation.js (Stack, Queue va Logika)
JavaScript
// ==============================================================================
// 1. STACK KLASSI (LIFO - Last In, First Out)
// ==============================================================================
class Stack {
  constructor() {
    this.items = [];
  }

  // Element qo'shish (oxiriga)
  push(item) {
    this.items.push(item);
  }

  // Elementni olib tashlash (oxirgisini)
  pop() {
    if (this.isEmpty()) {
      return null;
    }
    return this.items.pop();
  }

  // Eng ustki (oxirgi) elementni ko'rish
  peek() {
    if (this.isEmpty()) return null;
    return this.items[this.items.length - 1];
  }

  // Bo'shligini tekshirish
  isEmpty() {
    return this.items.length === 0;
  }
}

// ==============================================================================
// 2. QUEUE KLASSI (FIFO - First In, First Out)
// ==============================================================================
class Queue {
  constructor() {
    this.items = [];
  }

  // Navbatga qo'shish (oxiriga)
  enqueue(item) {
    this.items.push(item);
  }

  // Navbatdan chiqarish (birinchisini olib tashlash)
  dequeue() {
    if (this.isEmpty()) {
      return null;
    }
    return this.items.shift();
  }

  // Navbatdagi birinchi elementni ko'rish
  front() {
    if (this.isEmpty()) return null;
    return this.items[0];
  }

  // Bo'shligini tekshirish
  isEmpty() {
    return this.items.length === 0;
  }
}

// ==============================================================================
// 3. BRAUZER HISTORIYA VA DOWNLOAD MANAGER TIZIMI
// ==============================================================================

// Stack obyekti yaratiladi
const backStack = new Stack();
const forwardStack = new Stack();
let currentPage = null;

// Download Queue obyekti yaratiladi
const downloadQueue = new Queue();

// DOM Elementlari
const urlInput = document.getElementById('url-input');
const btnVisit = document.getElementById('btn-visit');
const btnBack = document.getElementById('btn-back');
const btnForward = document.getElementById('btn-forward');
const currentPageDisplay = document.getElementById('current-page');

const fileInput = document.getElementById('file-input');
const btnEnqueue = document.getElementById('btn-enqueue');
const btnDequeue = document.getElementById('btn-dequeue');
const downloadList = document.getElementById('download-list');
const errorBox = document.getElementById('error-box');

function showError(msg) {
  errorBox.textContent = msg;
  setTimeout(() => { errorBox.textContent = ''; }, 3000);
}

// --- STACK METODLARI (NAVIGATION) ---

function visitUrl(url) {
  if (!url) return;
  
  if (currentPage) {
    backStack.push(currentPage);
  }
  
  currentPage = url;
  
  // Yangi URL'ga o'tilganda forwardStack tozalanadi
  while (!forwardStack.isEmpty()) {
    forwardStack.pop();
  }

  updateNavUI();
}

function back() {
  // back() bo'sh backStack da xato chiqarsin (Talab bo'yicha)
  if (backStack.isEmpty()) {
    throw new Error("⚠️ Xatolik: Orqaga qaytish uchun tarix mavjud emas (backStack bo'sh)!");
  }

  forwardStack.push(currentPage);
  currentPage = backStack.pop();
  updateNavUI();
}

function forward() {
  // forward() bo'sh forwardStack da xato chiqarsin (Talab bo'yicha)
  if (forwardStack.isEmpty()) {
    throw new Error("⚠️ Xatolik: Oldinga o'tish uchun tarix mavjud emas (forwardStack bo'sh)!");
  }

  backStack.push(currentPage);
  currentPage = forwardStack.pop();
  updateNavUI();
}

function updateNavUI() {
  currentPageDisplay.textContent = currentPage || "Yo'q";
}

// --- QUEUE METODLARI (DOWNLOAD QUEUE) ---

function addDownload(fileName) {
  if (!fileName) return;
  downloadQueue.enqueue(fileName);
  updateQueueUI();
}

function processDownload() {
  if (downloadQueue.isEmpty()) {
    showError("⚠️ Download Queue bo'sh! Yuklab olish uchun fayl yo'q.");
    return;
  }

  const downloadedFile = downloadQueue.dequeue();
  alert(`✅ Fayl yuklab olindi: ${downloadedFile}`);
  updateQueueUI();
}

function updateQueueUI() {
  downloadList.innerHTML = '';
  
  downloadQueue.items.forEach((file, index) => {
    const li = document.createElement('li');
    li.innerHTML = `<span>#${index + 1} ${file}</span> <span>${index === 0 ? '⏳ [Navbatda birinchi]' : '⏸️ [Kutmoqda]'}</span>`;
    downloadList.appendChild(li);
  });
}

// --- EVENT LISTENERS ---

btnVisit.addEventListener('click', () => {
  const url = urlInput.value.trim();
  if (url) {
    visitUrl(url);
    urlInput.value = '';
  }
});

btnBack.addEventListener('click', () => {
  try {
    back();
  } catch (err) {
    showError(err.message);
  }
});

btnForward.addEventListener('click', () => {
  try {
    forward();
  } catch (err) {
    showError(err.message);
  }
});

btnEnqueue.addEventListener('click', () => {
  const file = fileInput.value.trim();
  if (file) {
    addDownload(file);
    fileInput.value = '';
  }
});

btnDequeue.addEventListener('click', () => {
  processDownload();
});

// ==============================================================================
// KAMIDA 5 TA URL VA 3 TA YUKLAB OLISH BILAN SINAB KO'RISH (TALAB BO'YICHA)
// ==============================================================================
function initTestData() {
  // 5 ta URL kiritilishi
  const testUrls = [
    "https://google.com",
    "https://github.com",
    "https://developer.mozilla.org",
    "https://stackoverflow.com",
    "https://javascript.info"
  ];

  testUrls.forEach(url => visitUrl(url));

  // 3 ta yuklab olish fayli qo'shilishi
  const testDownloads = [
    "document.pdf",
    "archive_v1.zip",
    "installer.exe"
  ];

  testDownloads.forEach(file => addDownload(file));
}

// Ilovani test ma'lumotlari bilan ishga tushirish
initTestData();
📑 3. README.md
Markdown
# 📚 Stack and Queue Data Structures Project

Ushbu loyihada **Stack (LIFO)** va **Queue (FIFO)** ma'lumotlar tuzilmalari sinflari (Class) orqali amalda qo'llanilgan.

## 🛠 Imkoniyatlar:
- **Stack Class (LIFO)**: `push()`, `pop()`, `peek()`, va `isEmpty()` metodlari bilan jihozlangan. Brauzer tarixdan orqaga (`back`) va oldinga (`forward`) harakatlanish uchun ishlatiladi.
- **Queue Class (FIFO)**: `enqueue()`, `dequeue()`, `front()`, va `isEmpty()` metodlariga ega. Yuklab olish navbati (Download Queue) uchun qo'llaniladi.
- **Error Handling**: `back()` bo'sh `backStack` bo'lganda, `forward()` esa bo'sh `forwardStack` bo'lganda maxsus xatolik chiqaradi.
- **Test Ma'lumotlari**: Dastlab kamida 5 ta URL va 3 ta yuklab olish fayli bilan avtomatik sinab ko'rilgan.
📌 Commit'lar Tarixi
Terminalda quyidagi buyruqlarni kiriting:

Bash
git add index.html
git commit -m "feat: setup HTML UI for Stack navigation and Queue download manager"

git add navigation.js
git commit -m "feat: implement Stack (LIFO) and Queue (FIFO) classes with error handling for empty stacks"

git add navigation.js README.md
git commit -m "feat: add 5 test URLs and 3 test downloads to satisfy requirements and update docs"

git push origin main
