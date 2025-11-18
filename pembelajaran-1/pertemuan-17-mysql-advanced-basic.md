# PERTEMUAN 17 — MySQL Advanced Basic

## 📚 Materi

### 1. WHERE Clause Lanjutan

**Operator Perbandingan:**
```sql
-- Sama dengan
SELECT * FROM mahasiswas WHERE umur = 20;

-- Tidak sama dengan
SELECT * FROM mahasiswas WHERE umur != 20;
SELECT * FROM mahasiswas WHERE umur <> 20;

-- Lebih besar/kecil
SELECT * FROM mahasiswas WHERE umur > 20;
SELECT * FROM mahasiswas WHERE umur < 20;
SELECT * FROM mahasiswas WHERE umur >= 20;
SELECT * FROM mahasiswas WHERE umur <= 20;

-- BETWEEN (rentang)
SELECT * FROM mahasiswas WHERE umur BETWEEN 18 AND 25;

-- IN (dalam list)
SELECT * FROM mahasiswas WHERE umur IN (20, 21, 22);

-- NOT IN
SELECT * FROM mahasiswas WHERE umur NOT IN (20, 21, 22);

-- IS NULL
SELECT * FROM mahasiswas WHERE email IS NULL;

-- IS NOT NULL
SELECT * FROM mahasiswas WHERE email IS NOT NULL;
```

**Operator Logika:**
```sql
-- AND
SELECT * FROM mahasiswas WHERE umur > 20 AND umur < 30;

-- OR
SELECT * FROM mahasiswas WHERE umur < 18 OR umur > 25;

-- NOT
SELECT * FROM mahasiswas WHERE NOT umur = 20;
```

### 2. LIKE Operator

**LIKE** digunakan untuk pencarian pattern dalam string.

**Wildcards:**
- `%` - Mencocokkan nol atau lebih karakter
- `_` - Mencocokkan tepat satu karakter

**Contoh:**
```sql
-- Nama dimulai dengan 'B'
SELECT * FROM mahasiswas WHERE nama LIKE 'B%';

-- Nama berakhiran 'o'
SELECT * FROM mahasiswas WHERE nama LIKE '%o';

-- Nama mengandung 'Santoso'
SELECT * FROM mahasiswas WHERE nama LIKE '%Santoso%';

-- Nama dengan 5 karakter
SELECT * FROM mahasiswas WHERE nama LIKE '_____';

-- Nama dimulai dengan 'B' dan 5 karakter
SELECT * FROM mahasiswas WHERE nama LIKE 'B____';

-- NOT LIKE
SELECT * FROM mahasiswas WHERE nama NOT LIKE '%Santoso%';
```

**Case Sensitivity:**
- MySQL default case-insensitive untuk string comparison
- Gunakan `BINARY` untuk case-sensitive:
```sql
SELECT * FROM mahasiswas WHERE BINARY nama LIKE 'B%';
```

### 3. ORDER BY

**ORDER BY** digunakan untuk mengurutkan hasil query.

**Ascending (A-Z, kecil ke besar):**
```sql
SELECT * FROM mahasiswas ORDER BY nama ASC;
SELECT * FROM mahasiswas ORDER BY nama; -- ASC adalah default
```

**Descending (Z-A, besar ke kecil):**
```sql
SELECT * FROM mahasiswas ORDER BY nama DESC;
SELECT * FROM mahasiswas ORDER BY umur DESC;
```

**Multiple Columns:**
```sql
-- Urutkan berdasarkan umur (DESC), lalu nama (ASC)
SELECT * FROM mahasiswas ORDER BY umur DESC, nama ASC;
```

**ORDER BY dengan Expression:**
```sql
-- Urutkan berdasarkan panjang nama
SELECT * FROM mahasiswas ORDER BY LENGTH(nama) DESC;
```

### 4. JOIN Dasar

**JOIN** digunakan untuk menggabungkan data dari beberapa tabel.

**Jenis JOIN:**
- **INNER JOIN** - Hanya data yang cocok di kedua tabel
- **LEFT JOIN** - Semua data dari tabel kiri + data yang cocok dari tabel kanan
- **RIGHT JOIN** - Semua data dari tabel kanan + data yang cocok dari tabel kiri
- **FULL OUTER JOIN** - Semua data dari kedua tabel (tidak didukung MySQL, gunakan UNION)

**Contoh Tabel:**

**Tabel: mahasiswas**
```
id | nama      | jurusan_id
1  | Budi      | 1
2  | Siti      | 2
3  | Ahmad     | 1
```

**Tabel: jurusans**
```
id | nama
1  | Informatika
2  | Teknik Sipil
3  | Arsitektur
```

**INNER JOIN:**
```sql
SELECT 
    mahasiswas.nama AS nama_mahasiswa,
    jurusans.nama AS nama_jurusan
FROM mahasiswas
INNER JOIN jurusans ON mahasiswas.jurusan_id = jurusans.id;
```

**Hasil:**
```
nama_mahasiswa | nama_jurusan
Budi           | Informatika
Siti           | Teknik Sipil
Ahmad          | Informatika
```

**LEFT JOIN:**
```sql
SELECT 
    mahasiswas.nama AS nama_mahasiswa,
    jurusans.nama AS nama_jurusan
FROM mahasiswas
LEFT JOIN jurusans ON mahasiswas.jurusan_id = jurusans.id;
```

**Hasil (termasuk mahasiswa tanpa jurusan):**
```
nama_mahasiswa | nama_jurusan
Budi           | Informatika
Siti           | Teknik Sipil
Ahmad          | Informatika
```

**Alias untuk Table:**
```sql
SELECT 
    m.nama AS nama_mahasiswa,
    j.nama AS nama_jurusan
FROM mahasiswas m
INNER JOIN jurusans j ON m.jurusan_id = j.id;
```

**Multiple JOIN:**
```sql
-- Jika ada tabel lain, misal dosens
SELECT 
    m.nama AS mahasiswa,
    j.nama AS jurusan,
    d.nama AS dosen
FROM mahasiswas m
INNER JOIN jurusans j ON m.jurusan_id = j.id
LEFT JOIN dosens d ON j.dosen_id = d.id;
```

### 5. Aggregate Functions

**Fungsi Agregat:**
```sql
-- COUNT - Menghitung jumlah baris
SELECT COUNT(*) FROM mahasiswas;
SELECT COUNT(*) AS total FROM mahasiswas WHERE umur > 20;

-- SUM - Menjumlahkan nilai
SELECT SUM(umur) AS total_umur FROM mahasiswas;

-- AVG - Rata-rata
SELECT AVG(umur) AS rata_umur FROM mahasiswas;

-- MIN - Nilai minimum
SELECT MIN(umur) AS umur_terendah FROM mahasiswas;

-- MAX - Nilai maksimum
SELECT MAX(umur) AS umur_tertinggi FROM mahasiswas;
```

**GROUP BY:**
```sql
-- Hitung jumlah mahasiswa per jurusan
SELECT 
    jurusan_id,
    COUNT(*) AS jumlah_mahasiswa
FROM mahasiswas
GROUP BY jurusan_id;

-- Dengan JOIN untuk nama jurusan
SELECT 
    j.nama AS jurusan,
    COUNT(*) AS jumlah_mahasiswa
FROM mahasiswas m
INNER JOIN jurusans j ON m.jurusan_id = j.id
GROUP BY j.id, j.nama;
```

**HAVING (Filter untuk GROUP BY):**
```sql
-- Jurusan dengan lebih dari 5 mahasiswa
SELECT 
    j.nama AS jurusan,
    COUNT(*) AS jumlah_mahasiswa
FROM mahasiswas m
INNER JOIN jurusans j ON m.jurusan_id = j.id
GROUP BY j.id, j.nama
HAVING COUNT(*) > 5;
```

### 6. Subquery

**Subquery** adalah query di dalam query.

**Contoh:**
```sql
-- Mahasiswa dengan umur di atas rata-rata
SELECT * FROM mahasiswas
WHERE umur > (SELECT AVG(umur) FROM mahasiswas);

-- Mahasiswa dari jurusan yang memiliki lebih dari 5 mahasiswa
SELECT * FROM mahasiswas
WHERE jurusan_id IN (
    SELECT jurusan_id 
    FROM mahasiswas 
    GROUP BY jurusan_id 
    HAVING COUNT(*) > 5
);
```

### 7. LIMIT dan OFFSET

**LIMIT** untuk membatasi jumlah hasil:
```sql
-- Ambil 5 data pertama
SELECT * FROM mahasiswas LIMIT 5;

-- Ambil 5 data mulai dari data ke-6 (OFFSET)
SELECT * FROM mahasiswas LIMIT 5 OFFSET 5;
-- atau
SELECT * FROM mahasiswas LIMIT 5, 5; -- LIMIT offset, count
```

**Pagination:**
```sql
-- Page 1 (data 1-10)
SELECT * FROM mahasiswas LIMIT 10 OFFSET 0;

-- Page 2 (data 11-20)
SELECT * FROM mahasiswas LIMIT 10 OFFSET 10;

-- Page 3 (data 21-30)
SELECT * FROM mahasiswas LIMIT 10 OFFSET 20;
```

### 8. Contoh Query Lengkap

**Query Kompleks:**
```sql
-- Cari mahasiswa Informatika yang umurnya antara 18-25, 
-- urutkan berdasarkan umur tertua, ambil 10 data pertama
SELECT 
    m.id,
    m.nama,
    m.umur,
    j.nama AS jurusan
FROM mahasiswas m
INNER JOIN jurusans j ON m.jurusan_id = j.id
WHERE j.nama = 'Informatika'
  AND m.umur BETWEEN 18 AND 25
ORDER BY m.umur DESC
LIMIT 10;
```

---

## 📝 Pretest (3 Soal Ringkas)

1. **Apa itu JOIN?**
   - Jawab: JOIN adalah operasi SQL untuk menggabungkan data dari dua atau lebih tabel berdasarkan relasi (foreign key). Jenis JOIN: INNER JOIN (hanya data yang cocok), LEFT JOIN (semua data kiri + cocok kanan), RIGHT JOIN (semua data kanan + cocok kiri).

2. **Apa fungsi ORDER BY?**
   - Jawab: ORDER BY digunakan untuk mengurutkan hasil query berdasarkan kolom tertentu. Bisa ascending (ASC) atau descending (DESC). Contoh: `ORDER BY nama ASC` untuk urut A-Z, `ORDER BY umur DESC` untuk urut dari tertua.

3. **Query mencari nama yang mengandung "ar"?**
   - Jawab: 
   ```sql
   SELECT * FROM mahasiswas WHERE nama LIKE '%ar%';
   ```
   Operator LIKE dengan wildcard `%` digunakan untuk pattern matching. `%ar%` berarti nama yang mengandung "ar" di mana saja.

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas 1: Query Mencari Data Mahasiswa Paling Tua
Buat query untuk mencari data mahasiswa dengan umur tertinggi (paling tua).

**Cara 1: Menggunakan MAX dan Subquery**
```sql
SELECT * FROM mahasiswas
WHERE umur = (SELECT MAX(umur) FROM mahasiswas);
```

**Cara 2: Menggunakan ORDER BY dan LIMIT**
```sql
SELECT * FROM mahasiswas
ORDER BY umur DESC
LIMIT 1;
```

**Cara 3: Jika ada lebih dari satu dengan umur sama**
```sql
SELECT * FROM mahasiswas
WHERE umur = (SELECT MAX(umur) FROM mahasiswas)
ORDER BY nama;
```

### Tugas 2: Contoh JOIN 2 Tabel
Buat contoh JOIN antara 2 tabel. Misalnya tabel `mahasiswas` dan `jurusans`.

**Langkah:**

1. **Buat tabel jurusans:**
```sql
CREATE TABLE jurusans (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100) NOT NULL
);

INSERT INTO jurusans (nama) VALUES
('Informatika'),
('Teknik Sipil'),
('Arsitektur');
```

2. **Tambahkan kolom jurusan_id ke mahasiswas:**
```sql
ALTER TABLE mahasiswas
ADD COLUMN jurusan_id INT;

-- Update beberapa data
UPDATE mahasiswas SET jurusan_id = 1 WHERE id = 1;
UPDATE mahasiswas SET jurusan_id = 2 WHERE id = 2;
UPDATE mahasiswas SET jurusan_id = 1 WHERE id = 3;
```

3. **Query JOIN:**
```sql
-- INNER JOIN
SELECT 
    m.id,
    m.nama AS nama_mahasiswa,
    m.umur,
    j.nama AS jurusan
FROM mahasiswas m
INNER JOIN jurusans j ON m.jurusan_id = j.id;

-- LEFT JOIN (termasuk mahasiswa tanpa jurusan)
SELECT 
    m.id,
    m.nama AS nama_mahasiswa,
    m.umur,
    j.nama AS jurusan
FROM mahasiswas m
LEFT JOIN jurusans j ON m.jurusan_id = j.id;
```

**Bonus:**
- Hitung jumlah mahasiswa per jurusan
- Cari jurusan yang memiliki mahasiswa paling banyak
- Tampilkan mahasiswa yang tidak punya jurusan (LEFT JOIN dengan WHERE IS NULL)

---

## 💡 Tips

- LIKE dengan `%` sangat berguna untuk search functionality
- ORDER BY bisa multiple columns untuk sorting kompleks
- INNER JOIN untuk data yang pasti ada relasi
- LEFT JOIN untuk data yang mungkin tidak punya relasi
- GROUP BY untuk agregasi data per kategori
- HAVING untuk filter hasil GROUP BY (beda dengan WHERE)
- LIMIT + OFFSET untuk pagination
- Subquery berguna untuk query kompleks, tapi bisa lambat untuk data besar

---

**Selamat belajar! 🚀**

