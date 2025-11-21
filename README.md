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

## 📷 Tampilan Tabel `data_barang` phpMyAdmin

<img width="1156" height="720" alt="Screenshot 2025-11-20 202154" src="https://github.com/user-attachments/assets/f5e0acbe-1d3d-45ac-a16e-c659e069363d" />




# Koneksi Database (koneksi.php)

```php
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

Kode PHP tersebut digunakan untuk membuat koneksi antara script PHP dan database MySQL. Bagian pertama berisi empat variabel yang menyimpan informasi koneksi, yaitu alamat server (`localhost`), username MySQL (`root`), password yang masih kosong, serta nama database yang ingin diakses (`latihan1`). Setelah itu, fungsi `mysqli_connect()` dipakai untuk mencoba menyambungkan PHP ke MySQL menggunakan informasi tersebut. Hasil koneksi disimpan dalam variabel `$conn`. Kemudian dilakukan pengecekan: jika koneksi gagal, program menampilkan pesan "Koneksi ke server gagal." dan langsung menghentikan proses menggunakan `die()`. Namun jika koneksi berhasil dibuat, program menampilkan pesan "Koneksi berhasil!" sebagai tanda bahwa PHP sudah terhubung dengan database dengan benar.


<img width="954" height="320" alt="image" src="https://github.com/user-attachments/assets/4f6f806f-a5f2-48e8-bda1-2a9d4146f978" />




# Menampilkan Data Read (index.php)

```php
<?php 
include 'koneksi.php';

$sql = "SELECT * FROM data_barang";
$result = mysqli_query($conn, $sql);
?>
<!DOCTYPE html>
<html>
<head>
    <title>Data Barang</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

<div class="container">
    <h1>Data Barang</h1>

    <a href="tambah.php" class="btn">+ Tambah Barang</a>

    <table>
        <tr>
            <th>Gambar</th>
            <th>Nama</th>
            <th>Kategori</th>
            <th>Harga Beli</th>
            <th>Harga Jual</th>
            <th>Stok</th>
            <th>Aksi</th>
        </tr>

        <?php while ($row = mysqli_fetch_assoc($result)) : ?>
            <tr>
                <td>
                    <?php if ($row["gambar"] != ""): ?>
                        <img src="gambar/<?php echo $row["gambar"]; ?>" class="thumb">
                    <?php else: ?>
                        -
                    <?php endif; ?>
                </td>
                <td><?= $row["nama"] ?></td>
                <td><?= $row["kategori"] ?></td>
                <td><?= number_format($row["harga_beli"]) ?></td>
                <td><?= number_format($row["harga_jual"]) ?></td>
                <td><?= $row["stok"] ?></td>
                <td>
                    <a class="link" href="ubah.php?id=<?= $row['id_barang'] ?>">Ubah</a> |
                    <a class="link" href="hapus.php?id=<?= $row['id_barang'] ?>" onclick="return confirm('Hapus data?')">Hapus</a>
                </td>
            </tr>
        <?php endwhile; ?>

    </table>

</div>

</body>
</html>
```

<img width="956" height="874" alt="image" src="https://github.com/user-attachments/assets/232ffb36-3c16-49e8-b476-ec1c5f8f170e" />


# Menambah Data Create (tambah.php)

```php
<?php
include 'koneksi.php';

if (isset($_POST['submit'])) {
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_beli = $_POST['harga_beli'];
    $harga_jual = $_POST['harga_jual'];
    $stok = $_POST['stok'];

    $gambar = $_FILES['gambar']['name'];
    $tmp = $_FILES['gambar']['tmp_name'];

    if ($gambar != "") {
        move_uploaded_file($tmp, "gambar/$gambar");
    }

    mysqli_query($conn, "INSERT INTO data_barang (nama, kategori, harga_beli, harga_jual, stok, gambar)
    VALUES ('$nama','$kategori','$harga_beli','$harga_jual','$stok','$gambar')");

    header("Location: index.php");
}
?>
<!DOCTYPE html>
<html>
<head>
    <title>Tambah Barang</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

<div class="container">
    <h1>Tambah Barang</h1>

    <form action="" method="post" enctype="multipart/form-data">

        <label>Nama Barang</label>
        <input type="text" name="nama" required>

        <label>Kategori</label>
        <select name="kategori">
            <option>Elektronik</option>
            <option>Komputer</option>
            <option>Hand Phone</option>
        </select>

        <label>Harga Beli</label>
        <input type="number" name="harga_beli">

        <label>Harga Jual</label>
        <input type="number" name="harga_jual">

        <label>Stok</label>
        <input type="number" name="stok">

        <label>Gambar</label>
        <input type="file" name="gambar">

        <button type="submit" name="submit" class="btn">Simpan</button>

    </form>
</div>

</body>
</html>
```

<img width="956" height="565" alt="image" src="https://github.com/user-attachments/assets/5ec2d7dc-4311-4fcd-95aa-5aa08ee69c6b" />


# Mengubah Data Update (ubah.php)

```php
<?php
include 'koneksi.php';

$id = $_GET['id'];
$q = mysqli_query($conn, "SELECT * FROM data_barang WHERE id_barang='$id'");
$data = mysqli_fetch_assoc($q);

if (isset($_POST['submit'])) {

    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_beli = $_POST['harga_beli'];
    $harga_jual = $_POST['harga_jual'];
    $stok = $_POST['stok'];

    $gambar_baru = $_FILES['gambar']['name'];
    $tmp = $_FILES['gambar']['tmp_name'];

    if ($gambar_baru != "") {
        move_uploaded_file($tmp, "gambar/$gambar_baru");
        $g = $gambar_baru;
    } else {
        $g = $data['gambar'];
    }

    mysqli_query($conn,
        "UPDATE data_barang SET 
            nama='$nama',
            kategori='$kategori',
            harga_beli='$harga_beli',
            harga_jual='$harga_jual',
            stok='$stok',
            gambar='$g'
        WHERE id_barang='$id'"
    );

    header("Location: index.php");
}
?>
<!DOCTYPE html>
<html>
<head>
    <title>Ubah Barang</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

<div class="container">
    <h1>Ubah Barang</h1>

    <form method="post" enctype="multipart/form-data">

        <label>Nama</label>
        <input type="text" name="nama" value="<?= $data['nama'] ?>">

        <label>Kategori</label>
        <select name="kategori">
            <option <?= ($data['kategori']=="Elektronik")?"selected":"" ?>>Elektronik</option>
            <option <?= ($data['kategori']=="Komputer")?"selected":"" ?>>Komputer</option>
            <option <?= ($data['kategori']=="Hand Phone")?"selected":"" ?>>Hand Phone</option>
        </select>

        <label>Harga Beli</label>
        <input type="number" name="harga_beli" value="<?= $data['harga_beli'] ?>">

        <label>Harga Jual</label>
        <input type="number" name="harga_jual" value="<?= $data['harga_jual'] ?>">

        <label>Stok</label>
        <input type="number" name="stok" value="<?= $data['stok'] ?>">

        <label>Gambar</label>
        <input type="file" name="gambar">

        <button class="btn" name="submit">Simpan</button>

    </form>
</div>

</body>
</html>
```

<img width="948" height="562" alt="image" src="https://github.com/user-attachments/assets/bb18f982-695e-4c8b-b647-08401be84796" />



# Menghapus Data Delete (hapus.php)

```php
<?php
include 'koneksi.php';

$id = $_GET['id'];
mysqli_query($conn, "DELETE FROM data_barang WHERE id_barang='$id'");

header("Location: index.php");
?>
```

<img width="955" height="793" alt="image" src="https://github.com/user-attachments/assets/a2144c9a-b9fb-4051-95d5-9769840b41ae" />




