# UTS-Pemrograman-Web-2
|Nama|NIM|Kelas|Mata Kuliah|
|----|---|-----|------|
|**Wishnu Aqbil Ramadani**|**312310591**|**TI.23.A6**|**Pemrograman Web 2**|

Membangun Aplikasi Chat Real-Time Sederhana dengan WebSocket: Eksperimen dan Panduan Praktis

Pendahuluan
Di era digital saat ini, kebutuhan akan komunikasi real-time menjadi semakin penting, terutama dalam pengembangan aplikasi web seperti layanan chat, notifikasi langsung, hingga sistem monitoring. Salah satu teknologi yang mendukung komunikasi dua arah secara efisien antara client dan server adalah WebSocket.

Artikel ini bertujuan menjelaskan secara teknis cara kerja WebSocket melalui eksperimen membangun aplikasi chat sederhana menggunakan Node.js dan JavaScript, sekaligus membuktikan bagaimana WebSocket mampu mengelola komunikasi real-time secara stabil dan ringan.

Apa Itu WebSocket?
WebSocket adalah protokol komunikasi berbasis TCP yang memungkinkan koneksi dua arah (full-duplex) antara server dan client. Tidak seperti HTTP yang stateless dan satu arah, WebSocket mempertahankan koneksi terbuka, sehingga komunikasi bisa berlangsung terus-menerus tanpa perlu membuat permintaan baru.

Eksperimen: Membangun Aplikasi Chat Real-Time
Alat dan Bahan:
Node.js

Library WebSocket (ws)

HTML + JavaScript (client)

Terminal (untuk menjalankan server)

Langkah-langkah Eksperimen
Inisialisasi Proyek

Buka terminal dan jalankan perintah berikut untuk membuat folder proyek dan menginstal dependencies:

bash
Copy
Edit
mkdir websocket-chat
cd websocket-chat
npm init -y
npm install ws
Membuat Server WebSocket (server.js)

Buat file server.js untuk membuat server WebSocket yang mendengarkan pada port 8080:

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
Membuat Client HTML (index.html)

Buat file index.html untuk antarmuka pengguna aplikasi chat menggunakan HTML dan JavaScript:

html
Copy
Edit
<!DOCTYPE html>
<html>
<head><title>Chat WebSocket</title></head>
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
Menjalankan Aplikasi

Jalankan server dengan perintah berikut di terminal:

bash
Copy
Edit
node server.js
Setelah itu, buka index.html di dua browser untuk melihat hasil komunikasi dua arah secara langsung.

Analisis Hasil
Eksperimen ini menunjukkan bahwa WebSocket:

Memungkinkan pertukaran data instan antara client dan server tanpa harus melakukan refresh halaman.

Menghemat bandwidth karena tidak perlu membuat banyak permintaan HTTP.

Cocok untuk aplikasi real-time seperti game, trading, atau monitoring sistem.

Namun, WebSocket juga memiliki tantangan seperti:

Harus dikelola secara hati-hati untuk menghindari kebocoran memori akibat koneksi yang dibiarkan terbuka.

Tidak cocok untuk semua jenis aplikasi, terutama yang tidak membutuhkan komunikasi real-time.

Kesimpulan
WebSocket adalah solusi efisien untuk komunikasi real-time dalam aplikasi web. Melalui eksperimen membangun aplikasi chat sederhana, terbukti bahwa implementasinya cukup mudah dan dapat meningkatkan pengalaman pengguna secara signifikan. Meskipun memiliki tantangan tersendiri, manfaatnya dalam konteks aplikasi yang membutuhkan interaksi cepat menjadikannya pilihan yang kuat bagi developer web modern.

Referensi
Mozilla Developer Network - WebSocket

WebSocket npm Package

Dokumentasi Node.js

