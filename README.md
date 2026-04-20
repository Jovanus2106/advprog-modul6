# Tutorial 6 - Concurrency

## Commit 1 Reflection notes

Pada milestone ini saya mempelajari cara kerja web server sederhana di Rust menggunakan `TcpListener`. Program menerima koneksi dari browser melalui port 7878. Saya juga belajar bahwa setiap request dari browser dikirim sebagai stream dan bisa dibaca menggunakan `TcpStream`.

Fungsi `handle_connection()` digunakan untuk memproses request yang masuk. Dengan `BufReader`, request HTTP dapat dibaca per baris dan dikumpulkan ke dalam vector. Dari sini saya memahami bahwa browser mengirim method, path, dan header ketika membuka halaman web. Saya juga menyadari bahwa server dan browser berkomunikasi menggunakan protokol HTTP dalam bentuk teks.

