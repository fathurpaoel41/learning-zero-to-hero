# PERTEMUAN 2 — Function & Control Flow

## 📚 Materi

### 1. Function (Fungsi)
Function adalah blok kode yang dapat dipanggil berulang kali untuk melakukan tugas tertentu.

**Sintaks Dasar:**
```php
function namaFunction() {
    // kode yang akan dieksekusi
    return nilai; // opsional
}
```

**Contoh Function Sederhana:**
```php
function sapa() {
    echo "Halo, selamat datang!";
}

// Memanggil function
sapa(); // Output: Halo, selamat datang!
```

### 2. Function dengan Parameter
Parameter adalah data yang dikirim ke function saat dipanggil.

**Contoh:**
```php
function sapaNama($nama) {
    echo "Halo, " . $nama . "!";
}

sapaNama("Budi"); // Output: Halo, Budi!
sapaNama("Siti"); // Output: Halo, Siti!
```

**Function dengan Multiple Parameter:**
```php
function hitungTotal($harga, $jumlah) {
    $total = $harga * $jumlah;
    return $total;
}

$hasil = hitungTotal(10000, 3);
echo "Total: Rp " . $hasil; // Output: Total: Rp 30000
```

### 3. Return Statement
Return digunakan untuk mengembalikan nilai dari function.

**Contoh:**
```php
function tambah($a, $b) {
    return $a + $b;
}

$hasil = tambah(5, 3);
echo $hasil; // Output: 8
```

### 4. If/Else Statement
Digunakan untuk membuat keputusan berdasarkan kondisi.

**Sintaks:**
```php
if (kondisi) {
    // kode jika kondisi true
} else {
    // kode jika kondisi false
}
```

**Contoh:**
```php
$nilai = 75;

if ($nilai >= 70) {
    echo "Lulus";
} else {
    echo "Tidak Lulus";
}
```

**If/Else If/Else:**
```php
$nilai = 85;

if ($nilai >= 90) {
    echo "Grade A";
} else if ($nilai >= 80) {
    echo "Grade B";
} else if ($nilai >= 70) {
    echo "Grade C";
} else {
    echo "Grade D";
}
```

### 5. Switch Statement
Digunakan untuk memilih salah satu dari banyak blok kode yang akan dieksekusi.

**Sintaks:**
```php
switch (variabel) {
    case nilai1:
        // kode
        break;
    case nilai2:
        // kode
        break;
    default:
        // kode default
}
```

**Contoh:**
```php
$hari = "Senin";

switch ($hari) {
    case "Senin":
        echo "Hari kerja";
        break;
    case "Sabtu":
    case "Minggu":
        echo "Hari libur";
        break;
    default:
        echo "Hari biasa";
}
```

### 6. Loop (Perulangan)

#### a. For Loop
Digunakan ketika kita tahu berapa kali perulangan akan dilakukan.

**Sintaks:**
```php
for (inisialisasi; kondisi; increment) {
    // kode yang akan diulang
}
```

**Contoh:**
```php
for ($i = 1; $i <= 10; $i++) {
    echo $i . "<br>";
}
// Output: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
```

#### b. While Loop
Digunakan ketika kita tidak tahu berapa kali perulangan akan dilakukan.

**Sintaks:**
```php
while (kondisi) {
    // kode yang akan diulang
}
```

**Contoh:**
```php
$i = 1;
while ($i <= 5) {
    echo $i . "<br>";
    $i++;
}
// Output: 1, 2, 3, 4, 5
```

#### c. Do-While Loop
Sama seperti while, tapi kode akan dieksekusi minimal sekali.

**Sintaks:**
```php
do {
    // kode yang akan diulang
} while (kondisi);
```

**Contoh:**
```php
$i = 1;
do {
    echo $i . "<br>";
    $i++;
} while ($i <= 5);
```

#### d. Foreach Loop
Digunakan khusus untuk array.

**Sintaks:**
```php
foreach ($array as $nilai) {
    // kode
}
```

**Contoh:**
```php
$buah = ["Apel", "Jeruk", "Mangga"];

foreach ($buah as $b) {
    echo $b . "<br>";
}
// Output: Apel, Jeruk, Mangga
```

---

## 📝 Pretest (3 Soal Ringkas)

1. **Apa itu function?**
   - Jawab: Function adalah blok kode yang dapat dipanggil berulang kali untuk melakukan tugas tertentu. Function dapat menerima parameter dan mengembalikan nilai.

2. **Buat contoh penggunaan if sederhana.**
   - Jawab:
   ```php
   $umur = 18;
   if ($umur >= 17) {
       echo "Bisa membuat KTP";
   } else {
       echo "Belum bisa membuat KTP";
   }
   ```

3. **Kapan kita menggunakan perulangan?**
   - Jawab: Perulangan digunakan ketika kita perlu mengeksekusi kode yang sama berulang kali. Contoh: menampilkan data dari array, menghitung dari 1 sampai 100, dll.

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas 1: Function hitungDiskon
Buat function `hitungDiskon($harga, $persen)` yang menghitung harga setelah diskon, lalu tampilkan hasilnya.

**Contoh:**
```php
function hitungDiskon($harga, $persen) {
    $diskon = $harga * ($persen / 100);
    $hargaSetelahDiskon = $harga - $diskon;
    return $hargaSetelahDiskon;
}

$hargaAwal = 100000;
$diskonPersen = 20;
$hargaFinal = hitungDiskon($hargaAwal, $diskonPersen);

echo "Harga Awal: Rp " . $hargaAwal . "<br>";
echo "Diskon: " . $diskonPersen . "%<br>";
echo "Harga Setelah Diskon: Rp " . $hargaFinal;
```

**Output yang diharapkan:**
```
Harga Awal: Rp 100000
Diskon: 20%
Harga Setelah Diskon: Rp 80000
```

**File:** `tugas1.php`

### Tugas 2: Program Perulangan 1-10
Buat program yang menampilkan angka 1–10 menggunakan for loop.

**Contoh:**
```php
for ($i = 1; $i <= 10; $i++) {
    echo "Angka: " . $i . "<br>";
}
```

**Bonus:** Buat juga versi dengan while loop dan do-while loop.

**File:** `tugas2.php`

---

## 💡 Tips

- Function harus didefinisikan sebelum dipanggil
- Gunakan `return` jika function perlu mengembalikan nilai
- Parameter bisa memiliki default value: `function sapa($nama = "Guest")`
- Perhatikan penggunaan `break` di switch untuk mencegah fall-through
- Gunakan loop yang tepat sesuai kebutuhan (for untuk yang diketahui jumlahnya, while untuk kondisi)

---

**Selamat belajar! 🚀**

