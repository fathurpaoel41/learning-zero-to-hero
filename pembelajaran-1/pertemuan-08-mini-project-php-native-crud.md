# PERTEMUAN 8 — Mini Project PHP Native CRUD

## 📚 Materi

### 1. Konsep CRUD Lengkap
CRUD (Create, Read, Update, Delete) adalah operasi dasar untuk mengelola data.

**Struktur File yang Dibutuhkan:**
```
project-crud/
├── config/
│   └── database.php      # Koneksi database
├── index.php             # Read - Tampilkan semua data
├── create.php            # Form Create
├── store.php             # Proses Create
├── edit.php              # Form Edit
├── update.php            # Proses Update
└── delete.php            # Proses Delete
```

### 2. File Konfigurasi Database

**File: `config/database.php`**
```php
<?php
$host = "localhost";
$username = "root";
$password = "";
$database = "belajar";

$conn = mysqli_connect($host, $username, $password, $database);

if (!$conn) {
    die("Koneksi gagal: " . mysqli_connect_error());
}
?>
```

### 3. CREATE - Tambah Data

**File: `create.php`**
```php
<?php
require_once 'config/database.php';
?>
<!DOCTYPE html>
<html>
<head>
    <title>Tambah Mahasiswa</title>
    <style>
        body { font-family: Arial; padding: 20px; }
        form { max-width: 400px; }
        input { width: 100%; padding: 8px; margin: 5px 0; }
        button { padding: 10px 20px; background: #4CAF50; color: white; border: none; cursor: pointer; }
        a { color: #2196F3; text-decoration: none; }
    </style>
</head>
<body>
    <h2>Tambah Mahasiswa</h2>
    <form method="POST" action="store.php">
        <label>Nama:</label>
        <input type="text" name="nama" required>
        
        <label>Email:</label>
        <input type="email" name="email" required>
        
        <label>Umur:</label>
        <input type="number" name="umur" required>
        
        <button type="submit">Simpan</button>
        <a href="index.php">Batal</a>
    </form>
</body>
</html>
```

**File: `store.php`**
```php
<?php
require_once 'config/database.php';

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $nama = mysqli_real_escape_string($conn, $_POST['nama']);
    $email = mysqli_real_escape_string($conn, $_POST['email']);
    $umur = (int)$_POST['umur'];
    
    $query = "INSERT INTO mahasiswas (nama, email, umur) 
              VALUES ('$nama', '$email', $umur)";
    
    if (mysqli_query($conn, $query)) {
        header("Location: index.php?success=Data berhasil ditambahkan");
        exit();
    } else {
        header("Location: create.php?error=" . urlencode(mysqli_error($conn)));
        exit();
    }
}

mysqli_close($conn);
?>
```

### 4. READ - Tampilkan Data

**File: `index.php`**
```php
<?php
require_once 'config/database.php';

$query = "SELECT * FROM mahasiswas ORDER BY id DESC";
$result = mysqli_query($conn, $query);
?>
<!DOCTYPE html>
<html>
<head>
    <title>Data Mahasiswa</title>
    <style>
        body { font-family: Arial; padding: 20px; }
        table { border-collapse: collapse; width: 100%; margin-top: 20px; }
        th, td { border: 1px solid #ddd; padding: 12px; text-align: left; }
        th { background-color: #4CAF50; color: white; }
        tr:hover { background-color: #f5f5f5; }
        .btn { padding: 5px 10px; text-decoration: none; border-radius: 3px; }
        .btn-edit { background: #2196F3; color: white; }
        .btn-delete { background: #f44336; color: white; }
        .btn-create { background: #4CAF50; color: white; padding: 10px 20px; }
        .success { background: #d4edda; color: #155724; padding: 10px; border-radius: 5px; margin-bottom: 20px; }
    </style>
</head>
<body>
    <h2>Data Mahasiswa</h2>
    
    <?php if (isset($_GET['success'])): ?>
        <div class="success"><?php echo $_GET['success']; ?></div>
    <?php endif; ?>
    
    <a href="create.php" class="btn btn-create">Tambah Data</a>
    
    <table>
        <tr>
            <th>ID</th>
            <th>Nama</th>
            <th>Email</th>
            <th>Umur</th>
            <th>Aksi</th>
        </tr>
        <?php if (mysqli_num_rows($result) > 0): ?>
            <?php while ($row = mysqli_fetch_assoc($result)): ?>
                <tr>
                    <td><?php echo $row['id']; ?></td>
                    <td><?php echo $row['nama']; ?></td>
                    <td><?php echo $row['email']; ?></td>
                    <td><?php echo $row['umur']; ?></td>
                    <td>
                        <a href="edit.php?id=<?php echo $row['id']; ?>" class="btn btn-edit">Edit</a>
                        <a href="delete.php?id=<?php echo $row['id']; ?>" 
                           class="btn btn-delete" 
                           onclick="return confirm('Yakin ingin menghapus?')">Hapus</a>
                    </td>
                </tr>
            <?php endwhile; ?>
        <?php else: ?>
            <tr>
                <td colspan="5" style="text-align: center;">Tidak ada data</td>
            </tr>
        <?php endif; ?>
    </table>
    
    <?php mysqli_close($conn); ?>
</body>
</html>
```

### 5. UPDATE - Edit Data

**File: `edit.php`**
```php
<?php
require_once 'config/database.php';

$id = (int)$_GET['id'];
$query = "SELECT * FROM mahasiswas WHERE id = $id";
$result = mysqli_query($conn, $query);
$row = mysqli_fetch_assoc($result);

if (!$row) {
    header("Location: index.php");
    exit();
}
?>
<!DOCTYPE html>
<html>
<head>
    <title>Edit Mahasiswa</title>
    <style>
        body { font-family: Arial; padding: 20px; }
        form { max-width: 400px; }
        input { width: 100%; padding: 8px; margin: 5px 0; }
        button { padding: 10px 20px; background: #2196F3; color: white; border: none; cursor: pointer; }
        a { color: #2196F3; text-decoration: none; }
    </style>
</head>
<body>
    <h2>Edit Mahasiswa</h2>
    <form method="POST" action="update.php">
        <input type="hidden" name="id" value="<?php echo $row['id']; ?>">
        
        <label>Nama:</label>
        <input type="text" name="nama" value="<?php echo $row['nama']; ?>" required>
        
        <label>Email:</label>
        <input type="email" name="email" value="<?php echo $row['email']; ?>" required>
        
        <label>Umur:</label>
        <input type="number" name="umur" value="<?php echo $row['umur']; ?>" required>
        
        <button type="submit">Update</button>
        <a href="index.php">Batal</a>
    </form>
</body>
</html>
```

**File: `update.php`**
```php
<?php
require_once 'config/database.php';

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $id = (int)$_POST['id'];
    $nama = mysqli_real_escape_string($conn, $_POST['nama']);
    $email = mysqli_real_escape_string($conn, $_POST['email']);
    $umur = (int)$_POST['umur'];
    
    $query = "UPDATE mahasiswas 
              SET nama = '$nama', email = '$email', umur = $umur 
              WHERE id = $id";
    
    if (mysqli_query($conn, $query)) {
        header("Location: index.php?success=Data berhasil diupdate");
        exit();
    } else {
        header("Location: edit.php?id=$id&error=" . urlencode(mysqli_error($conn)));
        exit();
    }
}

mysqli_close($conn);
?>
```

### 6. DELETE - Hapus Data

**File: `delete.php`**
```php
<?php
require_once 'config/database.php';

$id = (int)$_GET['id'];

$query = "DELETE FROM mahasiswas WHERE id = $id";

if (mysqli_query($conn, $query)) {
    header("Location: index.php?success=Data berhasil dihapus");
    exit();
} else {
    header("Location: index.php?error=" . urlencode(mysqli_error($conn)));
    exit();
}

mysqli_close($conn);
?>
```

### 7. Keamanan: mysqli_real_escape_string()
Fungsi ini digunakan untuk mencegah SQL Injection dengan meng-escape karakter khusus.

**Contoh:**
```php
$nama = mysqli_real_escape_string($conn, $_POST['nama']);
// Jika input: O'Brien
// Hasil: O\'Brien (aman untuk query)
```

---

## 📝 Pretest (3 Soal Ringkas)

1. **Apa saja 4 aksi dalam CRUD?**
   - Jawab: 
     - **C**reate - Membuat data baru (INSERT)
     - **R**ead - Membaca/menampilkan data (SELECT)
     - **U**pdate - Mengubah data (UPDATE)
     - **D**elete - Menghapus data (DELETE)

2. **File apa yang dibutuhkan untuk CRUD?**
   - Jawab: 
     - File konfigurasi database (config/database.php)
     - index.php (Read - tampilkan data)
     - create.php (Form create)
     - store.php (Proses create)
     - edit.php (Form edit)
     - update.php (Proses update)
     - delete.php (Proses delete)

3. **Apa fungsi mysqli_real_escape_string()?**
   - Jawab: `mysqli_real_escape_string()` digunakan untuk mencegah SQL Injection dengan meng-escape karakter khusus dalam string sebelum digunakan dalam query SQL. Fungsi ini membuat input user aman untuk digunakan dalam query.

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas: Selesaikan CRUD Mahasiswa Lengkap
Buat aplikasi CRUD lengkap untuk mengelola data mahasiswa dengan fitur:
- ✅ Create - Tambah data mahasiswa baru
- ✅ Read - Tampilkan semua data mahasiswa
- ✅ Update - Edit data mahasiswa
- ✅ Delete - Hapus data mahasiswa

**Struktur Folder:**
```
project-crud/
├── config/
│   └── database.php
├── index.php
├── create.php
├── store.php
├── edit.php
├── update.php
└── delete.php
```

**Fitur Tambahan:**
- Pesan sukses/error setelah operasi
- Konfirmasi sebelum delete
- Validasi form (required fields)
- Styling CSS sederhana
- Link navigasi antar halaman

**Contoh Alur:**
1. Buka `index.php` → Tampilkan semua data
2. Klik "Tambah Data" → Form di `create.php`
3. Submit form → Proses di `store.php` → Redirect ke `index.php`
4. Klik "Edit" → Form di `edit.php` dengan data terisi
5. Submit form → Proses di `update.php` → Redirect ke `index.php`
6. Klik "Hapus" → Konfirmasi → Proses di `delete.php` → Redirect ke `index.php`

**Kriteria Penilaian:**
- ✅ Semua operasi CRUD berfungsi
- ✅ Koneksi database terpisah (config)
- ✅ Menggunakan mysqli_real_escape_string()
- ✅ Ada validasi form
- ✅ Ada pesan sukses/error
- ✅ UI rapi dan mudah digunakan

---

## 💡 Tips

- Pisahkan koneksi database ke file terpisah (config)
- Selalu gunakan `mysqli_real_escape_string()` untuk input user
- Gunakan `header("Location: ...")` untuk redirect setelah operasi
- Tambahkan konfirmasi JavaScript untuk delete
- Validasi input di form (required, type, dll)
- Tampilkan pesan sukses/error untuk feedback ke user
- Gunakan CSS untuk mempercantik tampilan

---

**Selamat belajar! 🚀**

