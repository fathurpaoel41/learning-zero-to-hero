# PERTEMUAN 13 — JavaScript Fundamental

## 📚 Materi

### 1. Pengenalan JavaScript
JavaScript adalah bahasa pemrograman yang digunakan untuk membuat website interaktif. JavaScript berjalan di browser (client-side).

**Cara Menambahkan JavaScript:**
```html
<!-- Inline -->
<script>
    console.log('Hello JavaScript');
</script>

<!-- External -->
<script src="script.js"></script>
```

### 2. Variable

**var, let, const:**
```javascript
// var (old way, tidak disarankan)
var nama = "Budi";

// let (bisa diubah)
let umur = 20;
umur = 21; // Bisa diubah

// const (tidak bisa diubah)
const PI = 3.14;
// PI = 3.15; // Error!
```

**Perbedaan let dan const:**
- `let` - Bisa diubah nilainya
- `const` - Tidak bisa diubah (constant)

**Contoh:**
```javascript
let counter = 0;
counter = 1; // OK

const maxUsers = 100;
// maxUsers = 200; // Error!
```

### 3. Tipe Data

**Primitive Types:**
```javascript
// String
let nama = "Budi";
let alamat = 'Jakarta';

// Number
let umur = 20;
let harga = 99.99;

// Boolean
let isActive = true;
let isDeleted = false;

// Undefined
let x;
console.log(x); // undefined

// Null
let y = null;
```

**Type Checking:**
```javascript
typeof "Hello";    // "string"
typeof 42;         // "number"
typeof true;       // "boolean"
typeof undefined;  // "undefined"
typeof null;       // "object" (bug JavaScript)
```

### 4. Array

**Membuat Array:**
```javascript
// Cara 1
let buah = ["Apel", "Jeruk", "Mangga"];

// Cara 2
let angka = new Array(1, 2, 3);

// Array kosong
let kosong = [];
```

**Mengakses Array:**
```javascript
let buah = ["Apel", "Jeruk", "Mangga"];

console.log(buah[0]);  // "Apel"
console.log(buah[1]);  // "Jeruk"
console.log(buah.length); // 3
```

**Array Methods:**
```javascript
let buah = ["Apel", "Jeruk"];

// Push (tambah di akhir)
buah.push("Mangga");
// ["Apel", "Jeruk", "Mangga"]

// Pop (hapus dari akhir)
buah.pop();
// ["Apel", "Jeruk"]

// Shift (hapus dari awal)
buah.shift();
// ["Jeruk"]

// Unshift (tambah di awal)
buah.unshift("Pisang");
// ["Pisang", "Jeruk"]

// IndexOf
buah.indexOf("Jeruk"); // 1

// Includes
buah.includes("Apel"); // true
```

### 5. Object

**Membuat Object:**
```javascript
// Object literal
let mahasiswa = {
    nama: "Budi",
    umur: 20,
    jurusan: "Informatika"
};

// Mengakses property
console.log(mahasiswa.nama);     // "Budi"
console.log(mahasiswa["umur"]);  // 20

// Menambah property
mahasiswa.email = "budi@example.com";

// Menghapus property
delete mahasiswa.jurusan;
```

**Object dengan Method:**
```javascript
let person = {
    nama: "Budi",
    umur: 20,
    sapa: function() {
        return "Halo, saya " + this.nama;
    },
    // ES6 shorthand
    perkenalan() {
        return `Nama saya ${this.nama}, umur ${this.umur}`;
    }
};

console.log(person.sapa()); // "Halo, saya Budi"
```

### 6. Function

**Function Declaration:**
```javascript
function sapa(nama) {
    return "Halo, " + nama;
}

console.log(sapa("Budi")); // "Halo, Budi"
```

**Function Expression:**
```javascript
const sapa = function(nama) {
    return "Halo, " + nama;
};
```

**Arrow Function (ES6):**
```javascript
const sapa = (nama) => {
    return "Halo, " + nama;
};

// Single line
const sapa = (nama) => "Halo, " + nama;

// Single parameter (bisa tanpa kurung)
const sapa = nama => "Halo, " + nama;
```

**Function dengan Default Parameter:**
```javascript
function sapa(nama = "Guest") {
    return "Halo, " + nama;
}

console.log(sapa());        // "Halo, Guest"
console.log(sapa("Budi"));  // "Halo, Budi"
```

### 7. Loop

**for Loop:**
```javascript
for (let i = 0; i < 10; i++) {
    console.log(i);
}
// Output: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
```

**while Loop:**
```javascript
let i = 0;
while (i < 5) {
    console.log(i);
    i++;
}
```

**for...of (Array):**
```javascript
let buah = ["Apel", "Jeruk", "Mangga"];

for (let b of buah) {
    console.log(b);
}
// Output: Apel, Jeruk, Mangga
```

**for...in (Object):**
```javascript
let person = { nama: "Budi", umur: 20 };

for (let key in person) {
    console.log(key + ": " + person[key]);
}
// Output: nama: Budi, umur: 20
```

**Array Methods (forEach, map, filter):**
```javascript
let angka = [1, 2, 3, 4, 5];

// forEach
angka.forEach(function(num) {
    console.log(num);
});

// map (mengubah array)
let doubled = angka.map(num => num * 2);
// [2, 4, 6, 8, 10]

// filter (menyaring array)
let genap = angka.filter(num => num % 2 === 0);
// [2, 4]
```

### 8. Logika (If/Else, Switch)

**If/Else:**
```javascript
let umur = 20;

if (umur >= 18) {
    console.log("Dewasa");
} else {
    console.log("Anak-anak");
}

// Ternary operator
let status = umur >= 18 ? "Dewasa" : "Anak-anak";
```

**Switch:**
```javascript
let hari = "Senin";

switch (hari) {
    case "Senin":
        console.log("Hari kerja");
        break;
    case "Sabtu":
    case "Minggu":
        console.log("Hari libur");
        break;
    default:
        console.log("Hari biasa");
}
```

**Operator Perbandingan:**
```javascript
// Sama dengan
5 == "5"   // true (loose equality)
5 === "5"  // false (strict equality)

// Tidak sama
5 != "5"   // false
5 !== "5"  // true

// Lebih besar/kecil
5 > 3      // true
5 < 3      // false
5 >= 5     // true
5 <= 3     // false

// Logika
true && false  // false (AND)
true || false  // true (OR)
!true          // false (NOT)
```

### 9. Template Literal (ES6)

```javascript
let nama = "Budi";
let umur = 20;

// Old way
let pesan = "Nama: " + nama + ", Umur: " + umur;

// Template literal
let pesan = `Nama: ${nama}, Umur: ${umur}`;

// Multi-line
let html = `
    <div>
        <h1>${nama}</h1>
        <p>Umur: ${umur}</p>
    </div>
`;
```

---

## 📝 Pretest (3 Soal Ringkas)

1. **Apa itu array?**
   - Jawab: Array adalah struktur data yang menyimpan kumpulan nilai dalam satu variabel. Array di JavaScript bisa menyimpan berbagai tipe data dan diakses menggunakan index (dimulai dari 0).

2. **Apa bedanya let dan const?**
   - Jawab: 
     - `let` - Variabel yang bisa diubah nilainya setelah dideklarasikan
     - `const` - Variabel yang tidak bisa diubah nilainya setelah dideklarasikan (constant). Harus langsung diberi nilai saat deklarasi.

3. **Buat function sederhana.**
   - Jawab:
   ```javascript
   function tambah(a, b) {
       return a + b;
   }
   
   console.log(tambah(5, 3)); // 8
   ```

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas 1: Program Menampilkan Angka Ganjil 1-20
Buat program JavaScript yang menampilkan angka ganjil dari 1 sampai 20.

**Contoh:**
```javascript
// Menggunakan for loop
for (let i = 1; i <= 20; i++) {
    if (i % 2 !== 0) {
        console.log(i);
    }
}

// Atau lebih efisien
for (let i = 1; i <= 20; i += 2) {
    console.log(i);
}
```

**Output yang diharapkan:**
```
1
3
5
7
9
11
13
15
17
19
```

**File:** `tugas1.html` atau `tugas1.js`

### Tugas 2: Array Barang dan Tampilkan Nama
Buat array barang lalu tampilkan semua nama barang.

**Contoh:**
```javascript
let barang = [
    { nama: "Laptop", harga: 5000000 },
    { nama: "Mouse", harga: 50000 },
    { nama: "Keyboard", harga: 200000 },
    { nama: "Monitor", harga: 2000000 }
];

// Tampilkan semua nama barang
console.log("Daftar Barang:");
barang.forEach(function(item) {
    console.log("- " + item.nama);
});

// Atau dengan for...of
for (let item of barang) {
    console.log(item.nama);
}

// Atau dengan map
let namaBarang = barang.map(item => item.nama);
console.log(namaBarang);
```

**Output yang diharapkan:**
```
Daftar Barang:
- Laptop
- Mouse
- Keyboard
- Monitor
```

**Bonus:**
- Tampilkan juga harga setiap barang
- Hitung total harga semua barang
- Cari barang dengan harga termahal

**File:** `tugas2.html` atau `tugas2.js`

---

## 💡 Tips

- Gunakan `const` untuk nilai yang tidak berubah, `let` untuk yang berubah
- Array index dimulai dari 0
- Gunakan `===` untuk perbandingan strict (lebih aman)
- Template literal (`${}`) lebih mudah dibaca dari concatenation
- Arrow function lebih ringkas untuk function sederhana
- `forEach`, `map`, `filter` lebih modern dari for loop tradisional
- Selalu gunakan `console.log()` untuk debugging

---

**Selamat belajar! 🚀**

