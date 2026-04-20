# Tutorial 6 - Concurrency

## Commit 1 Reflection notes

Pada milestone ini saya mempelajari cara kerja web server sederhana di Rust menggunakan `TcpListener`. Program menerima koneksi dari browser melalui port 7878. Saya juga belajar bahwa setiap request dari browser dikirim sebagai stream dan bisa dibaca menggunakan `TcpStream`.

Fungsi `handle_connection()` digunakan untuk memproses request yang masuk. Dengan `BufReader`, request HTTP dapat dibaca per baris dan dikumpulkan ke dalam vector. Dari sini saya memahami bahwa browser mengirim method, path, dan header ketika membuka halaman web. Saya juga menyadari bahwa server dan browser berkomunikasi menggunakan protokol HTTP dalam bentuk teks.

## Commit 2 Reflection notes

Pada milestone ini saya mempelajari bagaimana fungsi handle_connection() dikembangkan agar server tidak hanya menerima koneksi, tetapi juga dapat mengirimkan halaman web ke browser. Sebelumnya server hanya mencetak informasi request ke terminal, sedangkan sekarang server membaca file HTML dan mengirimkannya sebagai response HTTP. Perubahan ini membuat saya lebih memahami alur komunikasi antara browser dan server.

Saya juga belajar penggunaan fs::read_to_string() untuk membaca isi file hello.html menjadi string. Isi file tersebut kemudian dimasukkan ke body response sehingga browser dapat merender halaman yang dikirim. Dengan cara ini, konten halaman dapat dipisahkan dari kode program sehingga lebih rapi dan mudah diubah.

Selain itu, saya memahami pentingnya status line seperti HTTP/1.1 200 OK yang memberi tahu browser bahwa request berhasil diproses. Header Content-Length juga penting karena memberi informasi ukuran data yang dikirim. Jika format response salah, browser bisa gagal menampilkan halaman dengan benar.

Dari milestone ini saya menyadari bahwa web server sederhana bekerja dengan prinsip membaca request, menyiapkan response, lalu mengirimkannya kembali melalui stream. Walaupun implementasinya masih sederhana, konsep dasarnya sama seperti server web pada umumnya.