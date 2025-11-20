# Lab8Web - Pratikum PHP dan Database MYSQL

Nama: Marsya Nabila Putri

NIM: 312410338

Kelas: TI.24.A4

Matakuliah : Pemograman Web 1

## Tujuan Pratikum

1. Memahami konsep dasar pengelolaan database menggunakan MySQL, termasuk pembuatan database, tabel, serta pengolahan data.
2. Mampu membuat aplikasi CRUD sederhana, mulai dari menambah, menampilkan, mengubah, hingga menghapus data barang.
3. Menerapkan operasi CRUD dalam aplikasi web sebagai dasar pengembangan sistem informasi.

## Deskripsi Program 
Program ini dibuat untuk menampilkan, menambah, mengubah, dan menghapus data barang menggunakan PHP dan MySQL. Setiap data disimpan di database dan ditampilkan dalam bentuk tabel. File tambah, ubah, dan hapus digunakan untuk memproses perubahan data, sedangkan index.php menampilkan seluruh data yang ada. Program juga mendukung upload gambar untuk setiap barang.

## Struktur Direktori
lab8_php_database/ ├── index.php -> Halaman utama (Read - Daftar Barang) ├── tambah.php -> Form tambah barang baru (Create) ├── ubah.php -> Form ubah data barang (Update) ├── hapus.php -> Proses hapus barang (Delete) ├── koneksi.php -> Koneksi ke database MySQL ├── style.css -> Styling tampilan ├── gambar/ -> Folder penyimpanan gambar barang yang di-upload └── db_latihan1.sql -> Script SQL database + tabel + data awal

## Langkah - Langkah Pratikum
Langkah pertama mempersiapkan database server menggunakan MySQL. Pastikan MySQL Server sudah dapat dijalankan melalui XAMPP.

## A. Membuat Database

`CREAT DATABASE latihan1;`

## B. Membuat Tabel

```sql
-- Membuat Database
CREATE DATABASE latihan1;

-- Membuat Tabel data_barang
CREATE TABLE data_barang (
    id_barang int(10) auto_increment Primary Key,
    kategori varchar(30),
    nama varchar(30),
    gambar varchar(100),
    harga_beli decimal(10,0),
    harga_jual decimal(10,0),
    stok int(4)
);
```

## C. Menambahkan Data Awal

```sql
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
 VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000,5),
 ('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
 ('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```

## 📷 Hasil Screenshoot Tampilan Tabel `data_barang` phpMyAdmin

<img width="1156" height="720" alt="Screenshot 2025-11-20 202154" src="https://github.com/user-attachments/assets/f5e0acbe-1d3d-45ac-a16e-c659e069363d" />




# Koneksi.php

```
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db   = "latihan1";

$conn = mysqli_connect($host, $user, $pass, $db);

if (!$conn) {
    echo "Koneksi ke server gagal.";
    die();
} else {
    echo "Koneksi berhasil!";
}
?>
```


<img width="954" height="320" alt="image" src="https://github.com/user-attachments/assets/4f6f806f-a5f2-48e8-bda1-2a9d4146f978" />

Kode PHP tersebut digunakan untuk membuat koneksi antara script PHP dan database MySQL. Bagian pertama berisi empat variabel yang menyimpan informasi koneksi, yaitu alamat server (`localhost`), username MySQL (`root`), password yang masih kosong, serta nama database yang ingin diakses (`latihan1`). Setelah itu, fungsi `mysqli_connect()` dipakai untuk mencoba menyambungkan PHP ke MySQL menggunakan informasi tersebut. Hasil koneksi disimpan dalam variabel `$conn`. Kemudian dilakukan pengecekan: jika koneksi gagal, program menampilkan pesan "Koneksi ke server gagal." dan langsung menghentikan proses menggunakan `die()`. Namun jika koneksi berhasil dibuat, program menampilkan pesan "Koneksi berhasil!" sebagai tanda bahwa PHP sudah terhubung dengan database dengan benar.


# Menampilkan Data (Read) Index.php


