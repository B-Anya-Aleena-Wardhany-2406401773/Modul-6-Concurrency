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