# PERTEMUAN 3 — PHP Native Dasar

## 📚 Materi

### 1. Struktur PHP
File PHP harus dimulai dengan tag pembuka `<?php` dan ditutup dengan `?>` (opsional jika file hanya berisi PHP).

**Struktur Dasar:**
```php
<?php
// Kode PHP di sini
?>
```

**File PHP Murni (tanpa HTML):**
```php
<?php
// Tidak perlu tag penutup ?>
```

### 2. Syntax Dasar PHP
- Setiap statement diakhiri dengan titik koma (`;`)
- Case-sensitive untuk nama variabel
- Tidak case-sensitive untuk nama function
- Komentar menggunakan `//` untuk single line atau `/* */` untuk multi-line

**Contoh:**
```php
<?php
// Ini komentar single line

/*
Ini komentar
multi-line
*/

$nama = "Budi"; // Variabel case-sensitive
$NAMA = "Siti"; // Berbeda dengan $nama

echo "Hello"; // Function tidak case-sensitive
ECHO "World"; // Sama dengan echo
?>
```

### 3. Echo dan Print
Digunakan untuk menampilkan output ke browser.

**Echo:**
```php
echo "Hello World";
echo "Nama: " . $nama; // Concatenation dengan titik (.)
echo "Umur: ", $umur; // Multiple parameter dengan koma
```

**Print:**
```php
print "Hello World";
print "Nama: " . $nama;
```

**Perbedaan:**
- `echo` bisa menampilkan multiple string, `print` hanya satu
- `echo` sedikit lebih cepat
- `print` mengembalikan nilai 1, `echo` tidak mengembalikan nilai

### 4. Tipe Data PHP

#### a. String
```php
$nama = "Budi";
$alamat = 'Jakarta'; // Single quote juga bisa
$kalimat = "Nama saya adalah $nama"; // Variable interpolation dengan double quote
```

**Fungsi String Umum:**
```php
$text = "Hello World";

echo strlen($text);        // 11 (panjang string)
echo strtoupper($text);    // HELLO WORLD
echo strtolower($text);    // hello world
echo substr($text, 0, 5); // Hello
echo str_replace("World", "PHP", $text); // Hello PHP
```

#### b. Integer
```php
$umur = 20;
$negatif = -10;
$hex = 0xFF; // Hexadecimal
$octal = 0777; // Octal
```

#### c. Float (Double)
```php
$tinggi = 170.5;
$berat = 65.75;
$pi = 3.14159;
```

#### d. Boolean
```php
$isActive = true;
$isDeleted = false;
```

#### e. Array
```php
// Array Indexed
$buah = array("Apel", "Jeruk", "Mangga");
// atau
$buah = ["Apel", "Jeruk", "Mangga"];

// Array Associative
$mahasiswa = [
    "nama" => "Budi",
    "umur" => 20,
    "jurusan" => "Informatika"
];

// Array Multidimensional
$data = [
    ["nama" => "Budi", "nilai" => 85],
    ["nama" => "Siti", "nilai" => 90]
];
```

**Mengakses Array:**
```php
echo $buah[0];              // Apel
echo $mahasiswa["nama"];    // Budi
echo $data[0]["nama"];      // Budi
```

#### f. NULL
```php
$kosong = null;
```

### 5. Variable Scope
- **Local** - Hanya bisa diakses dalam function
- **Global** - Bisa diakses di mana saja
- **Static** - Variabel lokal yang mempertahankan nilainya

**Contoh:**
```php
$global = "Ini global";

function test() {
    $local = "Ini local";
    global $global; // Mengakses variabel global
    echo $global;
    echo $local;
}

test();
```

### 6. Superglobals
Variabel built-in PHP yang bisa diakses di mana saja.

- `$_GET` - Data dari URL
- `$_POST` - Data dari form POST
- `$_SESSION` - Session data
- `$_COOKIE` - Cookie data
- `$_SERVER` - Server information

---

## 📝 Pretest (3 Soal Ringkas)

1. **Bagaimana cara membuka tag PHP?**
   - Jawab: Menggunakan `<?php` untuk membuka tag PHP. Tag penutup `?>` opsional jika file hanya berisi PHP.

2. **Apa fungsi echo?**
   - Jawab: `echo` digunakan untuk menampilkan output ke browser. Bisa menampilkan string, variabel, atau hasil ekspresi.

3. **Buat variabel PHP berisi nama Anda.**
   - Jawab:
   ```php
   $nama = "Nama Saya";
   echo $nama;
   ```

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas 1: Biodata Sederhana
Buat file PHP yang menampilkan biodata sederhana dengan informasi:
- Nama
- Umur
- Alamat
- Hobi (minimal 3)

**Contoh:**
```php
<?php
$nama = "Budi Santoso";
$umur = 20;
$alamat = "Jakarta";
$hobi = ["Membaca", "Menulis", "Coding"];

echo "<h1>Biodata</h1>";
echo "Nama: " . $nama . "<br>";
echo "Umur: " . $umur . " tahun<br>";
echo "Alamat: " . $alamat . "<br>";
echo "Hobi: ";
foreach ($hobi as $h) {
    echo $h . ", ";
}
?>
```

**File:** `biodata.php`

### Tugas 2: Array dan Menampilkan Nilai
Gunakan tipe data array dan tampilkan salah satu nilai.

**Contoh:**
```php
<?php
// Array Indexed
$buah = ["Apel", "Jeruk", "Mangga", "Pisang"];

echo "Buah favorit: " . $buah[2] . "<br>"; // Mangga

// Array Associative
$mahasiswa = [
    "nama" => "Budi",
    "nim" => "12345",
    "jurusan" => "Informatika"
];

echo "Nama: " . $mahasiswa["nama"] . "<br>";
echo "NIM: " . $mahasiswa["nim"] . "<br>";
echo "Jurusan: " . $mahasiswa["jurusan"] . "<br>";

// Tampilkan semua nilai array
echo "<br>Semua buah:<br>";
foreach ($buah as $index => $b) {
    echo ($index + 1) . ". " . $b . "<br>";
}
?>
```

**File:** `array.php`

---

## 💡 Tips

- Selalu gunakan titik koma (`;`) di akhir statement
- Gunakan `var_dump()` atau `print_r()` untuk debugging array
- Perhatikan perbedaan single quote dan double quote untuk string
- Array di PHP sangat fleksibel, bisa campur tipe data
- Gunakan `isset()` untuk mengecek apakah variabel sudah diset

---

**Selamat belajar! 🚀**

