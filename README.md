| Devi Silma Yuniar     |   312010458           |
|-----------------------|-----------------------|
|Praktikum 8            | Pertemuan 10          |

## Dipertemuan kali ini saya akan mempelajari PHP CRUD dan bagaimana mengneksikan database dengan PHP

## LANGKAH-LANGKAH

## 1. Menjalankan MYSQL server
![server](img/01.server.jpeg)

Penjelasan
menjalankan MySQL server dari XAMPP CONTROL

## 2. MEMBUAT DATABASE

create database latihan :

```php
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
![creat](img/02.database.jpeg)

## Menambahkan Data
![data](img/03.data.jpeg)
Menambahkan data pada DATABASE

```php
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg',
```
![data](img/04.data2.jpeg)
Data berhasil ditambahkan

![data](img/05.data3.jpeg)

select * from data_barang :

## 4. Membuat program CRUD

buat folder baru lab8_php_database pada root directory web server
![crud](img/06.crud.jpeg)

Kemudian akses dictory tersesebut
![crud](img/07.crud2.jpeg)

## 5. Membuat file koneksi database

Buat file baru dengan nama koneksi.php
![konesi](img/08.koneksi.jpeg)
Database berhasil terkoneksi

code php

```php
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db = "latihan1";

$conn = mysqli_connect($host, $user, $pass, $db);
if ($conn == false)
{
    echo "Koneksi ke server gagal.";
    die();
} //else echo "Koneksi berhasil";
?>
```

## Membuat file index untuk menampilkan data (READ)

Buat file baru dengan nama index.php
![index](img/09.index1.jpeg)

penjelasan 
tampilan dan hasil pada menu index.php atau program menampilkan data.

![index](img/10.index2.jpeg)

tampilan full index layout untuk menampilkan data.

code php

```php
<?php
include("koneksi.php");

// query untuk menampilkan data
$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);

?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Data Barang</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Data Barang</h1>
        <div class="main">
            <a href="tambah.php">Tambah Barang</a>
            <table>
                <tr>
                    <th>Gambar</th>
                    <th>Nama Barang</th>
                    <th>Kategori</th>
                    <th>Harga Jual</th>
                    <th>Harga Beli</th>
                    <th>Stok</th>
                    <th>Aksi</th>
                </tr>
            <?php if($result): ?>
            <?php while($row = mysqli_fetch_array($result)): ?> 
                <tr>
                    <td><img src="gambar/<?= $row['gambar'];?>" alt="<?=
$row['nama'];?>"></td>
                    <td><?= $row['nama'];?></td>
                    <td><?= $row['kategori'];?></td>
                    <td><?= $row['harga_beli'];?></td>
                    <td><?= $row['harga_jual'];?></td>
                    <td><?= $row['stok'];?></td>
                    <td>
                    <a href="ubah.php?id=<?= $row['id_barang'];?>">Ubah</a>
                    <a href="hapus.php?id=<?= $row['id_barang'];?>">Hapus</a> 
                    </td>
                </tr>
                <?php endwhile; else: ?>
                 <tr>
                 <td colspan="7">Belum ada data</td>
                 </tr>
                 <?php endif; ?>  
             </table> 
        </div>
    </div>
</body>
</html>
```

## 7. Menambahkan data (creat)

Buat file baru dengan nama tambah.php
![creat](img/11.tambah.jpeg)

program menambahkan data atau create

![creat](img/12.tamnah2.jpeg)

code php 

```php
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit']))
{
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;
    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_',$file_gambar['name']);
        $destination = dirname(_FILE_) .'/gambar/' . $filename;
        if(move_uploaded_file($file_gambar['tmp_name'], $destination))
        {
            $gambar = 'gambar/' . $filename;;
        }
    }
    $sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli,
    stok, gambar) ';
    $sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}',
    '{$harga_beli}', '{$stok}', '{$gambar}')";
         $result = mysqli_query($conn, $sql);
         header('location: index.php'); 
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tambah Barang</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Tambah Barang</h1>
        <div class="main">
            <form method="post" action="tambah.php"
enctype="multipart/form-data">
                <div class="input">
                    <label> Nama Barang</label>
                    <input type="text" name="nama"/>
                </div>
                <div class="input">
                    <label>Kategori</label>
                    <select name="kategori">
                        <option value="Komputer">Komputer</option>
                        <option value="Elektronik">Elektronik</option>
                        <option value="HandPhone">HandPhone</option>
                    </select>
                </div>
                <div class="input">
                    <label>Harga Jual</label>
                    <input type="text" name="harga_jual" />
                </div>
                <div class="input">
                    <label>Harga Beli</label>
                    <input type="text" name="harga_beli" />
                </div>
                <div class="input">
                    <label>Stok</label>
                    <input type="text" name="stok" />
                </div>
                <div class="input">
                    <label>File Gambar</label>
                    <input type="file" name="file_gambar" />
                </div>
                <div class="submit">
                    <input type="submit" name="submit" value="Simpan" />
                </div>
            </form>
        </div>
    </div>
</body>
</html>
```

## 8. Mengubah Data (UPDATE)

Buat file baru dengan nama ubah.php
![ubah](img/13.ubah.jpeg)

Mengubah isi dari data barang menjadi data barang yang baru seperti contoh gambar diatas.

![ubah](img/14.ubah2.jpeg)

maka hasilnya akan seperti dibawah

![ubah](img/15.hasilakhir.jpeg)

Diatas adalah hasil dari program ubah atau update

code php 

```php
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit']))
{
    $id = $_POST['id'];
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;

    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_', $file_gambar['name']);
        $destination = dirname(_FILE_) . '/gambar/' . $filename;
        if (move_uploaded_file($file_gambar['tmp_name'], $destination))
        {
            $gambar = 'gambar/' . $filename;;
        }    
    }

    $sql = 'UPDATE data_barang SET ';
    $sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
    $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok
    = '{$stok}' ";
    if (!empty($gambar))
         $sql .= ", gambar = '{$gambar}' ";
    $sql .= "WHERE id_barang = '{$id}'";
    $result = mysqli_query($conn, $sql);   
    
    header('location: index.php');
}

//$id = $_GET['id']; --> Ini tidak akan mendapatkan hasil atau value
$id = (isset($_POST['id']) ? $_POST['id'] : ''); // karena ingin mendapatkan form dengan methot post.
$sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
if (!$result) die('Error: Data tidak tersedia');
$data = mysqli_fetch_array($result);

function is_select($var, $val) {
    if ($var == $val) return 'selected="selected"';
    return false;
}

?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ubah Barang</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Ubah Barang</h1>
        <div class="main">
        <form method="post" action="ubah.php"
         enctype="multipart/form-data">
         <div class="input">
             <label>Nama Barang</label>
             <input type="text" name="nama" value="<?php echo
            $data['nama'];?>" />
         </div>
         <div class="input">
             <label>Kategori</label>
             <select name="kategori">
                <option <?php echo is_select
                ('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
                <option <?php echo is_select
                ('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
                <option <?php echo is_select
                ('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option>
             </select>
         </div>
         <div class="input">
             <label>Harga Jual</label>
             <input type="text" name="harga_jual" value="<?php echo
              $data['harga_jual'];?>" />
         </div>
         <div class="input">
             <label>Harga Beli</label>
             <input type="text" name="harga_beli" value="<?php echo
              $data['harga_beli'];?>" />
         </div>
         <div class="input">
             <label>Stok</label>
             <input type="text" name="stok" value="<?php echo
             $data['stok'];?>" />
         </div>
         <div class="input">
             <label>File Gambar</label>
             <input type="file" name="file_gambar" />
         </div>
         <div class="submit">
           <input type="hidden" name="id" value="<?php echo
           $data['id_barang'];?>" />
           <input type="submit" name="submit" value="Simpan" />
         </div>
     </form> 
     </div>
    </div>
</body>
</html>
```

----------------------------------------------------------------------------------------------------------

                                SEKIAN PROGRAM KALI INI TERIMAK

-----------------------------------------------------------------------------------------------------------