# PERTEMUAN 5 — MySQL Basic

## 📚 Materi

### 1. Pengenalan Database
Database adalah kumpulan data yang terorganisir dan tersimpan secara sistematis di komputer.

**Konsep Dasar:**
- **Database** - Kumpulan tabel
- **Table** - Kumpulan baris dan kolom
- **Row (Record)** - Satu baris data
- **Column (Field)** - Satu kolom data
- **Primary Key** - Kolom yang unik untuk setiap baris

### 2. Membuat Database

**Sintaks:**
```sql
CREATE DATABASE nama_database;
```

**Contoh:**
```sql
CREATE DATABASE belajar;
```

**Menggunakan Database:**
```sql
USE belajar;
```

**Menampilkan Semua Database:**
```sql
SHOW DATABASES;
```

**Menghapus Database:**
```sql
DROP DATABASE nama_database;
```

### 3. Membuat Table

**Sintaks Dasar:**
```sql
CREATE TABLE nama_tabel (
    kolom1 tipe_data,
    kolom2 tipe_data,
    ...
);
```

**Contoh:**
```sql
CREATE TABLE mahasiswas (
    id INT,
    nama VARCHAR(100),
    email VARCHAR(100),
    umur INT
);
```

### 4. Tipe Data MySQL

#### a. Tipe Data String
- **VARCHAR(n)** - String dengan panjang maksimal n karakter (contoh: VARCHAR(100))
- **CHAR(n)** - String dengan panjang tetap n karakter
- **TEXT** - String panjang (sampai 65,535 karakter)
- **LONGTEXT** - String sangat panjang

**Contoh:**
```sql
nama VARCHAR(100)      -- Maksimal 100 karakter
kode CHAR(10)         -- Tepat 10 karakter
deskripsi TEXT        -- Teks panjang
```

#### b. Tipe Data Numerik
- **INT** - Bilangan bulat (-2,147,483,648 sampai 2,147,483,647)
- **BIGINT** - Bilangan bulat besar
- **FLOAT** - Bilangan desimal
- **DOUBLE** - Bilangan desimal presisi ganda
- **DECIMAL(m,n)** - Bilangan desimal dengan presisi (m = total digit, n = digit desimal)

**Contoh:**
```sql
umur INT
harga DECIMAL(10,2)   -- 10 digit total, 2 digit desimal
nilai FLOAT
```

#### c. Tipe Data Date/Time
- **DATE** - Tanggal (YYYY-MM-DD)
- **TIME** - Waktu (HH:MM:SS)
- **DATETIME** - Tanggal dan waktu (YYYY-MM-DD HH:MM:SS)
- **TIMESTAMP** - Timestamp otomatis

**Contoh:**
```sql
tanggal_lahir DATE
waktu_masuk DATETIME
created_at TIMESTAMP
```

#### d. Tipe Data Boolean
- **BOOLEAN** atau **BOOL** - True (1) atau False (0)
- **TINYINT(1)** - Sering digunakan untuk boolean

**Contoh:**
```sql
is_active BOOLEAN
status TINYINT(1)
```

### 5. Primary Key
Primary Key adalah kolom yang unik untuk setiap baris dan tidak boleh NULL.

**Cara 1: Saat membuat tabel**
```sql
CREATE TABLE mahasiswas (
    id INT PRIMARY KEY,
    nama VARCHAR(100),
    email VARCHAR(100),
    umur INT
);
```

**Cara 2: Auto Increment (Recommended)**
```sql
CREATE TABLE mahasiswas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100),
    email VARCHAR(100),
    umur INT
);
```

### 6. Constraints (Batasan)

- **NOT NULL** - Kolom tidak boleh kosong
- **UNIQUE** - Nilai harus unik
- **DEFAULT** - Nilai default jika tidak diisi
- **AUTO_INCREMENT** - Otomatis bertambah

**Contoh Lengkap:**
```sql
CREATE TABLE mahasiswas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    umur INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 7. Menampilkan Struktur Table

**Menampilkan semua tabel:**
```sql
SHOW TABLES;
```

**Menampilkan struktur tabel:**
```sql
DESCRIBE mahasiswas;
-- atau
DESC mahasiswas;
```

**Menampilkan query CREATE TABLE:**
```sql
SHOW CREATE TABLE mahasiswas;
```

### 8. Mengubah Table

**Menambah kolom:**
```sql
ALTER TABLE mahasiswas 
ADD COLUMN alamat VARCHAR(200);
```

**Menghapus kolom:**
```sql
ALTER TABLE mahasiswas 
DROP COLUMN alamat;
```

**Mengubah kolom:**
```sql
ALTER TABLE mahasiswas 
MODIFY COLUMN nama VARCHAR(150);
```

**Menghapus tabel:**
```sql
DROP TABLE mahasiswas;
```

### 9. Contoh Lengkap

```sql
-- Membuat database
CREATE DATABASE belajar;
USE belajar;

-- Membuat tabel mahasiswas
CREATE TABLE mahasiswas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    umur INT DEFAULT 0,
    jurusan VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Menampilkan struktur
DESCRIBE mahasiswas;
```

---

## 📝 Pretest (3 Soal Ringkas)

1. **Apa itu database?**
   - Jawab: Database adalah kumpulan data yang terorganisir dan tersimpan secara sistematis di komputer. Database terdiri dari tabel-tabel yang berisi data.

2. **Apa fungsi tipe data VARCHAR?**
   - Jawab: VARCHAR digunakan untuk menyimpan string dengan panjang maksimal tertentu. Contoh: VARCHAR(100) berarti maksimal 100 karakter.

3. **Apa itu primary key?**
   - Jawab: Primary Key adalah kolom yang unik untuk setiap baris data dan tidak boleh NULL. Primary Key digunakan untuk mengidentifikasi setiap record secara unik.

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas 1: Membuat Database belajar
Buat database dengan nama `belajar`.

**Query:**
```sql
CREATE DATABASE belajar;
USE belajar;
```

**Verifikasi:**
```sql
SHOW DATABASES;
```

### Tugas 2: Membuat Tabel mahasiswas
Buat tabel `mahasiswas` dengan kolom:
- `id` - INT, AUTO_INCREMENT, PRIMARY KEY
- `nama` - VARCHAR(100), NOT NULL
- `email` - VARCHAR(100), UNIQUE, NOT NULL
- `umur` - INT, DEFAULT 0

**Query:**
```sql
USE belajar;

CREATE TABLE mahasiswas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    umur INT DEFAULT 0
);
```

**Verifikasi:**
```sql
DESCRIBE mahasiswas;
```

**Expected Output:**
```
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| nama  | varchar(100) | NO   |     | NULL    |                |
| email | varchar(100) | NO   | UNI | NULL    |                |
| umur  | int(11)      | YES  |     | 0       |                |
+-------+--------------+------+-----+---------+----------------+
```

---

## 💡 Tips

- Selalu gunakan `USE database_name;` sebelum membuat tabel
- Gunakan AUTO_INCREMENT untuk primary key agar otomatis bertambah
- Gunakan NOT NULL untuk kolom yang wajib diisi
- Gunakan UNIQUE untuk kolom yang harus unik (seperti email)
- Nama tabel biasanya menggunakan plural (mahasiswas, users, products)
- Selalu verifikasi dengan DESCRIBE setelah membuat tabel

---

**Selamat belajar! 🚀**

