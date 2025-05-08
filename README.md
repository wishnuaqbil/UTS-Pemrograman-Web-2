# UTS-Pemrograman-Web-2
|Nama|NIM|Kelas|Mata Kuliah|
|----|---|-----|------|
|**Wishnu Aqbil Ramadani**|**312310591**|**TI.23.A6**|**Pemrograman Web 2**|

# Membangun Aplikasi Chat Real-Time Sederhana dengan WebSocket

## Pendahuluan

Di era digital saat ini, kebutuhan akan komunikasi real-time menjadi semakin penting, terutama dalam pengembangan aplikasi web seperti layanan chat, notifikasi langsung, hingga sistem monitoring. Salah satu teknologi yang mendukung komunikasi dua arah secara efisien antara client dan server adalah WebSocket.

Artikel ini bertujuan menjelaskan secara teknis cara kerja WebSocket melalui eksperimen membangun aplikasi chat sederhana menggunakan Node.js dan JavaScript, sekaligus membuktikan bagaimana WebSocket mampu mengelola komunikasi real-time secara stabil dan ringan.

## Apa Itu WebSocket?

WebSocket adalah protokol komunikasi berbasis TCP yang memungkinkan koneksi dua arah (full-duplex) antara server dan client. Tidak seperti HTTP yang stateless dan satu arah, WebSocket mempertahankan koneksi terbuka, sehingga komunikasi bisa berlangsung terus-menerus tanpa perlu membuat permintaan baru.

## Eksperimen: Membangun Aplikasi Chat Real-Time

### Alat dan Bahan
- Node.js
- Library WebSocket (`ws`)
- HTML + JavaScript (client)
- Terminal (untuk menjalankan server)

### Langkah-langkah Eksperimen

#### 1. Inisialisasi Proyek

```bash
mkdir websocket-chat
cd websocket-chat
npm init -y
npm install ws
```

#### 2. Membuat Server WebSocket (`server.js`)

```javascript
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
```

#### 3. Membuat Client HTML (`index.html`)

```html
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
      document.getElementById('msg').value = '';
    }
  </script>
</body>
</html>
```

#### 4. Menjalankan Aplikasi

```bash
node server.js
```

Kemudian buka file `index.html` di dua tab browser untuk melihat komunikasi dua arah secara real-time.

# Output
![Screenshot 2025-05-08 230253](https://github.com/user-attachments/assets/f332300e-e8d0-44dc-a039-0bdc086dc53e)


## Analisis Hasil

WebSocket:
- Memungkinkan pertukaran data instan tanpa reload halaman.
- Menghemat bandwidth karena tidak perlu banyak permintaan HTTP.
- Cocok untuk aplikasi real-time seperti game, trading, monitoring.

Namun juga memiliki tantangan:
- Harus dikelola dengan baik untuk mencegah kebocoran memori.
- Tidak cocok untuk aplikasi non-real-time.

## Kesimpulan

WebSocket adalah solusi efisien untuk komunikasi real-time dalam aplikasi web. Melalui eksperimen ini, terbukti bahwa implementasinya mudah dan memberikan pengalaman pengguna yang interaktif dan modern.

## Referensi

- [Mozilla Developer Network - WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [WebSocket npm Package](https://www.npmjs.com/package/ws)
- [Dokumentasi Node.js](https://nodejs.org/en/docs)
