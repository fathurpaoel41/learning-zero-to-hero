# PERTEMUAN 6 — SQL CRUD

## 📚 Materi

### 1. Pengenalan CRUD
CRUD adalah singkatan dari:
- **C**reate - Membuat data baru (INSERT)
- **R**ead - Membaca/menampilkan data (SELECT)
- **U**pdate - Mengubah data (UPDATE)
- **D**elete - Menghapus data (DELETE)

### 2. CREATE - INSERT Data

**Sintaks Dasar:**
```sql
INSERT INTO nama_tabel (kolom1, kolom2, ...) 
VALUES (nilai1, nilai2, ...);
```

**Contoh:**
```sql
INSERT INTO mahasiswas (nama, email, umur) 
VALUES ('Budi Santoso', 'budi@example.com', 20);
```

**Insert Multiple Data:**
```sql
INSERT INTO mahasiswas (nama, email, umur) 
VALUES 
    ('Budi Santoso', 'budi@example.com', 20),
    ('Siti Nurhaliza', 'siti@example.com', 21),
    ('Ahmad Dahlan', 'ahmad@example.com', 19);
```

**Insert Tanpa Menyebutkan Kolom (Harus Urut):**
```sql
INSERT INTO mahasiswas 
VALUES (NULL, 'Budi Santoso', 'budi@example.com', 20);
-- NULL untuk AUTO_INCREMENT
```

### 3. READ - SELECT Data

**Menampilkan Semua Data:**
```sql
SELECT * FROM mahasiswas;
```

**Menampilkan Kolom Tertentu:**
```sql
SELECT nama, email FROM mahasiswas;
```

**Menampilkan dengan Kondisi (WHERE):**
```sql
SELECT * FROM mahasiswas WHERE umur > 20;
```

**Menampilkan dengan ORDER BY (Urutkan):**
```sql
-- Urutkan berdasarkan nama (A-Z)
SELECT * FROM mahasiswas ORDER BY nama ASC;

-- Urutkan berdasarkan umur (terbesar ke terkecil)
SELECT * FROM mahasiswas ORDER BY umur DESC;
```

**Menampilkan dengan LIMIT (Batasi Jumlah):**
```sql
-- Tampilkan 5 data pertama
SELECT * FROM mahasiswas LIMIT 5;

-- Tampilkan 5 data mulai dari data ke-3
SELECT * FROM mahasiswas LIMIT 2, 5;
```

### 4. WHERE Clause
Digunakan untuk memfilter data berdasarkan kondisi.

**Operator Perbandingan:**
- `=` - Sama dengan
- `!=` atau `<>` - Tidak sama dengan
- `>` - Lebih besar
- `<` - Lebih kecil
- `>=` - Lebih besar atau sama dengan
- `<=` - Lebih kecil atau sama dengan

**Operator Logika:**
- `AND` - Kedua kondisi harus true
- `OR` - Salah satu kondisi harus true
- `NOT` - Membalikkan kondisi

**Contoh:**
```sql
-- Umur lebih dari 20
SELECT * FROM mahasiswas WHERE umur > 20;

-- Nama sama dengan 'Budi'
SELECT * FROM mahasiswas WHERE nama = 'Budi Santoso';

-- Umur antara 18 dan 25
SELECT * FROM mahasiswas WHERE umur >= 18 AND umur <= 25;

-- Nama mengandung 'Budi' atau umur 20
SELECT * FROM mahasiswas WHERE nama LIKE '%Budi%' OR umur = 20;
```

**LIKE Operator:**
- `%` - Mencocokkan nol atau lebih karakter
- `_` - Mencocokkan satu karakter

```sql
-- Nama dimulai dengan 'B'
SELECT * FROM mahasiswas WHERE nama LIKE 'B%';

-- Nama berakhiran 'o'
SELECT * FROM mahasiswas WHERE nama LIKE '%o';

-- Nama mengandung 'Santoso'
SELECT * FROM mahasiswas WHERE nama LIKE '%Santoso%';
```

### 5. UPDATE - Mengubah Data

**Sintaks:**
```sql
UPDATE nama_tabel 
SET kolom1 = nilai1, kolom2 = nilai2, ...
WHERE kondisi;
```

**⚠️ PENTING: Selalu gunakan WHERE saat UPDATE, jika tidak semua data akan terupdate!**

**Contoh:**
```sql
-- Update satu data berdasarkan id
UPDATE mahasiswas 
SET umur = 21 
WHERE id = 1;

-- Update beberapa kolom
UPDATE mahasiswas 
SET nama = 'Budi Santoso Updated', umur = 22 
WHERE id = 1;

-- Update berdasarkan kondisi
UPDATE mahasiswas 
SET umur = umur + 1 
WHERE umur < 20;
```

### 6. DELETE - Menghapus Data

**Sintaks:**
```sql
DELETE FROM nama_tabel WHERE kondisi;
```

**⚠️ PENTING: Selalu gunakan WHERE saat DELETE, jika tidak semua data akan terhapus!**

**Contoh:**
```sql
-- Hapus data berdasarkan id
DELETE FROM mahasiswas WHERE id = 1;

-- Hapus berdasarkan kondisi
DELETE FROM mahasiswas WHERE umur < 18;

-- Hapus semua data (HATI-HATI!)
DELETE FROM mahasiswas;
-- atau
TRUNCATE TABLE mahasiswas;
```

### 7. Contoh Lengkap CRUD

```sql
-- CREATE - Insert data
INSERT INTO mahasiswas (nama, email, umur) 
VALUES 
    ('Budi Santoso', 'budi@example.com', 20),
    ('Siti Nurhaliza', 'siti@example.com', 21),
    ('Ahmad Dahlan', 'ahmad@example.com', 19);

-- READ - Tampilkan semua
SELECT * FROM mahasiswas;

-- READ - Tampilkan dengan kondisi
SELECT * FROM mahasiswas WHERE umur >= 20;

-- UPDATE - Ubah data
UPDATE mahasiswas 
SET umur = 22 
WHERE id = 1;

-- DELETE - Hapus data
DELETE FROM mahasiswas WHERE id = 3;

-- READ - Verifikasi
SELECT * FROM mahasiswas;
```

---

## 📝 Pretest (3 Soal Ringkas)

1. **Query untuk menampilkan semua data?**
   - Jawab: `SELECT * FROM nama_tabel;`

2. **Query untuk insert 1 data contoh.**
   - Jawab: 
   ```sql
   INSERT INTO mahasiswas (nama, email, umur) 
   VALUES ('Budi', 'budi@example.com', 20);
   ```

3. **Apa fungsi WHERE?**
   - Jawab: WHERE digunakan untuk memfilter data berdasarkan kondisi tertentu. WHERE digunakan di SELECT, UPDATE, dan DELETE untuk menentukan data mana yang akan ditampilkan, diubah, atau dihapus.

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas 1: Insert Minimal 3 Data
Insert minimal 3 data ke tabel `mahasiswas`.

**Query:**
```sql
USE belajar;

INSERT INTO mahasiswas (nama, email, umur) 
VALUES 
    ('Budi Santoso', 'budi@example.com', 20),
    ('Siti Nurhaliza', 'siti@example.com', 21),
    ('Ahmad Dahlan', 'ahmad@example.com', 19),
    ('Dewi Sartika', 'dewi@example.com', 22);
```

**Verifikasi:**
```sql
SELECT * FROM mahasiswas;
```

### Tugas 2: Update dan Delete Data
1. Update satu data (ubah umur mahasiswa dengan id = 1 menjadi 23)
2. Hapus satu data (hapus mahasiswa dengan id = 3)

**Query Update:**
```sql
UPDATE mahasiswas 
SET umur = 23 
WHERE id = 1;
```

**Query Delete:**
```sql
DELETE FROM mahasiswas WHERE id = 3;
```

**Verifikasi:**
```sql
SELECT * FROM mahasiswas;
```

**Bonus:**
- Tampilkan data yang umurnya lebih dari 20
- Tampilkan data yang namanya mengandung 'Budi'
- Urutkan data berdasarkan nama (A-Z)

---

## 💡 Tips

- **SELALU** gunakan WHERE pada UPDATE dan DELETE untuk menghindari perubahan/hapus semua data
- Gunakan SELECT untuk verifikasi sebelum UPDATE atau DELETE
- LIKE dengan % sangat berguna untuk pencarian
- ORDER BY untuk mengurutkan hasil query
- LIMIT untuk membatasi jumlah hasil
- Test query dengan SELECT dulu sebelum UPDATE/DELETE

---

**Selamat belajar! 🚀**

