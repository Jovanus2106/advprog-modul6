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

## Commit 4 Reflection notes

Pada milestone ini saya mempelajari dampak dari web server yang masih menggunakan single thread. Dengan menambahkan route /sleep, server sengaja menunda response selama 10 detik menggunakan thread::sleep(). Hal ini digunakan untuk mensimulasikan request yang lambat atau proses berat di server.

Saat saya membuka /sleep lalu mencoba membuka halaman / di tab lain, request kedua juga ikut menunggu sampai request pertama selesai. Dari percobaan ini saya memahami bahwa single-threaded server hanya bisa menangani satu request pada satu waktu. Semua request lain harus menunggu giliran.

Saya menyadari bahwa kondisi seperti ini akan menjadi masalah besar jika banyak pengguna mengakses server secara bersamaan. Satu request lambat dapat menghambat seluruh sistem. Inilah alasan mengapa server modern membutuhkan concurrency atau multithreading agar beberapa request dapat diproses secara paralel.

## Commit 5 Reflection notes

Pada milestone ini saya mempelajari bagaimana mengubah server single-threaded menjadi multithreaded menggunakan ThreadPool. ThreadPool berisi sejumlah worker thread yang siap menerima pekerjaan, sehingga server tidak perlu membuat thread baru setiap ada request masuk. Pendekatan ini lebih efisien dan lebih aman dibanding membuat thread tanpa batas.

Saya memahami bahwa setiap koneksi yang masuk akan dikirim ke salah satu worker melalui channel, lalu worker tersebut menjalankan `handle_connection()`. Dengan cara ini beberapa request dapat diproses secara bersamaan. Saat saya menguji route `/sleep`, request lain ke `/` tetap dapat dilayani dengan cepat karena diproses oleh worker yang berbeda.

Saya juga belajar penggunaan `mpsc`, `Arc`, dan `Mutex` dalam Rust untuk berbagi receiver antar thread secara aman. Milestone ini menunjukkan bagaimana concurrency dapat meningkatkan performa dan responsiveness server ketika ada banyak pengguna yang mengakses secara bersamaan.

## Commit Bonus Reflection notes

Pada bonus task ini saya mempelajari bagaimana membuat fungsi `build()` sebagai alternatif dari `new()` pada ThreadPool. Perbedaan utama adalah `build()` mengembalikan `Result`, sehingga error dapat ditangani dengan lebih baik dibanding langsung panic menggunakan `assert!`.

Saya memahami bahwa desain seperti ini membuat API lebih fleksibel dan lebih sesuai dengan praktik Rust yang mengutamakan error handling secara eksplisit. Jika ukuran thread pool tidak valid, program dapat memberikan pesan error yang jelas tanpa langsung menghentikan aplikasi.

Selain itu, saya melihat bahwa fungsi `new()` tetap bisa dipertahankan dengan memanggil `build().unwrap()`, sehingga kompatibilitas kode lama tetap terjaga. Dari bonus ini saya belajar pentingnya merancang constructor yang aman, reusable, dan mudah dikembangkan.
