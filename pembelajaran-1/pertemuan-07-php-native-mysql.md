# PERTEMUAN 7 — PHP Native + MySQL

## 📚 Materi

### 1. Koneksi Database dengan MySQLi

**MySQLi (MySQL Improved)** adalah extension PHP untuk berinteraksi dengan database MySQL.

**Membuat Koneksi:**
```php
<?php
$host = "localhost";
$username = "root";
$password = "";
$database = "belajar";

// Membuat koneksi
$conn = mysqli_connect($host, $username, $password, $database);

// Cek koneksi
if (!$conn) {
    die("Koneksi gagal: " . mysqli_connect_error());
}

echo "Koneksi berhasil!";
?>
```

**Atau dengan Error Handling:**
```php
<?php
$conn = mysqli_connect("localhost", "root", "", "belajar");

if (mysqli_connect_errno()) {
    echo "Koneksi database gagal: " . mysqli_connect_error();
    exit();
}
?>
```

### 2. Menutup Koneksi
```php
mysqli_close($conn);
```

### 3. SELECT Data (Fetch Data)

**Mengambil Semua Data:**
```php
<?php
$conn = mysqli_connect("localhost", "root", "", "belajar");

// Query SELECT
$query = "SELECT * FROM mahasiswas";
$result = mysqli_query($conn, $query);

// Cek apakah query berhasil
if ($result) {
    // Fetch data satu per satu
    while ($row = mysqli_fetch_assoc($result)) {
        echo "ID: " . $row['id'] . "<br>";
        echo "Nama: " . $row['nama'] . "<br>";
        echo "Email: " . $row['email'] . "<br>";
        echo "Umur: " . $row['umur'] . "<br>";
        echo "<hr>";
    }
} else {
    echo "Error: " . mysqli_error($conn);
}

mysqli_close($conn);
?>
```

**Fungsi Fetch:**
- `mysqli_fetch_assoc()` - Mengembalikan array associative
- `mysqli_fetch_array()` - Mengembalikan array numeric dan associative
- `mysqli_fetch_row()` - Mengembalikan array numeric
- `mysqli_fetch_object()` - Mengembalikan object

**Contoh dengan mysqli_fetch_assoc():**
```php
while ($row = mysqli_fetch_assoc($result)) {
    echo $row['nama'] . " - " . $row['email'] . "<br>";
}
```

**Menghitung Jumlah Data:**
```php
$num_rows = mysqli_num_rows($result);
echo "Total data: " . $num_rows;
```

### 4. INSERT Data dari PHP

**Insert Data Sederhana:**
```php
<?php
$conn = mysqli_connect("localhost", "root", "", "belajar");

$nama = "Budi Santoso";
$email = "budi@example.com";
$umur = 20;

$query = "INSERT INTO mahasiswas (nama, email, umur) 
          VALUES ('$nama', '$email', $umur)";

if (mysqli_query($conn, $query)) {
    echo "Data berhasil ditambahkan!";
} else {
    echo "Error: " . mysqli_error($conn);
}

mysqli_close($conn);
?>
```

**⚠️ PENTING: Gunakan mysqli_real_escape_string() untuk keamanan!**

```php
$nama = mysqli_real_escape_string($conn, $_POST['nama']);
$email = mysqli_real_escape_string($conn, $_POST['email']);
$umur = (int)$_POST['umur']; // Cast ke integer
```

### 5. Form dengan Method POST

**File: form_mahasiswa.php**
```php
<!DOCTYPE html>
<html>
<head>
    <title>Form Input Mahasiswa</title>
</head>
<body>
    <h2>Form Input Mahasiswa</h2>
    <form method="POST" action="proses_tambah.php">
        <label>Nama:</label><br>
        <input type="text" name="nama" required><br><br>
        
        <label>Email:</label><br>
        <input type="email" name="email" required><br><br>
        
        <label>Umur:</label><br>
        <input type="number" name="umur" required><br><br>
        
        <button type="submit">Simpan</button>
    </form>
</body>
</html>
```

**File: proses_tambah.php**
```php
<?php
$conn = mysqli_connect("localhost", "root", "", "belajar");

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    // Ambil data dari form
    $nama = mysqli_real_escape_string($conn, $_POST['nama']);
    $email = mysqli_real_escape_string($conn, $_POST['email']);
    $umur = (int)$_POST['umur'];
    
    // Query INSERT
    $query = "INSERT INTO mahasiswas (nama, email, umur) 
              VALUES ('$nama', '$email', $umur)";
    
    if (mysqli_query($conn, $query)) {
        echo "Data berhasil ditambahkan!<br>";
        echo "<a href='tampil_mahasiswa.php'>Lihat Data</a>";
    } else {
        echo "Error: " . mysqli_error($conn);
    }
}

mysqli_close($conn);
?>
```

### 6. UPDATE Data

```php
<?php
$conn = mysqli_connect("localhost", "root", "", "belajar");

$id = 1;
$nama = "Budi Santoso Updated";
$umur = 22;

$query = "UPDATE mahasiswas 
          SET nama = '$nama', umur = $umur 
          WHERE id = $id";

if (mysqli_query($conn, $query)) {
    echo "Data berhasil diupdate!";
} else {
    echo "Error: " . mysqli_error($conn);
}

mysqli_close($conn);
?>
```

### 7. DELETE Data

```php
<?php
$conn = mysqli_connect("localhost", "root", "", "belajar");

$id = 1;

$query = "DELETE FROM mahasiswas WHERE id = $id";

if (mysqli_query($conn, $query)) {
    echo "Data berhasil dihapus!";
} else {
    echo "Error: " . mysqli_error($conn);
}

mysqli_close($conn);
?>
```

### 8. Contoh Lengkap: Tampilkan Data

**File: tampil_mahasiswa.php**
```php
<?php
$conn = mysqli_connect("localhost", "root", "", "belajar");

if (!$conn) {
    die("Koneksi gagal: " . mysqli_connect_error());
}
?>
<!DOCTYPE html>
<html>
<head>
    <title>Data Mahasiswa</title>
</head>
<body>
    <h2>Data Mahasiswa</h2>
    <a href="form_mahasiswa.php">Tambah Data</a><br><br>
    
    <table border="1" cellpadding="10">
        <tr>
            <th>ID</th>
            <th>Nama</th>
            <th>Email</th>
            <th>Umur</th>
        </tr>
        <?php
        $query = "SELECT * FROM mahasiswas";
        $result = mysqli_query($conn, $query);
        
        if (mysqli_num_rows($result) > 0) {
            while ($row = mysqli_fetch_assoc($result)) {
                echo "<tr>";
                echo "<td>" . $row['id'] . "</td>";
                echo "<td>" . $row['nama'] . "</td>";
                echo "<td>" . $row['email'] . "</td>";
                echo "<td>" . $row['umur'] . "</td>";
                echo "</tr>";
            }
        } else {
            echo "<tr><td colspan='4'>Tidak ada data</td></tr>";
        }
        ?>
    </table>
    
    <?php mysqli_close($conn); ?>
</body>
</html>
```

---

## 📝 Pretest (3 Soal Ringkas)

1. **Fungsi mysqli_connect() untuk apa?**
   - Jawab: `mysqli_connect()` digunakan untuk membuat koneksi ke database MySQL. Fungsi ini memerlukan parameter: host, username, password, dan nama database.

2. **Bagaimana mengambil data dengan mysqli_query()?**
   - Jawab: 
   ```php
   $query = "SELECT * FROM mahasiswas";
   $result = mysqli_query($conn, $query);
   while ($row = mysqli_fetch_assoc($result)) {
       // proses data
   }
   ```

3. **Apa itu POST?**
   - Jawab: POST adalah method HTTP yang digunakan untuk mengirim data dari form ke server. Data POST tidak terlihat di URL dan lebih aman untuk data sensitif. Di PHP, data POST diakses melalui `$_POST`.

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas 1: Halaman PHP Menampilkan Semua Data Mahasiswa
Buat halaman PHP yang menampilkan semua data mahasiswa dalam bentuk tabel.

**File: `tampil_mahasiswa.php`**

**Contoh:**
```php
<?php
$conn = mysqli_connect("localhost", "root", "", "belajar");

if (!$conn) {
    die("Koneksi gagal: " . mysqli_connect_error());
}
?>
<!DOCTYPE html>
<html>
<head>
    <title>Data Mahasiswa</title>
    <style>
        table { border-collapse: collapse; width: 100%; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
        th { background-color: #4CAF50; color: white; }
    </style>
</head>
<body>
    <h2>Data Mahasiswa</h2>
    <table>
        <tr>
            <th>ID</th>
            <th>Nama</th>
            <th>Email</th>
            <th>Umur</th>
        </tr>
        <?php
        $query = "SELECT * FROM mahasiswas";
        $result = mysqli_query($conn, $query);
        
        if (mysqli_num_rows($result) > 0) {
            while ($row = mysqli_fetch_assoc($result)) {
                echo "<tr>";
                echo "<td>" . $row['id'] . "</td>";
                echo "<td>" . $row['nama'] . "</td>";
                echo "<td>" . $row['email'] . "</td>";
                echo "<td>" . $row['umur'] . "</td>";
                echo "</tr>";
            }
        } else {
            echo "<tr><td colspan='4'>Tidak ada data</td></tr>";
        }
        ?>
    </table>
    <?php mysqli_close($conn); ?>
</body>
</html>
```

### Tugas 2: Form Input Mahasiswa Baru
Tambahkan form input mahasiswa baru yang terhubung dengan database.

**File: `form_mahasiswa.php`**
```php
<!DOCTYPE html>
<html>
<head>
    <title>Form Input Mahasiswa</title>
</head>
<body>
    <h2>Form Input Mahasiswa</h2>
    <form method="POST" action="proses_tambah.php">
        <label>Nama:</label><br>
        <input type="text" name="nama" required><br><br>
        
        <label>Email:</label><br>
        <input type="email" name="email" required><br><br>
        
        <label>Umur:</label><br>
        <input type="number" name="umur" required><br><br>
        
        <button type="submit">Simpan</button>
        <a href="tampil_mahasiswa.php">Kembali</a>
    </form>
</body>
</html>
```

**File: `proses_tambah.php`**
```php
<?php
$conn = mysqli_connect("localhost", "root", "", "belajar");

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $nama = mysqli_real_escape_string($conn, $_POST['nama']);
    $email = mysqli_real_escape_string($conn, $_POST['email']);
    $umur = (int)$_POST['umur'];
    
    $query = "INSERT INTO mahasiswas (nama, email, umur) 
              VALUES ('$nama', '$email', $umur)";
    
    if (mysqli_query($conn, $query)) {
        header("Location: tampil_mahasiswa.php");
        exit();
    } else {
        echo "Error: " . mysqli_error($conn);
    }
}

mysqli_close($conn);
?>
```

---

## 💡 Tips

- Selalu gunakan `mysqli_real_escape_string()` untuk mencegah SQL Injection
- Selalu tutup koneksi dengan `mysqli_close()`
- Cek koneksi sebelum melakukan query
- Gunakan `mysqli_num_rows()` untuk mengecek apakah ada data
- Gunakan `header("Location: ...")` untuk redirect setelah insert/update
- Validasi input dari form sebelum disimpan ke database

---

**Selamat belajar! 🚀**

