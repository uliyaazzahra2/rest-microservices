# Flask Microservices

Project ini merupakan implementasi arsitektur **Monolith** dan **Microservices** menggunakan **Python Flask**.

---

## 1. Persiapan Project

Buat folder project dengan nama:

```text
flask_microservices
```

Struktur file project:

```text
flask_microservices/
├── monolith_app.py
├── book_service.py
└── order_service.py
```

Buka folder tersebut menggunakan **Visual Studio Code**.

---

## 2. Instalasi Library

Buka **Git Bash** atau terminal pada folder project, kemudian jalankan:

```bash
pip install Flask requests
```

Untuk memastikan library sudah terpasang:

```bash
pip show Flask
pip show requests
```

Jika informasi package ditampilkan, berarti instalasi berhasil.

---

## 3. Menjalankan Aplikasi Monolith

File yang digunakan:

```text
monolith_app.py
```

Jalankan aplikasi dengan:

```bash
python monolith_app.py
```

Monolith berjalan pada:

```text
http://localhost:5000
```

### Pengujian

Buka terminal baru tanpa menghentikan aplikasi, kemudian jalankan:

```bash
curl http://localhost:5000/books
```

Untuk menguji pembuatan pesanan:

```bash
curl -X POST -H "Content-Type: application/json" -d "{\"book_id\":1}" http://localhost:5000/orders
```

Jika berhasil, akan ditampilkan data buku dan informasi pesanan.

---

## 4. Menjalankan Book Service

File yang digunakan:

```text
book_service.py
```

Jalankan:

```bash
python book_service.py
```

Book Service berjalan pada:

```text
http://localhost:5001
```

### Pengujian

Buka terminal baru, kemudian jalankan:

```bash
curl http://localhost:5001/books
```

Untuk menguji data buku berdasarkan ID:

```bash
curl http://localhost:5001/books/1
```

Jika berhasil, akan ditampilkan informasi buku.

---

## 5. Menjalankan Order Service

File yang digunakan:

```text
order_service.py
```

Pastikan **Book Service masih berjalan pada port 5001**.

Buka terminal baru, kemudian jalankan:

```bash
python order_service.py
```

Order Service berjalan pada:

```text
http://localhost:5002
```

### Pengujian

Kirim request untuk membuat pesanan:

```bash
curl -X POST -H "Content-Type: application/json" -d "{\"book_id\":1}" http://localhost:5002/orders
```

Jika berhasil, akan ditampilkan informasi pesanan dengan status:

```text
berhasil
```

Pada tahap ini, **Order Service telah berhasil berkomunikasi dengan Book Service**.

---

## 6. Pengujian Ketika Book Service Down

Pengujian ini dilakukan untuk mengetahui respons sistem ketika salah satu service mengalami gangguan.

### Menghentikan Book Service

Pada terminal yang menjalankan **Book Service**, tekan:

```text
Ctrl + C
```

> Jangan hentikan Order Service.

Kemudian jalankan kembali request:

```bash
curl -X POST -H "Content-Type: application/json" -d "{\"book_id\":1}" http://localhost:5002/orders
```

### Hasil yang Diharapkan

```text
Book Service sedang down!
```

Hal tersebut menunjukkan bahwa **Order Service tetap berjalan**, tetapi tidak dapat membuat pesanan karena **Book Service sedang tidak tersedia**.

---

## 7. Menjalankan Kembali Book Service

Setelah pengujian selesai, jalankan kembali:

```bash
python book_service.py
```

Book Service kembali berjalan pada port:

```text
5001
```

Kemudian pengujian Order Service dapat dilakukan kembali:

```bash
curl -X POST -H "Content-Type: application/json" -d "{\"book_id\":1}" http://localhost:5002/orders
```

Jika berhasil, pesanan kembali dapat dibuat.

---

## 8. Ringkasan Port

| Aplikasi      | File               |   Port |
| ------------- | ------------------ | -----: |
| Monolith      | `monolith_app.py`  | `5000` |
| Book Service  | `book_service.py`  | `5001` |
| Order Service | `order_service.py` | `5002` |

---

## 9. Urutan Menjalankan Microservices

Untuk menjalankan bagian **Microservices**, gunakan urutan berikut.

### Terminal 1 — Book Service

```bash
python book_service.py
```

Book Service berjalan pada:

```text
http://localhost:5001
```

### Terminal 2 — Order Service

```bash
python order_service.py
```

Order Service berjalan pada:

```text
http://localhost:5002
```

### Terminal 3 — Pengujian

#### Uji Book Service

```bash
curl http://localhost:5001/books
```

#### Uji Order Service

```bash
curl -X POST -H "Content-Type: application/json" -d "{\"book_id\":1}" http://localhost:5002/orders
```

Untuk pengujian gangguan, hentikan **Book Service** dengan menekan:

```text
Ctrl + C
```

Kemudian jalankan kembali request **Order Service** untuk melihat respons ketika Book Service tidak tersedia.

---

## 10. Kesimpulan

Project ini menunjukkan perbedaan antara arsitektur **Monolith** dan **Microservices**.

Pada arsitektur **Monolith**, fitur buku dan pesanan berada dalam satu aplikasi pada port `5000`.

Pada arsitektur **Microservices**, fitur tersebut dipisahkan menjadi:

- **Book Service** pada port `5001`
- **Order Service** pada port `5002`

Order Service berkomunikasi dengan Book Service menggunakan **HTTP**.

Pengujian juga menunjukkan bahwa ketika **Book Service mengalami gangguan**, Order Service tetap berjalan, tetapi proses pembuatan pesanan tidak dapat dilakukan.

Dengan demikian, project ini memberikan gambaran sederhana mengenai cara kerja dan komunikasi antar-service pada arsitektur **Microservices**.
