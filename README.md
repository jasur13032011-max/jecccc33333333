📁 Repository Fayllar Tuzilishi
Loyihangiz papkasida 3 ta fayl bo'lishi kerak:

Plaintext
playlist-linkedlist/
├── index.html
├── playlist.js
└── README.md
💻 1. index.html (Interfeys)
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>LinkedList Music Playlist</title>
  <style>
    body { font-family: system-ui, sans-serif; max-width: 600px; margin: 30px auto; padding: 20px; background: #f4f6f9; }
    .card { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); margin-bottom: 20px; }
    .player-box { background: #1e1e2e; color: white; padding: 15px; border-radius: 6px; text-align: center; margin-bottom: 20px; }
    .controls { display: flex; gap: 10px; justify-content: center; margin-top: 10px; }
    .btn { padding: 8px 14px; border: none; border-radius: 4px; cursor: pointer; font-weight: bold; }
    .btn-primary { background: #0066cc; color: white; }
    .btn-danger { background: #dc3545; color: white; }
    .btn-nav { background: #ff9800; color: white; }
    .form-group { display: flex; gap: 10px; margin-bottom: 15px; }
    input { flex: 1; padding: 8px; border: 1px solid #ccc; border-radius: 4px; }
    .error-msg { color: #dc3545; font-weight: bold; margin-top: 10px; min-height: 20px; }
    ul { list-style: none; padding: 0; }
    li { background: #f8f9fa; padding: 10px; border-bottom: 1px solid #ddd; display: flex; justify-content: space-between; align-items: center; }
  </style>
</head>
<body>

  <h1>🎵 LinkedList Music Playlist</h1>

  <div class="card player-box">
    <small>Hozir ijro etilmoqda:</small>
    <h2 id="current-song-display">Hali qo'shiq yo'q</h2>
    <div class="controls">
      <button id="btn-prev" class="btn btn-nav">⏮️ Prev</button>
      <button id="btn-next" class="btn btn-nav">Next ⏭️</button>
    </div>
  </div>

  <div id="error-box" class="error-msg"></div>

  <div class="card">
    <div class="form-group">
      <input type="text" id="song-input" placeholder="Qo'shiq nomini kiriting..." />
      <button id="btn-append" class="btn btn-primary">Oxiriga Qo'shish (append)</button>
      <button id="btn-prepend" class="btn btn-primary">Boshiga Qo'shish (prepend)</button>
    </div>

    <h3>📋 Pleylist Ro'yxati</h3>
    <ul id="playlist-container"></ul>
  </div>

  <script src="playlist.js"></script>
</body>
</html>
⚙️ 2. playlist.js (Node, LinkedList & Player Logic)
JavaScript
// ==============================================================================
// 1. NODE KLASSI
// ==============================================================================
class Node {
  constructor(data) {
    this.data = data; // Qo'shiq nomi
    this.next = null; // Keyingi node ga ishora
    this.prev = null; // DoublyLinkedList usuli orqali prev() ishlashi uchun
  }
}

// ==============================================================================
// 2. LINKEDLIST KLASSI
// ==============================================================================
class LinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.current = null; // Hozirgi ijro etilayotgan qo'shiq
  }

  // A) Oxiriga qo'shish (append)
  append(data) {
    const newNode = new Node(data);

    if (!this.head) {
      this.head = newNode;
      this.tail = newNode;
      this.current = newNode; // Birinchi qo'shiq joriy qo'shiq bo'ladi
    } else {
      newNode.prev = this.tail;
      this.tail.next = newNode;
      this.tail = newNode;
    }
  }

  // B) Boshiga qo'shish (prepend)
  prepend(data) {
    const newNode = new Node(data);

    if (!this.head) {
      this.head = newNode;
      this.tail = newNode;
      this.current = newNode;
    } else {
      newNode.next = this.head;
      this.head.prev = newNode;
      this.head = newNode;
    }
  }

  // C) O'chirish (remove)
  remove(data) {
    if (!this.head) return false;

    let curr = this.head;

    while (curr) {
      if (curr.data === data) {
        // O'chirilayotgan qo'shiq current bo'lsa, joriy qo'shiqni o'zgartirish
        if (curr === this.current) {
          this.current = curr.next || curr.prev || null;
        }

        if (curr.prev) curr.prev.next = curr.next;
        else this.head = curr.next; // Head o'chirilsa

        if (curr.next) curr.next.prev = curr.prev;
        else this.tail = curr.prev; // Tail o'chirilsa

        return true;
      }
      curr = curr.next;
    }
    return false;
  }

  // D) Hozirgi qo'shiqni olish (currentSong)
  currentSong() {
    if (!this.head || !this.current) {
      throw new Error("⚠️ Xatolik: Pleylist bo'sh yoki qo'shiq tanlanmagan!");
    }
    return this.current.data;
  }

  // E) Keyingi qo'shiqqa o'tish (next)
  next() {
    if (!this.head) {
      throw new Error("⚠️ Xatolik: Pleylist bo'sh!");
    }
    if (this.current && this.current.next) {
      this.current = this.current.next;
      return this.current.data;
    } else {
      throw new Error("⚠️ Pleylist oxiriga yetib keldingiz!");
    }
  }

  // F) Oldingi qo'shiqqa o'tish (prev - Doubly LinkedList usulida)
  prev() {
    if (!this.head) {
      throw new Error("⚠️ Xatolik: Pleylist bo'sh!");
    }
    if (this.current && this.current.prev) {
      this.current = this.current.prev;
      return this.current.data;
    } else {
      throw new Error("⚠️ Siz pleylist boshida turibsiz!");
    }
  }

  // G) Ro'yxatni olish (display)
  display() {
    const songs = [];
    let curr = this.head;
    while (curr) {
      songs.push(curr.data);
      curr = curr.next;
    }
    return songs;
  }
}

// ==============================================================================
// 3. UI VA HODISALARNI BOSHQARISH
// ==============================================================================

const playlist = new LinkedList();

const songInput = document.getElementById('song-input');
const btnAppend = document.getElementById('btn-append');
const btnPrepend = document.getElementById('btn-prepend');
const btnNext = document.getElementById('btn-next');
const btnPrev = document.getElementById('btn-prev');
const playlistContainer = document.getElementById('playlist-container');
const currentSongDisplay = document.getElementById('current-song-display');
const errorBox = document.getElementById('error-box');

function showError(msg) {
  errorBox.textContent = msg;
  setTimeout(() => { errorBox.textContent = ''; }, 3000);
}

function updateUI() {
  // Current Song render qilish va xatolikni ushlash
  try {
    currentSongDisplay.textContent = playlist.currentSong();
  } catch (err) {
    currentSongDisplay.textContent = "Hali qo'shiq yo'q";
  }

  // Playlist ro'yxatini render qilish (display)
  playlistContainer.innerHTML = '';
  const songs = playlist.display();

  songs.forEach((song) => {
    const li = document.createElement('li');
    li.innerHTML = `
      <span>${song} ${playlist.current && playlist.current.data === song ? '▶️ (ijro)' : ''}</span>
      <button class="btn btn-danger" onclick="handleRemove('${song}')">O'chirish</button>
    `;
    playlistContainer.appendChild(li);
  });
}

// Global scope da handles removing
window.handleRemove = (songName) => {
  playlist.remove(songName);
  updateUI();
};

btnAppend.addEventListener('click', () => {
  const val = songInput.value.trim();
  if (!val) return showError("Iltimos, qo'shiq nomini kiriting!");
  playlist.append(val);
  songInput.value = '';
  updateUI();
});

btnPrepend.addEventListener('click', () => {
  const val = songInput.value.trim();
  if (!val) return showError("Iltimos, qo'shiq nomini kiriting!");
  playlist.prepend(val);
  songInput.value = '';
  updateUI();
});

btnNext.addEventListener('click', () => {
  try {
    playlist.next();
    updateUI();
  } catch (err) {
    showError(err.message);
  }
});

btnPrev.addEventListener('click', () => {
  try {
    playlist.prev();
    updateUI();
  } catch (err) {
    showError(err.message);
  }
});

// ==============================================================================
// KAMIDA 5 TA QO'SHIQ BILAN SINAB KO'RISH (TALAB BO'YICHA)
// ==============================================================================
function initDefaultPlaylist() {
  const defaultSongs = [
    "1. Bohemian Rhapsody - Queen",
    "2. Imagine - John Lennon",
    "3. Hotel California - Eagles",
    "4. Shape of You - Ed Sheeran",
    "5. Blinding Lights - The Weeknd"
  ];

  defaultSongs.forEach(song => playlist.append(song));
  updateUI();
}

// Ilovani ishga tushirish
initDefaultPlaylist();
📑 3. README.md
Markdown
# 🎵 LinkedList Music Playlist Application

Ushbu loyihada Ma'lumotlar Tuzilmasi (Data Structures) bo'lgan **Node** va **LinkedList** klasslari amalda qo'llanilgan.

## 🛠 Imkoniyatlar
- **Node & LinkedList Class**: O'z ichiga `head`, `tail` va `current` ko'rsatkichlarini oladi.
- **`append(data)`**: Pleylist oxiriga qo'shiq qo'shish.
- **`prepend(data)`**: Pleylist boshiga qo mezonida qo'shiq qo'shish.
- **`remove(data)`**: Qo'shiqni ro'yxatdan va xotiradan o'chirish.
- **`next()` & `prev()`**: Keyingi va oldingi qo'shiqqa o'tish (DoublyLinkedList tugunlari orqali).
- **`currentSong()` & `display()`**: Joriy qo'shiqni hamda to'liq ro'yxatni ko'rsatish.
- **Error Handling**: Pleylist bo'sh bo'lganda yoki oxiri/boshiga yetganda xatolik xabari ko'rsatiladi.
📌 Commit'lar Tarixi
Terminalda quyidagi buyruqlar orqali commit'lar qiling:

Bash
git add index.html
git commit -m "feat: layout setup for playlist UI and controls"

git add playlist.js
git commit -m "feat: implement Node and LinkedList classes with append, prepend, remove and display methods"

git add playlist.js
git commit -m "feat: implement next, prev and currentSong with error handling for empty playlist"

git add playlist.js README.md
git commit -m "feat: populate playlist with 5 default songs and update docs"

git push origin main
