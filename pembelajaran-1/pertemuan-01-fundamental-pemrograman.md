# PERTEMUAN 1 — Fundamental Pemrograman

## 📚 Materi

### 1. Variabel
Variabel adalah tempat untuk menyimpan data/nilai dalam program. Seperti kotak penyimpanan yang memiliki nama dan isi.

**Contoh:**
```php
$nama = "Budi";
$umur = 20;
$tinggi = 170.5;
```

### 2. Tipe Data
Tipe data menentukan jenis nilai yang bisa disimpan dalam variabel.

**Tipe Data Umum:**
- **String** - Teks/kata (contoh: "Hello", "Nama")
- **Integer** - Bilangan bulat (contoh: 10, -5, 0)
- **Float/Double** - Bilangan desimal (contoh: 3.14, 2.5)
- **Boolean** - True atau False (contoh: true, false)
- **Array** - Kumpulan data (contoh: [1, 2, 3])

**Contoh:**
```php
$nama = "Budi";        // String
$umur = 20;            // Integer
$tinggi = 170.5;       // Float
$isActive = true;      // Boolean
$hobi = ["Membaca", "Menulis", "Coding"]; // Array
```

### 3. Operator Aritmatika
Operator untuk melakukan perhitungan matematika.

| Operator | Simbol | Contoh |
|----------|--------|--------|
| Penjumlahan | + | 5 + 3 = 8 |
| Pengurangan | - | 5 - 3 = 2 |
| Perkalian | * | 5 * 3 = 15 |
| Pembagian | / | 10 / 2 = 5 |
| Modulus | % | 10 % 3 = 1 |

**Contoh:**
```php
$a = 10;
$b = 3;

$tambah = $a + $b;      // 13
$kurang = $a - $b;      // 7
$kali = $a * $b;        // 30
$bagi = $a / $b;        // 3.33...
$modulus = $a % $b;     // 1
```

### 4. Operator Logika
Operator untuk membandingkan atau menggabungkan kondisi.

| Operator | Simbol | Keterangan |
|----------|--------|------------|
| Sama dengan | == | Membandingkan nilai (tidak strict) |
| Identik | === | Membandingkan nilai dan tipe data (strict) |
| Tidak sama | != | Tidak sama dengan |
| Tidak identik | !== | Tidak identik (nilai dan tipe) |
| Lebih besar | > | Lebih besar dari |
| Lebih kecil | < | Lebih kecil dari |
| Lebih besar sama | >= | Lebih besar atau sama dengan |
| Lebih kecil sama | <= | Lebih kecil atau sama dengan |
| AND | && | Kedua kondisi harus true |
| OR | \|\| | Salah satu kondisi harus true |
| NOT | ! | Membalikkan nilai boolean |

**Contoh:**
```php
$a = 5;
$b = "5";
$c = 10;

var_dump($a == $b);   // true (nilai sama)
var_dump($a === $b);  // false (tipe berbeda)
var_dump($a > $c);    // false
var_dump($a < $c);    // true
var_dump($a >= 5);    // true
var_dump($a != $c);   // true
```

### 5. Struktur Dasar Program
Program biasanya memiliki struktur:
1. Deklarasi variabel
2. Input data
3. Proses/logika
4. Output hasil

**Contoh Program Sederhana:**
```php
<?php
// 1. Deklarasi variabel
$nama = "Budi";
$nilai1 = 80;
$nilai2 = 90;

// 2. Proses perhitungan
$rataRata = ($nilai1 + $nilai2) / 2;

// 3. Output hasil
echo "Nama: " . $nama . "<br>";
echo "Nilai 1: " . $nilai1 . "<br>";
echo "Nilai 2: " . $nilai2 . "<br>";
echo "Rata-rata: " . $rataRata;
?>
```

---

## 📝 Pretest (3 Soal Ringkas)

1. **Apa itu variabel dalam pemrograman?**
   - Jawab: Variabel adalah tempat untuk menyimpan data/nilai dalam program yang memiliki nama dan dapat diubah nilainya.

2. **Sebutkan 3 tipe data umum dalam bahasa pemrograman.**
   - Jawab: String (teks), Integer (bilangan bulat), Float (bilangan desimal), Boolean (true/false), Array (kumpulan data)

3. **Apa perbedaan == dan ===?**
   - Jawab: 
     - `==` membandingkan nilai saja (tidak strict), contoh: 5 == "5" hasilnya true
     - `===` membandingkan nilai dan tipe data (strict), contoh: 5 === "5" hasilnya false

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas 1: Variabel dengan Tipe Data Berbeda
Buat 5 variabel dengan tipe data berbeda dan tampilkan outputnya.

**Contoh Output:**
```
Nama: Budi (String)
Umur: 20 (Integer)
Tinggi: 170.5 (Float)
Status: true (Boolean)
Hobi: Array(3) { [0]=> string(7) "Membaca" [1]=> string(7) "Menulis" [2]=> string(6) "Coding" }
```

**File:** `tugas1.php`

### Tugas 2: Operasi Aritmatika Sederhana
Buat operasi aritmatika sederhana (penjumlahan, pembagian, modulus) dengan contoh:
- Penjumlahan: 15 + 25
- Pembagian: 100 / 4
- Modulus: 17 % 5

Tampilkan hasil dari setiap operasi.

**File:** `tugas2.php`

---

## 💡 Tips

- Gunakan `var_dump()` untuk melihat tipe data dan nilai variabel
- Gunakan `echo` atau `print` untuk menampilkan output
- Perhatikan penggunaan tanda kutip untuk string (single quote ' atau double quote ")
- Latih dengan membuat variasi contoh sendiri

---

**Selamat belajar! 🚀**

