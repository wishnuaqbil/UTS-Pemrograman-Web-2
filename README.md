# UTS-Pemrograman-Web-2
|Nama|NIM|Kelas|Mata Kuliah|
|----|---|-----|------|
|**Wishnu Aqbil Ramadani**|**312310591**|**TI.23.A6**|**Pemrograman Web 2**|

# Membangun Aplikasi Chat Real-Time Sederhana dengan WebSocket

## 📌 Pendahuluan
Di era digital saat ini, kebutuhan akan komunikasi real-time menjadi semakin penting, terutama dalam pengembangan aplikasi web seperti layanan chat, notifikasi langsung, hingga sistem monitoring. Salah satu teknologi yang mendukung komunikasi dua arah secara efisien antara client dan server adalah **WebSocket**.

Artikel ini menjelaskan secara teknis cara kerja WebSocket melalui eksperimen membangun aplikasi chat sederhana menggunakan **Node.js** dan **JavaScript**, sekaligus membuktikan bagaimana WebSocket mampu mengelola komunikasi real-time secara stabil dan ringan.

---

## 💡 Apa Itu WebSocket?
WebSocket adalah protokol komunikasi berbasis **TCP** yang memungkinkan koneksi dua arah (*full-duplex*) antara server dan client. Tidak seperti HTTP yang bersifat satu arah dan stateless, WebSocket mempertahankan koneksi terbuka, sehingga komunikasi bisa berlangsung terus-menerus tanpa membuat permintaan baru.

---

## 🧪 Eksperimen: Membangun Aplikasi Chat Real-Time

### 🛠️ Alat dan Bahan
- Node.js
- Library `ws` (WebSocket)
- HTML + JavaScript (untuk client)
- Terminal untuk menjalankan server

---

## 🔧 Langkah-langkah Eksperimen

### 1. Inisialisasi Proyek

```bash
mkdir websocket-chat
cd websocket-chat
npm init -y
npm install ws
2. Membuat Server WebSocket (server.js)
javascript
Copy
Edit
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', function connection(ws) {
  ws.on('message', function incoming(message) {
    console.log('received: %s', message);
    // Kirim ulang pesan ke semua klien
    wss.clients.forEach(function each(client) {
      if (client !== ws && client.readyState === WebSocket.OPEN) {
        client.send(message);
      }
    });
  });

  ws.send('Selamat datang di chat WebSocket!');
});
3. Membuat Client HTML (index.html)
html
Copy
Edit
<!DOCTYPE html>
<html>
<head>
  <title>Chat WebSocket</title>
</head>
<body>
  <h2>Chat</h2>
  <input type="text" id="msg" placeholder="Ketik pesan..." />
  <button onclick="send()">Kirim</button>
  <ul id="chat"></ul>

  <script>
    const ws = new WebSocket('ws://localhost:8080');
    const chat = document.getElementById('chat');

    ws.onmessage = (event) => {
      const li = document.createElement('li');
      li.textContent = event.data;
      chat.appendChild(li);
    };

    function send() {
      const msg = document.getElementById('msg').value;
      ws.send(msg);
    }
  </script>
</body>
</html>
4. Menjalankan Aplikasi
bash
Copy
Edit
node server.js
Lalu buka file index.html di dua tab browser berbeda untuk melihat komunikasi dua arah secara real-time.

🔍 Analisis Hasil
Eksperimen ini menunjukkan bahwa WebSocket:

Memungkinkan pertukaran data secara instan antara client dan server.

Menghemat bandwidth karena tidak ada request berkala seperti polling.

Cocok untuk aplikasi real-time seperti: game online, sistem trading, live chat, dan monitoring data.

⚠️ Tantangan
Koneksi terbuka terlalu lama dapat menyebabkan kebocoran memori.

Tidak cocok untuk aplikasi yang tidak memerlukan komunikasi dua arah secara real-time.

✅ Kesimpulan
WebSocket adalah solusi efisien untuk membangun komunikasi real-time dalam aplikasi web. Eksperimen ini membuktikan bahwa implementasi WebSocket cukup sederhana dan sangat bermanfaat untuk meningkatkan pengalaman pengguna dalam aplikasi interaktif.

📚 Referensi
Mozilla Developer Network - WebSocket

WebSocket npm Package

Node.js Documentation
