# Reflection

Anya Aleena Wardhany

2406401773

<details>
<summary><b>Milestone 1</b></summary>
Pada milestone ini, saya membuat single-threaded web server sederhana menggunakan Rust yang mendengarkan koneksi TCP pada alamat `127.0.0.1:7878`

Fungsi `handle_connection` menerima parameter `TcpStream` yang merepresentasikan koneksi aktif antara server dan browser. Di dalamnya, `BufReader` digunakan untuk membungkus stream agar bisa dibaca baris per baris secara efisien.

HTTP request dari browser dibaca menggunakan `.lines()`, kemudian di-map untuk unwrap setiap `Result`, dan dikumpulkan sampai bertemu baris kosong menggunakan `.take_while(|line| !line.is_empty())` yang menandakan akhir dari HTTP header. Hasilnya dikumpulkan ke dalam `Vec<String>` lalu dicetak ke konsol.

Dari output yang muncul di konsol, saya dapat melihat struktur HTTP request yang dikirim browser, seperti:
- Method (`GET`)
- Path (`/`)
- Versi protokol (`HTTP/1.1`)
- Berbagai header seperti `Host`, `User-Agent`, `Accept`, dan `Connection`

Hal ini membantu saya memahami bagaimana komunikasi antara browser dan server bekerja pada level protokol HTTP yang sebenarnya.

</details>

<details>
<summary><b>Milestone 2</b></summary>

**Screenshot hasil:**

![Commit 2 screen capture](assets/images/commit2.png)
Pada milestone ini, saya memodifikasi fungsi `handle_connection` agar server tidak hanya mencetak request ke konsol, tetapi juga mengirimkan HTTP response yang bisa dirender oleh browser.

Perubahan utama yang dilakukan adalah menambahkan `fs` ke dalam import dan menyusun HTTP response string yang valid. `fs::read_to_string("hello.html")` digunakan untuk membaca seluruh isi file HTML menjadi sebuah `String`. Response kemudian dibentuk menggunakan `format!()` dengan beberapa bagian yang dipisahkan oleh `\r\n` sesuai protokol HTTP/1.1:

- **Status line**: `HTTP/1.1 200 OK` memberi tahu browser bahwa request berhasil diproses.
- **Header**: `Content-Length: {length}` memberi tahu browser berapa byte yang akan diterima,
  sehingga browser tahu kapan response berakhir.
- **Blank line**: Urutan `\r\n\r\n` memisahkan header dari body, ini adalah syarat wajib
  dalam spesifikasi HTTP.
- **Body**: Isi konten HTML yang dibaca dari file `hello.html`.

`stream.write_all(response.as_bytes())` mengonversi response string menjadi raw bytes dan menulisnya ke TCP stream, yang kemudian diterima dan dirender oleh browser.

Yang menarik adalah server kita tidak perlu mengerti CSS, JavaScript, maupun font sama sekali. Server hanya bertugas mengirimkan file HTML mentah, dan browser yang mengurus semua proses parsing dan eksekusinya, termasuk mengambil Google Fonts dari URL eksternal.
Ini menunjukkan pemisahan tanggung jawab yang jelas antara server (mengirim konten) dan browser (merender dan mengeksekusi konten tersebut).

</details>