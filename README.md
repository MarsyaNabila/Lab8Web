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

## Langkah - Langkah Pratikum
Langkah pertama mempersiapkan database server menggunakan MySQL. Pastikan MySQL Server sudah dapat dijalankan melalui XAMPP.

## Membuat Database: Studi Kasus Data Barang dan Tabel

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

## Menambahkan Data Awal





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

# Menampilkan Data (Read) Index.php


