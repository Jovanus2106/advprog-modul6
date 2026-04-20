# Tutorial 6 - Concurrency

## Commit 1 Reflection notes

Pada milestone ini saya mempelajari cara kerja web server sederhana di Rust menggunakan `TcpListener`. Program menerima koneksi dari browser melalui port 7878. Saya juga belajar bahwa setiap request dari browser dikirim sebagai stream dan bisa dibaca menggunakan `TcpStream`.

Fungsi `handle_connection()` digunakan untuk memproses request yang masuk. Dengan `BufReader`, request HTTP dapat dibaca per baris dan dikumpulkan ke dalam vector. Dari sini saya memahami bahwa browser mengirim method, path, dan header ketika membuka halaman web. Saya juga menyadari bahwa server dan browser berkomunikasi menggunakan protokol HTTP dalam bentuk teks.

## Commit 2 Reflection notes

Pada milestone ini saya mempelajari bagaimana fungsi handle_connection() dikembangkan agar server tidak hanya menerima koneksi, tetapi juga dapat mengirimkan halaman web ke browser. Sebelumnya server hanya mencetak informasi request ke terminal, sedangkan sekarang server membaca file HTML dan mengirimkannya sebagai response HTTP. Perubahan ini membuat saya lebih memahami alur komunikasi antara browser dan server.

Saya juga belajar penggunaan fs::read_to_string() untuk membaca isi file hello.html menjadi string. Isi file tersebut kemudian dimasukkan ke body response sehingga browser dapat merender halaman yang dikirim. Dengan cara ini, konten halaman dapat dipisahkan dari kode program sehingga lebih rapi dan mudah diubah.

Selain itu, saya memahami pentingnya status line seperti HTTP/1.1 200 OK yang memberi tahu browser bahwa request berhasil diproses. Header Content-Length juga penting karena memberi informasi ukuran data yang dikirim. Jika format response salah, browser bisa gagal menampilkan halaman dengan benar.

Dari milestone ini saya menyadari bahwa web server sederhana bekerja dengan prinsip membaca request, menyiapkan response, lalu mengirimkannya kembali melalui stream. Walaupun implementasinya masih sederhana, konsep dasarnya sama seperti server web pada umumnya.

## Commit 3 Reflection notes

Pada milestone ini saya mempelajari bagaimana server dapat memvalidasi request dari browser dan memberikan response yang berbeda sesuai path yang diminta. Program sekarang membaca baris pertama request HTTP, lalu memeriksa apakah request tersebut menuju root path (/) atau halaman lain. Jika request sesuai, server mengirim file hello.html, sedangkan jika tidak sesuai server mengirim halaman 404.html.

Saya memahami bahwa proses ini penting karena server harus bisa menentukan resource mana yang tersedia dan mana yang tidak. Dengan menggunakan match, kode menjadi lebih rapi dan mudah dikembangkan untuk menambah route baru di masa depan. Saya juga belajar bahwa status code 404 NOT FOUND memberi tahu browser bahwa halaman yang diminta tidak ditemukan.

Melalui milestone ini saya semakin mengerti bahwa routing adalah bagian penting dari web server. Walaupun implementasi masih sederhana, konsepnya sama seperti framework web modern yang memetakan URL ke response tertentu.